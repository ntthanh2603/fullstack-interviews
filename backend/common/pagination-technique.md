# Các kĩ thuật phân trang

## Lựa chọn phương pháp phù hợp

#### Offset-based: Dữ liệu nhỏ, cần random access, prototype nhanh

#### Keyset: Dữ liệu lớn, phân trang tuần tự, cần hiệu năng cao, dữ liệu thay đổi ít

#### Seek Method: Dữ liệu lớn, cần phân trang nhanh, có column index tốt (timestamp, id)

#### Window Function: Dữ liệu phức tạp, cần ranking/grouping, báo cáo phân tích

#### Hybrid: Dữ liệu rất lớn, cần cả random access và hiệu năng, hệ thống phức tạp

#### Deferred Joins: Bảng có nhiều column, join phức tạp, chỉ cần vài trường hiển thị

#### Time-based Pagination: Feed realtime (social media, logs, chat), dữ liệu theo thời gian

#### Relay Cursor Pagination: GraphQL APIs, cần chuẩn formal specification, connection pattern

#### Token-based Pagination: API bảo mật cao, không muốn expose internal structure

#### Scroll-based (Infinite Scroll): Mobile apps, UX tốt, load tự động

#### Range Pagination: Filter phức tạp (price, date range), search với nhiều điều kiện

#### Snapshot Pagination: Đảm bảo consistency, data không thay đổi giữa các page

#### Batch Pagination: ETL/data processing, xử lý large datasets theo batch

#### Search-after (Elasticsearch): Elasticsearch/OpenSearch, hiệu năng cao với search engine

#### Scroll-based (Infinite Scroll): Mobile/web apps, UX mượt mà, load tự động khi scroll

#### Virtual Scrolling: Large lists UI, chỉ render visible items, tiết kiệm memory

#### Parallel Pagination: Multi-threaded processing, chia nhỏ dataset, xử lý song song

### 1. Offset-based Pagination (LIMIT/OFFSET):

Sử dụng LIMIT và OFFSET để phân trang truyền thống, thường dùng cho các API REST đơn giản.

```typescript
const [data, total] = await this.repo.findAndCount({
  skip: (page - 1) * limit,
  take: limit,
  order: { createdAt: "DESC" },
});
```

#### Ưu điểm

- Dễ dùng và phổ biến
- Tích hợp sẵn với nhiều ORM
- Cho phép truy cập trực tiếp tới bất kỳ trang nào
- Dễ tính toán tổng số trang

#### Nhược điểm

- Hiệu suất kém với OFFSET lớn (Ví dụ trên 10 000)
- Không phù hợp với dữ liệu thay đổi liên tục (bị mất hoặc trùng dữ liệu giữa các page)
- Database phải đếm và bỏ qua nhiều bản ghi

### 2. Keyset Pagination (Cursor-based Pagination)

Sử dụng giá trị của một cột làm "cursor" (con trỏ) để xác định vị trí bắt đầu của trang tiếp theo.

```typescript
// Lấy trang đầu tiên
const firstPage = await this.repo.find({
  take: limit,
  order: { id: "ASC" },
});

// Lấy trang tiếp theo sử dụng cursor
const nextPage = await this.repo.find({
  where: { id: MoreThan(lastId) },
  take: limit,
  order: { id: "ASC" },
});

// Hoặc sử dụng với created_at
const nextPageByDate = await this.repo.find({
  where: { createdAt: LessThan(lastCreatedAt) },
  take: limit,
  order: { createdAt: "DESC" },
});
```

#### Ưu điểm

- Hiệu suất cao, không bị ảnh hưởng bởi kích thước dữ liệu
- Nhất quán khi dữ liệu thay đổi
- Phù hợp cho real-time applications

#### Nhược điểm

- Không thể truy cập trực tiếp tới trang cụ thể
- Yêu cầu cột được sắp xếp unique hoặc có thứ tự
- Phức tạp hơn trong implementation

### 3. Seek Method (Tối ưu Keyset Pagination)

Phương pháp tối ưu hóa Keyset Pagination bằng cách sử dụng composite index và multiple columns.

```typescript
// Sử dụng composite cursor với multiple columns
interface SeekCursor {
  createdAt: Date;
  id: number;
}

const seekPagination = async (cursor?: SeekCursor, limit: number = 10) => {
  const queryBuilder = this.repo.createQueryBuilder("entity");

  if (cursor) {
    queryBuilder.where(
      "(entity.createdAt < :createdAt) OR (entity.createdAt = :createdAt AND entity.id < :id)",
      { createdAt: cursor.createdAt, id: cursor.id }
    );
  }

  return queryBuilder
    .orderBy("entity.createdAt", "DESC")
    .addOrderBy("entity.id", "DESC")
    .limit(limit)
    .getMany();
};

// Sử dụng
const result = await seekPagination(
  { createdAt: new Date("2023-01-01"), id: 100 },
  20
);
```

#### Ưu điểm

- Hiệu suất tốt nhất cho large datasets
- Tận dụng tối đa composite indexes
- Nhất quán với duplicate values

#### Nhược điểm

- Phức tạp nhất để implement
- Yêu cầu thiết kế index cẩn thận
- Khó debug và maintain

### 4. Pagination with Window Function (PostgreSQL only)

Sử dụng window functions để tối ưu hóa việc phân trang và lấy metadata.

```typescript
const windowPagination = async (page: number, limit: number) => {
  const offset = (page - 1) * limit;

  const result = await this.repo.query(
    `
    SELECT *,
           COUNT(*) OVER() as total_count,
           ROW_NUMBER() OVER(ORDER BY created_at DESC) as row_num
    FROM entities
    ORDER BY created_at DESC
    LIMIT $1 OFFSET $2
  `,
    [limit, offset]
  );

  return {
    data: result,
    total: result.length > 0 ? parseInt(result[0].total_count) : 0,
    page,
    totalPages: Math.ceil(
      result.length > 0 ? result[0].total_count / limit : 0
    ),
  };
};
```

#### Ưu điểm

- Lấy total count trong một query duy nhất
- Tối ưu hóa với window functions
- Có thể thêm ranking và analytics

#### Nhược điểm

- Chỉ hoạt động với PostgreSQL
- Vẫn có vấn đề performance với OFFSET lớn
- Phức tạp khi cần filter động

### 5. Hybrid Pagination (Offset + Cursor)

Kết hợp offset-based và cursor-based để tận dụng ưu điểm của cả hai.

```typescript
interface HybridPaginationOptions {
  page?: number;
  cursor?: string;
  limit: number;
}

const hybridPagination = async (options: HybridPaginationOptions) => {
  const { page, cursor, limit } = options;

  if (cursor) {
    // Sử dụng cursor-based cho performance
    const decodedCursor = JSON.parse(Buffer.from(cursor, "base64").toString());
    return this.repo.find({
      where: { id: MoreThan(decodedCursor.id) },
      take: limit,
      order: { id: "ASC" },
    });
  } else if (page) {
    // Sử dụng offset-based cho trang đầu hoặc khi cần random access
    const skip = (page - 1) * limit;
    return this.repo.find({
      skip,
      take: limit,
      order: { id: "ASC" },
    });
  }
};

// Tạo cursor cho trang tiếp theo
const createCursor = (lastItem: any) => {
  const cursorData = { id: lastItem.id, createdAt: lastItem.createdAt };
  return Buffer.from(JSON.stringify(cursorData)).toString("base64");
};
```

#### Ưu điểm

- Linh hoạt: hỗ trợ cả random access và sequential access
- Tối ưu performance cho use cases khác nhau
- Backward compatible với existing APIs

#### Nhược điểm

- Logic phức tạp
- Cần handle nhiều trường hợp khác nhau
- Có thể confusing cho developers

### 6. Deferred Joins

Tối ưu hóa offset-based pagination bằng cách defer việc join cho đến khi đã filter được IDs.

```typescript
const deferredJoinPagination = async (page: number, limit: number) => {
  const offset = (page - 1) * limit;

  // Bước 1: Lấy IDs với offset (chỉ scan index)
  const ids = await this.repo
    .createQueryBuilder("entity")
    .select("entity.id")
    .orderBy("entity.createdAt", "DESC")
    .limit(limit)
    .offset(offset)
    .getRawMany();

  if (ids.length === 0) return [];

  // Bước 2: Lấy full data với IDs đã filter
  const idsArray = ids.map((item) => item.entity_id);
  const result = await this.repo
    .createQueryBuilder("entity")
    .leftJoinAndSelect("entity.relatedTable", "related")
    .where("entity.id IN (:...ids)", { ids: idsArray })
    .orderBy("entity.createdAt", "DESC")
    .getMany();

  return result;
};

// Với raw SQL cho performance tốt hơn
const deferredJoinWithRawSQL = async (page: number, limit: number) => {
  const offset = (page - 1) * limit;

  const result = await this.repo.query(
    `
    SELECT e.*, r.name as related_name
    FROM entities e
    LEFT JOIN related_table r ON e.related_id = r.id
    WHERE e.id IN (
      SELECT id FROM entities
      ORDER BY created_at DESC
      LIMIT $1 OFFSET $2
    )
    ORDER BY e.created_at DESC
  `,
    [limit, offset]
  );

  return result;
};
```

#### Ưu điểm

- Cải thiện đáng kể performance cho offset-based pagination
- Giảm thiểu data transfer trong bước đầu
- Vẫn hỗ trợ random access

#### Nhược điểm

- Yêu cầu 2 queries
- Phức tạp với multiple joins
- Không giải quyết được vấn đề fundamental của OFFSET

### 7. Time-based Pagination

Sử dụng timestamp làm cursor, phù hợp cho dữ liệu real-time.

```typescript
const timeBasedPagination = async (
  beforeTimestamp?: Date,
  limit: number = 10
) => {
  const queryBuilder = this.repo.createQueryBuilder("entity");

  if (beforeTimestamp) {
    queryBuilder.where("entity.createdAt < :beforeTimestamp", {
      beforeTimestamp,
    });
  }

  return queryBuilder
    .orderBy("entity.createdAt", "DESC")
    .limit(limit)
    .getMany();
};

// Sử dụng cho feed realtime
const getFeed = async (lastSeenAt?: Date) => {
  return timeBasedPagination(lastSeenAt, 20);
};
```

#### Ưu điểm

- Tự nhiên với dữ liệu time-series
- Hiệu suất cao với timestamp index
- Phù hợp cho feeds và logs

#### Nhược điểm

- Cần xử lý duplicate timestamps
- Không phù hợp cho dữ liệu không có timestamp
- Có thể miss data nếu có nhiều records cùng timestamp

### 8. Relay Cursor Pagination

Chuẩn GraphQL Relay specification cho cursor-based pagination.

```typescript
interface RelayConnection<T> {
  edges: Array<{
    node: T;
    cursor: string;
  }>;
  pageInfo: {
    hasNextPage: boolean;
    hasPreviousPage: boolean;
    startCursor?: string;
    endCursor?: string;
  };
}

const relayPagination = async (
  first?: number,
  after?: string,
  last?: number,
  before?: string
): Promise<RelayConnection<Entity>> => {
  const queryBuilder = this.repo.createQueryBuilder("entity");

  if (after) {
    const afterId = decodeCursor(after);
    queryBuilder.where("entity.id > :afterId", { afterId });
  }

  if (before) {
    const beforeId = decodeCursor(before);
    queryBuilder.where("entity.id < :beforeId", { beforeId });
  }

  const limit = first || last || 10;
  const entities = await queryBuilder
    .orderBy("entity.id", "ASC")
    .limit(limit + 1)
    .getMany();

  const hasNextPage = entities.length > limit;
  const nodes = hasNextPage ? entities.slice(0, -1) : entities;

  return {
    edges: nodes.map((node) => ({
      node,
      cursor: encodeCursor(node.id),
    })),
    pageInfo: {
      hasNextPage,
      hasPreviousPage: !!after,
      startCursor: nodes.length > 0 ? encodeCursor(nodes[0].id) : undefined,
      endCursor:
        nodes.length > 0 ? encodeCursor(nodes[nodes.length - 1].id) : undefined,
    },
  };
};

const encodeCursor = (id: number) =>
  Buffer.from(id.toString()).toString("base64");
const decodeCursor = (cursor: string) =>
  parseInt(Buffer.from(cursor, "base64").toString());
```

#### Ưu điểm

- Chuẩn GraphQL specification
- Hỗ trợ bidirectional pagination
- Metadata phong phú

#### Nhược điểm

- Phức tạp implementation
- Overhead với GraphQL schema
- Không phù hợp cho REST APIs

### 9. Token-based Pagination

Sử dụng opaque token để bảo mật và control pagination.

```typescript
interface PaginationToken {
  lastId: number;
  timestamp: number;
  signature: string;
}

const tokenBasedPagination = async (token?: string, limit: number = 10) => {
  let lastId = 0;

  if (token) {
    const decodedToken = verifyAndDecodeToken(token);
    lastId = decodedToken.lastId;
  }

  const entities = await this.repo.find({
    where: { id: MoreThan(lastId) },
    take: limit + 1,
    order: { id: "ASC" },
  });

  const hasMore = entities.length > limit;
  const data = hasMore ? entities.slice(0, -1) : entities;

  const nextToken = hasMore
    ? generateToken({
        lastId: data[data.length - 1].id,
        timestamp: Date.now(),
      })
    : null;

  return {
    data,
    nextToken,
    hasMore,
  };
};

const generateToken = (payload: Omit<PaginationToken, "signature">) => {
  // Sử dụng JWT hoặc encryption
  return jwt.sign(payload, process.env.PAGINATION_SECRET);
};

const verifyAndDecodeToken = (token: string) => {
  return jwt.verify(token, process.env.PAGINATION_SECRET) as PaginationToken;
};
```

#### Ưu điểm

- Bảo mật cao, không expose internal structure
- Có thể embed metadata và expiration
- Control được access patterns

#### Nhược điểm

- Overhead với encryption/signing
- Phức tạp debugging
- Cần manage token lifecycle

### 10. Range Pagination

Phân trang dựa trên range values (price, date, etc.).

```typescript
interface RangePagination {
  minPrice?: number;
  maxPrice?: number;
  minDate?: Date;
  maxDate?: Date;
  limit: number;
}

const rangePagination = async (params: RangePagination) => {
  const { minPrice, maxPrice, minDate, maxDate, limit } = params;

  const queryBuilder = this.repo.createQueryBuilder("product");

  if (minPrice !== undefined) {
    queryBuilder.andWhere("product.price >= :minPrice", { minPrice });
  }

  if (maxPrice !== undefined) {
    queryBuilder.andWhere("product.price <= :maxPrice", { maxPrice });
  }

  if (minDate) {
    queryBuilder.andWhere("product.createdAt >= :minDate", { minDate });
  }

  if (maxDate) {
    queryBuilder.andWhere("product.createdAt <= :maxDate", { maxDate });
  }

  const products = await queryBuilder
    .orderBy("product.price", "ASC")
    .addOrderBy("product.createdAt", "DESC")
    .limit(limit)
    .getMany();

  return {
    data: products,
    nextRange:
      products.length === limit
        ? {
            minPrice: products[products.length - 1].price,
            maxDate: products[products.length - 1].createdAt,
          }
        : null,
  };
};
```

#### Ưu điểm

- Phù hợp cho filter phức tạp
- Tự nhiên với search interfaces
- Hiệu suất tốt với proper indexes

#### Nhược điểm

- Phức tạp với multiple ranges
- Khó handle edge cases
- Cần thiết kế index cẩn thận

### 11. Snapshot Pagination

Tạo snapshot để đảm bảo consistency.

```typescript
interface SnapshotPagination {
  snapshotId?: string;
  page: number;
  limit: number;
}

const snapshotPagination = async (params: SnapshotPagination) => {
  const { snapshotId, page, limit } = params;

  if (!snapshotId) {
    // Tạo snapshot mới
    const newSnapshotId = await createSnapshot();
    return {
      snapshotId: newSnapshotId,
      data: [],
      message: "Snapshot created, use this ID for pagination",
    };
  }

  // Lấy data từ snapshot
  const offset = (page - 1) * limit;
  const data = await this.snapshotRepo.find({
    where: { snapshotId },
    skip: offset,
    take: limit,
    order: { originalOrder: "ASC" },
  });

  return {
    data,
    snapshotId,
    page,
    hasMore: data.length === limit,
  };
};

const createSnapshot = async () => {
  const snapshotId = generateUUID();
  const allData = await this.repo.find({ order: { id: "ASC" } });

  await this.snapshotRepo.save(
    allData.map((item, index) => ({
      snapshotId,
      data: item,
      originalOrder: index,
    }))
  );

  return snapshotId;
};
```

#### Ưu điểm

- Đảm bảo consistency tuyệt đối
- Không bị ảnh hưởng bởi data changes
- Phù hợp cho reports và exports

#### Nhược điểm

- Tốn storage cho snapshots
- Overhead tạo snapshot
- Cần cleanup snapshots cũ

### 12. Batch Pagination

Xử lý dữ liệu theo batch cho ETL processes.

```typescript
const batchPagination = async function* (
  batchSize: number = 1000,
  startId: number = 0
) {
  let lastId = startId;
  let hasMore = true;

  while (hasMore) {
    const batch = await this.repo.find({
      where: { id: MoreThan(lastId) },
      take: batchSize,
      order: { id: "ASC" },
    });

    if (batch.length === 0) {
      hasMore = false;
      break;
    }

    yield batch;

    lastId = batch[batch.length - 1].id;
    hasMore = batch.length === batchSize;
  }
};

// Sử dụng
const processBatches = async () => {
  for await (const batch of batchPagination(1000)) {
    await processBatch(batch);
    console.log(`Processed batch with ${batch.length} items`);
  }
};
```

#### Ưu điểm

- Tối ưu cho large data processing
- Memory efficient
- Có thể resume từ vị trí bất kỳ

#### Nhược điểm

- Không phù hợp cho user-facing interfaces
- Phức tạp error handling
- Cần track progress manually

### 13. Search-after (Elasticsearch)

Elasticsearch/OpenSearch search_after pagination.

```typescript
const searchAfterPagination = async (
  query: any,
  searchAfter?: any[],
  size: number = 10
) => {
  const searchParams: any = {
    index: "products",
    size,
    sort: [
      { price: { order: "asc" } },
      { _id: { order: "asc" } }, // Tiebreaker
    ],
    body: {
      query,
    },
  };

  if (searchAfter) {
    searchParams.body.search_after = searchAfter;
  }

  const response = await esClient.search(searchParams);
  const hits = response.body.hits.hits;

  return {
    data: hits.map((hit) => hit._source),
    nextSearchAfter: hits.length > 0 ? hits[hits.length - 1].sort : null,
    hasMore: hits.length === size,
  };
};

// Sử dụng
const searchProducts = async (searchAfter?: any[]) => {
  const query = {
    match: {
      name: "laptop",
    },
  };

  return searchAfterPagination(query, searchAfter, 20);
};
```

#### Ưu điểm

- Hiệu suất cao với Elasticsearch
- Consistent results
- Hỗ trợ deep pagination

#### Nhược điểm

- Chỉ hoạt động với Elasticsearch
- Cần sort field làm tiebreaker
- Không thể jump to specific page

## Kết luận

Việc lựa chọn phương pháp phân trang phụ thuộc vào:

- **Kích thước dữ liệu**: Offset-based cho dữ liệu nhỏ, Cursor-based cho dữ liệu lớn
- **Use case**: Random access vs Sequential access
- **Performance requirements**: Latency vs Throughput
- **Consistency requirements**: Real-time vs Snapshot
- **Technical stack**: Database capabilities, Framework support
- **User experience**: Pagination UI vs Infinite scroll

Hãy cân nhắc trade-offs và chọn phương pháp phù hợp nhất cho từng tình huống cụ thể.
