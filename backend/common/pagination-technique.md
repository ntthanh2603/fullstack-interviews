# Các kĩ thuật phân trang

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

## So sánh hiệu suất

| Phương pháp     | Độ phức tạp | Hiệu suất          | Random Access | Real-time Consistency |
| --------------- | ----------- | ------------------ | ------------- | --------------------- |
| Offset-based    | Thấp        | Kém (O(n))         | ✅            | ❌                    |
| Keyset          | Trung bình  | Tốt (O(log n))     | ❌            | ✅                    |
| Seek Method     | Cao         | Rất tốt (O(log n)) | ❌            | ✅                    |
| Window Function | Trung bình  | Trung bình         | ✅            | ❌                    |
| Hybrid          | Cao         | Tốt                | ✅            | ✅                    |
| Deferred Joins  | Trung bình  | Tốt                | ✅            | ❌                    |

## Lựa chọn phương pháp phù hợp

- **Offset-based**: Dữ liệu nhỏ, cần random access, prototype nhanh
- **Keyset**: Dữ liệu lớn, real-time feeds, high performance
- **Seek Method**: Critical performance, dữ liệu rất lớn, có duplicate values
- **Window Function**: PostgreSQL, cần analytics, moderate performance
- **Hybrid**: Flexible requirements, complex applications
- **Deferred Joins**: Cải thiện existing offset-based, có nhiều joins
