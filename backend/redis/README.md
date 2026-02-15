# Redis Advanced Internals & Patterns

Tài liệu này tập trung vào các vấn đề **khó và chuyên sâu** khi làm việc với Redis trong các hệ thống High Concurrency (Chịu tải cao).

## 1. Các Vấn Đề "Kinh Điển" Khi Dùng Cache

### 1.1 Cache Penetration (Lủng Cache)

**Vấn đề:** Client liên tục query một dữ liệu **không tồn tại** trong cả Cache và Database (DB). Request đi xuyên qua Cache đánh thẳng vào DB.

- **Nguyên nhân:** Tấn công hoặc lỗi logic code.
- **Giải pháp:**
  - **Cache Null Value:** Nếu DB không có, cache key đó với value `null` (hoặc flag lỗi) với TTL ngắn (VD: 30s).
  - **Bloom Filter:** Dùng cấu trúc Bloom Filter để kiểm tra nhanh xem ID có tồn tại không trước khi query DB.

### 1.2 Cache Avalanche (Sập Cache Diện Rộng/Tuyết Lở)

**Vấn đề:** Hàng loạt key **hết hạn cùng một lúc** (hoặc Redis Node bị crash). Một lượng request khổng lồ ập vào DB cùng lúc gây sập DB.

- **Giải pháp:**
  - **Randomize TTL:** Thêm một khoảng thời gian ngẫu nhiên (Jitter) vào TTL (VD: TTL = 1 giờ + random(0-5 phút)).
  - **High Availability:** Dùng Redis Cluster/Sentinel.
  - **Rate Limiting:** Giới hạn request vào DB.

### 1.3 Cache Breakdown / Thundering Herd (Sập Cache Cục Bộ)

**Vấn đề:** Một **Key Hot** (được truy cập rất nhiều) bị hết hạn. Ngay lập tức, hàng ngàn request đồng thời phát hiện cache miss và cùng lúc chui xuống DB để load data.

- **Giải pháp:**
  - **Mutex Lock (Distributed Lock):** Chỉ cho phép 1 luồng được query DB và cập nhật lại Cache. Các luồng khác phải chờ hoặc retry.
  - **Logical Expiration:** Không set TTL TTL vật lý của Redis, mà lưu `expire_at` trong value. Khi thấy hết hạn, trả về data cũ và async start thread mới để cập nhật data.

---

## 2. Thiết Kế Cơ Chế "Look Aside with Distributed Lock" An Toàn

Dưới đây là implementation mẫu cho pattern **Mutex Lock** để giải quyết vấn đề **Cache Breakdown** và **Thundering Herd**. Code này đảm bảo:

1.  **Chống sập DB:** Chỉ 1 request được chui xuống DB.
2.  **Non-blocking (tương đối):** Các request khác chờ và retry nhẹ nhàng.
3.  **An toàn (Safety):** Dùng `SET NX PX` để atomic và tránh deadlock.
4.  **Hiệu năng:** Double-check locking.

### Implementation Code (TypeScript/NestJS)

```typescript
/**
 * Lấy chi tiết Post với cơ chế Distributed Lock để chống Cache Stampede/Breakdown.
 * @param id Post ID
 * @param currentUserId User đang request (để check like status)
 * @param retries Số lần thử lại (internal use)
 */
async getPostDetailsLock(id: number, currentUserId?: number, retries = 0): Promise<PostWithLikeInfo> {
  const key = `p:${id}`;
  const lockKey = `lock:p:${id}`;
  const MAX_RETRIES = 10;
  const LOCK_TTL_MS = 5000; // 5s tối đa giữ lock

  // 1. Kiểm tra cache trước (Fast path)
  const cached = await this.redisService.get(key);
  if (cached) {
    // Nếu cache hit, return ngay. Lưu ý xử lý phần dynamic data (như isLiked) riêng nếu cần
    return { ...JSON.parse(cached), isLiked: false };
  }

  // 2. Cache Miss -> Thử lấy Lock để rebuild cache
  // Tạo value ngẫu nhiên để định danh "chủ sở hữu" lock (Tránh việc A xoá nhầm lock của B)
  const lockValue = Math.random().toString(36).substring(2) + Date.now().toString(36);

  // SET resource_name my_random_value NX PX 5000
  // NX: Chỉ set nếu key chưa tồn tại (tức là lấy được lock)
  // PX: Tự động hết hạn sau 5000ms (tránh Deadlock nếu app crash)
  const acquired = await this.redisService.set(lockKey, lockValue, 'NX', 'PX', LOCK_TTL_MS);

  if (acquired === 'OK') {
    // Đã lấy được lock - chịu trách nhiệm query DB và update Cache
    try {
      // 2.1 Double check locking:
      // Có thể trong lúc chờ lấy lock, một thread khác đã update cache xong rồi.
      const cachedAgain = await this.redisService.get(key);
      if (cachedAgain) return { ...JSON.parse(cachedAgain), isLiked: false };

      // 2.2 Query DB
      const post = await this.postRepository.findOne({ where: { id }, relations: { author: true } });

      if (!post) {
        // Cache Penetration Protection: Cache null object
        await this.redisService.set(key, JSON.stringify(null), 'EX', 30); // TTL ngắn cho lỗi
        return { isLiked: false } as PostWithLikeInfo;
      }

      // 2.3 Set Cache với Jitter (Chống Cache Avalanche)
      // TTL gốc 300s + random 0-60s
      const ttl = 300 + Math.floor(Math.random() * 60);
      await this.redisService.set(key, JSON.stringify(post), 'EX', ttl);

      return { ...post, isLiked: false };
    } finally {
      // 3. Giải phóng lock an toàn bằng Lua Script
      // Chỉ owner của lock mới được quyền xoá lock
      const script = `
        if redis.call("get", KEYS[1]) == ARGV[1] then
          return redis.call("del", KEYS[1])
        else
          return 0
        end
      `;
      await this.redisService.eval(script, 1, lockKey, lockValue);
    }
  }

  // 4. Không lấy được lock -> Retry (Spin lock)
  if (retries < MAX_RETRIES) {
    // Backoff strategy: Đợi một chút rồi thử lại
    // Thêm Jitter vào thời gian chờ để tránh thundering stampede khi retry
    const delay = 50 + Math.random() * 50;
    await new Promise(res => setTimeout(res, delay));

    // Đệ quy thử lại
    return this.getPostDetailsLock(id, currentUserId, retries + 1);
  }

  // Quá số lần retry mà cache vẫn chưa có -> Hệ thống quá tải
  throw new Error("Hệ thống đang bận, vui lòng thử lại sau");
}
```

### Phân tích kỹ thuật trong đoạn code trên:

1.  **Lock Value Random:** Tại sao `lockValue` cần random? Để đảm bảo tính **Correctness**. Nếu Client A giữ lock 5s, nhưng bị treo 6s (lock tự hết hạn). Client B lấy được lock. Sau đó Client A tỉnh dậy và gọi lệnh DEL key. Nếu không check lockValue, Client A sẽ xoá nhầm lock của Client B.
2.  **Double Check:** Giúp tối ưu hiệu năng, tránh query DB dư thừa ngay sau khi lock vừa được giải phóng bởi thread trước.
3.  **Jitter TTL:** `300 + Math.random() * 60` giúp các key không hết hạn cùng lúc.
4.  **Lua Script:** Đảm bảo thao tác "Kiểm tra value + Xoá lock" diễn ra **Atomic** (nguyên tử), không bị chen ngang.

---

## 3. Distributed Lock - Những cạm bẫy

### 3.1 Single Instance vs Redlock

Mô hình `SET NX PX` ở trên chỉ an toàn tuyệt đối trên **Single Redis Instance**.
Trong môi trường Redis Cluster/Sentinel, có trường hợp:

1. Client A ghi lock vào Master.
2. Master crash **trước khi** kịp sync lock key sang Slave.
3. Slave lên làm Master.
4. Client B ghi lock đó (vì trên Master mới chưa có).
   => **Lock bị gãy (Broken Mutual Exclusion).**

=> **Giải pháp:** Nếu cần an toàn tuyệt đối (như giao dịch tiền tệ), cần dùng thuật toán **Redlock** (lock trên đa số > N/2 nodes). Tuy nhiên với bài toán Cache thông thường, `SET NX PX` là đủ tốt.

### 3.2 Watch Dog (Gia hạn lock)

Nếu query DB mất > 5s (TTL của lock), lock sẽ mất hiệu lực giữa chừng -> 2 thread cùng chạy.

- **Giải pháp:** Cần một background thread (Watch Dog) để "renew" (gia hạn) lock time nếu task chưa xong. (Thư viện Redisson trong Java làm điều này rất tốt).

---

## 4. Big Key & Hot Key

### 4.1 Big Key

- **Là gì?** Key chứa value quá lớn (VD: Hash 1 triệu field, String 500MB).
- **Tác hại:**
  - Làm nghẽn mạng (Network bottleneck).
  - Block Redis khi xoá (Lệnh DEL là O(N) với Hash/List -> Redis đơn luồng sẽ bị treo).
- **Xử lý:**
  - Dùng `UNLINK` thay vì `DEL` (xoá async).
  - Chia nhỏ key (Sharding).

### 4.2 Hot Key

- **Là gì?** Một key được truy cập hàng trăm nghìn qps.
- **Tác hại:** Dồn tải vào 1 node Redis duy nhất trong Cluster.
- **Xử lý:**
  - Local Cache (Guava/Caffeine/In-memory variable) tại phía Application Server.

---

## 5. Tính Nhất Quán Dữ Liệu (Cache Consistency)

Khi update DB, làm sao để Cache đúng?

### Pattern phổ biến: Cache Aside (Lazy Loading)

1. **Update DB.**
2. **Delete Cache.** (Không update cache, vì update phức tạp và race condition).

**Vấn đề Delayed Double Delete:**
Trong môi trường Master-Slave DB, data sync có độ trễ.

1. Thread A update DB (Master).
2. Thread A xoá Cache.
3. Thread B đọc Cache miss -> Đọc DB (có thể trúng Slave chưa có dữ liệu mới) -> Write lại Cache cũ.
   => **Dirty Cache vĩnh viễn.**

**Giải pháp:**

- **Delayed Double Delete:** Update DB -> Del Cache -> Sleep (1-2s) -> Del Cache again.
- **CDC (Change Data Capture):** Lắng nghe Binlog của MySQL để xoá Cache async (dùng Debezium/Canal).

---

## 6. Memory Fragmentation (Phân Mảnh Bộ Nhớ)

**Vấn đề:**
Bạn kiểm tra Redis thấy `used_memory` chỉ 5GB, nhưng hệ điều hành báo tiến trình Redis đang chiếm 10GB (`used_memory_rss`). 5GB kia đi đâu?

- **Nguyên nhân:**
  - Redis giải phóng bộ nhớ (xóa key) nhưng trình quản lý bộ nhớ (jemalloc/libc) chưa trả lại ngay cho OS.
  - Phân mảnh do cấp phát/giải phóng liên tục các object kích thước khác nhau.
- **Tác hại:** Lãng phí RAM, có thể bị OOM Killer của OS giết chết dù "tưởng" là còn RAM.
- **Giải pháp:**
  - **Active Defragmentation:** Bật `activedefrag yes` trong config (Redis 4.0+). Redis sẽ dùng CPU để sắp xếp lại bộ nhớ (trade-off CPU lấy RAM).
  - **Restart:** Cách "nông dân" nhất để reset fragmentation.
  - **Tối ưu Allocator:** Sử dụng jemalloc (default) thay vì libc.

---

## 7. Redis Cluster & Cross-Slot Limitations

**Vấn đề:**
Khi chuyển từ Single Node sang Cluster, rất nhiều feature bị **gãy**:

- `MGET key1 key2` trả về lỗi nếu key1, key2 nằm ở 2 node khác nhau.
- Transaction `MULTI/EXEC` không hoạt động trên nhiều node.
- Lua Script báo lỗi cross slot.

**Nguyên nhân:**
Redis Cluster chia 16384 slot. Dữ liệu sharding tự động. Redis không hỗ trợ atomic operation trên nhiều node vật lý.

**Giải pháp: Hash Tags**
Dùng `{...}` để ép các key liên quan về cùng 1 slot.

- Ví dụ: Thay vì `user:100:profile` và `user:100:posts`, hãy đặt là `{user:100}:profile` và `{user:100}:posts`.
- Redis sẽ chỉ hash phần chuỗi trong `{}` (tức là `user:100`) để tính slot -> Cùng slot -> Cùng node -> Transaction/Batch hoạt động bình thường.

---

## 8. Single Thread & Blocking Operations

**Vấn đề:**
Redis xử lý lệnh bằng **đơn luồng (Single Thread)**. Chỉ một lệnh chậm (Slow Query) sẽ chặn đứng 10,000 request khác đang xếp hàng.

- **Các lệnh tử thần:**
  - `KEYS *`: Quét toàn bộ DB. -> Dùng `SCAN`.
  - `FLUSHALL` / `FLUSHDB`: Xóa sạch DB. -> Dùng `FLUSHALL ASYNC`.
  - Lua Script chạy quá lâu (infinite loop hoặc xử lý nặng). -> Cấu hình `lua-time-limit`.
  - `DEL` một key cực lớn (như đã nói ở mục Big Key). -> Dùng `UNLINK`.

**Monitoring:**

- Dùng lệnh `SLOWLOG GET` để bắt các lệnh chạy chậm hơn quy định (VD > 10ms).

---

## 9. Persistence Nightmare (RDB vs AOF Forking)

**Vấn đề: Latency Spike bí ẩn.**
Hệ thống đang chạy ngon, cứ 5-10 phút lại bị khựng (stutter) khoảng 500ms - 1s mà không thấy Slow Query.

**Nguyên nhân: Copy-On-Write (COW)**

- Khi Redis save RDB hoặc rewrite AOF, nó phải `fork()` ra một process con.
- `fork()` trên Linux copy page table (bảng nhớ).
- Nếu Redis dùng 20GB RAM, việc copy page table tốn thời gian đáng kể, làm Block main thread.

**Giải pháp:**

- **Không bật RDB/AOF trên Master:** Chỉ bật persistence trên Slave (nếu chấp nhận mất ít dữ liệu khi cả cụm sập).
- **Tránh Huge Pages:** Tắt Transparent Huge Pages (THP) của Linux (`echo never > /sys/kernel/mm/transparent_hugepage/enabled`). Huge Pages làm chi phí copy khi fork tăng vọt.
- **Tối ưu tần suất:** Giãn thời gian rewrite AOF hoặc save RDB.

---

## 10. Client Output Buffer Limit

**Vấn đề:**
Redis chiếm RAM tăng đột biến rồi crash, dù lượng key không tăng.
**Nguyên nhân:**

- Có một Slow Consumer (Client đọc chậm) hoặc mạng chập chờn.
- Redis phải lưu kết quả query vào buffer chờ Client nhận. Server gom hàng đống data vào buffer -> Tràn RAM.
- Thường gặp với Pub/Sub khi Sub xử lý không kịp tốc độ Pub.

**Giải pháp:**

- Cấu hình `client-output-buffer-limit`. Nếu buffer vượt quá cache (VD 256MB) hoặc vượt quá soft limit trong thời gian quy định -> Redis chủ động ngắt kết nối Client đó để bảo vệ Server.
