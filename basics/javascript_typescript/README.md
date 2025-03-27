# [MODULE-1] Audit Module 1

##Nội dung câu hỏi và phần trả lời

## [ Câu hỏi ] 1. Có bao nhiêu Statement control trong Java Script  ?
```
Gồm có: if else, do while , while, for, swith case
```
## [ Câu hỏi ] 2. So sánh giống và khác nhau giữa for, while và do..while
```
Giống nhau: Đều là vòng lặp.
Khác nhau giữa for và while do...while: 
for là vòng lặp biết trước số lần lặp
do while và while: không biết trước số lần lặp tuy nhiên phải có điều kiện dừng hoặc break
Khác nhau giữa do while và while:
do while: khối lệnh sẽ được thực thi sau đó mới xét đến điều kiện lặp trong while
while: Điều kiện lặp sẽ được xét trước sau đó khối lệnh trong while mới được thực thi
```
## [ Câu hỏi ] 3. Cách khai báo biến bằng từ khóa let và var khác nhau như thế nào ?
```
var : có thể khai báo cùng 1 tên biến nhiều lần
let: không cho phép khai báo cùng một tên biến
Phạm vi của biến số sử dụng var là phạm vi hàm số hoặc bên ngoài hàm số, toàn cục.
Phạm vi của biến số sử dụng let là phạm vi một khối, xác định bởi cặp {}
```
## [ Câu hỏi ] 4. Các hàm dùng để tạo thông báo trong Javascript ?
```
Alert: Hiển thị thông báo 
prompt: Hiển thị thông báo tuy nhiên cho phép người dùng nhập dữ liệu
từ bàn phím
confirm: Hiển thị thông báo và cho phép người dùng xác nhận OK hoặc cancel
trước khi thực thi 1 lệnh khác
```
## [ Câu hỏi ] 5. Trong Javascript có bao nhiêu loại kiểu dữ liệu ?
```
có 5 kiểu dữ liệu đó là:
String
Number
Boolean
Object
Undefined
```
## [ Câu hỏi ] 6. So sánh null và rỗng
```
Null: Giá trị rỗng không tồn tại chưa được cấp phát vùng nhớ. Đây là 1 object
Rỗng : đã được cấp phát vùng nhớ nhưng giá trị ko có gì cả"
```
## [ Câu hỏi ] 7. Đặc điểm mảng một chiều trong Javascript
```
- truy xuất mảng thông qua index
- có kích thước không cố định"
```
## [ Câu hỏi ] 8. Các cách khởi tạo một mảng kiểu String trong JavaScript ?
```
var name_array = new Array(""kien"",""codegym"",""Da Nang"");
var name_array = [""kien"",""codegym"",""Da Nang""];"
```
## [ Câu hỏi ] 9. Phân biệt tham trị và tham chiếu trong Javascript
```
Tương tự by value và by references trong JavaCore
```
## [ Câu hỏi ] 10. Cách định nghĩa một hàm trong JavaScript
```
```
## [ Câu hỏi ] 11. Phân biệt giữa Hàm có return và hàm không có return
```
Hàm có return là hàm có sử dụng từ khóa return để đặt ở cuối hàm với mục đích trả kết quả về để sử dụng tiếp ở những đoạn code bên ngoài
Hàm không có return là hàm không có sử dụng từ khóa return đặt trong hàm"
```
## [ Câu hỏi ] 12. Biến có bao nhiêu phạm vi trong JavaScript ?
```
Biến có 2 phạm vi trong javascript: cục bộ và toàn cục
```
## [ Câu hỏi ] 13. Lập trình hướng đối tượng là gì ?
```
- Là phương pháp lập trình
- Áp dụng 4 nguyên tắc chính"
```
## [ Câu hỏi ] 14. Các đặc điểm trong Lập trình hướng đối tượng
```
- Abstract
- Encapsulation
- Inheritances
- Polymorphism"
```
## [ Câu hỏi ] 15. Đối tượng là gì ?
```
- Là các sự vật, sự việc, hiện tượng ....
bao gồm 2 thành phần chính: properties và method"
```
## [ Câu hỏi ] 16. Class là gì?
```
template mô tả chung cho object
```
## [ Câu hỏi ] 17. Constructor là gì ?
```
Là phương thức khởi tạo đối tượng
```
## [ Câu hỏi ] 18. Những phương thức nào cho phép tương tác với chuỗi ?
```
Tìm kiếm chuỗi con (indexOf(), lastIndexOf(),search())
Cắt chuỗi con 
(slice(start, end)
substring(start, end)
substr(start, length))
Chuyển chữ hoa thường và thường sang hoa (string.toUpperCase(),string.toLowerCase())
Chuyển đổi chuỗi sang mãng split()
Tìm kí tự trong chuỗi , charAt()"
```
## [ Câu hỏi ] 19. Casting data type trong JS
```
Gồm có Number , String, Boolean
```
## [ Câu hỏi ] 20. Toán tử == và === khác và giống nhau như thế nào ?
```
giống nhau: đều là toán tử so sánh các giá trị
khác nhau: 1 == ""1"" -> true nhưng 1 === ""1"" -> false
tức là 2 dấu bằng chỉ so sánh về giá trị còn 3 dấu = là phép so sánh
tuyệt đối cả về giá trị lẫn kiểu dữ liệu"
```

## Mục tiêu

* Audit module 1

## Youtube Link

* [[Install] Cài đặt webstorm](https://www.youtube.com/watch?v=KmRpCrBnpQM&list=PL9yu4ScFhKGwARkCAB-e0P-ebGDNEpjVm)
* [[GIT] Cài đặt và thực hiện các lệnh GIT cơ bản](https://www.youtube.com/watch?v=tLsFNLar1y4&list=PL9yu4ScFhKGwARkCAB-e0P-ebGDNEpjVm&index=3)
* [[CaseStudy] Case study Bootcamp Preparation](https://www.youtube.com/watch?v=8fvxPxDM3kk&list=PL9yu4ScFhKGwr24i_XvHL5nkRYDpVeYez)
