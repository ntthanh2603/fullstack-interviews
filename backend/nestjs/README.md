## Interview Backend Nestjs

### Phân biệt let, var, const:

- Var không nên dùng vì có vấn đề về hoisting( bị vấn đề không khai báo mà vẫn dùng được) và block scope( vấn đề không bị giới hạn trong block làm khó kiểm soát).
- Let nên dùng cho biến có thể thay đổi giá trị. Let có phạm vi trong block {} và phải khai báo trước khi dùng.
- Const nên dùng cho hằng số hoặc giá trị không thay đổi và phải khai báo trước khi dùng.

### So sáng express và nestjs:

- Expess: Không bắt buộc cấu trúc, và không ép buộc mô hình, dùng js hoặc ts, nhẹ nhanh hơn trong ứng dụng nhỏ,tùy chỉnh dễ dàng nhưng phải tùy chỉnh code.
- Nestjs: Có kiến trúc mạnh mẽ, ép buộc theo kiến trúc Module + Controller + Service (giống Angular), phải dùng ts, tối ưu cho ứng dụng lớn, có sẵn module giúp
  mở rộng dễ dàng. Nestjs có hỗ trợ theo MVC bằng template engine như Handlebars, Pug, hoặc EJS để render HTML động.

### Phân biệt module, controller, provider, service, midderware, intercepter, pipe, guard:

- Module: Nhóm các thành phần liên quan, giúp tổ chức code theo tính năng.
- Controler: Nhận request từ client, xử lý và trả về response.
- Provider: Chứa logic nghiệp vụ (business logic), có thể được inject vào nơi khác.
- Service: Một loại provider, chuyên xử lý logic nghiệp vụ và thao tác dữ liệu.
- Middleware: Xử lý yêu cầu trước khi vào controller.
- Interceptor: Thực hiện các tác vụ trước và sau khi xử lý yêu cầu. Bạn có thể sử dụng interceptors để thực hiện các tác vụ như logging, caching, đo lường thời gian thực thi, v.v.
- Pipe: Xử lý và chuyển đổi data trước khi handle trong controler, dùng validate, transform, xử lý dữ liệu trước khi chúng được xử lý.
- Guard: Kiểm tra điều kiện và quyền truy cập trước khi vào controller.

### Phân biệt phương thức Get, Post:

- Get: Dùng để lấy dữ liệu từ server, dữ liệu được đính kèm trên URL(Query, Param), không bảo mật vì có thể bị ghi lại trên trình duyệt, bị giới hạn độ dài dưới 2048 kí tự,
  Trình duyệt có thể lưu cache và dùng để tải file, phân trang, lấy profile user ...
- Post: Gửi dữ liệu lên server để xử lý, dữ liệu từ client trong body của request, an toàn hơn Get, trình duyệt không lưu cache và không phù hợp để tải file
  (do không tự động hiển thị hộp thoại tải file /download?file=report.pdf và không hỗ trợ tải file trực tiếp từ URL, không hỗ trợ cache gây tải lại file không cần thiết).

### Phân biệt HTTP(HyperText Transfer Protocol) và HTTPS(HTTP Secure):

- HTTP: Dữ liệu không cần mã hóa, dùng văn bản thuần, làm dễ bị đánh cắp dữ liệu. Không tối ưu SEO vì bảo mật kém.
- HTTPS: Dữ liệu bị mã hóa bằng TLS/SSL giúp khó bị đánh cắp nhưng chậm hơn chút vì mất thời gian mã hóa và đặc biệt cần chứng chỉ SSL để hoạt động.

### Giải thích cơ chế HTTP only: HttpOnly là một thuộc tính của cookie giúp ngăn trình duyệt truy cập cookie từ JavaScript. Điều này giúp bảo vệ các thông tin nhạy cảm

(như JWT token) khỏi các cuộc tấn công XSS (Cross-Site Scripting).

### Phòng chống tấn công Cross-Site Scripting (XSS) Kẻ tấn công chèn JavaScript độc hại vào trang web, đánh cắp cookie, dữ liệu hoặc thực thi mã độc.

- Dùng Helmet giúp thiết lập các CSP (Content Security Policy) để chặn JavaScript độc hại:

  ```
  # import lib: npm install helmet

  import helmet from 'helmet';
  import { NestFactory } from '@nestjs/core';
  import { AppModule } from './app.module';

  async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  // Cấu hình Helmet để chống XSS
  app.use(
  helmet({
  contentSecurityPolicy: {
  directives: {
  defaultSrc: ["'self'"], // Chỉ cho hép tài nguyên từ domain chính của trang
  scriptSrc: ["'self'", "https://trusted.cdn.com"],  // Ngăn chặn script lạ
  styleSrc: ["'self'", "https://trusted.styles.com"],
  },
  },
  xXssProtection: true, // Bật bảo vệ X-XSS-Protection cho trình duyệt cũ để ngăn chặn trình duyệt tải và thực thi các script đáng ngờ
  referrerPolicy: { policy: 'no-referrer' }, // Ngăn lộ thông tin referrer giảm nguy cơ leak dữ liệu
  noSniff: true, // Ngăn MIME sniffing. Điều này giúp tránh trường hợp trình duyệt tải JavaScript bị nhúng từ một nguồn không mong muốn.
  })
  );

  await app.listen(3000);
  console.log('Server is running on http://localhost:3000');
  }

  bootstrap();
  ```

- Escape output chuyển đổi các ký tự đặc biệt thành dạng an toàn trước khi hiển thị. Ví dụ:

  ```
  <script>alert('Hacked!')</script>
  # Nếu ứng dụng hiển thị trực tiếp nội dung này trên trang web, trình duyệt sẽ thực thi đoạn JavaScript này, gây ra XSS.
  # Thay vào đó, nếu ta escape output, trình duyệt sẽ chỉ hiển thị văn bản chứ không thực thi:
  &lt;script&gt;alert('Hacked!')&lt;/script&gt;
  ```

- Validate input.

### Không được lưu token nào trên server để đảm bảo an toàn thông tin.

### Phòng chống tấn công Cross-Site Request Forgery (CSRF) Kẻ tấn công lừa người dùng thực hiện hành động không mong muốn bằng cách gửi yêu cầu từ một trang giả mạo.

- Dùng CSRF Token:

  ```
  # Import lib: npm install csurf cookie-parser
  import { NestFactory } from '@nestjs/core';

  import { AppModule } from './app.module';
  import _ as cookieParser from 'cookie-parser';
  import _ as csurf from 'csurf';

  async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  // Sử dụng cookie-parser để xử lý cookies
  app.use(cookieParser());

  // Cấu hình CSRF protection middleware
  app.use(csurf({ cookie: true }));

  await app.listen(3000);
  }
  bootstrap();

  @Controller('csrf')

  export class CsrfController {
  // Route để gửi CSRF token tới client
  @Get('token')
  getCsrfToken(@Req() req: Request, @Res() res: Response) {
  res.json({ csrfToken: req.csrfToken() });
  }
  # Khi bạn gửi yêu cầu từ phía client (ví dụ như với fetch hoặc axios), bạn cần gửi CSRF token trong header của yêu cầu:
  // Lấy CSRF token từ route '/csrf/token'
  fetch('/csrf/token')
    .then(response => response.json())
    .then(data => {
      const csrfToken = data.csrfToken;

      // Gửi yêu cầu POST với CSRF token trong header
      fetch('/csrf/action', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'csrf-token': csrfToken, // Gửi CSRF token trong header
        },
        body: JSON.stringify({ data: 'some data' }),
      })
        .then(response => response.json())
        .then(result => {
          console.log('Action Result:', result);
        })
        .catch(error => console.error('Error:', error));
    });

  ```

- Bật CORS.

### Phòng chống tấn công Brute Force Attack Thử nhiều lần đăng nhập với mật khẩu đoán được. Cách phòng: Dùng bcrypt để băm mật khẩu, giới hạn request, dùng cơ chế 2FA.

### Phòng chống tấn công Denial of Service (DoS) & Distributed Denial of Service (DDoS) Gửi quá nhiều request khiến server quá tải.

- Rate limit để giới hạn request từ các IP.
- Sử dụng CDN & Firewall (Cloudflare, AWS Shield).
- Dùng throttling queue để kiểm soát request.
- Giới hạn kích thước request để tránh tấn công bằng payload lớn
  ```
  app.use(express.json({ limit: '1kb' }));
  ```
- Ẩn lỗi trong môi trường production:
  ```
  app.useGlobalFilters(new HttpExceptionFilter());
  ```
- Dùng jwt với các thuật toán mạnh(RS256, HS256) và thời gian sống phù hợp với accect token và refreshtoken.
- Load balencer.

### RESTful API là gì: là phong cách kiến trúc trong lập trình web dựa trên REST (Representational State Transfer) nó dùng HTTP để giao tiếp giữa

clien và server một cách đơn giản có tổ chức và dễ mở rộng.

### Nêu các HTTP method trong RESTful API: GET, POST, PATCH, PUT, DELETE.

### Phân biệt query và param trong URL:

- Query là các tham số không bắt buộc được truyền trong URL sau dấu "?" dùng khi cần lọc, tìm kiếm, phân trang. Ví dụ: ?key1=value1&key2=value2.
  Và không ảnh hưởng đến cấu trúc router.
- Param là các tham số bắt buộc được truyền vào URL dùng để xác định tài nguyên cụ thể. Ví dụ: /resource/:12345678 trong đó 12345678 là user id. Và bắt buộc có trên URL
  nếu thiếu sẽ lỗi.

### Nếu trong Controller có @Get('/abc') và @Get(':id') thì nên đặt cái nào trước và vì sao: Nên đặt @Get('/abc') trước vì nếu bỏ @Get(':id') trước thì sẽ tự động gán id = abc

nhưng abc lại không phải param.

### Tại sao NestJs lại dùng cơ chế dependency injection(DI): DI là mẫu thiết kế phần mềm phổ biến trong các framework hiện đại vì các lý do:

- Tăng tính mở rộng và dễ bảo trì. Ví dụ: Nếu bạn thay đổi cách lưu trữ dữ liệu (từ MongoDB sang PostgreSQL), bạn chỉ cần thay đổi cách triển khai dịch vụ tương ứng mà không ảnh hưởng đến các phần khác trong ứng dụng.
- Dễ dàng kiểm thử: Hỗ trợ kiểm thử đơn vị (Unit Testing): DI giúp dễ dàng "mock" hoặc thay thế các dịch vụ bên ngoài mà không cần thay đổi mã nguồn. Điều này rất quan trọng khi bạn cần kiểm tra các phần cụ thể trong ứng dụng mà không cần đến các phần phụ thuộc ngoài (như cơ sở dữ liệu, API từ bên ngoài, v.v.).
- Giảm sự phụ thuộc trực tiếp (Loose Coupling): Thay vì mỗi lớp tự tạo các đối tượng mà nó cần, DI cung cấp các đối tượng đó từ bên ngoài. Điều này giúp dễ dàng thay đổi hoặc tái sử dụng các lớp mà không gây ảnh hưởng đến hệ thống. Ví dụ: Một service không cần phải biết chi tiết về cách thức hoạt động của một repository, nó chỉ cần một đối tượng repository được cung cấp bởi DI.
- Tăng tính linh hoạt và dễ cấu hình: Bạn có thể cung cấp các giá trị, cấu hình và dịch vụ cần thiết một cách linh động mà không cần phải thay đổi code trong lớp đang sử dụng chúng. Điều này giúp cấu hình hệ thống linh động hơn, dễ dàng thay đổi khi cần thiết.
- Được hỗ trợ mạnh mẽ trong NestJS: Giúp mã dễ đọc, dễ hiểu và không có sự rối loại giữa các thành phần. DI giúp tự động quản lý và tiêm (inject) các phụ thuộc vào các lớp cần thiết, đơn giản hóa việc phát triển và kiểm soát phụ thuộc trong ứng dụng.

### Mã hóa(Encryption): Mã hóa giúp bảo vệ dữ liệu bằng cách chuyển đổi nó thành một dạng không thể đọc được nếu không có khóa giải mã dùng crypto( trong Nodejs) và jsonwebtoken. Các thuật toán mã hóa như: RSA,HMAC-SHA256,RS256,ES256...

### Băm(Hashing): Hash là quá trình chuyển đổi dữ liệu thành một chuỗi cố định, không thể đảo ngược dùng lib(crypto, bcrypt, argon2). Các thuật toán: MD5, SHA-256, SHA-512...

### CORS là gì: CORS (Cross-Origin Resource Sharing) là cơ chế giúp trình duyệt kiểm soát việc một website có thể gửi request đến một domain khác hay không. Nếu không cấu hình đúng, API có thể bị chặn request từ frontend hoặc dễ bị tấn công bảo mật.

### Jwt là gì: Jsonwebtoken là cơ chế xác thực người dùng theo mô hình stateful.

### Thành phần jwt gồm: Header chứa thuật toán ký (HMAC, RSA, ES), Payload chứa dữ liệu (userId, role, exp, ...), Signature được tạo bằng thuật toán ký số. Mỗi phần trong jwt được cách nhau bởi dấu ".".

### Tại sao jwt cần signature mà không để nguyên payload:

- Dùng signature để đảm bảo payload không bị sửa đổi, nếu sửa đổi chữ ký sẽ không khớp và token bị từ chối.
- Nếu không có signature bất kỳ ai cũng có thể tạo jwt giả mạo.
- Ngăn chặn tấn công "Replay Attack": Thêm timestamp(iat, exp) và kiểm tra hạn token kết hợp chữ ký đảm bảo token không bị sửa đổi.

### Tác dụng của async/await:

- Được sử dụng để giải quyết vấn đề bất đồng bộ (asynchronous programming) một cách dễ đọc và dễ quản lý hơn so với callback hoặc Promise chaining.
- Tránh callback hell dẫn đến code khó đọc và khó bảo trì.
- Dùng Promise chaining giúp tránh callback hell nhưng vẫn khó đọc khi có nhiều bước xử lý( Dùng .then() nhiều):
  ```bash
  readFile('file.txt')
  	.then(data => db.query('SELECT * FROM users'))
  	.then(users => sendEmail(users))
  	.then(() => console.log('Done'))
  	.catch(err => console.error(err));
  ```
- Có thể xử lý nhiều tác vụ bất đồng bộ cùng lúc với Promise.all để tăng tốc độ:

  ```bash
  async function fetchData() {
  	const [users, orders] = await Promise.all([
  			db.query('SELECT * FROM users'),
  			db.query('SELECT * FROM orders')
  	]);
  	console.log(users, orders);
  }
  ```

### Các chống SQL injection với TypeORM: SQL Injection là một lỗ hổng bảo mật cho phép kẻ tấn công chèn mã SQL độc hại vào các truy vấn cơ sở dữ liệu. Để phòng tránh, chúng ta không bao giờ nên xây dựng câu lệnh SQL bằng cách nối chuỗi trực tiếp từ dữ liệu đầu vào:

- Tách dữ liệu đầu vào khỏi câu lệnh SQL chứ không nối chuỗi trực tiếp:
  ```
  const user = await this.userRepo.query(
  	'SELECT * FROM users WHERE email = $1',
  	[email]
  );
  ```
- Dùng Query Builder hoặc 1 số hàm như FindOne, FindOneBy ...(của TypeORM):
  ```
  const user = await this.userRepo.createQueryBuilder('user')
  	.where('user.email = :email', { email })
  	.getOne();
  ```
- Đảm bảo mật khẩu luôn được băm.

### Even loop trong nodejs: Event Loop là cơ chế giúp Node.js xử lý non-blocking I/O (I/O bất đồng bộ) bằng cách sử dụng một luồng chính (single-thread) mà không cần nhiều luồng (thread) kết hợp với các callback để xử lý nhiều tác vụ cùng lúc mà không bị chặn.6 giai đoạn của Event Loop: Timers → I/O Callbacks → Idle → Poll → Check → Close Callbacks. Khi Nodejs chạy thì nó sẽ chạy theo luồng sau:

- 1️⃣ Chạy mã đồng bộ (Synchronous Code Execution).
- 2️⃣ Xử lý các callback từ các tác vụ bất đồng bộ (Asynchronous Callbacks).
- 3️⃣ Sử dụng Event Loop để kiểm tra hàng đợi (queue) và thực thi các tác vụ chờ xử lý.

### Phân biệt SQL và NoSQL:

- Cấu trúc dữ liệu: SQL (Structured Query Language): Cơ sở dữ liệu quan hệ, sử dụng bảng (tables) với các hàng (rows) và cột (columns). Dữ liệu có cấu trúc cố định. NoSQL: Cơ sở dữ liệu phi quan hệ, lưu trữ dữ liệu dưới dạng tài liệu (documents), cặp khóa-giá trị (key-value), đồ thị (graph), hoặc cột (column). Dữ liệu có thể không cấu trúc hoặc linh hoạt.
- Mở rộng: SQL: Tối ưu cho các hệ thống có quan hệ và yêu cầu dữ liệu liên kết chặt chẽ, mở rộng theo chiều ngang khó khăn. NoSQL: Phù hợp với các hệ thống phân tán, có thể mở rộng linh hoạt theo chiều ngang.
- Tính nhất quán: SQL: Dựa trên nguyên lý ACID (Atomicity, Consistency, Isolation, Durability), đảm bảo tính nhất quán dữ liệu. NoSQL: Thường sử dụng nguyên lý BASE (Basically Available, Soft state, Eventually consistent), có thể chấp nhận một số sự không nhất quán trong thời gian ngắn.
- Đặc điểm ứng dụng: SQL: Thích hợp cho các ứng dụng với dữ liệu có cấu trúc rõ ràng và quan hệ phức tạp. NoSQL: Phù hợp với ứng dụng yêu cầu xử lý dữ liệu lớn, không cấu trúc hoặc thay đổi nhanh chóng (ví dụ: mạng xã hội, phân tích big data).
- NoSQL không có quan hệ truyền thống như SQL không có ràng buộc giữa các bảng. Tuy nhiên cơ sở dữ liệu dạng đồ thị như Neo4j có thể lưu trữ và quản lý quan hệ các đối tượng dựa vào cạnh(edges) và đỉnh(nodes).

### Nên dùng if else hay object mapping hay switch case trong trường hợp sau:

```
// Cách dùng object mapping
const foodMap = {
  'apple': 20,
  'orange': 30,
  'banana': 10
};

const getPrice = name => {
  return foodMap[name] || '';
};

console.log(getPrice('apple')); // Kết quả: 20

// Cách dùng if else
const getPrice = name => {
  if (name === 'apple') {
    return 20;
  } else if (name === 'orange') {
    return 30;
  } else if (name === 'banana') {
    return 10;
  }
  return '';
};

console.log(getPrice('apple')); // Kết quả: 20

// Dùng switch case
const getPrice = name => {
  switch (name) {
    case 'apple':
      return 20;
    case 'orange':
      return 30;
    case 'banana':
      return 10;
    default:
      return '';
  }
};

console.log(getPrice('apple')); // Kết quả: 20
```

- Dùng object mapping:
  Ưu điểm: O(1), dễ mở rộng(chỉ cần thêm cặp key-value), code ngắn và dễ hiểu.
  Nhược điểm: Chỉ phù hợp với trường hợp ánh xạ đơn giản và phải có sẵn các trường hợp(cần biết trước cặp key-value), không xử lý được điều kiện phức tạp.
- Dùng if else:
  Ưu điểm: Linh hoạt, có thể xử lý các điều kiện phức tạp, không cần biết trước các trường hợp.
  Nhược điểm: O(n) với n là số điều kiện, khó bảo trì, dài dòng.
- Dùng switch case:
  Ưu điểm: Dễ đọc hơn if else, có thể nhanh hơn if else trong 1 số trình biên dịch.
  Nhược điểm: O(n) với n là số điều kiện trong trường hợp xấu nhất, phải liệt kê tất cả các key, nếu nhiều trường hợp thì khó đọc.

### Cách tối ưu hóa Docker:

- Giảm kích thước image: Dùng các image như Alpine, Sử dụng multi-stage builds(Phân chia quá trình xây dựng thành nhiều giai đoạn để chỉ giữ lại những gì cần thiết cho môi trường production. Ví dụ, biên dịch mã nguồn trong một giai đoạn và chỉ sao chép tệp thực thi sang giai đoạn cuối cùng), Dùng .dockerignore để bỏ tệp không cần thiết, gộp nhiều lệnh RUN thành 1 để giảm số lượng layer từ đó làm giảm kích thước images.
- Cải thiện hiệu suất của container: Giới hạn tài nguyên memory và cpu để tránh dùng quá nhiều tài nguyên hệ thống, dùng mạng phù hợp(như bridge, host, hoặc overlay) để cải thiện hiệu suất mạng của container, lưu trữ dữ liệu ngoài container thì dùng volume để cải thiện hiệu suất I/O và quản lý dễ dàng hơn.
  Dùng các công cụ tối ưu hóa Docker:
  - docker-slim: Công cụ này giúp giảm dung lượng hình ảnh Docker bằng cách loại bỏ các tệp, thư viện hoặc thành phần không cần thiết cho ứng dụng chạy, đồng thời tăng cường bảo mật.
  - dive: dive cho phép bạn xem chi tiết từng layer trong hình ảnh Docker, từ đó xác định các tệp hoặc dữ liệu dư thừa để tối ưu hóa Dockerfile.
  - Docker BuildKit: Backend xây dựng hình ảnh mới, hỗ trợ xây dựng song song và cache thông minh, giúp tăng tốc quá trình.

### Dùng "??":

```
const object = {
  name: 'tuanthanh',
  age: 21,
  sex: null,
}

console.log(object.name ?? 'unknown') // -> tuanthanh
console.log(object.age ?? 25) // -> 21
console.log(object.sex ?? 'male') // -> male
```

Trả về bên phải khi bên trái là null hoặc undefined.

### Return function:

```
// senior
const fetchDate = should => {
  return should && fetchData()
}
// junior
const fetchDate = should => {
  if (should){
    return fetchData()
  }
  else {
    return null
  }
}
```

### Thêm phần tử vào mảng:

```
// Good
const addItemToCart = (cart, item) => {
  return [
    ...cart,
    {
      item,
      date: Date.now()
    }
  ];
}

// Not good
const addItemToCart = (cart, item) => {
  cart.push({
    item,
    date: Date.now()
  });
}
```

- Cách 1 không làm thay đổi mảng cũ tránh tác dụng phụ ngoài ý muốn và dễ đọc dễ hiểu nhưng nếu cart là mảng lớn thì việc sao chép dữ liệu sẽ tốn tài nguyên hơn.
- Không tạo mảng mới mà chỉ thêm vào mảng cũ giúp nhanh hơn khi làm việc với mảng lớn, nên dùng nếu không quan tâm tính bất biến của mảng cũ, nhược điểm ảnh hưởng dến mảng cũ làm không an toàn nếu mảng cũ đang dùng ở chỗ khác.

### Dùng Stream trong Nodejs để đọc ghi file lớn.

### Đọc ghi cache tối ưu nhất để có tính nhất quán:

![](./assets/io-cache.png)
![](./assets/io-cache-use-rabbitmq.png)

### Message queue:

- Tại sao phải dùng message queue: Tách biệt và bất đồng bộ, Cân băng tải, Độ tin cậy, Khả năng mở rộng,Xử lý tác vụ nền, Giao tiếp giữa các hệ thống.

- Ưu điểm và nhược điểm của message queue:
  - Ưu điểm: Tách biệt và bất đồng bộ, Cân băng tải, Độ tin cậy, Khả năng mở rộng,Xử lý tác vụ nền, Giao tiếp giữa các hệ thống.
  - Nhược điểm: Độ phức tạp tăng, Độ trễ trong 1 task có thể cao hơn, Có thể message queue có thể bị nghẽn, Cần thêm tài nguyên để duy trì queue, Chi phí bảo trì cao hơn.

- Tại sao dùng RabbitMQ: Dùng làm message queue vì:
  - Hỗ trợ nhiều giao thức: AMQP, MQTT, STOMP, và HTTP.
  - Dễ triển khai( Có UI thân thiện).
  - Độ tin cậy cao(message không mất ngay cả khi hệ thống bị lỗi).
  - Khả năng mở rộng cao( Hỗ trợ clustering và federation giúp mở rộng tuy nhiên không mạnh bằng kafka thì xử lý lượng data lớn).
  - Cộng đồng lớn và mã nguồn mở (Được hỗ trợ bởi VMware).
  - Hỗ trợ đa nền tảng: RabbitMQ chạy tốt trên Linux, Windows, macOS và có thư viện client cho hầu hết các ngôn ngữ lập trình phổ biến như Python, Java, Node.js, .NET, v.v.

- Phân biệt RabbitMQ và Kafka:
  - RabbitMQ: Phát triển bằng ngôn ngữ Scala, độ tin cậy của dữ liệu trong RabbitMQ cao hơn Kafka. Thường dùng trong hệ thống tài chính thì đảm bảo tính nhất quán dữ liệu hơn Kafka.
    => Nếu dự án yêu cầu về chức năng thì nên dùng RabbitMQ.
  - Kafka: Được phát triển bằng ngôn ngữ Golang bởi Linkedin rồi sau đó Linkedin tặng cho Apache, thường dùng để ghi log, khả năng mở rộng của kafka thì mạnh hơn
    => Nếu hệ thống yêu cầu về hiêụ suất hơn thì nên dùng kafka.
    ==> Còn nếu phải lựa chọn chức năng và hiệu suất thì hiệu suất của Kafka là lựa chọn để dùng.
    ==> Kafka với triển khai fsync + epoch number + Raft protocol cho consistency + durability + reliability rất tốt, không thể nói là kém hơn rabbitmq được :D Cái đặc trưng nhất giữa 2 thằng cần nói ở đây là trade-off giữa low latency và high throughput -> Kafka dùng concept batch processing, nên cho throughput cao, độ trễ mỗi message ổn định, còn RabbitMQ ưu tiên low latency cho mỗi message, dùng cho real-processing application, bù lại nếu tải cao thì system bị stress dẫn đến tail latency cao chứ không ổn định theo batch như kafka.

### JWT:

- Cơ chế khi login(không dùng 2FA: nếu dùng thì thêm bước xác thực bằng OTP và OTP đó cache ở memcache hoặc redis):
  Khi login bằng email, password thì backend trả về refresh token thông qua httpOnly còn access token thì qua body -> frontend nhận được access token thì gán vào localStorage .

- Cơ chế khi logout:
  Gửi request lên backend -> backend xóa phiên đăng nhập + xóa refreshToken khỏi httpOnly -> frontend xóa accessToken khỏi localStorage.

- Cơ chế cấp lại refresh token:
  Gửi request lên backend, backend kiểm tra deviceId và refeshToken(có trong httpOnly) để xem coi có đúng user và thiết bị không, nếu đúng thì gen cặp refreshToken và accessToken mới rồi lưu refreshToken vào db/cache và trả về cặp 2 token cho frontend.

- Cơ chế xác minh trong mỗi request:
  Lấy accessToken rồi gán vào header của request rồi gửi lên backend .

  ```bash
  Authorization: Bearer <access_token>
  ```

### Cơ chế IoC(Inversion of Control):

- IoC là một nguyên lý thiết kế phần mềm trong đó quyền điều khiển việc khởi tạo và quản lý vòng đời của các đối tượng được chuyển từ lập trình viên sang cho framework xử lý.
- Thay vì một class tự tạo mới các phụ thuộc (dependencies) của nó bằng từ khóa `new`, các phụ thuộc này sẽ được framework "tiêm" (inject) vào từ bên ngoài (thường thông qua constructor).
- Giúp giảm sự phụ thuộc chặt chẽ (tight coupling) giữa các thành phần, giúp mã nguồn trở nên linh hoạt, dễ bảo trì và dễ dàng thay thế các module khi cần thiết.
- Tạo điều kiện thuận lợi cho việc viết Unit Test bằng cách cho phép dễ dàng thay thế các dịch vụ thật bằng các đối tượng giả (Mock/Stub).

  **Ví dụ:**

  ```typescript
  // 1. Khai báo Service (Provider)
  @Injectable()
  export class UsersService {
    findAll() {
      return [{ id: 1, name: "John Doe" }];
    }
  }

  // 2. Sử dụng IoC trong Controller
  @Controller("users")
  export class UsersController {
    // NestJS IoC Container sẽ tự động khởi tạo UsersService
    // và "tiêm" (inject) vào constructor này.
    constructor(private readonly usersService: UsersService) {}

    @Get()
    findAll() {
      return this.usersService.findAll();
    }
  }
  ```
