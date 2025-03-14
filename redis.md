# Hướng dẫn sử dụng Redis trong NestJS với thư viện ioredis

## Mục lục
1. [Giới thiệu](#giới-thiệu)
2. [Cài đặt](#cài-đặt)
3. [Cấu hình module Redis](#cấu-hình-module-redis)
4. [Tạo service Redis](#tạo-service-redis)
5. [Các thao tác cơ bản](#các-thao-tác-cơ-bản)
   - [Lưu trữ và truy xuất dữ liệu](#lưu-trữ-và-truy-xuất-dữ-liệu)
   - [Làm việc với hạn sử dụng](#làm-việc-với-hạn-sử-dụng)
   - [Xử lý Hash](#xử-lý-hash)
   - [Xử lý List](#xử-lý-list)
   - [Xử lý Set](#xử-lý-set)
   - [Xử lý Sorted Set](#xử-lý-sorted-set)
6. [Pipeline và Transaction](#pipeline-và-transaction)
7. [Pub/Sub](#pubsub)
8. [Các mẫu ứng dụng thực tế](#các-mẫu-ứng-dụng-thực-tế)

## Giới thiệu

Redis là một hệ thống lưu trữ dữ liệu key-value trong bộ nhớ với hiệu năng cao. Trong NestJS, chúng ta có thể tích hợp Redis thông qua thư viện ioredis. Hướng dẫn này sẽ giúp bạn hiểu cách cấu hình và sử dụng Redis trong ứng dụng NestJS.

## Cài đặt

Đầu tiên, cài đặt các package cần thiết:

```bash
npm install --save ioredis @nestjs/common
```

## Cấu hình module Redis

Tạo module Redis để cung cấp client Redis cho toàn bộ ứng dụng:

```typescript
// redis.module.ts
import { Module, DynamicModule } from '@nestjs/common';
import { Redis } from 'ioredis';
import { RedisService } from './redis.service';

@Module({})
export class RedisModule {
  static register(options: { url: string }): DynamicModule {
    return {
      module: RedisModule,
      providers: [
        {
          provide: 'REDIS_CLIENT',
          useFactory: async () => {
            const redis = new Redis(options.url);
            return redis;
          },
        },
        RedisService,
      ],
      exports: [RedisService],
    };
  }
}
```

## Tạo service Redis

```typescript
// redis.service.ts
import { Injectable, Inject } from '@nestjs/common';
import { Redis } from 'ioredis';

@Injectable()
export class RedisService {
  constructor(@Inject('REDIS_CLIENT') private readonly redis: Redis) {}

  /**
   * Lưu giá trị vào Redis
   * @param key - Khóa để lưu trữ giá trị
   * @param value - Giá trị cần lưu
   * @param ttl - Thời gian sống (tính bằng giây), tùy chọn
   */
  async set(key: string, value: string, ttl?: number): Promise<'OK'> {
    if (ttl) {
      return this.redis.set(key, value, 'EX', ttl);
    }
    return this.redis.set(key, value);
  }

  /**
   * Lấy giá trị từ Redis theo khóa
   * @param key - Khóa cần truy vấn
   * @returns Giá trị của khóa, hoặc null nếu không tồn tại
   */
  async get(key: string): Promise<string | null> {
    return this.redis.get(key);
  }

  /**
   * Xóa một hoặc nhiều khóa khỏi Redis
   * @param keys - Các khóa cần xóa
   * @returns Số lượng khóa đã xóa thành công
   */
  async del(...keys: string[]): Promise<number> {
    return this.redis.del(keys);
  }

  /**
   * Kiểm tra xem khóa có tồn tại không
   * @param key - Khóa cần kiểm tra
   * @returns 1 nếu khóa tồn tại, 0 nếu không
   */
  async exists(key: string): Promise<number> {
    return this.redis.exists(key);
  }

  /**
   * Đặt thời gian sống cho khóa
   * @param key - Khóa cần đặt thời gian sống
   * @param seconds - Số giây
   * @returns 1 nếu thành công, 0 nếu khóa không tồn tại
   */
  async expire(key: string, seconds: number): Promise<number> {
    return this.redis.expire(key, seconds);
  }

  /**
   * Tăng giá trị số nguyên của khóa lên 1
   * @param key - Khóa cần tăng giá trị
   * @returns Giá trị sau khi tăng
   */
  async incr(key: string): Promise<number> {
    return this.redis.incr(key);
  }

  /**
   * Tăng giá trị số nguyên của khóa theo một số lượng nhất định
   * @param key - Khóa cần tăng giá trị
   * @param increment - Số lượng cần tăng
   * @returns Giá trị sau khi tăng
   */
  async incrBy(key: string, increment: number): Promise<number> {
    return this.redis.incrby(key, increment);
  }

  /**
   * Giảm giá trị số nguyên của khóa xuống 1
   * @param key - Khóa cần giảm giá trị
   * @returns Giá trị sau khi giảm
   */
  async decr(key: string): Promise<number> {
    return this.redis.decr(key);
  }

  /**
   * Giảm giá trị số nguyên của khóa theo một số lượng nhất định
   * @param key - Khóa cần giảm giá trị
   * @param decrement - Số lượng cần giảm
   * @returns Giá trị sau khi giảm
   */
  async decrBy(key: string, decrement: number): Promise<number> {
    return this.redis.decrby(key, decrement);
  }

  // ------ CÁC PHƯƠNG THỨC LÀM VIỆC VỚI HASH ------

  /**
   * Đặt một trường trong hash
   * @param key - Khóa của hash
   * @param field - Tên trường
   * @param value - Giá trị của trường
   * @returns 1 nếu trường được tạo mới, 0 nếu trường đã tồn tại và được cập nhật
   */
  async hSet(key: string, field: string, value: string | number): Promise<number> {
    return this.redis.hset(key, field, value.toString());
  }

  /**
   * Đặt nhiều trường trong hash cùng lúc
   * @param key - Khóa của hash
   * @param fieldValues - Object chứa các cặp trường/giá trị
   * @returns OK nếu thành công
   */
  async hMSet(key: string, fieldValues: Record<string, any>): Promise<'OK'> {
    return this.redis.hmset(key, fieldValues);
  }

  /**
   * Lấy giá trị của một trường trong hash
   * @param key - Khóa của hash
   * @param field - Tên trường
   * @returns Giá trị của trường, hoặc null nếu không tồn tại
   */
  async hGet(key: string, field: string): Promise<string | null> {
    return this.redis.hget(key, field);
  }

  /**
   * Lấy giá trị của nhiều trường trong hash
   * @param key - Khóa của hash
   * @param fields - Danh sách tên trường
   * @returns Mảng các giá trị tương ứng
   */
  async hMGet(key: string, ...fields: string[]): Promise<(string | null)[]> {
    return this.redis.hmget(key, ...fields);
  }

  /**
   * Lấy tất cả các trường và giá trị trong hash
   * @param key - Khóa của hash
   * @returns Object chứa các cặp trường/giá trị
   */
  async hGetAll(key: string): Promise<Record<string, string>> {
    return this.redis.hgetall(key);
  }

  /**
   * Xóa một hoặc nhiều trường khỏi hash
   * @param key - Khóa của hash
   * @param fields - Danh sách tên trường cần xóa
   * @returns Số lượng trường đã xóa
   */
  async hDel(key: string, ...fields: string[]): Promise<number> {
    return this.redis.hdel(key, ...fields);
  }

  /**
   * Kiểm tra xem trường có tồn tại trong hash không
   * @param key - Khóa của hash
   * @param field - Tên trường
   * @returns 1 nếu trường tồn tại, 0 nếu không
   */
  async hExists(key: string, field: string): Promise<number> {
    return this.redis.hexists(key, field);
  }

  // ------ CÁC PHƯƠNG THỨC LÀM VIỆC VỚI LIST ------

  /**
   * Thêm một hoặc nhiều giá trị vào đầu list
   * @param key - Khóa của list
   * @param values - Các giá trị cần thêm
   * @returns Độ dài của list sau khi thêm
   */
  async lPush(key: string, ...values: string[]): Promise<number> {
    return this.redis.lpush(key, ...values);
  }

  /**
   * Thêm một hoặc nhiều giá trị vào cuối list
   * @param key - Khóa của list
   * @param values - Các giá trị cần thêm
   * @returns Độ dài của list sau khi thêm
   */
  async rPush(key: string, ...values: string[]): Promise<number> {
    return this.redis.rpush(key, ...values);
  }

  /**
   * Lấy và xóa phần tử đầu tiên của list
   * @param key - Khóa của list
   * @returns Phần tử đầu tiên, hoặc null nếu list rỗng
   */
  async lPop(key: string): Promise<string | null> {
    return this.redis.lpop(key);
  }

  /**
   * Lấy và xóa phần tử cuối cùng của list
   * @param key - Khóa của list
   * @returns Phần tử cuối cùng, hoặc null nếu list rỗng
   */
  async rPop(key: string): Promise<string | null> {
    return this.redis.rpop(key);
  }

  /**
   * Lấy phần tử của list theo vị trí
   * @param key - Khóa của list
   * @param index - Vị trí của phần tử (0 là đầu tiên, -1 là cuối cùng)
   * @returns Phần tử tại vị trí đó, hoặc null nếu vị trí không hợp lệ
   */
  async lIndex(key: string, index: number): Promise<string | null> {
    return this.redis.lindex(key, index);
  }

  /**
   * Lấy danh sách các phần tử trong một phạm vi của list
   * @param key - Khóa của list
   * @param start - Vị trí bắt đầu
   * @param stop - Vị trí kết thúc
   * @returns Mảng các phần tử trong phạm vi
   */
  async lRange(key: string, start: number, stop: number): Promise<string[]> {
    return this.redis.lrange(key, start, stop);
  }

  /**
   * Lấy độ dài của list
   * @param key - Khóa của list
   * @returns Độ dài của list
   */
  async lLen(key: string): Promise<number> {
    return this.redis.llen(key);
  }

  // ------ CÁC PHƯƠNG THỨC LÀM VIỆC VỚI SET ------

  /**
   * Thêm một hoặc nhiều thành viên vào set
   * @param key - Khóa của set
   * @param members - Các thành viên cần thêm
   * @returns Số lượng thành viên mới được thêm vào (không tính các thành viên đã tồn tại)
   */
  async sAdd(key: string, ...members: string[]): Promise<number> {
    return this.redis.sadd(key, ...members);
  }

  /**
   * Xóa một hoặc nhiều thành viên khỏi set
   * @param key - Khóa của set
   * @param members - Các thành viên cần xóa
   * @returns Số lượng thành viên đã được xóa
   */
  async sRem(key: string, ...members: string[]): Promise<number> {
    return this.redis.srem(key, ...members);
  }

  /**
   * Lấy tất cả các thành viên của set
   * @param key - Khóa của set
   * @returns Mảng các thành viên
   */
  async sMembers(key: string): Promise<string[]> {
    return this.redis.smembers(key);
  }

  /**
   * Kiểm tra xem một thành viên có tồn tại trong set không
   * @param key - Khóa của set
   * @param member - Thành viên cần kiểm tra
   * @returns 1 nếu thành viên tồn tại, 0 nếu không
   */
  async sIsMember(key: string, member: string): Promise<number> {
    return this.redis.sismember(key, member);
  }

  /**

/**
   * Lấy số lượng thành viên trong set
   * @param key - Khóa của set
   * @returns Số lượng thành viên
   */
  async sCard(key: string): Promise<number> {
    return this.redis.scard(key);
  }

  /**
   * Tìm phần giao của hai hoặc nhiều set
   * @param keys - Các khóa của các set
   * @returns Mảng các phần tử chung giữa các set
   */
  async sInter(...keys: string[]): Promise<string[]> {
    return this.redis.sinter(...keys);
  }

  /**
   * Tìm phần hợp của hai hoặc nhiều set
   * @param keys - Các khóa của các set
   * @returns Mảng các phần tử có trong ít nhất một set
   */
  async sUnion(...keys: string[]): Promise<string[]> {
    return this.redis.sunion(...keys);
  }

  /**
   * Tìm phần hiệu của hai set (phần tử có trong set thứ nhất nhưng không có trong set thứ hai)
   * @param key1 - Khóa của set thứ nhất
   * @param key2 - Khóa của set thứ hai
   * @returns Mảng các phần tử thuộc phần hiệu
   */
  async sDiff(key1: string, key2: string): Promise<string[]> {
    return this.redis.sdiff(key1, key2);
  }

  // ------ CÁC PHƯƠNG THỨC LÀM VIỆC VỚI SORTED SET ------

  /**
   * Thêm một hoặc nhiều thành viên vào sorted set với điểm số
   * @param key - Khóa của sorted set
   * @param scoreMembers - Mảng các cặp [score, member]
   * @returns Số lượng thành viên mới được thêm vào
   */
  async zAdd(key: string, ...scoreMembers: (string | number)[]): Promise<number> {
    return this.redis.zadd(key, ...scoreMembers);
  }

  /**
   * Lấy điểm số của một thành viên trong sorted set
   * @param key - Khóa của sorted set
   * @param member - Thành viên cần lấy điểm số
   * @returns Điểm số của thành viên, hoặc null nếu thành viên không tồn tại
   */
  async zScore(key: string, member: string): Promise<string | null> {
    return this.redis.zscore(key, member);
  }

  /**
   * Lấy danh sách các thành viên trong sorted set theo phạm vi điểm số
   * @param key - Khóa của sorted set
   * @param min - Điểm số nhỏ nhất
   * @param max - Điểm số lớn nhất
   * @returns Mảng các thành viên trong phạm vi điểm số
   */
  async zRangeByScore(key: string, min: number | string, max: number | string): Promise<string[]> {
    return this.redis.zrangebyscore(key, min, max);
  }

  /**
   * Lấy danh sách các thành viên trong sorted set theo phạm vi thứ hạng
   * @param key - Khóa của sorted set
   * @param start - Thứ hạng bắt đầu (0 là thành viên có điểm số thấp nhất)
   * @param stop - Thứ hạng kết thúc
   * @returns Mảng các thành viên trong phạm vi thứ hạng
   */
  async zRange(key: string, start: number, stop: number): Promise<string[]> {
    return this.redis.zrange(key, start, stop);
  }

  /**
   * Xóa một hoặc nhiều thành viên khỏi sorted set
   * @param key - Khóa của sorted set
   * @param members - Các thành viên cần xóa
   * @returns Số lượng thành viên đã xóa
   */
  async zRem(key: string, ...members: string[]): Promise<number> {
    return this.redis.zrem(key, ...members);
  }

  /**
   * Lấy số lượng thành viên trong sorted set
   * @param key - Khóa của sorted set
   * @returns Số lượng thành viên
   */
  async zCard(key: string): Promise<number> {
    return this.redis.zcard(key);
  }

  /**
   * Tăng điểm số của một thành viên trong sorted set
   * @param key - Khóa của sorted set
   * @param increment - Số điểm cần tăng
   * @param member - Thành viên cần tăng điểm
   * @returns Điểm số mới của thành viên
   */
  async zIncrBy(key: string, increment: number, member: string): Promise<string> {
    return this.redis.zincrby(key, increment, member);
  }

  // ------ CÁC PHƯƠNG THỨC KHÁC ------

  /**
   * Lấy thời gian sống còn lại của khóa (tính bằng giây)
   * @param key - Khóa cần kiểm tra
   * @returns Thời gian sống còn lại (giây), -1 nếu khóa không có thời gian sống, -2 nếu khóa không tồn tại
   */
  async ttl(key: string): Promise<number> {
    return this.redis.ttl(key);
  }

  /**
   * Tìm kiếm các khóa phù hợp với mẫu
   * @param pattern - Mẫu tìm kiếm (VD: user:*, user:???)
   * @returns Mảng các khóa phù hợp
   */
  async keys(pattern: string): Promise<string[]> {
    return this.redis.keys(pattern);
  }

  /**
   * Tạo transaction để thực hiện nhiều lệnh liên tiếp
   * @returns Đối tượng transaction
   */
  multi(): Redis.Pipeline {
    return this.redis.multi();
  }

  /**
   * Tạo pipeline để thực hiện nhiều lệnh liên tiếp mà không cần đợi kết quả
   * @returns Đối tượng pipeline
   */
  pipeline(): Redis.Pipeline {
    return this.redis.pipeline();
  }

  /**
   * Đăng ký một callback để xử lý tin nhắn từ kênh
   * @param channel - Tên kênh
   * @param callback - Hàm xử lý tin nhắn
   */
  async subscribe(channel: string, callback: (channel: string, message: string) => void): Promise<void> {
    await this.redis.subscribe(channel);
    this.redis.on('message', callback);
  }

  /**
   * Gửi tin nhắn đến một kênh
   * @param channel - Tên kênh
   * @param message - Nội dung tin nhắn
   * @returns Số lượng client đã nhận tin nhắn
   */
  async publish(channel: string, message: string): Promise<number> {
    return this.redis.publish(channel, message);
  }

  /**
   * Hủy đăng ký khỏi một kênh
   * @param channel - Tên kênh
   */
  async unsubscribe(channel: string): Promise<void> {
    await this.redis.unsubscribe(channel);
  }

  /**
   * Lấy client Redis gốc để thực hiện các thao tác nâng cao
   * @returns Client Redis gốc
   */
  getClient(): Redis {
    return this.redis;
  }
}
```

## Đăng ký module Redis trong ứng dụng NestJS

```typescript
// app.module.ts
import { Module } from '@nestjs/common';
import { RedisModule } from './redis/redis.module';

@Module({
  imports: [
    RedisModule.register({
      url: 'redis://localhost:6379',
    }),
  ],
})
export class AppModule {}
```

## Các thao tác cơ bản

### Lưu trữ và truy xuất dữ liệu

#### Lưu dữ liệu

```typescript
// Lưu dữ liệu vào Redis
await this.redisService.set('key', 'value');

// Lưu dữ liệu với thời gian sống (TTL)
await this.redisService.set('key', 'value', 3600); // hết hạn sau 1 giờ
```

#### Truy xuất dữ liệu

```typescript
// Lấy dữ liệu từ Redis
const value = await this.redisService.get('key');

// Kiểm tra xem khóa có tồn tại không
const exists = await this.redisService.exists('key');
```

#### Xóa dữ liệu

```typescript
// Xóa một khóa
await this.redisService.del('key');

// Xóa nhiều khóa
await this.redisService.del('key1', 'key2', 'key3');
```

### Làm việc với hạn sử dụng

```typescript
// Đặt thời gian sống cho khóa
await this.redisService.expire('key', 3600); // hết hạn sau 1 giờ

// Kiểm tra thời gian sống còn lại của khóa
const ttl = await this.redisService.ttl('key');
```

### Xử lý Hash

Hash là một cấu trúc dữ liệu có nhiều trường giá trị, rất hữu ích để lưu trữ đối tượng.

```typescript
// Lưu trữ một trường trong hash
await this.redisService.hSet('user:100', 'name', 'Nguyễn Văn A');
await this.redisService.hSet('user:100', 'email', 'nguyenvana@example.com');

// Lưu trữ nhiều trường cùng lúc
await this.redisService.hMSet('user:100', {
  name: 'Nguyễn Văn A',
  email: 'nguyenvana@example.com',
  age: 30,
});

// Lấy một trường từ hash
const name = await this.redisService.hGet('user:100', 'name');

// Lấy nhiều trường từ hash
const [name, email] = await this.redisService.hMGet('user:100', 'name', 'email');

// Lấy tất cả các trường trong hash
const user = await this.redisService.hGetAll('user:100');
```

### Xử lý List

List là danh sách các phần tử có thứ tự, cho phép thêm/xóa từ đầu hoặc cuối danh sách.

```typescript
// Thêm phần tử vào đầu danh sách
await this.redisService.lPush('notifications', 'New message');

// Thêm phần tử vào cuối danh sách
await this.redisService.rPush('notifications', 'New message');

// Lấy và xóa phần tử đầu tiên
const firstItem = await this.redisService.lPop('notifications');

// Lấy và xóa phần tử cuối cùng
const lastItem = await this.redisService.rPop('notifications');

// Lấy tất cả các phần tử trong danh sách
const allItems = await this.redisService.lRange('notifications', 0, -1);
```

### Xử lý Set

Set là tập hợp các phần tử không có thứ tự và không có phần tử trùng lặp.

```typescript
// Thêm phần tử vào set
await this.redisService.sAdd('tags', 'nestjs', 'redis', 'typescript');

// Kiểm tra phần tử có trong set không
const isMember = await this.redisService.sIsMember('tags', 'nestjs');

// Lấy tất cả các phần tử trong set
const allTags = await this.redisService.sMembers('tags');

// Xóa phần tử khỏi set
await this.redisService.sRem('tags', 'typescript');
```

### Xử lý Sorted Set

Sorted Set là tập hợp các phần tử không trùng lặp, mỗi phần tử được gắn với một điểm số, giúp sắp xếp theo thứ tự.

```typescript
// Thêm phần tử vào sorted set với điểm số
await this.redisService.zAdd('leaderboard', 100, 'user1', 200, 'user2', 150, 'user3');

// Lấy điểm số của một phần tử
const score = await this.redisService.zScore('leaderboard', 'user1');

// Lấy danh sách phần tử theo thứ hạng (từ thấp đến cao)
const topUsers = await this.redisService.zRange('leaderboard', 0, 2);

// Lấy danh sách phần tử theo phạm vi điểm số
const usersInRange = await this.redisService.zRangeByScore('leaderboard', 100, 200);
```

## Pipeline và Transaction

Pipeline cho phép gửi nhiều lệnh cùng lúc để giảm độ trễ mạng. Transaction đảm bảo các lệnh được thực hiện cùng nhau hoặc không thực hiện gì cả.

## Pipeline và Transaction

Pipeline cho phép gửi nhiều lệnh cùng lúc để giảm độ trễ mạng. Transaction đảm bảo các lệnh được thực hiện cùng nhau hoặc không thực hiện gì cả.

### Pipeline
```typescript
// Tạo pipeline
const pipeline = this.redisService.pipeline();
// Thêm các lệnh vào pipeline
pipeline.set('key1', 'value1');
pipeline.set('key2', 'value2');
pipeline.get('key1');
pipeline.get('key2');
// Thực thi pipeline
const results = await pipeline.exec();
// Kết quả trả về là mảng [err, result] cho mỗi lệnh
```

### Transaction
Transaction trong Redis được thực hiện bằng cách sử dụng lệnh `MULTI` và `EXEC`. Các lệnh bên trong transaction sẽ được thực thi tuần tự và đồng bộ.
```typescript
// Bắt đầu transaction
const multi = this.redisService.multi();
multi.set('balance', '100');
multi.decrby('balance', 10);
multi.incrby('balance', 20);
// Thực thi transaction
const transactionResults = await multi.exec();
```

## Pub/Sub
Redis hỗ trợ mô hình **Publisher/Subscriber** để gửi và nhận thông điệp theo thời gian thực.

### **Publisher**
```typescript
await this.redisService.publish('notifications', 'New user signed up!');
```

### **Subscriber**
```typescript
this.redisService.subscribe('notifications', (message, channel) => {
  console.log(`Received message: ${message} from channel: ${channel}`);
});
```



# Redis Interview Knowledge Guide

## Nội dung

- [Kiến thức cơ bản về Redis](#kiến-thức-cơ-bản-về-redis)
- [Cấu trúc dữ liệu và lệnh Redis](#cấu-trúc-dữ-liệu-và-lệnh-redis)
- [Kiến trúc và hoạt động của Redis](#kiến-trúc-và-hoạt-động-của-redis)
- [Chiến lược lưu trữ và quản lý bộ nhớ](#chiến-lược-lưu-trữ-và-quản-lý-bộ-nhớ)
- [Redis Transactions và Concurrency](#redis-transactions-và-concurrency)
- [Mở rộng và High Availability](#mở-rộng-và-high-availability)
- [Bảo mật Redis](#bảo-mật-redis)
- [Use cases và Best Practices](#use-cases-và-best-practices)
- [Câu hỏi thường gặp trong phỏng vấn](#câu-hỏi-thường-gặp-trong-phỏng-vấn)

## Kiến thức cơ bản về Redis

### Redis là gì?
Redis (Remote Dictionary Server) là một hệ thống lưu trữ dữ liệu key-value trong bộ nhớ, mã nguồn mở, có hiệu năng cao và hỗ trợ nhiều kiểu dữ liệu khác nhau.

### Những đặc điểm chính của Redis
1. **In-memory storage**: Lưu trữ dữ liệu trong RAM, mang lại tốc độ cực nhanh.
2. **Persistence**: Hỗ trợ lưu trữ dữ liệu xuống disk để tránh mất dữ liệu khi khởi động lại.
3. **Đa dạng kiểu dữ liệu**: Strings, Lists, Sets, Sorted Sets, Hashes, Streams, Bitmaps, HyperLogLogs, Geospatial indexes.
4. **Pub/Sub**: Hỗ trợ giao tiếp đa chiều giữa các ứng dụng.
5. **Lua scripting**: Cho phép thực hiện các script Lua trên server.
6. **Transactions**: Hỗ trợ nhóm các lệnh để thực hiện một cách nguyên tử.
7. **Automatic failover**: Với Redis Sentinel hoặc Redis Cluster.

### Điểm mạnh của Redis
- Tốc độ xử lý nhanh (thường đạt 100,000+ operations/second)
- Đơn giản, dễ sử dụng
- Hỗ trợ nhiều ngôn ngữ lập trình
- Phù hợp cho cache, bộ đếm, hàng đợi, real-time analytics

### Điểm yếu của Redis
- Giới hạn về kích thước dữ liệu (bởi RAM)
- Không phải giải pháp lưu trữ dữ liệu chính thức dài hạn
- Không hỗ trợ truy vấn phức tạp như SQL

## Cấu trúc dữ liệu và lệnh Redis

### String
- Kiểu dữ liệu cơ bản nhất, có thể lưu trữ binary data
- **Lệnh**: SET, GET, INCR, DECR, EXPIRE

```
SET user:1:name "John"
GET user:1:name
INCR counter
EXPIRE session:1234 3600
```

### Lists
- Danh sách các strings được liên kết theo thứ tự
- **Lệnh**: LPUSH, RPUSH, LPOP, RPOP, LRANGE

```
LPUSH notifications "New message"
RPOP notifications
LRANGE notifications 0 -1
```

### Sets
- Tập hợp các string không có thứ tự và không trùng lặp
- **Lệnh**: SADD, SREM, SMEMBERS, SINTER, SUNION

```
SADD tags "redis" "database" "nosql"
SMEMBERS tags
SINTER tags1 tags2
```

### Sorted Sets
- Giống Sets nhưng mỗi phần tử được gắn với điểm số, sắp xếp theo điểm số
- **Lệnh**: ZADD, ZRANGE, ZRANK, ZSCORE

```
ZADD leaderboard 100 "player1" 200 "player2"
ZRANGE leaderboard 0 -1 WITHSCORES
```

### Hashes
- Lưu trữ hash table của các cặp key-value
- **Lệnh**: HSET, HGET, HMSET, HGETALL

```
HSET user:1 name "John" email "john@example.com"
HGET user:1 name
HGETALL user:1
```

### Streams
- Kiểu dữ liệu mới dùng cho message broker
- **Lệnh**: XADD, XREAD, XRANGE

```
XADD mystream * name "John" age "30"
XREAD COUNT 2 STREAMS mystream 0
```

### Bitmaps và HyperLogLogs
- Bitmap: Chuỗi bit có thể sử dụng làm bộ đếm hiệu quả
- HyperLogLog: Cấu trúc dữ liệu để ước tính số lượng phần tử riêng biệt

```
SETBIT users:active:20240313 123 1
PFADD visitors "user1" "user2" "user3"
PFCOUNT visitors
```

## Kiến trúc và hoạt động của Redis

### Single-threaded model
Redis chủ yếu hoạt động trên một luồng duy nhất để xử lý lệnh nhưng có thể sử dụng nhiều luồng cho một số tác vụ như:
- I/O đĩa
- Giải phóng bộ nhớ
- Xử lý connections

### Event loop
- Redis sử dụng mô hình I/O multiplexing (select/epoll/kqueue)
- Xử lý đồng thời nhiều connections mà không cần nhiều thread

### Thời gian thực thi lệnh
- Hầu hết các lệnh có độ phức tạp O(1) hoặc O(log n)
- Một số lệnh có thể chặn luồng chính (như KEYS, FLUSHALL) và nên tránh trong môi trường production

### Ảnh hưởng của thiết kế single-threaded
- Đơn giản hóa mã nguồn, giảm lỗi đồng thời
- Không cần mutexes hoặc locks
- Mỗi lệnh Redis được thực hiện tuần tự, đảm bảo tính nhất quán

## Chiến lược lưu trữ và quản lý bộ nhớ

### Persistence
- **RDB (Redis Database)**: Snapshot của dữ liệu tại thời điểm cụ thể
    - Ưu điểm: Tệp nhỏ, khôi phục nhanh
    - Nhược điểm: Có thể mất dữ liệu giữa các lần snapshot
- **AOF (Append Only File)**: Ghi lại tất cả các lệnh ghi
    - Ưu điểm: Bền vững hơn, ít mất dữ liệu
    - Nhược điểm: Tệp lớn hơn, khôi phục chậm hơn
- Có thể kết hợp cả hai để tận dụng ưu điểm

### Eviction policies
Redis có các chính sách xóa khi bộ nhớ đầy:
- **noeviction**: Báo lỗi khi bộ nhớ đầy
- **allkeys-lru**: Xóa ít được sử dụng nhất
- **volatile-lru**: Xóa ít được sử dụng nhất trong số khóa có TTL
- **allkeys-random**: Xóa ngẫu nhiên
- **volatile-random**: Xóa ngẫu nhiên trong số khóa có TTL
- **volatile-ttl**: Xóa khóa có TTL ngắn nhất

### Tối ưu hóa bộ nhớ
- **Đặt maxmemory**: Giới hạn bộ nhớ Redis được sử dụng
- **Redis Ziplist**: Biểu diễn nhỏ gọn cho các collections nhỏ
- **Redis Modules**: Mở rộng Redis với các module tùy chỉnh

## Redis Transactions và Concurrency

### Transactions
Redis hỗ trợ transactions thông qua lệnh MULTI, EXEC, DISCARD và WATCH:

```
MULTI
SET user:1:name "John"
INCR user:1:visits
EXEC
```

### Đặc điểm của Redis transactions
- **Atomicity**: Tất cả lệnh trong transaction được thực hiện hoặc không có lệnh nào được thực hiện
- **Isolation**: Các lệnh trong transaction không bị ảnh hưởng bởi những lệnh từ transaction khác
- **Không có rollback**: Nếu một lệnh thất bại, các lệnh khác vẫn được thực hiện

### Optimistic locking với WATCH
- WATCH dùng để theo dõi sự thay đổi của một khóa
- Transaction thất bại nếu khóa được watch thay đổi

```
WATCH user:1:balance
MULTI
DECRBY user:1:balance 100
INCRBY user:2:balance 100
EXEC
```

## Mở rộng và High Availability

### Redis Replication
- **Master-Replica**: Một master có thể có nhiều replicas
- Replicas có thể đọc từ master và phục vụ truy vấn đọc
- Hỗ trợ asynchronous replication

### Redis Sentinel
- Giải pháp high availability cho Redis
- Theo dõi master và replicas
- Thực hiện failover tự động khi master gặp sự cố
- Thông báo cho các ứng dụng về thay đổi cấu hình

### Redis Cluster
- Mô hình phân tán cho Redis
- Tự động phân vùng dữ liệu
- Không cần proxy
- Linear scalability đến hàng nghìn node
- Tự động phát hiện và xử lý khi node fail

## Bảo mật Redis

### Phương pháp xác thực
- Xác thực bằng mật khẩu
- TLS/SSL cho mã hóa kết nối
- ACL (Access Control Lists) từ Redis 6.0

### Bảo mật Redis
- Không để Redis mở ra internet
- Đặt mật khẩu mạnh
- Vô hiệu hóa các lệnh nguy hiểm (như FLUSHALL)
- Sử dụng Redis với tài khoản hạn chế quyền
- Thiết lập tường lửa

## Use cases và Best Practices

### Các ứng dụng phổ biến
- **Caching**: Giảm tải cho database
- **Session storage**: Lưu trữ phiên đăng nhập
- **Queues/Job systems**: Hàng đợi công việc
- **Leaderboards/Counting**: Bảng xếp hạng, đếm số lượt truy cập
- **Real-time analytics**: Phân tích dữ liệu thời gian thực
- **Pub/Sub**: Messaging giữa các services

### Best Practices
- Đặt TTL cho các khóa khi phù hợp
- Sử dụng naming conventions cho khóa
- Tránh các lệnh chặn như KEYS trong production
- Sử dụng pipeline để giảm độ trễ mạng
- Định kỳ backup dữ liệu
- Theo dõi memory usage và hit rate

## Câu hỏi thường gặp trong phỏng vấn

### Câu hỏi cơ bản

1. **Redis là gì và nó khác với database quan hệ như thế nào?**
   - Redis là in-memory data store, tập trung vào tốc độ và đơn giản
   - DB quan hệ phức tạp hơn, có schema, và hỗ trợ các truy vấn phức tạp

2. **Khi nào nên sử dụng Redis?**
   - Khi cần tốc độ cao
   - Làm cache
   - Lưu trữ dữ liệu tạm thời
   - Messaging
   - Bảng xếp hạng, đếm, phân tích thời gian thực

3. **Redis lưu trữ dữ liệu như thế nào?**
   - Dữ liệu được lưu trong RAM
   - Có thể persistence xuống disk bằng RDB hoặc AOF
   - Sử dụng cấu trúc dữ liệu đơn giản và hiệu quả

4. **Giải thích về Redis persistence**
   - RDB: Snapshot tại thời điểm cụ thể
   - AOF: Ghi lại tất cả các lệnh ghi

### Câu hỏi trung cấp

5. **Làm thế nào để cache dữ liệu hiệu quả với Redis?**
   - Sử dụng TTL cho các cache entries
   - Sử dụng cấu trúc dữ liệu phù hợp
   - Xử lý cache invalidation
   - Xử lý cache stampede (nhiều request cùng build cache)

6. **Các chiến lược cache invalidation?**
   - Time-based expiration
   - Write-through cache
   - Cache-aside pattern
   - Event-based invalidation

7. **Redis transactions hoạt động như thế nào?**
   - Bắt đầu với MULTI
   - Thêm các lệnh vào queue
   - EXEC để thực thi tất cả
   - Không có rollback

8. **Làm thế nào để xử lý race conditions trong Redis?**
   - Sử dụng WATCH để optimistic locking
   - Sử dụng Lua scripts để đảm bảo atomicity
   - Thiết kế các operations không xung đột

### Câu hỏi nâng cao

9. **So sánh Redis Sentinel và Redis Cluster**
   - **Redis Sentinel**:
     - Tập trung vào high availability và failover
     - Không hỗ trợ sharding dữ liệu
     - Đơn giản hơn để triển khai và quản lý
     - Thích hợp cho hệ thống nhỏ và trung bình
   - **Redis Cluster**:
     - Hỗ trợ sharding dữ liệu tự động
     - Khả năng mở rộng theo chiều ngang
     - Phức tạp hơn trong triển khai và quản lý
     - Thích hợp cho hệ thống lớn với nhiều dữ liệu

10. **Làm thế nào để giải quyết vấn đề tràn bộ nhớ trong Redis?**
    - Sử dụng eviction policies phù hợp
    - Thiết lập giới hạn maxmemory
    - Sử dụng expire cho các khóa
    - Tối ưu hóa cấu trúc dữ liệu
    - Shard dữ liệu qua nhiều instances

11. **Làm thế nào để tối ưu hóa hiệu suất Redis?**
    - Sử dụng pipelining để giảm round-trip
    - Sử dụng cấu trúc dữ liệu phù hợp
    - Tránh các lệnh chặn như KEYS
    - Sử dụng Lua scripts
    - Tối ưu hóa network và cấu hình máy chủ
    - Theo dõi và phân tích số liệu hiệu suất

12. **Giải thích về Redis Pub/Sub**
    - Cơ chế gửi và nhận tin nhắn
    - Publishers gửi tin nhắn đến channels
    - Subscribers đăng ký channels để nhận tin nhắn
    - Không lưu trữ tin nhắn
    - Phù hợp cho truyền thông thời gian thực

13. **Redis Modules là gì và khi nào nên sử dụng?**
    - Mở rộng chức năng Redis với modules
    - Ví dụ: RediSearch (full-text search), RedisJSON, RedisTimeSeries
    - Sử dụng khi cần chức năng đặc biệt không có sẵn
    - Viết modules tùy chỉnh cho trường hợp đặc biệt

14. **Làm thế nào để giải quyết vấn đề single point of failure trong Redis?**
    - Sử dụng Redis Sentinel hoặc Redis Cluster
    - Cấu hình replication
    - Triển khai backup và restore strategy
    - Theo dõi và cảnh báo

15. **Redis Streams là gì và khi nào nên sử dụng?**
    - Kiểu dữ liệu dạng append-only log
    - Phù hợp cho event sourcing và message broker
    - Consumer groups cho việc xử lý tin nhắn theo nhóm
    - Phù hợp cho ứng dụng phân tán và real-time

### Câu hỏi thực tiễn

16. **Làm thế nào để theo dõi và debug Redis trong môi trường production?**
    - Sử dụng Redis CLI và lệnh MONITOR (cẩn thận với overhead)
    - Redis INFO command để lấy metrics
    - Slow log để theo dõi các lệnh chậm
    - Sử dụng công cụ monitoring như Prometheus, Grafana
    - Redis Latency Monitoring

17. **Làm thế nào để xử lý big keys trong Redis?**
    - Tránh lưu trữ các khóa lớn
    - Phân tách thành nhiều khóa nhỏ hơn
    - Sử dụng SCAN thay vì KEYS
    - Sử dụng tools như redis-cli --bigkeys

18. **Làm thế nào để xử lý migration dữ liệu Redis?**
    - Sử dụng Redis replication
    - Sử dụng công cụ như redis-dump và redis-load
    - Sử dụng MIGRATE command
    - Sử dụng AOF để restore dữ liệu

19. **Làm thế nào để xử lý các lệnh có tiềm năng gây chặn trong Redis?**
    - Tránh sử dụng KEYS trong production
    - Sử dụng SCAN thay thế
    - Sử dụng Lua scripts với EVAL nhưng cẩn thận về thời gian thực thi
    - Chia nhỏ các lệnh có khả năng chặn

20. **Làm thế nào để implement rate limiting với Redis?**
    - Sử dụng INCR và EXPIRE
    - Sliding window với sorted sets
    - Token bucket algorithm
    - Sử dụng Lua scripts để đảm bảo atomicity

### Câu hỏi về kiến thức thực tế

21. **Mô tả một vấn đề bạn đã gặp với Redis và cách bạn giải quyết nó**
    - Ví dụ: Memory issues, connection problems, data loss
    - Phân tích nguyên nhân
    - Cách tiếp cận giải quyết
    - Bài học rút ra

22. **Làm thế nào để thiết kế một hệ thống cache hiệu quả với Redis?**
    - Phân tích traffic pattern
    - Xác định thời gian sống của các đối tượng khác nhau
    - Chiến lược invalidation
    - Xử lý cache miss
    - Giám sát hit rate và memory usage

23. **Làm thế nào để xử lý việc cập nhật cache khi dữ liệu thay đổi?**
    - Write-through cache: Cập nhật cache cùng lúc với database
    - Write-behind cache: Cập nhật cache trước, sau đó cập nhật database
    - Cache-aside: Cập nhật database trước, sau đó invalidate cache
    - Event-based invalidation: Sử dụng pub/sub để thông báo thay đổi

24. **So sánh các chiến lược sharding data trong Redis**
    - Client-side sharding
    - Proxy-based sharding
    - Redis Cluster
    - Ưu nhược điểm của mỗi phương pháp

25. **Làm thế nào để xử lý việc kết nối đến Redis khi có nhiều services?**
    - Connection pooling
    - Redis Sentinel cho discovery
    - Cấu hình timeout và reconnection
    - Xử lý lỗi kết nối

### Câu hỏi về Redis Lua Scripts

26. **Tại sao và khi nào nên sử dụng Lua scripts trong Redis?**
    - Đảm bảo atomicity của nhiều lệnh
    - Giảm network round-trips
    - Implement logic phức tạp trên server-side
    - Ví dụ: Conditional updates, atomic counters with limits

27. **Ví dụ về Lua script để implement rate limiting**
```lua
local key = KEYS[1]
local limit = tonumber(ARGV[1])
local window = tonumber(ARGV[2])

local current = redis.call('GET', key)
if current and tonumber(current) > limit then
    return 0
end

redis.call('INCR', key)
redis.call('EXPIRE', key, window)
return 1
```

28. **Những rủi ro khi sử dụng Lua scripts?**
    - Scripts có thể chặn Redis server
    - Khó debug và maintain
    - Cần cẩn thận với độ phức tạp của scripts
    - Giới hạn bộ nhớ cho scripts

### Câu hỏi về Redis trong microservices

29. **Làm thế nào để sử dụng Redis trong kiến trúc microservices?**
    - Caching
    - Service discovery
    - Distributed locking
    - Messaging giữa services
    - Rate limiting

30. **Cách implement distributed locking với Redis**
    - Sử dụng SET với NX và EX options
    - Sử dụng unique identifier cho lock
    - Giải quyết vấn đề của fencing tokens
    - Tránh deadlocks với timeout

31. **Làm thế nào để implement một hệ thống message queue với Redis?**
    - Sử dụng Lists với LPUSH và BRPOP
    - Sử dụng Pub/Sub cho real-time notifications
    - Sử dụng Streams cho message broker có độ bền vững cao
    - Xử lý message retries và dead-letter queues

32. **Redis trong một hệ thống phân tán có độ tin cậy cao**
    - Sử dụng Redis Sentinel hoặc Redis Cluster
    - AOF với fsync=always cho durability
    - Replication giữa nhiều data centers
    - Recovery strategies

### Câu hỏi về các tính năng mới

33. **Redis 6.0 có gì mới?**
    - Access Control Lists (ACLs)
    - SSL/TLS support
    - Nhiều threaded I/O
    - Redis Cluster proxies
    - RESP3 protocol

34. **Redis 7.0 có gì mới?**
    - Redis Functions (thay thế cho Lua scripts)
    - Sharded Pub/Sub
    - ACL enhancements
    - Command key-space identification
    - Multi-part AOF

35. **Redis Stack là gì?**
    - Gói các tính năng mở rộng cho Redis
    - Bao gồm: RedisJSON, RediSearch, RedisGraph, RedisTimeSeries
    - Use cases cho các modules khác nhau

### Câu hỏi về hiệu suất

36. **Làm thế nào để chẩn đoán và giải quyết vấn đề về hiệu suất Redis?**
    - Sử dụng INFO command để lấy metrics
    - Theo dõi memory usage và fragmentation
    - Phân tích slow log
    - Tìm hiểu về các lệnh chặn
    - Network latency và throughput

37. **Làm thế nào để tối ưu hóa memory usage trong Redis?**
    - Sử dụng cấu trúc dữ liệu phù hợp
    - Tránh big keys
    - Sử dụng compression
    - Đặt TTL cho các khóa
    - Sử dụng maxmemory và eviction policies

38. **Các strategies để tối ưu hóa Redis cho write-heavy workloads**
    - Sử dụng pipeline để giảm round-trips
    - Điều chỉnh AOF fsync policy
    - Cân nhắc giữa durability và performance
    - Redis Cluster để phân tán write load

39. **Làm thế nào để scale Redis theo chiều ngang và dọc?**
    - Vertical scaling: Tăng dung lượng RAM
    - Horizontal scaling: Redis Cluster
    - Read replicas cho read-heavy workloads
    - Sharding strategies khác nhau

### Câu hỏi tổng hợp

40. **Redis Enterprise có gì khác so với Redis mã nguồn mở?**
    - Active-Active geo-distribution
    - Redis on Flash (ROF)
    - Dynamic scaling
    - Enhanced security features
    - CRDT-based conflict resolution

41. **Các thách thức khi migrate từ Redis standalone sang Redis Cluster**
    - Xử lý slot migration
    - Thay đổi client library
    - Multi-key operations
    - Transaction limitations
    - Testing và validation

42. **Làm thế nào để bảo vệ Redis khỏi các cuộc tấn công DoS?**
    - Sử dụng firewall và security groups
    - Giới hạn số lượng kết nối
    - Rate limiting
    - Cấu hình timeout hợp lý
    - Theo dõi và phát hiện các kết nối bất thường

43. **Làm thế nào để implement và quản lý một Redis Cluster lớn?**
    - Cấu hình số lượng masters và replicas phù hợp
    - Balanced slot distribution
    - Theo dõi và quản lý failovers
    - Backup và restore strategies
    - Performance tuning và monitoring

44. **Khi nào nên sử dụng Redis, Memcached, và MongoDB?**
    - **Redis**: Khi cần cấu trúc dữ liệu phong phú, persistence, replication
    - **Memcached**: Khi cần cache đơn giản, phân tán, và không cần persistence
    - **MongoDB**: Khi cần lưu trữ dữ liệu phức tạp, truy vấn phức tạp, và schema linh hoạt

45. **Mô tả một kiến trúc sử dụng Redis cho một hệ thống có lưu lượng cao**
    - Caching layer với Redis
    - Session management
    - Rate limiting
    - Distributed locking
    - Message broker
    - Replication và HA strategy
    - Sharding strategy
    - Backup và disaster recovery

## Tài liệu tham khảo

- [Redis Documentation](https://redis.io/documentation)
- [Redis Commands](https://redis.io/commands)
- [Redis University](https://university.redis.com/)
- [Redis Best Practices](https://redis.io/topics/cluster-tutorial)
- [Redis Persistence](https://redis.io/topics/persistence)
- [Redis Security](https://redis.io/topics/security)
