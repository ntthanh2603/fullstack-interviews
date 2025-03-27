# HTTP
- MITM (main in the middle attack)
- sniff packet
- fiddler
- extension: editthiscookie

# XSS (Cross Site Scripting) là một lỗi bảo mật cho phép hacker nhúng mã độc (javascript) vào
một trang web khác.
- OWASP (Open Web Application Security Project).
- Server XSS
	+) Persistent XSS
	+) Reflected XSS
- Client XSS
Solution:
	- Encoding
	- Validation/Sanitize
	- CSP (Content Security Policy)
	- Ref: https://excess-xss.com/, https://www.owasp.org/index.php/Cross-site_Scripting_(XSS)

# Cookie
- Cách chôm: 
	+) Sniff cookie qua mạng: Fiddler, Wireshark,
	+) Chôm cookie (Cookie thief) bằng XSS:
- Tấn công theo kiểu: CSRF (Cross-site request forgery). 
	Gửi 1 link ảnh `<img src="http://bank.example.com/withdraw?account=bob&amount=1000000&for=mallory" />`
- Solution:
	- Set Expired và Max-Age:
	- Sử dụng Flag HTTP Only:
	- Sử dụng Flag Secure:
**Lưu ý**: Nếu website của bạn sử dụng RESTful API, đừng sử dụng cookie để authorize người
dùng mà hãy dùng OAuth hoặc WebToken. Token này được vào Header của mỗi request nên
sẽ không bị dính lỗi CSRF.

Các bạn có thể tìm hiểu thêm về cookie và các lỗi bảo mật liên quan ở đây:
- http://resources.infosecinstitute.com/securing-cookies-httponly-secure-flags/
- http://www.ibm.com/support/knowledgecenter/SSZLC2_7.0.0/com.ibm.commerce.admin.doc/concepts/csesmsession_mgmt.htm
- https://www.nczonline.net/blog/2009/05/05/http-cookies-explained/
- https://en.wikipedia.org/wiki/HTTP_cookie#Secure_and_HttpOnly
- http://programmers.stackexchange.com/questions/298973/rest-api-security-stored-token-vs-jwt-vs-oauth

# SQL Injection

- http://expressmagazine.net/development/1512/tan-cong-kieu-sql-injection-va-cac-phong-chong-trong-aspnet.

## Solution

- Sử dụng ORM (Object-Relational Mapping) framework.
- Lọc dữ liệu từ người dùng:
- Không cộng chuỗi để tạo SQL:
- Không hiển thị exception, message lỗi:
- Phân quyền rõ ràng trong DB:
- Backup dữ liệu thường xuyên:

## Ref

- http://www.w3schools.com/sql/sql_injection.asp
- http://expressmagazine.net/development/1512/tan-cong-kieu-sql-injection-va-cac-phong-chong-trong-aspnet
- http://freetuts.net/ky-thuat-tan-cong-sql-injection-va-cach-phong-chong-trong-php-107.html
- http://kienthucweb.net/sql-injection-la-gi.html

# INSECURE DIRECT OBJECT REFERENCES

## Các keyword
- brute force, ví dụ https://junookyo.blogspot.com/2016/10/ro-ri-3-trieu-thong-tin-ca-nhan.html

## Solution

- Test cẩn thận
- Bảo vệ dữ liệu “nhảy cảm”
- Kiểm tra chặt chẽ quyền truy cập của
- Tránh để lộ key của đối tượng

## Ref

- https://www.owasp.org/index.php/Top_10_2013-A4-Insecure_Direct_Object_References
- http://lockmedown.com/secure-from-insecure-direct-object-reference/
- https://codedx.com/insecure-direct-object-references/

# CSRF
CSRF có tên đầy đủ là Cross Site Request Forgery (Tên khác là XSRF).

## Các kiểu tấn công
- Kiểu 1. Dùng form
- Kiểu 2. Dùng thẻ img

## Solution

- **Sử dụng CSRF Token**: Trong mỗi form hay request, ta đính kèm một CSRF token. Token
này được tạo ra dựa theo session của user. Khi gửi về server, ta kiểm tra độ xác thực
của session này. Do token này được tạo ngẫu nhiên dựa theo session nên hacker
không thể làm giả được (Các framework như RoR, CodeIgniter, ASP.NET MVC đều hỗ
trợ CSRF token).
- **Kiểm tra giá trị Referer và Origin trong header**: Origin cho ta biết trang web gọi
request này. Giá trị này được đính kèm trong mỗi request, hacker không chỉnh sửa
được. Kiểm tra giá trị này, nếu nó là trang lạ thì không xử lý request.
- **Kiểm tra header X-Requested-With**: Request chứa header này là request an toàn, vì
header này ngăn không cho ta gửi request đến domain khác (chi tiết).
- **Cần cẩn thận đề phòng lỗi XSS**: Với XSS, hacker có thể cài mã độc trên chính trang
web cần tấn công. Lúc này, mọi phương pháp phòng chống CSRF như token,
referrer đều bị vô hiệu hoá. Bản thân bác juno_okyo từng áp dụng lỗi CSS kết hợp
CSRF để tấn công sinhvienit.net (Chi tiết).

## Ref

- https://en.wikipedia.org/wiki/Cross-site_request_forgery
- https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)
- http://stackoverflow.com/questions/17478731/whats-the-point-of-the-x-requested-with-header
- https://www.youtube.com/watch?v=m0EHlfTgGUU

# ẨN GIẤU THÔNG TIN HỆ THỐNG – TRÁNH CON MẮT NGƯỜI ĐỜI VÀ KẺ XẤU
- Thông tin hệ thống là gì?
- Chúng ta để thông tin hệ thống “hớ hênh” như thế nào?
- Truy cập buildwith.com, trang này hoạt động trên nguyên lý đọc các header trả về từ server, xem HTML include các thư viện nào.
- Những hậu quả của việc “lộ hàng”: truy cập nvd.nist.gov để tìm hiểu thông tin thêm về version các database. Từ đó biết được các đường dẫn của admin page như wordpress, joomla, phpadmin...
- Giấu như thế nào cho đúng?
	+) Config server hoặc viết code để loại bỏ những HTTP header dư thừa.
	+) Khi deploy, ta obfustace hoặc uglify code để code trở nên khó đọc. Để tránh việc
hacker biết các thư viện JS sử dụng thì ta có thể bundle toàn bộ thư viện và code thành 1 file luôn.
	+) Khi hệ thống bị thống, hiển thị custom Error Page. Lỗi trong trang này nên giải thích rõ ràng cho người dùng hiểu. Nhưng tuyệt đối không hiển thị trực tiếp error/exception để tránh hacker tấn công.
	+) Thường xuyên cập nhật/nâng cấp framework lên phiên bản mới nhất để vá các lỗ hỗng, tránh việc hacker lợi dụng những lỗ hổng đã phát hiện ở phiên bản cũ.

# QUẢN LÝ NGƯỜI DÙNG – TƯỞNG DỄ ĂN MÀ KHÔNG ĐƠN GIẢN

## Úi giời! Đăng kí đăng nhập có gì khó?

Các tính năng có thể được yêu cầu:

- Cho phép người dùng đăng kí, đăng nhập bằng email
- Phân quyền người dùng
- Tích hợp với Gmail, Facebook
- Tích hợp với hệ thống người dùng có sẵn trong doanh nghiệp
- Reset mật khẩu khi người dùng quên
- Block account khi người dùng nhập sai pass nhiều lần
- Bảo mật cho API với app di động
- Bảo mật 2 lớp (Two factor authentication) với các account quan trọng
- Quản lý: Thêm bớt xoá sửa người dùng

## Quan trọng nhất – Không lưu mật khẩu!

Tuyệt đối không bao giờ lưu mật khẩu khách hàng, dù sếp có nói gì đi nữa! Là một developer có tâm, bạn không bao giờ được lưu mật khẩu của khách hàng vào database (nhắc lại lần thứ ba cho nhớ).

## Làm thế nào khi người dùng quên mật khẩu?
- Cách 1: Reset mật khẩu mới ngẫu nhiên rồi gửi cho người dùng
- Cách 2: Gửi link để người dùng reset

## Chống việc đoán mò mật khẩu

- Khi người dùng đăng nhập sai, đừng báo là sai username hay sai password. Chỉ cần báo username hay password không match, hacker sẽ gặp khó khăn hơn.
- Hacker lợi dụng chức năng reset mật khẩu để dò xem người dùng có email trên trang đó hay không. Dù account có tồn tại hay không, ta vẫn chỉ hiện thông báo: đã gửi message.
- Hạn chế số lần đăng nhập khi nhập mật khẩu sai. Ví dụ sau 3 lần nhập pass sai thì khoá account trong 10 phút. Hacker có thể dùng cách này để khoá tài khoản người dùng, nên cẩn thận. Có thể kết hợp thêm capcha.

## Những biện pháp nho nhỏ tăng cường bảo mật

- Với các thao tác quan trọng như đổi email, đổi pass, xoá nick, cần bắt người dùng nhập lại password. Lý do là đôi khi người dùng bị chôm cookie, hoặc lơ là quên khoá máy. Hãy nhìn Facebook và Google, cả 2 trang này đều bắt ta phải nhập lại mật khẩu khi muốn đổi pass.
- Với các ứng dụng cần bảo mật cao, phải có Two-factor verification (Gửi tin nhắn, device tạo authentication token). Mình hiện tại cũng đang dùng nó, dù các bạn có biết mật khẩu Gmail hay WordPress của mình cũng “éo” thể nào đăng nhập được.
- Nên khuyến khích (hoặc bắt buôc) người dùng sử dụng mật khẩu dài, đi kèm chữ và số, viết hoa viết thường và kí tự đặc biệt. Máy móc rất hiện đại khi crack mật khẩu, có thể vào đây để test xem máy mất bao lâu để mò ra mật khẩu của bạn.
- Nếu site của bạn không có HTTPS, hoặc team không có kinh nghiệm làm bảo mật, cứ để cho bọn khác lo. Bạn có thể dùng OAuth, cho phép người dùng đăng nhập từ Google, Facebook.
- Lúc này Google, Facebook sẽ chịu trách nhiệm quản lý mật khẩu và dữ liệu của người dùng. Người dùng thì không cần phải đăng kí nhiều tài khoản, một công đôi việc. Tìm hiểu thêm tại https://oauth.io/ hoặc https://auth0.com/.

# Các keyword quan trọng
- sniffing
- impersonation
- deface website
- obfuscate code
- chuẩn OOP và SOLID
- tool chuyên dụng (Kali Linux, Tool PenetrationTest)
- (Còn vô số điều hay ho như: social engineering, row hammering
- dictionary attack – hash
- Cách tạo mật khẩu:
	1. Khi tạo mật khẩu, tạo random một chuỗi kí tự gọi là salt.
	2. Salt sẽ được cộng vào sau mật khẩu, toàn bộ chuỗi mật khẩu và salt sẽ bị băm (hash).
	3. Lưu salt và giá trị đã băm xuống database (Một người dùng sẽ có 1 salt riêng).
	4. Khi người dùng đăng nhập, lấy salt của người dùng, cộng nó với mật khẩu họ nhập
	vào, hash ra rồi so với giá trị trong database.

Với cách này, khi người dùng quên mật khẩu, hệ thống không tài nào mò ra mật khẩu để gửi cho họ. Cách giải quyết duy nhất là reset mật khẩu, random ra một mật khẩu mới rồi gửi cho họ.
- Thử hack bằng cách giả vờ quên mật khẩu. Xem nếu như có gửi thẳng giá trị mật khẩu về không. Nếu có thì sao, thì lưu vào database mà.

# Ref
- https://www.pluralsight.com/courses/hack-yourself-first 
- https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project
- https://drive.google.com/file/d/0B-hTIM3aG_oJWnpxdGtXV2Z2YlE/view