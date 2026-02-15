# Tổng hợp 100 câu hỏi phỏng vấn Golang thường gặp

Nguồn: [Viblo - Tổng hợp 100 câu hỏi phỏng vấn Golang thường gặp](https://viblo.asia/p/tong-hop-100-cau-hoi-phong-van-golang-thuong-gap-gwd432oQVX9)

---

### 1. Golang là gì?

Go, hay Golang, là một ngôn ngữ lập trình mã nguồn mở được phát triển bởi Google. Nó là ngôn ngữ biên dịch, kiểu tĩnh và được thiết kế để xây dựng các ứng dụng mở rộng và hiệu suất cao.

### 2. Các tính năng chính của Go là gì?

- Hỗ trợ concurrency bằng cách sử dụng Goroutines.
- Garbage collection.
- Statically typed và dynamic behavior.
- Cú pháp đơn giản.
- Biên dịch nhanh.

### 3. Goroutines là gì, làm thế nào để tạo goroutine?

Goroutines là các thread nhẹ được quản lý bởi runtime của Go. Chúng là các function hoặc method chạy đồng thời với các function hoặc method khác.
Sử dụng từ khóa `go` trước một function:

```go
go myFunction()
// With anonymous function
go func(){ }()
```

### 4. Channel trong Go là gì, làm thế nào để khai báo một channel?

Channels là cách để các Goroutines giao tiếp với nhau và đồng bộ hóa việc thực thi. Chúng cho phép gửi và nhận các giá trị.

```go
ch := make(chan int)
```

### 5. Buffered channel là gì?

Buffered channel có một dung lượng xác định và cho phép gửi các giá trị (mà không block bên gửi) cho đến khi buffer đầy. Nó không yêu cầu phải luôn có một receiver sẵn sàng để nhận và xử lý dữ liệu.

```go
func main(){
    ch1 := make(chan int, 1)
    ch1 <- 1 // this will not block, thanks to buffer

    ch2 := make(chan int)
    ch2 <- 1 // this will block main because no other goroutine will read it
}
```

### 6. Làm thế nào để đóng một channel?

Sử dụng hàm `close()` (thường được gọi ở phía gửi msg vào channel, hoặc đối tượng quản lý channel đó):

```go
close(ch)
```

Kiểm tra channel đã đóng hay chưa:

```go
// Case 1
value, ok := <-ch
if !ok {
    // Channel is closed
}

// Case 2
for value := range ch {
    // Process value
}
// Channel is closed when the loop exits
```

### 7. Struct trong Go là gì, thế nào để định nghĩa một struct?

Struct là một kiểu dữ liệu do người dùng định nghĩa cho phép nhóm các trường có kiểu dữ liệu khác nhau vào một thực thể duy nhất.

```go
type Person struct {
    Name string
    Age int
}

// Hoặc sử dụng anonymous struct
func someFunc(){
    arr := []struct{ UserId int }{{UserId: 200}}
    var m map[string]struct { UserId string }
    fmt.Println("Hello ", arr, m)
}
```

### 8. Interface trong Go là gì, làm thế nào để triển khai một interface?

Interface trong Go là một kiểu dữ liệu định nghĩa tập hợp các method signatures. Nó tạo tính đa hình polymorphism bằng cách cho phép đa dạng hóa việc định nghĩa các hành vi.
Một kiểu dữ liệu (struct) triển khai một interface bằng cách thực hiện tất cả các method của nó:

```go
type Animal interface {
    Speak() string
}

type Dog struct{}

func (d Dog) Speak() string {
    return "Woof!"
}
```

### 9. Những ảnh hưởng đến performance khi sử dụng các struct lớn làm tham số cho hàm trong Go là như thế nào? Làm thế nào để tối ưu hóa?

Việc truyền các struct lớn theo giá trị (value) dẫn đến việc tạo ra một bản sao (copy), điều này có thể tốn kém về mặt bộ nhớ và hiệu suất. Để tối ưu hóa, nên truyền các con trỏ đến struct `(*Struct)` thay vì sao chép toàn bộ struct.

### 10. interface{} trong Go là gì, và nó liên quan đến empty interfaces như thế nào?

`interface{}` là một empty interface, có nghĩa là nó có thể chứa các giá trị của bất kỳ kiểu nào vì mọi kiểu đều triển khai (implement) zero methods. Nó thường được sử dụng cho các cấu trúc dữ liệu generic hoặc khi làm việc với các function chấp nhận bất kỳ kiểu nào.

### 11. Một số chiến lược để giảm tải GC overhead là gì?

- Sử dụng object pooling (`sync.Pool`) để tái sử dụng bộ nhớ.
- Giảm việc tạo ra các đối tượng có tuổi thọ ngắn.
- Dự trữ trước bộ nhớ cho slices hoặc maps (`make([]T, 0, capacity)`) để tránh việc cấp phát lại thường xuyên.

### 12. Bạn sẽ implement việc xử lý lỗi và logging trong một dự án Go lớn như thế nào?

- Sử dụng custom error types để thêm ngữ cảnh và bọc lỗi bằng `fmt.Errorf`.
- Sử dụng logging có cấu trúc (như `zap` hoặc `logrus`).
- Sử dụng `defer` để đóng tài nguyên và `recover` để xử lý panics.

### 13. Làm thế nào để tránh các circular dependencies trong Go packages?

- Refactor các chức năng chung vào một package riêng.
- Sử dụng interfaces để tách rời (decoupling) các dependencies.
- Tổ chức lại cấu trúc thư mục/package để đồ thị phụ thuộc không bị vòng lặp.

### 14. Từ khóa defer là gì, và nó hoạt động như thế nào?

`defer` được sử dụng để hoãn việc thực thi một hàm cho đến khi hàm bao quanh nó trả về. Các hàm `defer` được thực thi theo thứ tự LIFO (Last In, First Out).

### 15. How do you implement graceful shutdown of a Go HTTP server?

Sử dụng `context` với phương thức `Shutdown` của `http.Server`. Lắng nghe tín hiệu hệ thống (ví dụ `SIGINT`, `SIGTERM`) bằng `os/signal` và gọi `Shutdown` khi nhận được tín hiệu.

```go
srv := &http.Server{Addr: ":8080"}
go func() {
    if err := srv.ListenAndServe(); err != http.ErrServerClosed {
        log.Fatalf("Server error: %v", err)
    }
}()

c := make(chan os.Signal, 1)
signal.Notify(c, os.Interrupt)
<-c
srv.Shutdown(context.Background())
```

### 16. Pointer trong Go là gì, và khai báo thế nào?

Pointer giữ địa chỉ bộ nhớ của một giá trị.

```go
x := 5
var p *int
p = &x // lấy địa chỉ của x
fmt.Println(*p) // lấy giá trị tại địa chỉ p trỏ tới
```

### 17. Sự khác biệt giữa new và make là gì?

- `new(T)`: Cấp phát bộ nhớ zero-initialized cho kiểu T và trả về con trỏ `*T`.
- `make(T, args)`: Cấp phát và khởi tạo bộ nhớ cho slices, maps, và channels. Trả về giá trị kiểu T (không phải con trỏ).

### 18. Slice trong Go là gì?

Slice là một mảng có kích thước động, linh hoạt hơn mảng cố định.

### 19. Làm thế nào để tạo một slice?

```go
s := make([]int, 0)
s2 := make([]int, 3, 5)
s3 := []int{}
```

### 20. Map trong Go là gì?

Map là một tập hợp các cặp key-value.

### 21. Làm thế nào để tạo và kiểm tra phần tử trong map?

```go
m := make(map[string]int)
m["key"] = 1
if val, ok := m["key"]; ok {
    // làm gì đó với val
}
```

### 22. Câu lệnh select là gì?

`select` cho phép một Goroutine đợi trên nhiều hoạt động giao tiếp channel.

### 23. Làm thế nào để sử dụng select?

```go
select {
case msg := <-ch:
    fmt.Println(msg)
case <-time.After(time.Millisecond*500):
    fmt.Println("timeout")
default:
    fmt.Println("no message")
}
```

### 24. Channel nil là gì?

Channel nil sẽ chặn (block) cả việc gửi và nhận. Có thể dùng để vô hiệu hóa một nhánh trong `select`.

### 25. Hàm init là gì?

`init` là hàm đặc biệt dùng để khởi tạo trạng thái package, chạy trước hàm `main`.

### 26. Có thể có nhiều hàm init không?

Có, chúng sẽ chạy theo thứ tự xuất hiện trong file hoặc theo thứ tự import.

### 27. Struct rỗng struct{} là gì?

Chiếm 0 byte bộ nhớ. Thường dùng cho signaling trong channel hoặc làm value trong map khi chỉ cần quan tâm đến key (tương tự Set).

### 28. Làm thế nào để xử lý lỗi trong Go?

Go xử lý lỗi bằng cách trả về giá trị kiểu `error` từ hàm và kiểm tra `if err != nil`.

### 29. Type assertion là gì?

Dùng để lấy giá trị thực từ một interface: `value, ok := x.(T)`.

### 30. Lệnh go fmt là gì?

Dùng để tự động định dạng mã nguồn theo chuẩn chung của Go.

### 31. Mục đích của các lệnh go mod, go mod tidy, go clean -modcache là gì?

- `go mod`: Quản lý dependencies.
- `go mod tidy`: Dọn dẹp `go.mod` và `go.sum`, xóa deps thừa, thêm deps thiếu.
- `go clean -modcache`: Xóa cache các module đã tải về.

### 32. Làm thế nào để tạo một module?

`go mod init <module-name>`

### 33. Package trong Go là gì?

Là cách nhóm các file mã nguồn liên quan lại với nhau.

### 34. Làm thế nào để import một package?

`import "fmt"`

### 35. Quy tắc hiển thị (visibility rules) của các hàm, biến trong Go thế nào?

- Viết hoa chữ cái đầu: Exported (Public).
- Viết thường chữ cái đầu: Unexported (Private).

### 36. Sự khác biệt giữa var và := là gì?

- `var`: Khai báo biến với kiểu tường minh hoặc ngầm định, dùng được ở scope package.
- `:=`: Khai báo ngắn gọn, tự suy luận kiểu, chỉ dùng được bên trong hàm.

### 37. panic trong Go là gì?

Dùng để dừng chương trình ngay khi gặp lỗi nghiêm trọng không thể phục hồi.

### 38. recover là gì?

Dùng để lấy lại quyền điều khiển sau khi xảy ra `panic`, ngăn chương trình bị crash.

### 39. Làm thế nào để sử dụng recover?

Phải được gọi bên trong một hàm `defer`.

### 40. Constant trong Go là gì?

Các giá trị không đổi trong suốt quá trình chạy, khai báo bằng `const`.

### 41. Làm thế nào để khai báo một constant?

`const Pi = 3.14`

### 42. iota trong Go là gì?

Là bộ đếm số nguyên tự động tăng, thường dùng để định nghĩa các hằng số dạng Enum.

### 43. go test là gì?

Công cụ chạy các unit test. Các lệnh mở rộng: `-v` (verbose), `-bench` (benchmark), `-cover` (coverage), `-race` (race detector).

### 44. Làm thế nào để viết một hàm test?

Tên hàm bắt đầu bằng `Test`, tham số `(t *testing.T)`, nằm trong file `*_test.go`.

### 45. Benchmarking trong Go là gì?

Đo lường hiệu suất thực thi của một hàm.

### 46. Làm thế nào để viết một hàm benchmark?

Tên hàm bắt đầu bằng `Benchmark`, tham số `(b *testing.B)`, sử dụng vòng lặp `b.N`.

### 47. Build constraint là gì?

Dùng để chỉ định file nào được tham gia quá trình biên dịch dựa trên hệ điều hành, kiến trúc CPU...

### 48. Làm thế nào để thiết lập một build constraint?

Sử dụng comment ở đầu file: `// +build linux` hoặc `//go:build linux`.

### 49. Slices được backed bởi arrays nghĩa là gì?

Bản chất slice chứa một con trỏ tới một mảng ẩn (underlying array) bên dưới.

### 50. Garbage Collection trong Go là gì?

Cơ chế tự động giải phóng bộ nhớ của các đối tượng không còn được sử dụng.

### 51. Mục đích của package context trong Go là gì?

Quản lý deadline, tín hiệu hủy (cancellation) và truyền dữ liệu trong phạm vi một request.

### 52. Làm thế nào để sử dụng context trong Go?

Dùng `context.WithTimeout`, `context.WithCancel`, hoặc `context.WithValue`. Kiểm tra channel `ctx.Done()` trong các goroutine.

### 53. sync.WaitGroup là gì?

Dùng để đợi một nhóm goroutine hoàn thành.

### 54. Làm thế nào để sử dụng sync.WaitGroup?

Dùng `Add()` để set số lượng, `Done()` khi hoàn thành và `Wait()` để chặn cho đến khi xong hết.

### 55. Mục đích của sync.Mutex là gì?

Khóa (lock) để tránh race condition khi nhiều goroutine truy cập chung một tài nguyên.

### 56. Làm thế nào để sử dụng sync.Mutex?

`mu.Lock()` trước khi truy cập và `mu.Unlock()` (thường dùng qua `defer`) sau khi xong.

### 57. select được sử dụng thế nào với channels?

Dùng để lắng nghe đồng thời trên nhiều channel.

### 58. go generate là gì?

Lệnh chạy các công cụ tạo mã nguồn dựa trên comment đặc biệt trong code.

### 59. Method receiver trong Go là gì?

Xác định method đó thuộc về kiểu dữ liệu nào (value receiver hoặc pointer receiver).

### 60. Sự khác biệt giữa value receiver và pointer receiver là gì?

- Value: Làm việc trên bản sao.
- Pointer: Làm việc trên dữ liệu gốc, cho phép thay đổi giá trị và tránh copy dữ liệu lớn.

### 61. Variadic function là gì?

Hàm chấp nhận số lượng tham số không cố định (dùng `...T`).

### 62. rune trong Go là gì?

Là alias cho `int32`, đại diện cho một Unicode code point.

### 63. select block không có default case hoạt động thế nào?

Sẽ chặn (block) cho đến khi có ít nhất một case sẵn sàng.

### 64. Mục đích Ticker trong Go là gì?

Tạo ra các sự kiện lặp lại sau một khoảng thời gian đều đặn.

### 65. Làm thế nào để xử lý JSON trong Go?

Sử dụng package `encoding/json` với `Marshal` (encode) và `Unmarshal` (decode).

### 66. go vet là gì?

Công cụ phân tích tĩnh để tìm các lỗi tiềm ẩn mà compiler có thể bỏ qua.

### 67. Anonymous function trong Go là gì?

Hàm không tên, có thể định nghĩa và thực thi ngay lập tức hoặc gán cho biến.

### 68. Sự khác biệt giữa == và reflect.DeepEqual() là gì?

- `==`: So sánh các kiểu cơ bản.
- `reflect.DeepEqual()`: So sánh sâu các cấu trúc phức tạp như slice, map, struct.

### 69. time.Duration trong Go là gì?

Đại diện cho khoảng thời gian trôi qua (kiểu `int64` nanoseconds).

### 70. Làm thế nào để xử lý timeout với context?

`ctx, cancel := context.WithTimeout(context.Background(), time.Second)`

### 71. Pipeline trong Go là gì?

Chuỗi các giai đoạn xử lý kết nối với nhau bằng các channel.

### 72. Quy ước thư mục pkg trong Go là gì?

Chứa mã nguồn có thể được sử dụng bởi các project khác hoặc các package khác trong project (không bắt buộc).

### 73. Làm thế nào để debug mã Go?

Dùng `dlv` (Delve), log, hoặc printf.

### 74. type alias trong Go là gì?

Tạo tên mới cho một kiểu đã có: `type MyInt = int`.

### 75. Sự khác biệt giữa append và copy trong slices là gì?

- `append`: Thêm phần tử, có thể tạo slice mới nếu vượt capacity.
- `copy`: Sao chép phần tử từ slice này sang slice khác.

### 76. Mục đích của go doc là gì?

Hiển thị tài liệu hướng dẫn (comment) của code.

### 77. Làm thế nào để xử lý panics trong production code?

Dùng `recover` trong `defer` và log lại thông tin lỗi.

### 78. Sự khác biệt giữa map và struct là gì?

- Map: Động, tập hợp key-value.
- Struct: Tĩnh, tập hợp các trường cố định.

### 79. Package unsafe dùng làm gì?

Thao tác trực tiếp với bộ nhớ (không an toàn, nên hạn chế).

### 80. Làm thế nào để thực hiện dependency injection trong Go?

Sử dụng interface và truyền vào qua tham số constructor hoặc hàm khởi tạo.

### 81. Goroutine khác gì với một thread?

Goroutine nhẹ hơn (stack khởi điểm 2KB so với MB của thread), được quản lý bởi Go runtime thay vì OS.

### 82. Go scheduler hoạt động như thế nào?

Sử dụng mô hình M:N và thuật toán work-stealing để lên lịch goroutine trên các thread OS.

### 83. Memory leak là gì và làm thế nào để ngăn chặn nó trong Go?

Bộ nhớ không được giải phóng (do goroutine không kết thúc hoặc tham chiếu thừa). Ngăn chặn bằng cách kết thúc goroutine đúng lúc (dùng context, timeout).

### 84. Garbage collection hoạt động như thế nào trong Go?

Sử dụng thuật toán concurrent mark-and-sweep, chạy song song với ứng dụng.

### 85. Giải thích sự khác nhau giữa sync.Mutex và sync.RWMutex.

- `Mutex`: Chỉ cho phép 1 goroutine truy cập (bất kể đọc hay ghi).
- `RWMutex`: Cho phép nhiều goroutine đọc cùng lúc nhưng chỉ 1 goroutine ghi.

### 86. Race condition là gì và làm thế nào để phát hiện chúng trong Go?

Nhiều goroutine truy cập chung một biến mà không đồng bộ. Phát hiện bằng cờ `-race`.

### 87. Struct tag là gì và được sử dụng như thế nào?

Metadata gắn vào trường của struct, thường dùng để cấu hình mapping JSON/DB.

### 88. Làm thế nào để tạo một custom error trong Go?

Implement phương thức `Error() string` cho một struct.

### 89. Nil pointer dereference là gì và làm thế nào để tránh nó?

Truy cập vào con trỏ đang trỏ tới nil. Tránh bằng cách luôn kiểm tra `if p != nil`.

### 90. Explain the difference between sync.Pool and garbage collection.

`sync.Pool` giúp tái sử dụng object để giảm bớt số lần cấp phát bộ nhớ, từ đó giảm tải cho GC.

### 91. Làm thế nào để implement một worker pool trong Go?

Dùng channel để gửi công việc (jobs) và một nhóm các goroutine lắng nghe trên channel đó để xử lý.

### 92. reflect trong Go là gì?

Khả năng kiểm tra kiểu dữ liệu và giá trị tại thời điểm runtime.

### 93. Sự khác biệt giữa buffered và unbuffered channels là gì?

- Unbuffered: Chặn cho đến khi có người nhận.
- Buffered: Chỉ chặn khi buffer đầy.

### 94. Làm thế nào để tránh Goroutine leaks?

Luôn đảm bảo goroutine có điểm dừng (dùng context cancellation hoặc gửi tín hiệu qua channel).

### 95. Sự khác nhau chính giữa panic và error là gì?

- Error: Các lỗi bình thường có thể xử lý.
- Panic: Các lỗi nghiêm trọng khiến chương trình dừng lại.

### 96. Giải thích các interface io.Reader và io.Writer.

- `io.Reader`: Định nghĩa cách đọc dữ liệu vào một buffer byte.
- `io.Writer`: Định nghĩa cách ghi dữ liệu từ một buffer byte.

### 97. nil value interface là gì và tại sao nó có thể gây lỗi?

Mội interface gồm (type, value). Nếu value là nil nhưng type không nil thì interface đó KHÔNG bằng nil, có thể gây sai sót khi check `if i == nil`.

### 98. Làm thế nào để ngăn chặn deadlock?

- Luôn lock theo cùng một thứ tự.
- Dùng `defer Unlock()`.
- Tránh giữ lock khi thực hiện các hoạt động block (như call channel hoặc gọi hàm khác giữ cùng lock).

### 99. Làm thế nào để tối ưu hóa hiệu suất encode/decode JSON trong Go?

- Dùng thư viện nhanh hơn (`jsoniter`, `easyjson`).
- Dùng struct tags chính xác.
- Tái sử dụng encoder/decoder qua `sync.Pool`.

### 100. Sự khác biệt giữa GOMAXPROCS và runtime.Gosched() là gì?

- `GOMAXPROCS`: Thiết lập số lượng thread OS chạy code Go đồng thời.
- `runtime.Gosched()`: Nhường quyền điều khiển của goroutine hiện tại cho goroutine khác.
