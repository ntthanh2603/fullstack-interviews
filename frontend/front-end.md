# Phỏng vấn Front-end

## Nguồn <a href="https://h5bp.org/Front-end-Developer-Interview-Questions" target="_blank" rel="noopener noreferrer">h5bp.org</a>

<a id="top"></a>
## Mục lục
1. [Các câu hỏi chung](#general_questions)
2. [Các câu hỏi về HTML](#html_questions)
3. [Các câu hỏi về CSS](#css_questions)
4. [Các câu hỏi về JavaScript](#js_questions)
5. [Các câu hỏi về Testing](#testing_questions)
6. [Các câu hỏi về Performance](#performance_questions)
7. [Các câu hỏi về Network](#network_questions)
8. [Các câu hỏi về việc viết code](#coding_questions)
9. [Các câu hỏi vui](#fun_questions)

<a id="general_questions"></a>
### 1. Các câu hỏi chung

- Bạn đã học được gì trong ngày hôm qua / tuần này?\
	...

- Điều gì về lập trình làm bạn hứng thú?\
	Tôi thích việc viết code để tạo ra các ứng dụng, lập trình rất khó, mà khó thì tôi lại càng thích.

- Một thử thách về mặt kĩ thuật bạn đã trải qua gần đây là gì và bạn đã giải quyết nó như thế nào?\
	Tội gặp vấn đề về UX, Performance khi tôi làm project cá nhân. Tôi đã phải bỏ bớt các phần để trang nhanh hơn, và vẫn đang học hỏi cách tối ưu hóa khi xây dựng website.

- Bạn cân nhắc những Giao diện người dùng (UI), Vấn đề bảo mật (Security), Hiệu suất (Performance), Khả năng tối ưu cho các bộ máy tìm kiếm (SEO), Khả năng bảo trì (Maintainability) hay Công nghệ (Technology) nào khi xây dựng một ứng dụng/trang web?\
	...

- Hãy nói về môi trường lập trình mà bạn muốn.\
	Tôi muốn làm trong 1 trường với những người có cùng đam mê lập trình như mình.

- Những hệ thống quản lý phiên bản (version control systems) nào mà bạn đã sử dụng và cảm thấy quen thuộc?\
	Git.

- Bạn có thể trình bày mạch làm việc (workflow) của bạn khi bạn tạo một trang web không?\
	Tìm các thông tin về chủ đề của trang web, các trang web liên quan -> Dựa vào các trang có sẵn để Mockup Layout -> Chọn theme màu, font, framework html, css, js(ví dụ: bootstrap) -> chọn framework JS, css preprocessor -> tổ chức thư mục dự án -> dựng từng block html + fetch API (nếu cần) -> responsive -> Viết JS logic cho các phần cần xử lí -> kiểm tra hiển thị trên các trình duyệt khác như nhau -> clean up + minimize fize -> deploy.

- Nếu bạn có 5 stylesheet khác nhau, bạn sẽ tích hợp chúng vào trang như thế nào là tốt nhất?\
	Gom chung lại thành 1 file css.

- Bạn có thể trình bày sự khác nhau giữa progressive enhancement và graceful degradation không?
	- <a href="https://developer.mozilla.org/en-US/docs/Glossary/Progressive_enhancement" target="__blank" rel="noopener noreferrer">Progressive Enhancement</a> là 1 triết lí thiết kế mang lại trải nghiệm tốt nhất cho các người dùng sử dụng trình duyệt hiện đại.
	- <a href="https://developer.mozilla.org/en-US/docs/Glossary/Graceful_degradation" target="__blank" rel="noopener noreferrer">Graceful degradation</a> là 1 triết lí thiết kế rơi vào 1 trải nghiệm không tốt nhưng vẫn cung cấp nội dung và chức năng cho các trình duyệt cũ.

- Bạn sẽ tối ưu các tài nguyên (assets/resources) của một website như thế nào?
	- Tối ưu hóa kích thước hình ảnh.
	- Không sử dụng **@import** trong file **css**, không sử dụng một file css quá nhỏ, tốt nhất nên gộp nó vào 1 file.
	- Nối tệp (**File concatenation**) là kết hợp nhiều file thành một file duy nhất để làm giảm số lượng yêu cầu HTTP
	- Rút gọn tệp (**File minification**) là loại bỏ tất cả các ký tự không cần thiết khỏi mã nguồn của chương trình mà không làm thay đổi chức năng của nó.
	- Bộ nhớ đệm (**Caching**) có hai dạng:
	- Bộ nhớ đệm phía máy chủ sử dụng để chuẩn bị một tài nguyên thường được yêu cầu để máy chủ sẵn sàng gửi tài nguyên đó ngay lập tức
	- Bộ nhớ đệm phía máy khách theo dõi các tệp quan trọng trong bộ nhớ phiên hoặc trong bộ nhớ cục bộ của trình duyệt để khi người dùng truy cập lại một trang web, họ không cần tải lại tất cả các tệp của trang đó.

- Một trình duyệt sẽ tải bao nhiêu tài nguyên cùng một lúc từ một tên miền (domain) cho trước?\
	Tôi ko biết chính xác. Phụ thuộc vào trình duyệt, trình duyệt càng hiện đại thì hỗ trợ tải xuống càng nhiều hơn.

- Các exception là gì?\
	**Exception** là một sự kiện xảy ra trong quá trình thực thi một chương trình làm phá vỡ **flow** của chương trình đó.

- Nêu 3 cách để giảm tải trang web (perceived hoặc thời gian tải thực tế (actual load time).
	- Giảm thiểu nội dung, giảm số lượng yêu cầu HTTP cho tệp
	- G-zipping
	- Tối ưu hóa hình ảnh của bạn
	- Tìm nạp trước tài nguyên
	- Lazy loading img

- Nếu bạn tham gia vào một dự án và họ xài nút Tab trong khi bạn sử dụng nút dấu cách (spaces), bạn sẽ làm gì?\
	Chuyển qua xài nút Tab hoặc config lại nút space cho giống nút tab của họ.

- Trình bày cách bạn sẽ làm một trang slideshow đơn giản.\
 	Sử dụng thẻ label và input radio, input đặt id và đặt trước các slide element, label làm navigation bar và đặt id giống input, để khi click vào label nó sẽ check input và sử dụng css để hiển thị các slide sau ô input, đây là <a href="https://codepen.io/hoanggaphan/pen/QWEymEM" target="__blank" rel="noopener noreferrer">code mẫu</a>.

- Nếu bạn có thể chuyên sâu về một công nghệ (technology) trong năm nay thì nó sẽ là công nghệ gì?\
	ReactJS / NextJS

- Giải thích tầm quan trọng của các standards và standards bodies.
	- Với việc sử dụng standards và standards bodies, các nhà phát triển và kỹ sư sẽ có thể đạt được một trang web ổn định hơn với sự ra đời liên tục của phần cứng và phần mềm mới.
	- Các tiêu chuẩn cho phép khả năng tương thích ngược và xác nhận vì chúng được viết để tuân thủ các phiên bản trình duyệt cũ hơn. Mã tuân thủ này có thể được xác thực thông qua một dịch vụ xác thực, giúp công việc của nhà phát triển dễ dàng hơn rất nhiều, dẫn đến ít thời gian sản xuất hơn
	- Ngoài ra còn thể làm tăng sự thành công của công cụ tìm kiếm.

- Flash of Unstyled Content là gì? Bạn tránh FOUC như thế nào?
	- **FOUC** là viết tắt của **Flash of Unstyled Content** chỉ xuất hiện trong lần tải trang đầu tiên, nó thể hiện thoáng qua nội dung trang chưa được định kiểu, là tác dụng phụ khi sử dụng** CSS @import** trong file css.
	- Để giải quyết ta nên tách phần** CSS @import** đó ra thành 1 file css riêng.

- Giải thích ARIA và screen readers là gì, và làm thế nào để làm cho một trang web có thể truy cập được.
	- **ARIA** là viết tắt của **Accessible Rich Internet Applications**. ARIA là một tập hợp các thuộc tính mà bạn có thể thêm vào các phần tử HTML, xác định các cách để làm cho nội dung web và ứng dụng có thể truy cập được cho người dùng khuyết tật. Khi các vấn đề về khả năng truy cập không thể được quản lý bằng HTML gốc, ARIA có thể giúp thu hẹp những khoảng cách đó.
	- **Screen readers** là một công nghệ hỗ trợ được sử dụng bởi những người mù, khiếm thị hoặc cần hỗ trợ thêm trong việc điều hướng trang web

- Giải thích một vài ưu và khuyết điểm của CSS animations so với JavaScript animations.\
	**Ưu điểm:** Các animation dựa trên **CSS** và **Web Animation** được hỗ trợ native và được xử lí trong tiến trình **compositor thread**. Khác với **main thread** của trình duyệt, nơi mà styling, layout, painting và code Javascript được thực thi. Nghĩa là nếu như trình duyệt đang bận xử lý 1 số tác vụ nặng trên **main thread** thì những animation đó vẫn có thể tiếp tục thực hiện mà không bị can thiệp.
	**Khuyết điểm:** Phải tính toán các thông số trước cho các animations vì vậy mức độ kiểm soát các animation sẽ kém linh hoạt. Ngoài ra 1 vài trường hợp sử dụng thuộc tính css sẽ trigger layout, paint hoặc cả 2 sẽ khiến **main thread** tham gia xử lí, điều này có thể ảnh hưởng đến hiệu suất.

- CORS là từ viết tắt của cái gì và nó đề cập đến vấn đề nào?\
	**CORS** viết tắt của từ **Cross-origin resource sharing**, là một cơ chế cho phép nhiều tài nguyên khác nhau (fonts, Javascript, v.v…) của một trang web có thể được truy vấn từ domain khác với domain của trang đó.

<a id="html_questions"></a>
### 2. Các câu hỏi về HTML:

- Một doctype làm cái gì?\
	**DOCTYPE** là viết tắt của "document type". Nó là một khai báo được sử dụng trong HTML để phân biệt giữa chế độ tiêu chuẩn và chế độ quirks (không tiêu chuẩn). Sự hiện diện của nó cho phép trình duyệt hiển thị trang web ở chế độ tiêu chuẩn.

- Sự khác nhau giữa chế độ full standards, chế độ almost standards và chế độ quirks?
	- **Quirks Mode** bố cục mô phỏng hành vi không chuẩn trong Navigator 4 và Internet Explorer 5.
	- **Full Standards Mode** là hành vi được mô tả bởi các đặc tả HTML và CSS.
	- **Almost Standards** chỉ có một số lượng rất nhỏ các câu hỏi được thực hiện.

- Sự khác nhau giữa HTML và XHTML là gì?
	- **HTML** là ngôn ngữ đánh dấu tiêu chuẩn để tạo các trang web và ứng dụng web.
	- **XHTML** là một ngôn ngữ đánh dấu có cùng các khả năng như HTML, nhưng có cú pháp chặt chẽ hơn. Sự khác biệt chính giữa HTML và XHML là, HTML dựa trên SGML trong khi XHTML dựa trên XML.

- Có vấn đề nào không khi cung cấp (serving) các trang theo kiểu application/xhtml+xml?\
	...

- Bạn cung cấp một trang web có nội dung được viết bằng nhiều ngôn ngữ như thế nào?\
	Cho phép người dùng chọn ngôn ngữ trước khi truy cập trang, sau đó lưu lại trong store của browser để lần sau không phải chọn lại. Đồng thời thêm 1 danh sách ngôn ngữ trên trang để user có thể chuyển đổi.

- Bạn phải cảnh giác những điều gì khi thiết kế hoặc phát triển các trang web đa ngôn ngữ?
	- Chuyển đổi ngôn ngữ chỉ mang tính tương đối nếu sử dụng API translate.
	- Dịch thủ công sẽ tốn kém thời gian và chi phí dịch thuật.

- Những thuộc tính data- có lợi cho cái gì?\
	Thuộc tính data được giới thiệu trong HTML5 dùng để lưu dữ liệu trực tiếp trên trang HTML.

- Hãy xem HTML5 như một nền tảng web mở (open web platform). Những building blocks của HTML5 là gì?
	- Ngữ nghĩa (Semantics): Cho phép bạn mô tả chính xác hơn nội dung của bạn.
	- Kết nối (Connectivity): Cho phép bạn giao tiếp với máy chủ theo những cách mới và sáng tạo.
	- Ngoại tuyến và lưu trữ (Offline and storage): Cho phép các trang web lưu trữ dữ liệu ở phía máy client và hoạt động ngoại tuyến hiệu quả hơn.
	- Đa phương tiện (Multimedia): Video và âm thanh hoàn hảo hơn trong Open Web.
	- Đồ họa và hiệu ứng (2D/3D graphics and effects): Cho phép nhiều tùy chọn trình bày đa dạng hơn.
	- Hiệu suất và tích hợp (Performance and integration): Cung cấp việc tối ưu hóa tốc độ và sử dụng phần cứng máy tính tốt hơn.
	- Truy cập thiết bị (Device access): Cho phép sử dụng các thiết bị đầu vào và đầu ra khác nhau.
	- Styling: Cho phép tác giả viết các chủ đề phức tạp hơn.

- Trình bày sự khác nhau giữa một cookie, sessionStorage và localStorage.

|   | cookie | localStorage | sessionStorage |
| ------------ | ------------ | ------------ | ------------ |
| Initiator | Client or server. Server can use Set-Cookie header | Client | Client |
| Expiry | Manually set | Forever | On tab close |
| Persistent across browser sessions | Depends on whether expiration is set | Yes | No |
| Sent to server with every HTTP request | Cookies are automatically being sent via Cookie header | No | No |
| Capacity (per domain) | 4kb | 5MB | 5MB |
| Accessibility | Any window | Any window | Same tab |

- Trình bày sự khác nhau giữa `<script>`, `<script async>` và `<script defer>`.
	- `<script>`:  Quá trình phân tích HTML bị chặn, tập lệnh được tìm nạp và được thực hiện ngay lập tức, cú pháp HTML sẽ tiếp tục phân tích sau khi tập lệnh được thực thi.
	- `<script async>`:  Tập lệnh sẽ được tìm nạp song song với phân tích cú pháp HTML và được thực thi ngay sau khi nó có sẵn (có khả năng trước khi phân tích cú pháp HTML hoàn thành). Sử dụng async khi tập lệnh độc lập với bất kỳ tập lệnh nào khác trên trang, ví dụ: **analytics**
	- `<script defer>`: Tập lệnh sẽ được tìm, nạp song song với phân tích cú pháp **HTML** và được thực thi khi trang đã phân tích xong. Nếu có nhiều tập lệnh thì chúng được thực thi theo thứ tự trong tài liệu. Nếu tập lệnh được đặt hoàn toàn trong DOM, thuộc tính defer sẽ hữu ích trong việc đảm bảo rằng **HTML** được phân tích cú pháp đầy đủ trước khi thực thi. Không có nhiều khác biệt trong việc đặt `<script>` bình thường ở cuối `<body>`
**Note**: Lưu ý: Các thuộc tính **async** và **defer** được bỏ qua cho các tập lệnh không có thuộc tính src.

- Tại sao việc đặt các thẻ (tag) `<link>` CSS giữa 2 thẻ `<head></head>` và các thẻ `<script>` JS ngay trước thẻ `</body>` về cơ bản là một ý tưởng tốt? Bạn có biết những trường hợp ngoại lệ nào khác không?
	- Đặt thẻ `<link>` css vào thẻ `<head>` nhằm load trước css, để khi trang load nội dung trang thì css đã chuẩn bị sẵn. Nhầm tránh hiện tượng nội dung đã tải nhưng chưa có css.
	- Thẻ `<script>` sẽ chặn việc phân tích cú pháp HTML cho đến khi các **script** được tải xuống và thực thi xong. Tải xuống các **script** ở dưới cùng sẽ cho phép HTML phân tích cú pháp và hiển thị UI cho người dùng trước tiên.

- Progressive rendering là gì?
	- **Progressive rendering** là kỹ thuật hiển thị tuần tự các phần của trang trong máy chủ và truyền trực tuyến đến máy khách từng phần mà không cần đợi toàn bộ trang được hiển thị.

- Tại sao bạn sẽ sử dụng thuộc tính srcset trong 1 tag img? Giải thích quá trình mà trình duyệt sẽ sử dụng khi phân tích nội dung của thuộc tính này.
	- Khi bạn muốn cung cấp hình ảnh chất lượng khác nhau tùy vào độ rộng hiển thị của thiết bị. Cung cấp hình ảnh chất lượng cao cho các thiết bị có độ phân giải cao, và ngược lại cung cấp hình ảnh chất lượng thấp cho các thiết bị có độ phân giải thấp để tránh lãng phí.
	- Ví dụ: đoạn code `<img srcset="small.jpg 500w, medium.jpg 1000w, large.jpg 2000w" src="..." alt="">`. Giá trị đầu tiên là tên hình ảnh, thứ hai là chiều rộng của hình ảnh theo pixel. Đối với chiều rộng thiết bị 320px, các tính toán sau được thực hiện như sau:
		- 500 / 320 = 1.5625
		- 1000 / 320 = 3.125
		- 2000 / 320 = 6.25
	- Nếu độ phân giải của client là 1x, 1.5625 gần với giá độ phân giải này nhất nên browser sẽ sử dụng ảnh **small.jpg**
	- Nếu độ phân giải là 2x (retina), browser sẽ chọn 3.125. Bởi vì 1.5625 nhỏ hơn 2x và bức ảnh sẽ bị vỡ khi hiển thị. Browser sẽ chọn ảnh tỉ lệ ít nhất phải lớn hơn 2x, là 1000w (3.125).

- Trước đây bạn đã bao giờ sử dụng những ngôn ngữ template HTML nào khác chưa?\
	Đã sử dụng qua **Pug**, **EJS** trong nodejs, template HTML hỗ trợ 1 số chức năng như kế thừa layout, include block, conditional, ... trực tiếp trên HTML.

<a id="css_questions"></a>
### 3. Các câu hỏi về CSS:

- Điểm khác biệt giữa class và ID trong CSS là gì?\
	Class có thể sử dụng cho nhiều tag, còn id chỉ gán cho 1 tag duy nhất

- Sự khác nhau giữa “resetting” và “normalizing” CSS là gì? Bạn sẽ chọn cái nào, và tại sao?
	-  **Reseting** sẽ loại bỏ tất cả kiểu trình duyệt đã tích hợp sẵn. Các yếu tố tiêu chuẩn như h1-6, p, ... sẽ bị loại bỏ kiểu tích hợp sẵn và trông giống hệt nhau.
	- **Normalizing** làm cho kiểu trình duyệt tích hợp sẵn nhất quán trên nhiều trình duyệt web khác nhau. Các yếu tố tiêu chuẫn như h1-6,... sẽ có cùng kiểu định sẵn trên các trình duyệt khác nhau. 

- Trình bày về Floats và cách chúng hoạt động.
	- **Float** là 1 thuộc tính css sử dụng để định dạng bố cục trang, di chuyển 1 phần tử sang trái hoặc phải trong không gian bao quanh nó.
	- Thuộc tính:
		- **left**: trượt về bên trái.
		- **right**: trượt về bên phải.
		- **none**: nằm tại vị trí của nó.
		- **inherit**: kế thừa thuộc tính float từ phần tử cha.
	- **Chú ý**: các phần tử cùng cấp với phần tử float sẽ tràn lên và lắp đầy chỗ trống, để tránh việc này ta phải sử dụng thuộc tính **clear css**.

- Trình bày về z-index và làm thế nào để nội dung stack với nhau được định hình.
	- **z-index** css đặt mức độ ưu tiên của phần tử và các thành phần con bên trong phần tử đó. phần tử có mức ưu tiên cao sẽ nằm trên phần tử có mức ưu tiên thấp hơn.
	- Để nội dung stack với nhau được định hình:
	- Xác định mức độ ưu tiên hiện tại của phần tử chỉ định so với các mức độ nội dung stack.
	- Xác định xem các phần tử cha bao quanh phần tử chỉ định có được đặt mức độ ưu tiên hay không.

- Trình bày về BFC (Block Formatting Context) và cách nó hoạt động.\
	BFC là một phần của một render CSS hình ảnh của một trang web. Đó là khu vực mà bố cục của các hộp khối xuất hiện và trong đó các khối nổi tương tác với các phần tử khác.
	Ngữ cảnh định dạng khối là một hộp HTML đáp ứng ít nhất một trong các điều kiện sau:
	- Giá trị của float không phải là none
	- Giá trị của position không phải static cũng không relative
	- Giá trị của display là table-cell, table-caption, inline-block, flex, hoặc inline-flex
	- Giá trị của overflow không phải là visible.

- Các kĩ thuật clearing khác nhau là những kĩ thuật nào và phù hợp với hoàn cảnh nào?
	- Khi một phần tử cha chứa một phần tử con có thuộc tính float, thì các phần tử sau nó vẫn sẽ bị ảnh hưởng bởi float, ta phải sử dụng kỹ thuật **clearfix** để fix trường hợp này, dưới đây là 1 số cách:
	- Thêm `overflow: auto;` vào trong phần tử cha chứa phần tử sử dụng float để tạo ra một **BFC**, đây là cách sử dụng đơn giản nhất nhưng khuyết điểm là nó có thể hiển thị thanh scroll không mong muốn.
	- Sử dụng `display: flow-root;` thay vì `overflow: auto;` giống như overflow tạo ra một **BFC** nhưng không gây ra bất kì tác dụng phụ nào.
	- Tạo 1 thẻ `<div>` có thuộc tính css `clear: both` đặt vào cuối trong phần tử cha chứa phần tử float.
	- Còn 1 cách được sử dụng khá phổ biến là sử dụng class Pseudo và để thêm thuộc tính `clear: both`
		- Link tham khảo: <a href="https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Block_formatting_context" target="__blank" rel="noopener noreferrer">https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Block_formatting_context</a>.
		- <a href="https://codepen.io/hoanggaphan/pen/rNLLOqr" target="__blank" rel="noopener noreferrer">Code mẫu</a>.

- Giải thích về CSS sprites, và làm thế nào để bạn thực hiện chúng trên một trang web.\
	Sprites là một hình ảnh lớn được tạo ra bằng cách gộp nhiều ảnh nhỏ lại với nhau. Để hiển thị được một ảnh nhỏ từ Sprite Image, thay vì sử dụng qua thẻ `<img />` thì ta phải sử dụng thuộc tính **background** kết hớp với **background-position** để xác định vị trí chính xác của bức ảnh cần, Chúng ta có thể tạo ra **image sprites** bằng cách sử dụng 1 số tool như **photoshop** hoặc các trình edit online như:  [https://www.toptal.com/developers/css/sprite-generator](https://www.toptal.com/developers/css/sprite-generator "https://www.toptal.com/developers/css/sprite-generator").

- Bạn sẽ tiếp cận như thế nào để khắc phục các sự cố tạo kiểu cụ thể cho trình duyệt?
	- Sử dụng các thư viện như Bootstrap đã xử lý các vấn đề tạo kiểu này cho bạn.
	- Sử dụng **autoprefixer** để tự động thêm tiền tố của nhà cung cấp vào mã của bạn.
	- Sử dụng Reset.css hoặc Normalize.css.

- Bạn sẽ cung cấp các trang của bạn trên các trình duyệt hạn chế tính năng như thế nào? Bạn sử dụng những kỹ thuật / quy trình nào?
	- **Graceful degradation** - xây dựng ứng dụng cho các trình duyệt hiện đại nhưng vẫn đảm bảo hoạt động trong các trình duyệt cũ
	- **Progressive enhancement** -  xây dựng ứng dụng cho mức trải nghiệm người dùng cơ bản, nhưng thêm các chức năng cải tiến khi trình duyệt hỗ trợ nó.
	- Sử dụng caniuse.com để kiểm tra hỗ trợ tính năng.
	- Trình sửa lỗi tự động để chèn tiền tố nhà cung cấp tự động.
	- Phát hiện tính năng bằng Modernizr .

- Các cách khác nhau để ẩn nội dung một cách trực quan (và chỉ hiển thị cho trình đọc màn hình)?
	- **visibility: hidden**. Tuy nhiên, phần tử vẫn nằm trong trên trang và vẫn chiếm dung lượng.
	- **width: 0; height: 0**. Làm cho phần tử không chiếm bất kỳ không gian nào trên màn hình, dẫn đến không hiển thị phần tử đó.
	- **position: absolute; left: -99999px**. Đặt nó bên ngoài màn hình.
	- **text-indent: -9999px**. Điều này chỉ hoạt động khi văn bản nằm trong phần tử block.
	- Tôi sẽ sử dụng** position: 'absolute'**, vì nó có ít cảnh báo nhất và hoạt động với hầu hết các yếu tố.

- Bạn đã bao giờ sử dụng hệ thống lưới chưa, và nếu có, bạn thích cái nào hơn?\
	Tôi thích hê thống flex-box, grid. Nó đã được sử dụng cho Bootstrap trong nhiều năm và đã chứng minh sự tồn tại của mình.

- Bạn đã sử dụng hoặc triển khai các truy vấn phương tiện hoặc bố cục / CSS dành riêng cho thiết bị di động chưa?\
	Rồi, một ví dụ điển hình là chuyển đổi thanh navigation bar của header thành một hamburger button, khi click sẽ show ra list navigation.

- Bạn có quen với việc tạo kiểu SVG không?\
	Nope... (==)!

- Làm cách nào để bạn tối ưu hóa các trang web của mình để in?
	- Tạo css để in hoặc sử dụng các truy vấn phương tiện.
		```html
		<!-- Main stylesheet on top -->
		<link rel="stylesheet" href="/global.css" media="all" />
		<!-- Print only, on bottom -->
		<link rel="stylesheet" href="/print.css" media="print" />
		```
	- Đảm bảo đặt các kiểu không in bên trong **@media screen { ... }**.
	- Thêm ngắt trang.
		```html
		<style>
		.page-break {
		  display: none;
		  page-break-before: luôn luôn; 
		}
		</style>

		Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Fusce eu felis. Curabitur ngồi amet magna. Nullam aliquet. Aliquam ut diam ... 
		<div class = "page-break"> </div> 
		Lorem ipsum dolor sit amet, consectetuer adipiscing elit ....
		```
	- Tham khảo: 
		- <a href="https://davidwalsh.name/optimizing-structure-print-css" target="__blank" rel="noopener noreferrer">https://davidwalsh.name/optimizing-structure-print-css</a>.
		- <a href="https://davidwalsh.name/optimizing-content-print-css" target="__blank" rel="noopener noreferrer">https://davidwalsh.name/optimizing-content-print-css</a>.

- Một số“bí quyết”để viết CSS hiệu quả là gì?
	- Khai bao css theo 1 khối thứ tự (**Declaration order**)
	- Không sử dụng **@import** trong css
	- Đặt **@media** gần với css có liên quan không nên tách ra 1 file css khác
	- Học thêm **OOCSS (Object Oriented CSS)** và **BEM**, **SASS**
	- Link tham khảo style guide:
		- <a href="https://codeguide.co/" target="__blank" rel="noopener noreferrer">https://codeguide.co/</a>
		- <a href="https://airbnb.io/javascript/" target="__blank" rel="noopener noreferrer">https://airbnb.io/javascript/</a>
		- <a href="https://google.github.io/styleguide/htmlcssguide.html" target="__blank" rel="noopener noreferrer">https://google.github.io/styleguide/htmlcssguide.html</a>
	- Link tham khảo đầy đủ về bô quy tắc **BEM, OOCSS, SMACSS, SUITCSS, Atomic**: <a href="http://getbem.com/introduction/" target="__blank" rel="noopener noreferrer">http://getbem.com/introduction/</a>

- Ưu điểm / nhược điểm của việc sử dụng bộ tiền xử lý CSS là gì?
	- **Ưu điểm:**
		- css được tạo ra dễ bảo trì hơn.
		- Sử dụng css lồng nhau.
		- Mixin để tạo CSS lặp lại
		- Có thể quản lí nhiều têp khác nhau bới 1 têp.
	- **Nhược điểm:**
		- Yêu cầu các công cụ để xử lý trước. Thời gian biên dịch lại có thể chậm.

- Mô tả những gì bạn thích và không thích về bộ tiền xử lý CSS mà bạn đã sử dụng.
	- **Thích:**
		- Chủ yếu là những ưu điểm nêu trên.
	- **Không thích:**
		- Sử dụng Sass qua node-sass. Thường xuyên phải biên dịch lại nó khi chuyển đổi giữa các phiên bản node.

- Bạn sẽ triển khai một comp thiết kế web sử dụng phông chữ không chuẩn như thế nào?\
	Sử dụng **@font-face** và xác định **font-family** cho các **font-weights** khác nhau .

- Giải thích cách trình duyệt xác định phần tử nào phù hợp với bộ chọn CSS.
	- Bộ chọn CSS được đối sánh bởi các công cụ trình duyệt từ phải sang trái. Vì vậy, trước tiên họ tìm thành phần con và sau đó kiểm tra cha mẹ chúng để xem chúng có khớp với các phần còn lại của quy tắc hay không, Chiều dài của chuỗi bộ chọn càng ngắn, trình duyệt xác định phần tử đó khớp với bộ chọn càng nhanh.
	- Ví dụ: Bộ chọn `p span` trình duyệt sẽ tìm tất cả phần tử `<span>` và duyệt qua phần tử gốc của nó đến tận gốc để tìm cha mẹ của nó là `<p>`.

- Trình bày về các pseudo-elements và thảo luận xem chúng dùng để làm gì
	- ** pseudo-elements** - **phần tử giả** trong CSS cho phép bạn chèn nội dung vào một trang mà không cần phải có trong HTML. Mặc dù kết quả cuối cùng không thực sự nằm trong DOM, nhưng nó xuất hiện trên trang như thể nó có
	- Được sử dụng để tạo **clearfix**, hoặc để sử dụng trang trí mà ko cần thêm thẻ div.

- Giải thích những hiểu biết của bạn về box model và làm thế nào bạn báo với trình duyệt trong CSS để render layout của bạn trong các box models khác nhau.
	- **CSS box model** chịu trách nhiệm tính toán:
		- 1 **block-level element** chiếm bao nhiêu không gian.
		- `border` và/hoặc `margin` có chồng lên nhau hoặc thu gọn hay không.
		- Kích thước của một hộp.
	- **Box model** có các qui tắc sau:
		- Kích thước của 1 phần tử khối được tính toán bởi `width`, `height`, `padding`, `borders`, và `margins`.
		- Nếu `height` không được chỉ định, một **block element** sẽ có chiều cao bằng với nội dung mà nó chứa + với `padding`.
		- Nếu `width` không được chỉ định, một **block element** sẽ có chiều rộng bằng với phần tử cha chứa nó - `padding`.
		- `height` của 1 phần tử được tính theo chiều cao của nội dung.
		- `width` của 1 phần tử được tính theo chiều rộng của nội dung.
		- Mặc định `padding`, `border` không phải là 1 phần của `width` và `height` của 1 phần tử

- Đoạn code này * { box-sizing: border-box; } sẽ làm điều gì? Những ưu điểm của nó là gì?
	- Mặc dinh các phần tử được áp dụng `box-sizing: content-box` và chỉ kích thước nội dung đang được tính đến.
	-  `box-sizing: border-box` thay đổi tính toán `width`, `height` của 1 phần tử. Bao gồm `border` và `padding` được đưa vào để tính toán.
	- Chiều cao của 1 phần tử hiện tại = `height` + `padding` + vertical `border` width.
	- Chiều rộng của 1 phần tử hiện tại = `width` + `padding` + horizontal  `border` width.

- Liệt kê các giá trị của thuộc tính display mà bạn có thể nhớ.\
	none, block, inline, inline-block, flex

- Sự khác nhau giữa inline và inline-block là gì?
	- **Block**:
		- Kích thước: Làm đầy chiều rộng của phần tử cha mẹ chứa nó.
		- Vị trí: Bắt đầu 1 dòng mới và không có phần tử nào theo sau nó.
		- Có thể đặt `width`, `height`.
	- **Inline**:
		- Kích thước: phụ thuộc vào nội dung.
		- Vị trí: nằm cùng với nội dung khác và cho phép phần tử bên cạnh nó.
		- Không thể đặt `width`, `height`.
	- **Inline block**:
		- Kích thước: phụ thuộc vào nội dung.
		- Vị trí: nằm cùng với nội dung khác và cho phép phần tử bên cạnh nó.
		- Có thể đặt `width`, `height`.

- Sự khác nhau giữa một thành phần có thuộc tính position với giá trị: **relative**, **fixed**, **absolute**, **static**?\
	1 phần tử có thuộc tính **position** có 1 trong 2 giá trị **relative**, **absolute**, **fixed** hoặc **sticky**.
	- **static**: vị trí mặc định, thuộc tính **top**, **right**, **bottom**, **left** và **z-index** không được áp dụng
	- **relative**: vị trí của phần tử điều chỉnh so với chính nó, thuộc tính **top**, **right**, **bottom**, **left** và **z-index** được áp dụng.
	- **absolute**: phần tử bị xóa khỏi luồng trang và vị trí được định vị so với tổ tiên (**position: relative**) gần nhất, thuộc tính **top**, **right**, **bottom**, **left** và **z-index** được áp dụng.
	- **fixed**: phần tử bị xóa khỏi luồng trang và vị trí nằm ở vị trí mà ta chỉ định không di chuyển khi ta scroll chuột , thuộc tính **top**, **right**, **bottom**, **left** và **z-index** được áp dụng.
	- **sticky**: có đặc điểm lai giữa **relative** và **fixed**. Phần tử vẫn được coi là **relative** cho đến khi ta scroll chuột qua nó. Tại thời điểm này nó dc coi là **fixed**, thuộc tính **top**, **right**, **bottom**, **left** và **z-index** được áp dụng..

- Chữ cái  **C** trong **CSS** là viết tắt của từ **Cascading**. Mức độ ưu tiên được xác định trong việc gán style như thế nào (cho vài ví dụ)? Bạn có thể tận dụng hệ thống này như thế nào?
	- Các mức độ ưu tiên **specific** có quy tắc như sau:
		1. **Inline styles** - Viết **CSS** trực tiếp trong **HTML** qua thuộc tính `style`, luôn có mức độ ưu tiên cao nhất.
		2. **IDs** - được sử dụng để định danh duy nhất cho 1 phần tử element. Mức độ ưu tiên chỉ xếp sau **Inline style**.
		3. **Classes, attributes** và **pseudo-classes** - Selector này thường chỉ đến những định danh theo nhóm, custom mà người dùng đặt ra. Có mức độ ưu tiên cao hơn Selector theo tên element.
		4. **Elements and pseudo-elements** - đây là Selector có kiểu chung chung nhất nên mức độ ưu tiên thấp nhất.
	**Note**: 1 số trường hợp custom lại css bên thư viện thứ 3, css selector ở mức cao nhất ta phải sử dụng đến `!important` để ghi đè. Tôi thường viết **Css** ở mức thấp để có thể dễ dàng ghi đè
	- Ví dụ 1:
		```css
		#heading {
		  color: orange;
		  color: grey;
		}

		.heading {
		  color: red;
		}

		h1 {
		  color: blue;
		}
		```

		Ở ví dụ 1 sẽ apply `color: grey` vì **IDs selector** có mức độ ưu tiên cao nhất trong trường hợp này. Vậy tại sao ko phải `color: orange`? Câu trả lời là khi có cùng mức độ ưu tiên thì nó sẽ xét đến thứ tự xuất hiện, CSS nào được viết sau thì sẽ ghi đè CSS được viết trước.
	- Ví dụ 2:
		```css
		//custom.css
		h1 {
		  color: red;
		}

		//custom2.css
		h1 {
		  color: blue;
		}
		```
		Ở ví dụ 2 sẽ apply `color: blue` vì **h1** trong **custom.css** và **h1** trong **custom2.css** có cùng 1 mức độ nhưng `color: blue` khai báo sau `color: red` nên n1 sẽ ghi đè.

- Những framework CSS nào bạn đã sử dụng trên máy của bạn, hoặc trong sản phẩm nào đó? Bạn sẽ thay đổi/cải tiến chúng như thế nào?\
	**Bootstrap** - Chu kỳ phát hành chậm. Bootstrap 4 đã ở giai đoạn alpha được gần 2 năm. Tôi sẽ lược bớt và chỉ tải những phần cần xài như **Grid** của bootstrap để ko giảm tải trang.

- Bạn có bao giờ sử dụng CSS Flexbox hay Grid chưa?
	- Tôi đã sử dụng qua cả 2
	- **Flexbox** giải quyết nhiều vấn đề phổ biến trong CSS, như căn giữa theo chiều dọc, ngang của các phần tử trong vùng chứa. Có thể dùng để dàn layout 1 chiều và 1 số bố cục nhỏ trên trang.
	- **Grid** dùng để dàn layout 2 chiều, nên tạo 1 layout dựa trên grid. Nhưng hiện tại hỗ trợ trình duyệt chưa rộng rãi.

- Responsive design khác adaptive design như thế nào?
	- **Responsive design** và **Adaptive design** đều cố gắng tối ưu hóa trải nghiệm của người dùng trên các thiết bị khác nhau.
	- **Responsive design**: hoạt động trên nguyên tắc linh hoạt, 1 trang web linh hoạt trông đẹp trên mọi thiết bị. Sử dụng các **media queries**, **flexbox**, **grid**, **reponsive images** để tạo ra trải ngiệm người dùng linh hoạt và thay đổi dựa trên kích thước khung nhìn.
	- **Adaptive design**: Thay vì một thiết kế linh hoạt, **adaptive design** sẽ phát hiện thiết bị và các tính năng khác, sau đó cung cấp tính năng và bố cục thích hợp dựa trên tập hợp các kích thước khung nhìn và các đặc điểm khác được xác định trước.

- Bạn có bao giờ làm việc với các thiết bị màn hình retina chưa? Nếu có, bạn đã sử dụng khi nào và dùng những kĩ thuật nào?
	- Sử dụng media query như `@media only screen and (min-device-pixel-ratio: 2) { ... }` để **resize image** trên các màn hình có độ phân giải cao.
	- Sử dụng hình ảnh **SVG** để làm logo hoặc 1 số hình ảnh ta muốn nó rõ nét để tránh bị vỡ hình khi độ phân giải cao.
	- Sử dụng JavaScript để thay thế thuộc tính **src** của `<img />` bằng các phiên bản có độ phân giải cao hơn sau khi kiểm tra `window.devicePixelRatio`.

- Có những lý do nào bạn muốn sử dụng translate() hơn thay vì absolute positioning không, hoặc ngược lại? Và tại sao?
	- **translate** là 1 giá trị của CSS `transform`. Thay đổi `transform` hoặc `opacity` sẽ không kích hoạt trình duyệt **reflow** hoặc **repaint**, mặt khác `translate()` sử dụng 1 lớp riêng của nó trên **GPU** (gọi là RenderLayer). Giờ đây bất kì thay đổi nào về việc chuyển đổi 2D, 3D, `opacity` đều xảy ra trên **GPU**.
	- **absolute positioning** ngược lại sẽ kich hoat trình duyệt **reflow** hoặc **repaint**, ngoài ra nó được tính toán trên **CPU** nên về mặt hiệu suất sẽ kém hơn **translate**.
	- **Note**: Khi sử dụng `translate()` phần tử vẫn chiếm không gian ban đầu của nó (giống `position: relative`), không giống như `position: absolute` không gian nó chiếm sẽ  thay đổi theo vị trí mà nó được đặt.

<a id="js_questions"></a>
### 4. Các câu hỏi về JS:

- Giải thích về event delegation
	- **Event delegation** cho phép bạn tránh thêm trình nghe sự kiện vào từng phần tử cụ thể của 1 danh sách các phần tử, thay vào đó trình nghe sự kiện được thêm vào phần tử cha mẹ chứa danh sách các phần tử đó, ví dụ ta có 1 `<ul>` chứa 1 danh sách các `</li>` thay vì đặt sự kiện **click** ở từng `<li>` ta có thể đặt tại phần tử `<ul>`.
	- Giả sử ta có 1 danh sách `<li>` khi click vào thẻ `<li>` sẽ in ra màn hình **console** id của thẻ đó:
		```html
		<ul id="parent-list">
		  <li id="post-1">Item 1</li>
		  <li id="post-2">Item 2</li>
		  <li id="post-3">Item 3</li>
		</ul>
		```
	- Nếu ta thêm trình lắng nghe sự kiện **onclick** vào thẻ `<li>` để log ra id thì nó vẫn hoạt động tốt. Nhưng trường hợp thẻ `<li>` thường được thêm hoặc xóa khỏi danh sách thì sao? ta lại phải thêm hoặc xóa trình lắng nghe lại sự kiện, giải pháp tốt nhất là sủ dụng **Event delegation** cho thẻ `<ul>`.
		```javascript
		document.getElementById("parent-list").addEventListener("click", (e) => {
		  if(e.target && e.target.nodeName == "LI") {
		    console.log(e.target.id);
		  }
		});
		```
	- Đoạn mã trên kiểm tra thuộc tính đích của đối tượng sự kiện để có được tham chiếu đến nút được nhấp thực sự.

- Giải thích cách mà **this** hoạt động trong JavaScript?\
	Trong **non–strict mode**, luôn luôn tham chiếu tới một đối tượng và trong **strict mode** có thể là bất kỳ giá trị nào.

	- ### This trả về window (global object)
		Tất cả code của ta viết đều được bao bởi window ( global object ), nên khi ta gọi **this** mà ko được bao bởi một object khác thì **this** sẽ tham chiếu đến đối tượng window.
		```javascript
		console.log(this); // trả về window

		function func() {
		  console.log(this);
		}

		func(); // trả về window
		```
	- ### This trả về một object khác window
		```javascript
		// 'this' nằm bên trong một object
		const obj = {
		  method: function() {
		    return this;
		  }
		};

		console.log(obj.method()); // trả về obj
		```

		```javascript
		/*
		  'this' nằm bên trong object được
		  khởi tạo từ Constructor Function
		*/
		function ConstFunc() {
		  return this;
		}

		console.log(new ConstFunc()); // trả về obj vì từ khóa 'new' đã tạo ra 1 object
		```

	- ### This trong các Event
		Tất cả mọi thứ trong **Javascript** đều là **object** và **DOM element** cũng không phải ngoại lệ. Chính vì vậy, **This** trong các event của một **DOM element** sẽ trả về chính element chứa nó.
		```javascript
		document.getElementById("btn").onclick = function() {
		  console.log(this); // trả về Html Element Object
		};
		```

	- ### Strict mode
		Nếu ta dùng **strict mode** thì **this** trong các function sẽ trả về **undefined** (trừ method).
		```javascript
		"use strict"

		function func() {
		  return this;
		}

		console.log(func()); // this sẽ trả về undefine
		```

		```javascript
		"use strict"

		const obj = {
		  method: function() {
		    return this;
		  }
		};

		console.log(obj.method()); // this của method vẫn sẽ trả về obj
		```

	- ### Arrow function
		Giá trị của **this** trong một **arrow function** sẽ được kế thừa từ **this** của **function/method**, nơi mà **arrow function** đó được khai báo chứ không còn trả trả về object gần nhất chứa nó nữa.
		```javascript
		const obj = {
		  methoddd: function() {
		    console.log(this); // trả về obj

		    const arrowFunc = () => {
		      console.log(this);
		    };

		    /*
		      arrowFunc đc khai báo trong phương thức
		      methoddd nên 'this' của arrowFunc sẽ kế thừa
		      'this' của methoddd và trả về obj
		    */
		    arrowFunc();
		  }
		};

		console.log(obj.methoddd());
		```

		```javascript
		const arrowFunc = () => {
		  console.log(this); // this của arrowFunc lần này sẽ là window
		};

		const obj = {
		  methoddd: function() {
		    console.log(this); // trả về obj

		    /*
		      'this' sẽ trả về object window do arrowFunc không
		      còn được khai báo bên trong methoddd nữa
		    */
		    arrowFunc();
		  }
		};

		console.log(obj.methoddd());
		```

		**Chú ý**: **this** của arrow function sẽ luôn trả về window nếu nó không được khai báo bên trong bất kỳ một **function** nào, nên **arrow function** không được sử dụng để làm **constructor function** hay **method** của một **object** được khởi tạo bằng **object initializer**.
		```javascript
		// dùng arrow function làm constructor function
		const Human = (name, age) => {
		  this.name = name;
		  this.age = age;
		};

		const man = new Human('Hunq', 20); // Uncaught TypeError: Human is not a constructor

		```

		```javascript
		/*
		  dùng arrow function làm method của một object
		  được khởi tạo bằng Object Inintializer
		*/
		const obj = {
		  method: () => this
		};

		/*
		  this của method sẽ trả về window
		  vì trên thực tế, method đang được khai báo
		  độc lập (không nằm trong function nào)
		*/
		console.log(obj.method());
		```

- Giải thích cách mà prototypal inheritance hoạt động
	- Mọi thứ trong javascript đều là một **Object**, ngay cả khi tạo **Class** thông qua **Constructor Function**. Đây là cách javascript lập trình trên **Class** giống như các ngôn ngữ lập trình hướng đội tượng truyền thống khác, nơi chúng sử dụng từ khóa **Class** và **Inheritance**.
	- Phiên bản lập trình dựa trên **Class** trong javascript so với các ngôn ngữ lập trình truyền thống khác hoạt động cùng 1 khái niệm nhưng không hoạt động hoàn toàn giống nhau. Khác biệt về từ khóa, cú pháp và cách hoạt động.
	- Vì vậy, ý tưởng cốt lõi của **Prototypal Inheritance** (kế thừa nguyên mẫu) là 1 object A có thể trỏ đến 1 object B và kế thừa tất cả thuộc tính của object B. Mục đính chính là cho phép nhiều **instance** (thể hiện) của 1 obejct chia sẻ những thuộc tính chung.
	- <a href="https://codepen.io/hoanggaphan/pen/dyXWgBo" target="__blank" rel="noopener noreferrer">Ví dụ về prototypal inheritance</a>.

- Sự khác nhau giữa biến: **null**, **undefined** hoặc **undeclared**?
Bạn sẽ kiểm tra các trạng thái này như thế nào?
	- **Undefined** xảy ra khi khai báo 1 biến nhưng ko gán giá trị cho nó, giá trị của biến đó sẽ là **undefined**.
	- **Null** nghĩa là giá trị rỗng hoặc không tồn tại, nó được sử dụng để gán cho 1 biến như là 1 đại diện không có giá trị.
	- **Undeclared** là 1 giá trị được khai báo ko có từ khóa `var`, ví dụ: `testVar = ‘hello world’;`. Khi code được thực thi các biến này sẽ được tạo dưới dạng **global variables**
	- Để check **Null** hoặc **Undefined** sử dụng toán tử `=== null // or === undefined`. Đối với **Undeclared** khi sử dụng `var` nên khai báo `'use strict'` ở đầu chương trình hoặc sử dụng `let, cost` thay cho `var`.

- Một closure là gì, và bạn sẽ sử dụng nó như thế nào / tại sao bạn sử dụng nó?
	- **Closure là gì:** Closure là 1 **inner function** nằm trong 1 **outer function**. Chúng sở hữu 1 local scope riêng và có thể truy cập vào scope, parameters của outer function và cũng có quyền truy cập vào global variables.
	- **Tại sao sử dụng:** closure là 1 cách ngắn gọn để giải quyết các vấn đề về scope (phạm vi). Lí do sử dụng closure là vì javascript là 1 **function-level scope** khác với các ngộn ngữ khác **block-level scope**, và javascript là 1 ngôn ngữ hướng sự kiện/bất đồng bộ.
	- **Hoạt động:**
		- Sau khi **outer function** được thực thi và trả về giá trị, **closure** có thể vẫn còn chạy.
		- **Closure** lưu trữ tham chiếu đến các variables của **outer function**. Vì vậy chúng ta luôn có thể truy cập để cập nhật các variables này.
	- <a href="https://codepen.io/hoanggaphan/pen/XWKPdEZ" target="__blank" rel="noopener noreferrer">Ví dụ sử dụng **closure** với **IIFE**.</a>

- Bạn sử dụng cấu trúc ngôn ngữ nào để lặp qua các thuộc tính của object và các array items?\
	**for...in** lặp qua thuộc tính object. **for**, **forEarch**, **map** lặp qua phần tử của array.

- Bạn có thể mô tả sự khác biệt chính giữa Array.forEach() và Array.map() và tại sao bạn chọn cái này so với cái kia không?
	- **forEach()** lặp qua các phần tử trong mảng nhưng sẽ trả về `undefined`.
	- **map()** lặp qua các phần tử trong mảng và sẽ tạo ra 1 mảng mới với mỗi phần tử là kết quả của 1 callback function.
	- Khi ta muốn duyệt mảng và tạo ra 1 mảng mới thì sử dụng **map**, còn chỉ duyệt qua mảng thì sử dụng **forEarch**.

- Trường hợp sử dụng điển hình của anonymous functions là gì?\
	**Anonymous function** là 1 biểu thức hàm (hàm không tên) chứ không phải là 1 khai báo hàm. Biểu thức hàm rất linh hoạt, ta có thể gán các biểu thức hàm cho biến, thuộc tính của object, làm đối số của các hàm khác. Ngoài ra có thể sử dụng như **IIFE**(Immediate Invoked Function Expression)
	Ví dụ:
	```javascript
	// Gán cho biến
	var  anon  =  () => console.log("anonymous function")
	anon ();

	// Làm thuộc tính của object
	var obj = {
	  name: "anonymous",
	  anon: () => console.log("anonymous function")
	}

	// Làm đối số của hàm khác
	var newArr = arr.map(x => x * x);

	// IIFE
	(() => { })();
	```

- Sự khác biệt giữa host objects và native objects là gì?
	- **Host object** là những object được cung cấp bởi 1 môi trường nhất định. Ví dụ, môi trường browser cung cấp object **window**, Nodejs cung cấp object **NodeList**.
	- **Native object** là các đối tượng tiêu chuẩn được cung cấp bởi **Javascript**. **Native object** đôi khi được gọi là **Global object** vì chúng là object được **Javascript** cung cấp sẵn để sử dụng.
	- **Javascript** chủ yếu được xây dựng bởi các **Native object**. Những object này có thể sử dụng dưới dạng **Constructor** (String(), Number(), Boolean())

- Giải thích sự khác nhau giữa: `function Person(){}`, `var person = Person()`, và `var person = new Person()` ?
	- `function Person(){}` - **Function Declaration**: khai báo hàm **Person**, nhưng không thực thi.
	- `var person = Person()` - **Function Expression**: biểu thức hàm **Person** được gán vào biến **person**.
	- `var person = new Person()` - **Function Constructor**: bằng cách thêm từ khóa **new**, ta đang tạo 1 object mới từ **Class Person**.

- Giải thích sự khác biệt về cách sử dụng **foo** giữa `function foo() {}` và `var foo = function() {}` ?
	- `function foo() {}`: đây là khai báo 1 hàm (**Function Declaration**).
	- `var foo = function() {}`: đây là 1 biến được gán 1 hàm ẩn danh (**Function Expression**).
	- **Function Declaration** tải trước khi thực thi bất kì mã nào trong Javascript, **Function Expression** chỉ tải khi trình biên dịch đi đến dòng mã đó.

- Bạn có thể giải thích **Function.call** và **Function.apply** làm gì? Sự khác biệt đáng chú ý giữa hai là gì?
	- Việc gọi 1 **function** với **.call()** hoặc **.apply** sẽ trỏ 1 **object** đến **function** được gọi với **object** là tham số đầu tiên. Sự khác biệt ở đây là **.call()** cho phép truyền vào nhiều tham số, còn **.apply** chỉ chấp nhận tham số thứ 2 là 1 mảng, <a href="https://codepen.io/hoanggaphan/pen/GRqYOyZ" target="__blank" rel="noopener noreferrer">ví dụ</a>.

- Giải thích Function.prototype.bind ?
	- **.bind()** là phương thức tạo ra 1 hàm mới, với đối số đầu tiên là 1 **object**, và các tham số theo sau có thể có hoặc không.
	- **.bind()** hoạt động giống như **.call()**, điểm khác biệt là **.bind()**  không gọi hàm trực tiếp mà nó sẽ trả về một hàm mới để gọi sau, còn **.call()** sẽ gọi hàm trực tiếp, <a href="https://codepen.io/hoanggaphan/pen/JjKejGa" target="__blank" rel="noopener noreferrer">ví dụ</a>.

- Sự khác biệt giữa **feature detection**, **feature inference** và **UA string** là gì?
	-  **feature detection**: kiểm tra xem 1 tính năng có hỗ trợ trên trình duyệt bằng cách chạy các đoạn code khác nhau tùy thuộc vào việc **có** hay **không**. Điều này cho phép trình duyệt luôn cung cấp trải nghiệm hoạt động tốt thay vì gặp sự cố. Ví dụ:
		```javascript
		if ("geolocation" in navigator) {
		  navigator.geolocation.getCurrentPosition(function(position) {
		    // hiển thị vị trí trên bản đồ, bằng cách sử dụng API Google Maps
		  });
		} else {
		  // Cung cấp cho người dùng lựa chọn bản đồ tĩnh
		}
		```
	- **feature inference**: tương tự như **feature detection**, nhưng thay vì cung cấp code dự phòng khi tính năng không dc hỗ trợ, thì nó sẽ giả định rằng tính năng đó cũng sẽ tồn tại.
		```javascript
		if (document.getElementsByTagName) {
		  element = document.getElementById(id);
		}
		// Điều này ko được khuyến khích. Thay vào đó nên sử dụng feature detection
		```
	- **UA string** hay **User Agent String**:
		- Là một chuỗi văn bản dữ liệu có thể truy cập thông qua Navigator.userAgent. **Chuỗi văn bản dữ liệu** này chứa thông tin về môi trường trình duyệt mà bạn đang sử dụng.
		- Mở console và chạy đoạn mã này: `navigator.userAgent`
		- Bạn sẽ thấy một **chuỗi văn bản dữ liệu** chứa đầy đủ thông tin về môi trường bạn hiện đang sử dụng. Tuy nhiên, chuỗi này khó phân tích cú pháp và có thể bị giả mạo.

- Giải thích **hoisting**
	- **Hoisting** là hành động mặc định của Javascript, nó sẽ chuyển phần khai báo lên phía trên top trong Javascript.
	Xem ví dụ sau:
	```javascript
	console.log(a);
	var a = 'Hello Hoisting'
	```
	- Kết quả sẻ trả về **undefined** vì trên thực tế sau khi chạy code sẽ như sau:
	```javascript
	var a;
	console.log(a);
	a = 'Hello Hoisting';
	```
	- Trình biên dịch của Javascript sẽ phân tách phần `var a = 'Hello Hoisting'` thành 2 phần là **khai báo** và **gán giá trị**:
		- Khai báo `var a;`
		- Gán giá trị `a = 'Hello Hoisting';`
		- Theo hoisting javascript chuyển phần khái báo lên top, còn phần giá trị giữ nguyên vị trí.

	- Đối với khai báo **function** cũng sẽ hoạt động như trên:
	```javascript
	say_something('YOLO');
	function say_something(a){
	  console.log(a);
	}
	```
	Và kết quả trả về `'YOLO'`.

- Miêu tả event bubbling.\
	**Event bubbling** sự kiện sẽ bắt đầu từ phần tử đích đi đến phần tử ngoài cùng.
	####HTML
	```html
	<div class="ancestor">
	  <div class="parent">
	    <button> Click me! </button>
	  </div>
	</div>
	```
	####Javascript
	```javascript
	$( "button" ).click(function(event) {
	  console.log( "button was clicked!" );
	});
	
	$( ".parent" ).click(function(event) {
	  console.log( "child element was clicked!" );
	});
	
	$( ".ancestor" ).click(function(event) {
	  console.log( "descendant element was clicked!" );
	});
	```
	Khi người dùng nhấp vào nút, sự kiện bắt đầu từ **button** và đi ra ngoài cùng, do đó kết quả là **"button was clicked!"**. Sau đó **"child element was clicked!"** và cuối cùng **"descendant element was clicked!"**.
	####Dừng event bubbling
	Nếu bạn chỉ muốn sự kiện kích hoạt trên chính phần tử đó mà ko ảnh hưởng đến các phần tử bên ngoài, tham khảo đoạn mã sau:
	```javascript
	$( "button" ).click(function(event) {
	  event.stopPropagation(); // <-- Dòng này
	  console.log( "button was clicked!" );
	});

	$( ".parent, .ancestor" ).click(function(event) {
	  console.log( "don't click me!" );
	});
	```
	Bây giờ sự lan truyền đã dừng lại tại nút **button** và chỉ hiển thị kết quả **"button was clicked!"**.

- Miêu tả event capturing.\
	**Event capturing** ngược lại với **Event bubbling** sự kiện sẽ bắt đầu từ phần tử ngoài cùng đi đến phần tử đích.
	####HTML
	```html
	<div id="parent">
	  <button id="child">Child</button>
	</div>
	```
	#### Javascript
	```javascript
	var parent = document.querySelector('#parent');
	var child = document.querySelector('#child');

	parent.addEventListener('click', function(){
	  console.log("Parent clicked");
	}, true);

	child.addEventListener('click', function(){
	  console.log("Child clicked");
	});
	```
	Chúng ta phải dăt tham số thứ 3 của **addEventListener** thành true để kích hoạt **event capturing** của phần tử cha.
	Khi người dùng click vào **button**, sự kiện của **parent div** sẽ chạy trước do ta đã kích hoạt **event capturing**. Kết quả sẽ là **"Parent clicked"** sau đó sẽ là **"Child clicked"**.

- Sự khác biệt giữa **attribute** và **property**?\
	**Attribute** và **Property** đều là các thuật ngữ mang nghĩa là **thuộc tính**. Sự khác nhau ở đây là:
	- **Attribute** dùng cho **HTML**, ví dụ: thẻ `<img src="img.jpg" alt="image">` sẽ có 2 **attribute** là 'src' và 'alt'.
	- **Properties** dùng cho **object**, ví dụ: `var img = { src: "img.jpg", alt="image" }` có 2 **property** là 'src' và 'alt'.

- Nêu ưu điểm và nhược điểm của việc kế thừa (extend) các built-in JavaScript objects?
	- **Ưu điểm**: giúp viết mã nhanh hơn, tiết kiệm thời gian.
	- **Nhược điểm**: trong tương lai ta không biết là trình duyệt có triển khai thêm **method** khác trong các **built-in objects** này ko. Điều này dẫn đến việc bạn sẽ có thể **overide** 1 số **method** nếu ko cẩn thận. Vì vậy tốt nhất ko nên **extend** các **built-in JavaScript objects** ngay từ đầu.

- Sự khác nhau giữa '==' và '===' ?
	- '==' bỏ qua so sánh kiểu dữ liệu, chỉ so sánh giá trị.
	- '===' vừa so sánh kiểu dữ liệu và giá trị.

- Giải thích same-origin policy trong JavaScript
	- **Same-origin policy** hạn chế các **tệp (script**) từ trang web thực hiện trên trang của bạn. 1 cuộc tấn công như thế này được gọi là **Cross Site Scripting (XSS)**.
	 - 2 URL có cùng **origin** nếu như: **protocol** (http or https), **host** và **port** giống nhau.
	- Ví dụ từ <a href="https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy" target="__blank" rel="noopener noreferrer">MDN - Same-origin policy:</a>
		So sánh `http://store.company.com/dir/page.html` với bảng dưới.
		<a href="https://i.ibb.co/rGtP3T2/same-origin.png" target="__blank" rel="noopener noreferrer"><img src="https://i.ibb.co/rGtP3T2/same-origin.png" height="155" alt="same-origin"></a>

- Tại sao gọi là **Ternary Operator**, từ **"Ternary"** biểu thị điều gì?
	- Trước hết hãy trả lời cho **Ternary** biểu thị điều gì?
	- Theo <a href="https://en.wikipedia.org/wiki/Ternary_operation" target="__blank" rel="noopener noreferrer">Wikipedia</a> từ **Ternary** bắt nguồn từ cách thiết lập từ **n-ary**. Ví dụ: un**ary**, bin**ary**, tern**ary** lần lượt là toán tử bậc 1, 2 và 3. Tất cả những điều này gọi chung là toán hạng (toán hạng **ternary**), phần tiền tố trong tên của chúng liệt kê số lượng đầu vào mà toán hạng chấp nhận.
	- Toán hạng un**ary** nhận 1 đối số, ví dụ: `-1`, với `-` là toán hạng, và **1** là đối số.
	- Toán hạng bin**ary** nhận 2 đối số, ví dụ: `2 - 1`, với `-` là toán hạng và **2**, **1** là các đối số.
	- Vậy toán hạng tern**ary** nhận 3 đối số.
	- Trong lập trình toán hạng **ternary** thường được sử dụng với `?:` để viết tắt 1 dòng cho lệnh **if else**.
	- Ví dụ:
	```javascript
	if (fooTrue) {
	  foo();
	} else {
	  bar();
	}
	```
	Viết lại với `?:`
	```javascript
	fooTrue ? foo() : bar();
	```
	- Trong Javascript, toán hạng này còn được xem là 1 biểu thức, không chỉ điều kiện.
	```javascript
	// gán vào 1 biến
	var isFoo = fooTrue ? "yes" : "no";

	// truyền dưới dạng tham số
	getFoo(fooTrue ? "yes" : "no");
	```

- Chế độ **sctrict mode** là gì? ưu điểm / nhược điểm của việc sử dụng?
	- Nếu đặt `"use strict"` ở đầu **code** hoặc **function**, thì JS sẽ chạy ở chế độ **strict mode**. Trong chế độ này nó sẽ kiểm tra code nghiêm ngặt hơn và ném ra nhiều lỗi, vô hiệu hóa 1 số tính năng ko cần thiết làm cho code bạn viết chính xác, dễ đọc và mạnh mẽ hơn.
	- #### Ưu điểm:
		- Nó bắt 1 số lỗi ngớ ngẩn phổ biến, và nám ra các lỗi.
		- Ngăn chặn hoặc ném lỗi khi thực hiện các hành động không an toàn (chẳng hạn như dành quyền truy cập vào global object).
		- Vô hiệu hóa các tính năng khó hiểu hoặc không được nghĩ ra.
	- #### Nhược điểm:
	Nhược điểm có lẻ là mã sẽ gây khó hiểu và xung đột nếu trộn lẫn 2 chế độ **normal mode** và **sctrict mode**.

- Ưu điểm / Nhược điểm cửa việc viết mã Javascript bằng ngôn ngữ biên dịch sang Javascript là gi?
	- #### Ưu điểm:
		- Cung cấp nhiều **types** để việc kiểm soát mã tốt hơn.
		- Cú pháp **OOP** rõ ràng và dễ hiểu.
		- Các tính năng mới của **ECMAScript** sẵn sàn để sử dụng
	- #### Nhược điểm:
		- Khó để tìm được lập trình viên quen thuộc cho ngỗn ngữ đó.
		- Sẽ có ít sự lựa chọn về tools, library hoặc frameworks được sử dụng cho ngôn ngữ đó.

- Bạn sử dụng công cụ và kỹ thuật nào để gỡ lỗi mã JavaScript?\
	Chrome Dev Tools — Debugger

- Giải thích sự khác nhau giữa các **mutable** và **immutable** object ?\
	Ví dụ về **immutable** object trong javascript ?
	Ưu / Nhược điểm của **immutability** ?
	Làm thế nào để đạt được **immutable** trong mã của bạn ?
	- #### Mutable vs Immutable
		- **Mutable** object là đối tượng có thể thay đổi sau khi được tạo.
		- **Immutable** object là đối tượng **không** thể thay đổi được sau khi được tạo. Quy tắc này khá đơn giản, nếu bạn muốn sửa đổi một số thuộc tính của một đối tượng, bạn phải thực hiện nó trên một bản sao.
		- **Chú ý:** Trong Javascript chỉ có **object** và **array** là **mutable**, còn lại đều là **immutable**.

	- #### Ví dụ về Immutable object
		```javascript
		let pikachu = {
		  type: "electric"
		};

		Object.freeze(pikachu);

		pikachu.color = "yellow";

		console.log(pikachu) // { type: "electric" }
		```
		**Object.freeze()** là method đóng băng 1 **object** làm cho nó ko thể thay đổi được, thêm hay xóa thuộc tính.
	- #### Ưu điểm của **immutability**
	Ít phức tạp và code ít lỗi hơn hơn vì ko phải lo lắng về vấn đề tham chiếu.
	- #### Nhược điểm của **immutability**
	Do ko thể thay đổi được đối tượng đã tạo ra, nên dần theo thời gian khi ta muốn thay đổi phải tạo ra nhiều đối tượng mới thay vì thay đổi trực tiếp trên đối tượng cũ. Điều này sẽ tác động đến hiệu suất.
	- #### Làm thế nào để đạt được immutable trong mã của bạn ?
	Như đã nói ở trên **object** và **array** là **mutable**, để đạt được tính **immutable** thì ta phải tạo ra bản sao hoặc sử dụng **Object.freeze** như ví dụ trên, dưới đây là các cách tạo ra bản sao:
    - **Object.assign()**
  	```javascript
  	const pikachu = {
  	  type: "electric"
  	};

  	const pikachuClone = Object.assign({}, pikachu); // Tạo bản sao mới
  	```

    - **Spread operator**
    ```javascript
    const pikachu = {
      type: "electric"
    };

    const pikachuClone = {
      ...pikachu,
      color: "yellow"
    }; // Tạo bản sao mới và thêm thuộc tính color
    ```

    - **Array.concat()**
    ```javascript
    const pokemons = ["pikachu", "mewtwo"];

    const pokemonsClone = [].concat(pokemons, "deoxys"); // Tạo bản sao mới và thêm "deoxys"
    ```

    - **Spread operator** cũng hoạt động với array
    ```javascript
    const pokemons = ["pikachu", "mewtwo"];

    const pokemonsClone = [
      ...pokemons,
      "deoxys"
    ]; // Tạo bản sao mới và thêm "deoxys"
    ```

- Giải thích sự khác nhau giữa chức năng **synchronous** và **asynchronous**.
	- **Synchronous** là xử lí **đồng bộ**, chương trình sẽ chạy theo từng bước, khi chương trình chạy xong bước này thì mới đến bước kế tiếp.
		- **Good:**
		Chương trình chạy theo từng bước.
		Ít gặp lỗi.
		Dễ debug.
		- **Bad**
		Performance kém.
		Dễ gây tràn bộ nhớ/Crash ứng dụng vì những chỗ phải "chờ" vô duyên
	- **Asynchronous** là xử lí **bất đồng bộ**, có nghĩa là xử lí nhiều việc 1 lúc mà ko chờ bất kì tác vụ nào khác.
		- **Good:**
		Performance tốt.
		Xử lí được nhiều việc 1 lúc.
		- **Bad**
		Xử lí không tốt có thể gây ra bugs.
		Khó debug.

- **Event loop** là gì ?\
	Sự khác nhau giữa **Call Stack** và **Task Queue** ?
	- **Event loop** là 1 vòng lặp vô tận trong Javascript Runtime (V8 trong Google Chrome) dùng để lắng nghe các event. Nhiệm vụ của nó là quan sát **Call Stack** và **Task Queue**. Khi quan sát thấy **Call Stack** rỗng nó sẽ đẩy tác vụ từ **Task Queue** vào để xử lí.
	- **Call Stack** là nơi các lệnh gọi hàm được thực hiện. Khi 1 hàm được gọi nó sẽ được đẩy vào 1 hàng đợi có tên là **stack**. **Stack** là hàng đợi kiểu LIFO (Last In First Out) nghĩa là vào đầu tiên thì ra sau cùng. Một hàm chỉ được lấy ra khỏi **stack** khi nó hoàn thành và return.
	- **Task Queue** là nơi mã không đồng bộ được đẩy dến và chờ thực thi, là hàng đợi kiểu FIFO (First In First Out) nghĩa là vào đầu tiên thì ra đầu tiên.

- Sự khác biệt giữa các biến tạo bởi **var, let** hoặc **const** ?
	- ### Scope
		- #### var
		Nếu 1 biến được tạo bên trong 1 function, phạm vi của nó sẽ nằm trong 1 function.
		Nếu 1 biến được tạo ko nằm trong bất kì function nào, phạm vi của nó sẽ là global.
		- #### let, const
		Nằm trong **block scope**.
		Chỉ có thể truy cập trong cập ngoặc nhọn gần nhất (if-else, for, ...).
	- ### Hoisting
		- Di chuyển tất cả khai báo lên đầu của scope hiện tại (script / function hiện tại), vì vậy 1 biến có thể được sử dụng trước cả khi nó được khai báo.
		- **var** cho phép hoisted.
		- **let, const** ko cho phép hoisted.
	- ### Redeclaring
		- Khai báo lại 1 biến với **var** sẽ ko bị lỗi.
		- Khai báo lại 1 biến với **let** hoặc **const** sẽ ném ra 1 lỗi.
	- ### Reasigning
		- **let**, **var** cho phép gán lại giá trị.
		- **const** ko cho phép gán lại giá trị.

- Sự khác nhau giữa **Class ES6** và **Function Constructor ES5** ?\
	Sự khác biệt là cú pháp khác nhau, tham khảo 2 đoạn code dưới đây:
	- #### Function Constructor ES5
		```javascript
		'use strict';
		/**
		* City class.
		*
		* @constructor
		* @param {String} name - tên.
		* @param {Number} x - Toạ độ x.
		* @param {Number} y  - Toạ độ y.
		*/
		function City(name, x, y) {
		  this.name = name;
		  this.setLocation(x, y);
		}

		/**
		* Thiết lập toạ độ cho City
		*
		* @param {Number} - Toạ độ x.
		* @param {Number} - Toạ độ y.
		*/
		City.prototype.setLocation = function(x, y) {
		  this.x = x;
		  this.y = y;
		};

		/**
		* Get toạ độ của City
		*
		* @return {Object}
		*/
		City.prototype.getLocation = function() {
		  return {
	      x: this.x,
		    y: this.y
		  };
		};

		/**
		* Lấy tên của city
		*
		* @return {String}
		*/
		City.prototype.getName = function() {
		  return 'City("' + this.name + '")';
		};

		const _city = new City('Hồ Chí Minh', 100, 200);

		console.log(_city.getName(), _city.getLocation());
		```

	- #### Class ES6
		```javascript
		'use strict';

		/**
		* City class.
		*
		* @constructor
		* @param {String} name - tên.
		* @param {Number} x - Toạ độ x.
		* @param {Number} y  - Toạ độ y.
		*/
		class City {
		  constructor(name, x, y) {
		    this.name = name;
		    this.setLocation(x, y);
		  }

		  /**
		  * Thiết lập toạ độ cho City
		  *
		  * @param {Number} - Toạ độ x.
		  * @param {Number} - Toạ độ y.
		  */
		  setLocation(x, y) {
		    this.x = x;
		    this.y = y;
		  }

		  /**
		  * Get toạ độ của City
		  *
		  * @return {Object}
		  */
		  getLocation() {
		    return {
		      x: this.x,
		      y: this.y
		    };
		  }

		  /**
		  * Lấy tên của city 
		  *
		  * @return {String}
		  */
		  getName() {
		    return `City is name ("${this.name}")`;
		  }
		}

		const _city = new City('Hồ Chí Minh', 100, 200);

		console.log(_city.getName(), _city.getLocation());
		```

- Bạn có thể nêu trường hợp sử dụng **Arrow function** ? Cú pháp này khác với **function** bình thường như thế nào.\
**Arrow Function** thường dùng để viết gọn cho **Anonymous Function**.
	```javascript
	array.map((item) => item.key);

	aAsync().then(() => bAsync()).then(() => cAsync()).done(() => finish);
	```
- Lợi ích của việc sử dụng **Arrow Function** cho method trong **contructor**.\
	Tham Khảo code dưới đây:
	```javascript
	class Hero {
	  constructor(heroName) {
	    this.heroName = heroName;
	  }

	  logName() {
	    console.log(this.heroName);
	  }
	}

	const batman = new Hero('Batman');

	batman.logName() // trả về 'Batman'

	setTimeout(batman.logName, 1000); // sau 1s trả về undefined

	setTimeout(batman.logName.bind(batman), 1000); // trả về 'Batman'
	```
	Đoạn mã **setTimeout()** trả về **undefined** là do <a href="https://dmitripavlutin.com/gentle-explanation-of-this-in-javascript/#32-pitfall-separating-method-from-its-object" target="__blank" rel="noopener noreferrer">phương thức bị tách ra khỏi đối tượng</a>. nên **this** lúc này sẽ là window hoặc undefine trong **strict mode**, để khắc phục ta đã dùng **.bind()**. Ngoài ra ta có thể sử dụng **Arrow function** để khắc phục, mã trên sửa lại như sau.
	```javascript
	class Hero {
	  constructor(heroName) {
	    this.heroName = heroName;
	  }

	  logName = () => {
	    console.log(this.heroName);
	  }
	}

	const batman = new Hero('Batman');

	batman.logName() // trả về 'Batman'

	setTimeout(batman.logName, 1000); // sau 1s trả về 'Batman'
	```
	**Arrow function** sẽ kế thừa ngữ cảnh **this** gần nhất, trong trường hợp này ta đã sử dụng từ khóa `new` nên **this** sẽ trỏ tới batman, đây là cách thường dùng khi viết React với class.

- Định nghĩa của **Higher-Order Function** là gì ?\
**Higher-Order Function** là hàm có hoạt động dựa trên hàm khác, tức là nó có thể nhận hàm (function) làm tham số đầu vào, hoặc sẽ return ra 1 hàm khác. 1 trong 2 điều kiện đó xảy ra gọi là **Higher-Order Function**.
Ví dụ:
	```javascript
	function formalGreeting() {
	  console.log("How are you?");
	}

	function casualGreeting() {
	  console.log("What's up?");
	}

	function greet(type, greetFormal, greetCasual) {
	  if(type === 'formal') {
	    greetFormal();
	  } else if(type === 'casual') {
	    greetCasual();
	  }
	}

	// prints 'What's up?'
	greet('casual', formalGreeting, casualGreeting);
	```
	Hàm **greet** có nhận vào 2 tham số **formalGreeting**, **casualGreeting** là function nên nó là 1 **Higher-Order Function**.

- Bạn có thể cho 1 ví dụ về **destructing** object hoặc array ?
	#### Destructing array
	```javascript
	const [a, b] = [10, 20];

	console.log(a); // 10

	console.log(b); // 20

	const [c, d, ...rest] = [10, 20, 30, 40, 50];

	console.log(rest); // [30, 40, 50]
	```
	#### Destructing object
	```javascript
	const hero = {
	  name: 'Batman',
	  realName: 'Bruce Wayne'
	};

	const { name, realName } = hero;

	console.log(name); // 'Batman',
	console.log(realName); // 'Bruce Wayne'
	```

- Bạn có thể cho 1 ví dụ tạo 1 chuỗi với ES6 Template Literals ?
	```javascript
	const name = "Pikachu"; 
	const result = `${name} tớ chọn cậu`; // <= Template Literals
	console.log(result); // 'Pikachu tớ chọn cậu',
	```

- Bạn có thể cho ví dụ tạo về **Curry Function** và tại sao cú pháp này có lợi ?
Thí dụ bạn có một hàm để tính giá trị discount, giảm ngay 10% cho khách hàng thân thiết.
	```javascript
	function discount(price, discount) {
	  return price * discount
	}

	// Giảm ngay 50 đồng khi khách hàng đã tiêu 500 đồng.
	const price = discount(500, 0.10); // $50 
	```

	Khách hàng tiêu tiền điên cuồng, chúng ta gọi hàm này say mê:
	```javascript
	const price = discount(1500, 0.10); // $150

	const price = discount(2000 ,0.10); // $200

	const price = discount(50, 0.10); // $5
	```
	
	Như bạn có thể thấy việc truyền tham số **0.10** đang lặp lại, để linh hoạt hơn ta sẽ sử dụng **Curry Function** như sau:
	```javascript
	function discount(discount) {
	  return (price) => {
	    return price * discount;
	  }
	}

	const tenPercentDiscount = discount(0.1);
	tenPercentDiscount(500); // $50

	const twentyPercentDiscount = discount(0.2);
	twentyPercentDiscount(500); // 100
	twentyPercentDiscount(5000); // 1000
	```

	Cú pháp này mạng lại lợi thế vì ta có thể sử dụng linh hoạt trong nhiều tình huống như ví dụ trên.

- Lợi ích của việc sử dụng cú pháp **Spread**, nó khác cú pháp **Rest** như thế nào.\
	Cú pháp **Spread** cho phép một tệp có thể lặp lại, chẳng hạn như array được mở rộng ở những nơi có nhiều đối số (đối với lệnh gọi hàm) hoặc một object được mở rộng ở những nơi có nhiều cặp khóa-giá trị.
	Ví dụ về **Spread**:
	```javascript
	function sum(x, y, z) {
	  return x + y + z;
	}

	const numbers = [1, 2, 3];

	console.log(sum(...numbers)); // 6
	```
	
	Cú pháp **Spread** trông giống cú pháp **Rest**, nhưng 2 cú pháp này hoạt động đối lập với nhau. Cú pháp **Spread** mở rộng 1 phần tử thành các thành phần của nó, trong khi đó cú pháp **Rest** gom nhiều phần tử thánh 1 phần tử duy nhất.
	Ví dụ về **Rest**:
	```javascript
	function sum(...args) {
	  return args.reduce((sum, current) => sum + current);
	}

	console.log(sum(1, 2, 3)); // 6
	```

- Làm sao để chia sẻ mã giữa các tệp ?\
	Để chia sẽ mã giữa các tệp ra sử dụng **Module Javascript**, để hiểu rõ xem ví dụ sau:

	#### Module ES5
	##### log.js
	```javascript
	module.exports.log = function (msg) { 
	  console.log(msg);
	};

	/* cách viết trên có thể viết như sau
	  exports.log = function (msg) { 
	    console.log(msg);
	  };
	*/
	```

	##### app.js
	```javascript
	const msg = require(./log.js);
	msg.log("Hello World") // "Hello World"
	```

	#### Module ES6
	##### log.js
	```javascript
	export function log (msg) { 
	  console.log(msg);
	};
	```

	##### app.js
	```javascript
	import { log } from (./log.js);
	log("Hello World") // "Hello World"
	```

- Tại sao bạn muốn tạo static cho các class ?\
	**Static** định nghĩa một phương thức tĩnh hoặc thuộc tính tĩnh cho một **class**. Cả phương thức và thuộc tính đều không thể được gọi trên các thể hiện của lớp. Thay vào đó, chúng được gọi trên chính lớp đó. Các phương thức tĩnh thường là các hàm tiện ích, chẳng hạn như các hàm để tạo hoặc sao chép các đối tượng, trong khi các thuộc tính tĩnh hữu ích cho bộ nhớ đệm, cấu hình cố định hoặc bất kỳ dữ liệu nào khác mà bạn không cần sao chép qua các phiên bản.
	```javascript
	class ClassWithStaticMethod {
	  static property = 'value';

	  static method() {
	    return 'call method.';
	  }
	}

	console.log(ClassWithStaticMethod.property); // 'value'
	console.log(ClassWithStaticMethod.method()); // 'call method.'

	const newObj = new ClassWithStaticMethod();
	console.log(newObj.property); // undefined
	console.log(newObj.method()); // Error: newObj.method is not a function
	```

- Làm cho hàm này hoạt động:\
`duplicate([1,2,3,4,5]); // [1,2,3,4,5,1,2,3,4,5]`
	```javascript
	function duplicate(arr) {
	  return [...arr, ...arr];
	}
	```

- Tạo một vòng lặp for cho output từ 1 đến 100, trong đó output “fizz” thay cho số chia hết cho 3, “buzz” thay cho số chia hết cho 5 và “fizzbuzz” thay cho số chia hết cho cả 3 và 5.
	```javascript
	for(let i = 1; i <= 100; i++) {
	  if (i % 3 === 0 && i % 5 === 0) {
	    console.log("fizzbuzz")
	  }
	  else if(i % 3 === 0) {
	    console.log("fizz")
	  }
	  else if (i % 5 === 0) {
	    console.log("buzz")
	  }
	  else {
	    console.log(i);
	  }
	}
	```

<a id="testing_questions"></a>
### 5. Các câu hỏi về Testing

- Vài điểm lợi và bất lợi trong việc kiểm thử code của bạn là gì?
	- #### Lợi ích
		- Nhiều mã có thể tái sử dụng và gỡ lỗi dễ dàng hơn.
		- Tăng hiệu quả nâng cấp và bảo trì mã.
		- Tương tác được đơn giản hóa do thử nghiệm các mô-đun riêng biệt.
		- Giảm chi phí kiểm tra và sửa lỗi.
	- #### Bất lợi
		- Rất khó để viết các **unit test** chất lượng và toàn bộ quá trình có thể tốn nhiều thời gian.
		- Không phải tất cả các lỗi đều có thể được phát hiện vì mỗi mô-đun được kiểm tra riêng biệt và các tích hợp khác có thể xuất hiện lỗi sau này.

- Bạn sẽ sử dụng những công cụ nào để kiểm thử chức năng của code của bạn?\
	Sử dụng **Mocha, chai** để viết unit test cho Front-End, Back-End.

- Sự khác nhau giữa một unit test và một functional/integration test là gì?\
**Unit Test** là kiểm tra từng **module** 1 cách riêng biệt (Không phụ thuộc vào bất kì thành phần mã bên ngoài nào).
**Functional / Intergration Test** là kiểm tra các chức năng được kết hợp từ các **module** khác nhau có hoạt động tốt hay không.
- Mục đích của một công cụ kiểm tra code style (code style linting tool) là gì?
**Code Lighting** là 1 loại phân tích tĩnh thường sử dụng để tìm các **patern** hoặc **code** không tuân thủ các nguyên tắc kiểu nhất định.
**Code Style Linting tool** không phải công cụ **debug** nhưng có thể được sử dụng hiệu quả trong quá trình gở lỗi để tìm ra những lỗi khó bắt gặp.
1 số tool như: ESLint, CSS Lint, W3C, CleanCSS, ...

<a id="performance_questions"></a>
### 6. Các câu hỏi về Performance

- Bạn sẽ sử dụng công cụ nào để tìm lỗi hiệu suất trong mã của mình ?\
Chrome DevTools

- Chỉ ra vài cách mà bạn có thể cải thiện hiệu suất cuộn trang **(scrolling performance)** trên website của bạn?
	- Sử dụng những hình ảnh có kích thước chính xác thay vì thay đổi kích thước chúng bằng CSS.
	- Tránh layout (reflows hoặc repaints).
	- **Lazy load**: Chỉ hiển thị dữ liệu cần thiết.
	- **Debouncing**: nhóm nhiều cuộc gọi thành 1 cuộc gọi duy nhất.

- Giải thích sự khác nhau giữa **layout**, **painting** và **compositing** ?\
**Layout**: trình duyệt tính toán không gian và vị trí của các phần tử trên màn hình.
**Painting**: Đây là quá trình điền vào các pixel. Nó liên quan đến việc vẽ ra các phần tử trên nhiều lớp.
**Compositing**: Trình duyệt vẽ các lớp khác nhau lên màn hình theo đúng thứ tự để trang hiển thị chính xác

<a id="network_questions"></a>
### 7. Các câu hỏi về Network
- Theo truyền thống, tại sao việc cung cấp các tài nguyên của trang web từ nhiều tên miền khác nhau là việc có lợi hơn ?\
	Trình duyệt giới hạn số lượng kết nối đang hoạt động trên mỗi miền. Để cho phép tải xuống đồng thời nội dung vượt quá giới hạn đó, tính năng phân tách miền (**domain sharding**) sẽ chia nội dung trên nhiều miền phụ. Khi nhiều miền được sử dụng để phân phát nội dung, các trình duyệt có thể tải xuống đồng thời nhiều tài nguyên hơn, dẫn đến thời gian tải trang nhanh hơn và cải thiện trải nghiệm người dùng.

- Hãy trình bày một cách toàn diện nhất quá trình từ lúc bạn nhập vào URL của một trang web đến khi nó hoàn thành việc tải và hiện thị trên màn hình của bạn ?
	- **Bước 1**: lấy địa chỉ IP của URL
		Lấy địa IP của URL gồm các bước sau:
		- Hệ thống kiểm tra bộ nhớ cache của bowser.
		- Nếu ko tìm thấy dữ liệu DNS trong **Browser Cache**, hệ thống sẽ kiểm tra trong bộ nhớ cache của hệ điều hành (OS).
		- Nếu ko tìm thấy dữ liệu DNS trong **OS Cache**, tiếp tục kiểm tra bộ nhớ cache DNS duy trì bởi router.
		- Nếu ko tìm thấy dữ liệu DNS trong **Router Cache**, nó sẽ chuyển sang tìm kiếm trong DNS cache của ISP (Internet Service Provider).
		- Nếu ko tìm thấy dữ liệu DNS trong **ISP Cache**, thì máy chủ DNS của ISP thực hiện truy vấn DNS để tìm kiếm dữ liệu DNS được yêu cầu.
	- **Bước 2**: Sau khi trình duyệt nhận được địa chỉ IP, nó sẽ mở kết nối TCP và gửi yêu cầu HTTP đến máy chủ web.
	- **Bước 3**:  Máy chủ web sẽ xử lý yêu cầu và gửi phản hồi HTTP về máy khách / trình duyệt.
	- **Bước 4**:  Trình duyệt phân tích cú pháp tài liệu HTML và hiển thị nó.

- Những điểm khác nhau giữa Long-Polling, Websockets và Server-Sent Events ?
	#### Long-Polling
	Thăm dò dài không tạo ra một kết nối liên tục, mà thay vào đó, thăm dò máy chủ với một yêu cầu vẫn mở cho đến khi máy chủ phản hồi , lúc này kết nối đóng và một kết nối mới được yêu cầu ngay lập tức. Điều này có thể gây ra một số độ trễ trong khi thiết lập lại kết nối.
	#### Websockets
	Websocket là một kênh giao tiếp song công đầy đủ trên một kết nối TCP duy nhất. Khi cả máy chủ và trình duyệt hỗ trợ, nó là phương tiện truyền tải duy nhất thiết lập kết nối hai chiều, liên tục thực sự giữa máy khách và máy chủ.
	#### Server-Sent Events
	Còn được gọi là **EventSource** là công nghệ nơi trình duyệt nhận các bản cập nhật tự động từ máy chủ thông qua kết nối HTTP. API EventSource của sự kiện do máy chủ gửi được W3C chuẩn hóa như một phần của HTML5.

- Giải thích các request header và response header sau:
	- Sự khác nhau giữa **Expires**, **Date**, **Age** và **If-Modified-…**
		**Expires Header**: Cung cấp date / time mà sau đó phản hồi được coi là cũ.
		**Date Header**: Cung cấp date / time khi trang được khởi tạo.
		**Age Header**: Tuổi đối tượng đã ở trong bộ nhớ cache proxy tính bằng giây.
		**If-Modified-… Header**: sẽ trả về mã trạng thái không thay đổi 304 nếu nội dung không thay đổi.
	- **Do Not Track**: Chỉ ra mức ưu tiên theo dõi của người dùng. Cho biết nơi họ muốn quyền riêng tư thay vì nội dung được cá nhân hóa.
	- **Cache-Control**: Chỉ định các chỉ thị cho cơ chế bộ nhớ đệm trong cả request và response.
	- **Transfer-Encoding**: Trong trường hợp bình thường, máy chủ sẽ cho khách hàng biết độ dài nội dung mà máy chủ đã gửi, nhưng trong một số trường hợp, độ dài nội dung vẫn không xác định và máy chủ sẽ gửi nội dung theo từng phần. Chỉ định điều này cho khách hàng biết máy chủ đang gửi theo nào.
	- **ETag**: Một mã định danh được sử dụng để xác định phiên bản của file / source.  Nó cho phép bộ nhớ đệm hiệu quả hơn và tiết kiệm băng thông, vì web server không cần gửi lại response đầy đủ nếu nội dung không thay đổi. Ngoài ra, thẻ etags giúp ngăn các bản cập nhật đồng thời của tài nguyên ghi đè lẫn nhau.
	- **X-Frame-Options**: Cho biết liệu trình duyệt có được phép hiển thị trang trong `<Frame>`, `<iFrame>` hoặc `<object>` → ngăn chặn các cuộc tấn công clickjacking.

- Các HTTP methods là gì? Liệt kê tất cả HTTP methods mà bạn biết, và giải thích chúng ?\
	**HTTP methos** chỉ ra các hành động mong muốn được thực hiện trên resouce được xác định.
	**GET**: yêu cầu resource.
	**POST**: tạo hoặc cập nhật resource.
	**PUT**: cập nhật resource hiện có.
	**PATCH**: sửa đổi 1 phần resource.
	**DELETE**: xóa resource.
	**TRACE**: lặp lại yêu cầu đã nhận để client có thể thấy những thay đổi hoặc bổ sung (nếu có) đã được thực hiện bởi các server trung gian.
	**HEAD**: yêu cầu phản hồi giống với yêu cầu GET, nhưng không có phần response body. Điều này rất hữu ích để truy xuất siêu thông tin được viết trong response header mà không cần phải trả về toàn bộ nội dung.
	**OPTIONS**: trả về các HTTP method mà server hỗ trợ cho URL được chỉ định.
	**CONNECT**: chuyển đổi kết nối yêu cầu thành đường hầm TCP / IP trong suốt, thường là để tạo điều kiện cho giao tiếp được mã hóa SSL (HTTPS) thông qua proxy HTTP không được mã hóa.

- CDN là gì và lợi ích của việc sử dụng CDN là gì?
	- CDN là gì ?
		- **CDN (Content Delivery Network)** là 1 hệ thống các server nằm rải rác ở nhiều nơi, nhằm lưu trữ và cung cấp dữ liệu cho người dùng.
		- Ta vẫn có 1 server chính (origin server), 1 hệ thống các server phụ chứa dữ liệu (edge server). Những file tĩnh như ảnh, video, CSS, JS sẽ được lưu trữ tại các CDN này.
		- Với CDN, thay vì kết nối trực tiếp tới server chính, client sẽ kết nối tới server gần nhất để lấy dữ liệu, cải thiện tốc độ tải.
	- Lợi ích ?
		- **Tăng bảo mật**: Ta có thể <a href="https://toidicodedao.com/2016/10/04/bao-mat-cua-giao-thuc-http/" target="__blank" rel="noopener noreferrer">cài đặt SSL</a> edge server trong CDN để tăng tính bảo mật cho hệ thống.
		- **Chống DDOS**: Một số <a href="https://www.cloudflare.com/cdn/" target="__blank" rel="noopener noreferrer">CDN provider như Cloudflare</a> còn đi kèm luôn dịch vụ chống DDOS. Các CDN này có khả năng chịu tải cao, có sẵn bộ lọc để chống DDOS trước khi những request này tới được server chính.
		- **Caching, tiết kiệm băng thông**: Thông thường, người ta lưu trữ những file tĩnh như ảnh, css … trên CDN. Tuy nhiên, một số CDN có thể dùng để <a href="https://toidicodedao.com/2017/08/01/thiet-ke-he-thong-trieu-nguoi-dung-high-scalability/" target="__blank" rel="noopener noreferrer">cache kết quả từ server</a> (dynamic caching).
		- Thay vì truy cập và lấy dữ liệu đến server chính (origin server), người dùng lấy dữ liệu từ cache của CDN, nhanh và tiết kiệm băng thông hơn nhiều.
		- **Tăng tính ổn định của hệ thống**: Khi hệ thống chỉ hoạt động dựa trên 1 server, nếu server đó tèo đồng nghĩa với toàn bộ hệ thống di tong.
		- Với CDN, ta có nhiều server nên nếu một server nào đó có bị sập, người dùng vẫn có thể truy cập được dữ liệu trên server khác của hệ thống CDN.

<a id="coding_questions"></a>
### 8. Các câu hỏi về việc viết code

- Question: What is the value of foo?\
	`var foo = 10 + '20';`
	=> **foo = '1020'**. Vì ép kiểu từ **number** sang **string**.

- Question: What will be the output of the code below?\
	`console.log(0.1 + 0.2 == 0.3);`
	=> **false** Nguyên nhân là do cách chúng được lưu trữ trong phần cứng. Nếu mún biết tại sao <a href="https://floating-point-gui.de/basic/" target="__blank" rel="noopener noreferrer">xem ở đây</a>.

- Question: How would you make this work?
	```javascript
	add(2, 5); // 7
	add(2)(5); // 7
	```

	**Answer:**
	```javascript
	function add(a, b) {
	  if(a && b) return a + b;

	  return function(c) {
	    return a + c;
	  }
	}
	```
	Liên quan đến **Closure** nếu gọi hàm theo cách 2.
	Function add(2) trả về 1 **anonymous function**: function(c) { return 2 + c; }.\
	Sau đó add(2)(5) gọi hàm **anonymous function** và truyền vào tham số 5: function(5) { return 2 + 5; } sẽ trả về kết quả là 7.

- Question: What value is returned from the following statement?\
	`"i'm a lasagna hog".split("").reverse().join("");`
	=> **"goh angasal a m'i"**

- Question: What is the value of window.foo?\
	`( window.foo || ( window.foo = "bar" ) );`
	kết quả là **"bar"** nếu window ko có thuộc tính **foo**.

- Question: What is the outcome of the two alerts below?
	```javascript
	var foo = "Hello";
	(function() {
	  var bar = " World";
	  alert(foo + bar);
	})();
	alert(foo + bar);
	```

	Alert 1: **"Hello World"**.
	Alert 2: **ReferenceError because bar is not defined** vì biến **bar** nằm trong [function scope](https://dev.to/sandy8111112004/javascript-introduction-to-scope-function-scope-block-scope-d11#:~:text=A%20block%20scope%20is%20the,only%20within%20the%20corresponding%20block.).

- Question: What is the value of foo.length?
	```javascript
	var foo = [];
	foo.push(1);
	foo.push(2);
	```
	=> **2**

- Question: What is the value of foo.x?
	```javascript
	var foo = {n: 1};
	var bar = foo;
	foo.x = foo = {n: 2};
	```

	=> Kết quả là **Undefined**, giải thích:
	- Toán tử gán được thực hiện từ phải sang trái. Do đó câu lệnh trên có thể được viết lại như sau: `foo.x = (foo = {n: 2})`.
	- <a href="http://www.ecma-international.org/ecma-262/6.0/index.html#sec-assignment-operators-runtime-semantics-evaluation" target="__blank" rel="noopener noreferrer">Theo thông số kỹ thuật</a> phía bên trái của biễu thức được đánh giá đầu tiên, mặc dù toán tử gán được thực hiện từ phải sang trái.
	- Ta gọi **foo.x** là **L.H.E** (Left Hand Expression) và **(foo = {n: 2})**  là **R.H.E**
		- Đánh giá **L.H.E** để xác định nơi giá trị của **R.H.E** sẽ được gán cho. Lúc này **foo** của **foo.x** đang giữ tham chiếu đến **{n: 1}**.
		- Đánh giá **R.H.E** để nhận giá trị sẽ được gán. Lúc này **R.H.E** lại là 1 biểu thức gán, vì vậy nó được đánh giá cùng 1 cách trên.
			- Đánh giá **foo** để xác định nơi được gán.
			- Đánh giá biểu thức **{n: 2}**, điều này tạo ra 1 object mới với vùng tham chiếu mới được dùng gán sau đó.
			- Gán **{n: 2}** cho **foo**, hay nói cách khác **foo** giữ tham chiếu mới đến **{n: 2}**.
			- Và sau đó **R.H.E** trả về **{n: 2}**.
		- Gán giá trị trả về từ **R.H.E** là **{n: 2}** cho **foo.x** đã được giải quyết ở bước trên tức là **{n: 1}**.
		- Lưu ý là **foo** được gán giá trị mới bên **R.H.E** ko ảnh hưởng đến **foo** bên **L.H.E** vì như ta đã nói ở trên **foo** bên **L.H.E** được đánh giá trước. Nói cách khác **foo** của 2 bên đang tham chiếu đến 2 object khác nhau.
		- Khi code chạy, **bar** vẫn là 1 tham chiếu đến **foo** ban đầu (**foo** trước khi thay đổi) nên **bar** có thuộc tính **x** tham chiếu đến **{n: 2}**. Mà **foo** mới cũng tham chiếu đến **{n: 2}** => **bar.x === foo**

- Question: What does the following code print?
	```javascript
	console.log('one');

	setTimeout(function() {
	  console.log('two');
	}, 0);

	Promise.resolve().then(function() {
	  console.log('three');
	})

	console.log('four');
	```
	**"one"**, **"four"**, **"three"**, **"two"** vì **Promise** được đặt vào hàng đợi **Micro Task**, **setTimeout** đặt vào hàng đợi **Macro Task**. Khi **Call Stack** trống nó sẽ thực hiện toàn bộ **cb** trong **Micro Task**, sau đó  **Call Stack** trống nó sẽ thực hiện tiếp trong **Macro Task**.

- Question: What is the difference between these four promises?
	```javascript
	doSomething().then(function () {
	  return doSomethingElse();
	});

	doSomething().then(function () {
	  doSomethingElse();
	});

	doSomething().then(doSomethingElse());

	doSomething().then(doSomethingElse);
	```
	- **Trường hợp 1:**
		Thực thi hàm `doSomething()`, khi nó được **resolve**.
		Tiếp tục thực thi hàm `doSomethingElse()`.
		Giá trị trả về của chuỗi **promise** này là giá trị đã được **resolve** (hoặc trả về) của hàm `doSomethingElse()`.
	- **Trường hợp 2:**
		Thực thi hàm `doSomething()`, khi nó được **resolve**.
		Tiếp tục thực thi hàm `doSomething()`.
		Giá trị trả về của chuỗi **promise** này là **undefined** do ko có giá trị nào được trả về từ **then**. Bất kì giá trị nào trà về từ `doSomethingElse()` dù là 1 **promise** hay giá trị đều bị bỏ qua.
	- **Trường hợp 3:**
		Thực thi hàm `doSomething()`, trước khi hàm được **resolve**, đồng thời thực thi  `doSomethingElse()`.
		Đây là 1 **Bug**.
	- **Trường hợp 4:**
		Thực thi hàm `doSomething()`, khi nó được **resolve**.
		Truyền giá trị được **resolve** từ `doSomething()` vào `doSomethingElse` dưới đạng đối số đầu tiên
		Tiếp tục thực thi hàm `doSomethingElse()`.
		Giá trị trả về của chuỗi **promise** này là giá trị đã được **resolve** (hoặc trả về) của hàm `doSomethingElse()`.
		Mã tương tự: `  doSomething().then((first_argument) => doSomethingElse);`

<a id="fun_questions"></a>
### 9. Các câu hỏi về việc viết code
- Gần đây bạn đã làm việc với những dự án thú vị nào?
- Vài điều bạn thích về các công cụ dành cho nhà phát triển mà bạn sử dụng là gì?
- Ai là người truyền cảm hứng cho bạn trong cộng đồng front-end?
- Bạn có dự án ngắn hạn nào không? Kiểu dự án gì?
- Những tính năng của Internet Explorer mà bạn thích là gì?
- Bạn có thích dùng cà phê không?

---

<a href="#top"><g-emoji class="g-emoji" alias="arrow_up" fallback-src="https://github.githubassets.com/images/icons/emoji/unicode/2b06.png">⬆</g-emoji>Lên đầu trang</a>
