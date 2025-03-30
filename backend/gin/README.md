# 📚 300 Câu Hỏi Phỏng Vấn Senior Backend Golang/Gin

## 📋 Mục Lục

- [Golang Cơ Bản](#golang-cơ-bản)
- [Golang Nâng Cao](#golang-nâng-cao)
- [Concurrency & Goroutines](#concurrency--goroutines)
- [Gin Framework](#gin-framework)
- [RESTful API & Microservices](#restful-api--microservices)
- [Cơ Sở Dữ Liệu & ORM](#cơ-sở-dữ-liệu--orm)
- [Testing & Debugging](#testing--debugging)
- [Performance & Optimization](#performance--optimization)
- [Design Patterns & Architecture](#design-patterns--architecture)
- [DevOps & Deployment](#devops--deployment)
- [Security](#security)

## Golang Cơ Bản

### 🔹 Kiến Thức Nền Tảng

## 1. 🤔 Giải thích các đặc điểm chính của ngôn ngữ Go?

Go (Golang) do Google thiết kế nhằm đơn giản, nhanh và hiệu quả.

- Đơn giản, dễ học, loại bỏ tính năng phức tạp.
- Biên dịch nhanh, không cần JVM hay interpreter.
- Hỗ trợ Goroutines (đa luồng nhẹ, nhanh hơn thread).
- Garbage Collector (GC) tự động quản lý bộ nhớ.
- Strongly typed, kiểm tra kiểu khi biên dịch.
- Hỗ trợ struct + method + interface thay vì OOP truyền thống.
- Interface linh hoạt, không cần khai báo explicit implementation.
- Goroutine + Channel giúp giao tiếp giữa các tiến trình an toàn.
- Quản lý dependencies bằng Go Modules.

## 2. 🔄 So sánh Go với các ngôn ngữ lập trình khác?

- **Go vs Java**: Go nhanh hơn do biên dịch thành binary code trực tiếp, không cần JVM.
- **Go vs Python**: Go nhanh hơn do biên dịch, Python linh hoạt hơn nhưng chạy chậm hơn.
- **Go vs Node.js**: Go mạnh về concurrency với Goroutines, Node.js dùng event loop.

## 3. 🛠️ Giải thích cách làm việc của GOPATH và GOROOT?

- **GOROOT**: Thư mục chứa bộ cài đặt Go.
- **GOPATH**: Thư mục chứa code và dependencies của dự án Go (không cần thiết khi dùng Go Modules).

## 4. 📚 Go Modules là gì?

Go Modules quản lý dependencies, giúp dự án không phụ thuộc vào GOPATH.

- **Khởi tạo module**: `go mod init example.com/mymodule`
- **Cài package**: `go get github.com/gin-gonic/gin`

## 5. 🔍 Sự khác biệt giữa `go get`, `go install`, và `go build`?

- **go get**: Tải package và thêm vào Go Modules.
- **go install**: Biên dịch và cài binary vào `$GOPATH/bin`.
- **go build**: Biên dịch chương trình nhưng không cài đặt.

## 6. 🧪 Workspace trong Go là gì?

Workspace là thư mục chứa dự án Go, cáu trúc thường gồm:

```
myproject/
  |-- go.mod
  |-- main.go
  |-- pkg/
  |-- internal/
```

## 7. 🔄 Vòng đời chương trình Go?

1. **Viết code** (`main.go`).
2. **Biên dịch** (`go build`).
3. **Chạy** (`./main`).

## 8. 🧬 Giải thích garbage collector trong Go?

Go GC tự động thu hồi bộ nhớ không còn dùng, giúm tránh memory leaks.

## 9. 💮 Các kiểu dữ liệu cơ bản trong Go?

- **Số**: int, float64.
- **Chuỗi**: string.
- **Boolean**: bool.
- **Collection**: array, slice, map.
- **Struct**: Như class nhưng không có kế thừa.

## 10. 🎭 Interface trong Go?

Interface là tập hợp các method, không cần explicit implementation.

```go
type Speaker interface {
    Speak()
}

type Dog struct {}

func (d Dog) Speak() {
    fmt.Println("Woof!")
}
```

### 🔹 Cú Pháp & Cấu Trúc

#### 11. 🔀 Sự khác biệt giữa `var` và `:=` khi khai báo biến?

- **`var` declaration**:

  - Có thể sử dụng ở cả package level và function level
  - Khai báo rõ ràng kiểu dữ liệu: `var name string = "Go"`
  - Nếu không khởi tạo, biến sẽ có zero value: `var count int` (count = 0)
  - Có thể khai báo nhiều biến cùng lúc: `var i, j int = 1, 2`

- **`:=` short declaration**:
  - Chỉ có thể sử dụng trong function, không dùng ở package level
  - Tự động suy luận kiểu dữ liệu: `name := "Go"`
  - Phải khởi tạo giá trị khi khai báo
  - Ngắn gọn hơn, thường dùng trong các function
  - Có thể khai báo nhiều biến: `i, j := 1, 2`

```go
var globalVar string = "Global" // Hợp lệ ở mức package

func example() {
    var localVar int = 10       // Cách 1
    shortVar := "Short form"    // Cách 2
}
```

#### 12. 🧠 Giải thích zero values trong Go?

Go tự động gán giá trị mặc định (zero value) cho biến khi khai báo mà không khởi tạo:

- **Số**:

  - `int`, `int8`, `int16`, `int32`, `int64`, `uint`, `uint8`, etc.: `0`
  - `float32`, `float64`: `0.0`
  - `complex64`, `complex128`: `0+0i`

- **Boolean**: `false`

- **String**: `""` (chuỗi rỗng)

- **Pointer types**: `nil`

- **Reference types**:

  - `slice`: `nil`
  - `map`: `nil`
  - `channel`: `nil`
  - `interface`: `nil`
  - `function`: `nil`

- **Structs**: Mỗi field trong struct có zero value tương ứng với kiểu dữ liệu

```go
var i int             // i = 0
var f float64         // f = 0.0
var b bool            // b = false
var s string          // s = ""
var p *int            // p = nil
var arr [3]int        // arr = [0, 0, 0]
var slice []int       // slice = nil
var m map[string]int  // m = nil
```

#### 13. 🔄 Pointers trong Go hoạt động như thế nào?

Pointers là biến chứa địa chỉ bộ nhớ của biến khác:

- **Khai báo pointer**: `var p *int`
- **Lấy địa chỉ của biến**: Sử dụng operator `&`
  - `x := 10`
  - `p := &x` (p giờ trỏ đến x)
- **Truy cập giá trị từ pointer**: Sử dụng operator `*` (dereferencing)
  - `*p = 20` (thay đổi giá trị của x thành 20)
- **Zero value** của pointer là `nil`

**Ví dụ**:

```go
func main() {
    x := 10
    p := &x         // p lưu địa chỉ của x

    fmt.Println(x)  // 10
    fmt.Println(p)  // 0xc000018030 (địa chỉ - sẽ khác trên máy của bạn)
    fmt.Println(*p) // 10 (giá trị tại địa chỉ p)

    *p = 20         // Thay đổi giá trị tại địa chỉ p
    fmt.Println(x)  // 20 (x đã bị thay đổi)
}
```

**Use cases**:

- Truyền biến tham chiếu thay vì copy (hiệu quả với dữ liệu lớn)
- Thay đổi biến trong các function khác
- Implement cấu trúc dữ liệu như linked list, tree

#### 14. 📊 Arrays, slices và maps: Sự khác biệt và use cases?

**1. Arrays**:

- Kích thước cố định, không thể thay đổi
- Kiểu dữ liệu bao gồm cả kích thước: `[5]int` khác với `[10]int`
- Khi truyền array vào function, Go tạo bản sao toàn bộ array (pass by value)
- Ít sử dụng trực tiếp trong Go

```go
var nums [5]int = [5]int{1, 2, 3, 4, 5}
// hoặc
nums := [5]int{1, 2, 3, 4, 5}
// hoặc
nums := [...]int{1, 2, 3, 4, 5} // Compiler tự đếm phần tử
```

**2. Slices**:

- Dynamic arrays - có thể thay đổi kích thước
- Là reference type (trỏ đến một array)
- Có 3 thành phần: pointer (đến array), length, capacity
- Phổ biến nhất trong các collection của Go

```go
// Tạo slice từ array
arr := [5]int{1, 2, 3, 4, 5}
slice := arr[1:4] // [2, 3, 4]

// Tạo slice trực tiếp
slice := []int{1, 2, 3, 4, 5}

// Tạo slice với make
slice := make([]int, 5)      // length=5, capacity=5
slice := make([]int, 3, 5)   // length=3, capacity=5

// Thêm phần tử
slice = append(slice, 6, 7, 8)
```

**3. Maps**:

- Cấu trúc key-value, tương tự hash table/dictionary
- Là reference type
- Keys phải cùng kiểu dữ liệu và comparable (có thể so sánh)
- Values có thể là bất kỳ kiểu nào

```go
// Khởi tạo map
m := map[string]int{
    "apple": 1,
    "banana": 2,
}

// Tạo map rỗng
m := make(map[string]int)

// Thêm hoặc cập nhật
m["orange"] = 3

// Kiểm tra key tồn tại
value, exists := m["apple"]

// Xóa key
delete(m, "apple")
```

**Use cases**:

- **Arrays**: Khi cần kích thước cố định, ít thay đổi
- **Slices**: Collection động, thêm/xóa phần tử thường xuyên
- **Maps**: Lookup dựa trên key, cấu trúc dữ liệu dictionary

#### 15. 🔄 Sự khác biệt giữa methods và functions trong Go?

**Functions**:

- Độc lập, không gắn với kiểu dữ liệu cụ thể
- Khai báo theo format: `func name(params) return_type`
- Có thể là standalone hoặc trong package

```go
func add(a, b int) int {
    return a + b
}
```

**Methods**:

- Gắn với một kiểu dữ liệu cụ thể (receiver type)
- Khai báo theo format: `func (receiver) name(params) return_type`
- Receiver có thể là value hoặc pointer
- Tạo object-oriented style với Go

```go
type Rectangle struct {
    width, height float64
}

// Value receiver - nhận bản sao của Rectangle
func (r Rectangle) Area() float64 {
    return r.width * r.height
}

// Pointer receiver - có thể thay đổi fields gốc
func (r *Rectangle) Scale(factor float64) {
    r.width *= factor
    r.height *= factor
}
```

**Khi nào dùng method?**

- Khi muốn implement behavior cho một kiểu dữ liệu
- Khi muốn implement interface
- Khi logic gắn với một kiểu dữ liệu cụ thể

**Khi nào dùng value vs pointer receiver?**

- **Value receiver**: Khi không cần thay đổi receiver
- **Pointer receiver**: Khi cần thay đổi receiver hoặc receiver có kích thước lớn

#### 16. 🔍 Exported và unexported identifiers trong Go?

Go sử dụng quy ước đặt tên để xác định visibility (accessibility) của identifiers (variables, types, functions, methods, fields):

**Exported identifiers (Public)**:

- Bắt đầu bằng chữ cái in hoa: `User`, `PrintData`, `Config`
- Có thể truy cập từ các package khác
- Phải luôn có documentation nếu là part của public API

```go
// Trong package "user"
type User struct {
    Name string        // Exported - accessible from other packages
    Email string       // Exported
    password string    // Unexported - chỉ truy cập trong package user
}

func NewUser() *User {}  // Exported function
func validateEmail() {}  // Unexported function
```

**Unexported identifiers (Private)**:

- Bắt đầu bằng chữ cái thường: `user`, `printData`, `config`
- Chỉ có thể truy cập trong cùng package
- Tạo "information hiding" và encapsulation

**Best practices**:

- Chỉ export những gì cần thiết cho API
- Giữ implementation details là unexported
- Sử dụng exported vs unexported để tạo clean API

```go
package database

// Client là exported type - có thể dùng bởi các package khác
type Client struct {
    connectionString string  // Unexported field
}

// NewClient là exported function - public API
func NewClient(conn string) *Client {
    validateConnection(conn)  // Private helper function
    return &Client{
        connectionString: conn,
    }
}

// Connect là public method
func (c *Client) Connect() error {
    return c.openConnection()  // Gọi private method
}

// openConnection là private method
func (c *Client) openConnection() error {
    // Implementation
}

// validateConnection là private function
func validateConnection(conn string) bool {
    // Validation logic
}
```

#### 17. 📚 Packages trong Go hoạt động như thế nào?

**Package** là cơ chế tổ chức và phân chia code trong Go:

**Cơ bản về packages**:

- Mỗi file `.go` phải khai báo package nó thuộc về
- Files cùng thư mục phải thuộc cùng package (ngoại trừ test files)
- Package name thường là tên thư mục (best practice)
- `main` package đặc biệt - dùng cho executable, phải có hàm `main()`

```go
// file: user/profile.go
package user

func GetProfile() {...}
```

**Import packages**:

- Sử dụng lệnh `import` để sử dụng code từ packages khác
- Có thể import nhiều packages
- Packages được tìm kiếm theo GOPATH hoặc Go Modules

```go
import (
    "fmt"                     // Standard library
    "github.com/user/project" // External package
    "myapp/models"            // Local package
)
```

**Package organization**:

- **Standard library**: `fmt`, `os`, `net/http`,...
- **External packages**: Từ Github hoặc package managers
- **Internal packages**: Code của riêng bạn

**Imports đặc biệt**:

- **Dot imports**: `import . "fmt"` (không khuyến khích)
- **Aliased imports**: `import myfmt "fmt"`
- **Blank imports**: `import _ "driver/mysql"` (chỉ chạy init())

**Initialization**:

- Hàm `init()` chạy khi package được import
- Có thể có nhiều hàm `init()` (chạy theo thứ tự xuất hiện)
- Package được init theo dependency graph

**Best practices**:

- Giữ package nhỏ, tập trung vào một nhiệm vụ
- Đặt tên package ngắn gọn, mô tả chức năng
- Chỉ export những gì cần thiết
- Tránh circular imports

#### 18. 💼 Giải thích cách closures hoạt động trong Go?

**Closure** là function có thể truy cập và thay đổi biến từ scope bên ngoài nó:

**Định nghĩa**:

- Function + environment nơi nó được tạo ra
- "Closes over" các biến bên ngoài (capture biến)
- Có thể truy cập và thay đổi giá trị biến đó ngay cả khi function original scope không còn tồn tại

**Ví dụ cơ bản**:

```go
func main() {
    x := 10

    // Closure captures variable x
    f := func() {
        fmt.Println(x) // Truy cập x từ scope bên ngoài
    }

    f() // In ra: 10
}
```

**Function trả về closure**:

```go
func createCounter() func() int {
    count := 0            // Biến này được capture bởi closure

    return func() int {
        count++           // Thay đổi biến đã capture
        return count
    }
}

func main() {
    counter := createCounter()
    fmt.Println(counter()) // 1
    fmt.Println(counter()) // 2
    fmt.Println(counter()) // 3

    // Mỗi lần gọi createCounter() tạo instance mới
    counter2 := createCounter()
    fmt.Println(counter2()) // 1
}
```

**Use cases**:

1. **Stateful functions**: Giữ state giữa các lần gọi
2. **Callbacks** và event handlers
3. **Decorators** và middleware
4. **Partial application** (currying)
5. **Generators**

**Lưu ý quan trọng**:

- Closure lưu tham chiếu đến biến, không phải giá trị tại thời điểm tạo
- Goroutines dùng closure phải cẩn thận về race conditions
- Capture loop variables cần cẩn trọng:

```go
// WRONG
for i := 0; i < 3; i++ {
    go func() {
        fmt.Println(i) // Có thể in ra 3, 3, 3
    }()
}

// CORRECT
for i := 0; i < 3; i++ {
    i := i // Shadow variable
    go func() {
        fmt.Println(i) // In ra 0, 1, 2
    }()
}

// HOẶC
for i := 0; i < 3; i++ {
    go func(val int) {
        fmt.Println(val) // In ra 0, 1, 2
    }(i)
}
```

#### 19. 🔄 Type assertion và type conversion trong Go khác nhau như thế nào?

**Type Conversion**:

- Chuyển đổi từ một kiểu dữ liệu sang kiểu khác
- Cần kiểu nguồn và đích phải compatible
- Sử dụng cú pháp `Type(value)`
- Hoạt động với kiểu cơ bản (non-interface)

```go
var i int = 42
var f float64 = float64(i)  // Convert int to float64

var b []byte = []byte("Hello")  // Convert string to []byte
var s string = string(b)        // Convert []byte to string

type MyInt int
var mi MyInt = MyInt(i)         // Convert int to MyInt
```

**Type Assertion**:

- Kiểm tra và truy xuất kiểu cụ thể từ interface value
- Chỉ áp dụng với interface types
- Sử dụng cú pháp `value.(Type)`
- Có thể gây panic nếu assertion không đúng

```go
var i interface{} = "hello"

// Single value form - có thể panic
s := i.(string)      // OK: s = "hello"
n := i.(int)         // PANIC: interface holds string, not int

// Two value form - an toàn hơn
s, ok := i.(string)  // s = "hello", ok = true
n, ok := i.(int)     // n = 0 (zero value), ok = false
```

**Type Switch**:

- Kiểm tra nhiều kiểu dữ liệu cùng lúc
- An toàn hơn multiple assertions

```go
var i interface{} = 42

switch v := i.(type) {
case int:
    fmt.Printf("Integer: %d\n", v)
case string:
    fmt.Printf("String: %s\n", v)
case bool:
    fmt.Printf("Boolean: %v\n", v)
default:
    fmt.Printf("Unknown type\n")
}
```

**So sánh**:

- **Type conversion**:
  - Dành cho biến thông thường, không phải interface
  - Tường minh, compiler kiểm tra tính hợp lệ
  - Không có kiểm tra runtime
- **Type assertion**:
  - Dành cho interface values
  - Kiểm tra và extract giá trị cụ thể từ interface
  - Có kiểm tra runtime, có thể dẫn đến panic

#### 20. 🏗️ Struct embedding trong Go là gì?

**Struct embedding** là cơ chế để "kế thừa" fields và methods trong Go:

**Cơ bản về struct embedding**:

- Không phải kế thừa truyền thống OOP
- Composition over inheritance
- Nhúng một struct vào struct khác không cần đặt tên field

```go
type Person struct {
    Name string
    Age  int
}

func (p Person) Greet() string {
    return fmt.Sprintf("Hi, I'm %s", p.Name)
}

type Employee struct {
    Person       // Embedded struct (không đặt tên field)
    JobTitle string
    Salary   float64
}
```

**Truy cập fields và methods**:

```go
func main() {
    emp := Employee{
        Person: Person{
            Name: "John",
            Age:  30,
        },
        JobTitle: "Developer",
        Salary:   75000,
    }

    // Truy cập fields của embedded struct
    fmt.Println(emp.Name)       // "John" (promoted field)
    fmt.Println(emp.Person.Name) // "John" (explicit)

    // Gọi methods của embedded struct
    fmt.Println(emp.Greet())    // "Hi, I'm John" (promoted method)
}
```

**Đặc điểm quan trọng**:

1. **Field promotion**: Fields và methods của embedded struct được "promoted" lên struct chính
2. **Method overriding**: Struct chính có thể override methods của embedded struct
3. **Multiple embedding**: Có thể embed nhiều structs
4. **Interface satisfaction**: Struct chính tự động thỏa mãn interface mà embedded struct thỏa mãn

**Method overriding**:

```go
func (e Employee) Greet() string {
    return fmt.Sprintf("Hi, I'm %s, I work as a %s", e.Name, e.JobTitle)
}
```

**Multiple embedding**:

```go
type Human struct {
    Name string
}

type Worker struct {
    JobTitle string
}

// Person có fields và methods của cả Human và Worker
type Person struct {
    Human
    Worker
    Age int
}
```

**Embedding interfaces**:

```go
type Reader interface {
    Read(p []byte) (n int, err error)
}

type Writer interface {
    Write(p []byte) (n int, err error)
}

// ReadWriter có methods của cả Reader và Writer
type ReadWriter interface {
    Reader
    Writer
}
```

**Best practices**:

- Dùng embedding thay cho struct fields khi muốn "is-a" relationship
- Khi có name conflicts, phải gọi tường minh
- Không lạm dụng multiple embedding (dễ gây confusion)
- Cẩn thận với cyclic embedding

### 🔹 Xử Lý Lỗi & Exception

## 21. 🐞 Go xử lý lỗi khác với các ngôn ngữ khác như thế nào?

Go không sử dụng cơ chế try-catch như các ngôn ngữ khác (Java, C#, Python). Thay vào đó, Go coi lỗi là một giá trị trả về thông thường (`error` type) và khuyến khích xử lý lỗi một cách rõ ràng tại nơi chúng xảy ra. Điều này giúp code dễ đọc, dễ debug và tránh việc lỗi bị ẩn đi như trong các ngôn ngữ sử dụng exception.

## 22. 🔄 Giải thích pattern returning errors trong Go?

Trong Go, các hàm thường trả về một cặp giá trị: kết quả và lỗi (`result, error`). Pattern này yêu cầu người dùng kiểm tra lỗi trước khi sử dụng kết quả. Ví dụ:

```go
func divide(a, b int) (int, error) {
    if b == 0 {
        return 0, errors.New("division by zero")
    }
    return a / b, nil
}
```

Người gọi hàm phải kiểm tra `err != nil` để xử lý lỗi trước khi dùng kết quả.

## 23. 🚨 Cách tốt nhất để xử lý lỗi trong Go?

Cách tốt nhất là:

- Kiểm tra lỗi ngay sau khi hàm trả về (`if err != nil`).
- Xử lý lỗi cụ thể thay vì bỏ qua (`_`) hoặc panic ngay lập tức.
- Truyền lỗi lên cấp cao hơn nếu không thể xử lý tại chỗ.
- Sử dụng wrapping error (Go 1.13+) để cung cấp ngữ cảnh bổ sung.

Ví dụ:

```go
if err != nil {
    return fmt.Errorf("something failed: %w", err)
}
```

## 24. 📋 Giải thích về `defer`, `panic`, và `recover` trong Go?

- **`defer`**: Hoãn thực thi một hàm cho đến khi hàm chứa nó kết thúc. Thường dùng để dọn dẹp tài nguyên (đóng file, unlock mutex).
  ```go
  defer file.Close()
  ```
- **`panic`**: Gây ra lỗi nghiêm trọng, dừng chương trình ngay lập tức trừ khi được xử lý bởi `recover`. Dùng cho các tình huống bất khả thi.
  ```go
  panic("unrecoverable error")
  ```
- **`recover`**: Bắt và xử lý `panic`, ngăn chương trình crash. Chỉ hoạt động trong `defer`.
  ```go
  func safe() {
      defer func() {
          if r := recover(); r != nil {
              fmt.Println("Recovered:", r)
          }
      }()
      panic("test panic")
  }
  ```

## 25. 🔄 Custom error types trong Go được implement như thế nào?

Custom error được tạo bằng cách implement interface `error` (có phương thức `Error() string`):

```go
type MyError struct {
    Msg string
    Code int
}

func (e *MyError) Error() string {
    return fmt.Sprintf("error %d: %s", e.Code, e.Msg)
}
```

Sử dụng:

```go
err := &MyError{Msg: "not found", Code: 404}
```

## 26. 🔍 Cách wrap và unwrap errors trong Go 1.13+?

- **Wrap**: Dùng `fmt.Errorf` với `%w` để bọc lỗi, thêm ngữ cảnh:
  ```go
  err := fmt.Errorf("failed to open file: %w", os.ErrNotExist)
  ```
- **Unwrap**: Dùng `errors.Unwrap()` để lấy lỗi gốc hoặc `errors.Is()`/`errors.As()` để kiểm tra loại lỗi:
  ```go
  if errors.Is(err, os.ErrNotExist) {
      fmt.Println("file does not exist")
  }
  ```

## 27. 🧠 Giải thích về Sentinel errors và khi nào nên sử dụng chúng?

Sentinel errors là các giá trị lỗi cố định (như `io.EOF`, `sql.ErrNoRows`) dùng để báo hiệu một trạng thái cụ thể. Nên sử dụng khi:

- Lỗi đại diện cho một điều kiện chung, tái sử dụng được.
- Cần so sánh trực tiếp bằng `==` thay vì `errors.Is()`.

Ví dụ:

```go
var ErrNotFound = errors.New("not found")
if err == ErrNotFound {
    // Xử lý
}
```

Hạn chế dùng quá nhiều để tránh phụ thuộc chặt chẽ vào giá trị cụ thể.

## 28. 🤔 So sánh việc sử dụng error codes và error types?

- **Error Codes**: Là các giá trị số (hoặc hằng số) đại diện cho lỗi. Nhẹ, dễ so sánh nhưng thiếu ngữ cảnh và không linh hoạt.
  ```go
  const ErrInvalidInput = 1
  ```
- **Error Types**: Là struct implement `error`. Linh hoạt, chứa thêm thông tin (mã, message), nhưng phức tạp hơn.
  ```go
  type ValidationError struct { Field string }
  ```
  Dùng error codes cho hệ thống đơn giản, error types cho hệ thống phức tạp cần chi tiết.

## 29. 🔄 Cách xử lý lỗi khi làm việc với goroutines?

- Truyền lỗi qua channel để đồng bộ hóa:
  ```go
  errCh := make(chan error)
  go func() {
      errCh <- someFunc()
  }()
  if err := <-errCh; err != nil {
      // Xử lý
  }
  ```
- Hoặc dùng `sync.WaitGroup` kết hợp với một cơ chế tập hợp lỗi.

## 30. 🚨 Cách sử dụng `errgroup` package để xử lý lỗi trong concurrent code?

Package `golang.org/x/sync/errgroup` giúp quản lý lỗi trong goroutines:

```go
import "golang.org/x/sync/errgroup"

func main() {
    var g errgroup.Group
    g.Go(func() error {
        return errors.New("task 1 failed")
    })
    g.Go(func() error {
        return nil
    })
    if err := g.Wait(); err != nil {
        fmt.Println("Error:", err)
    }
}
```

- `g.Go()` chạy goroutine và trả về lỗi.
- `g.Wait()` chờ tất cả goroutines hoàn thành và trả về lỗi đầu tiên (nếu có).

# Golang Nâng Cao

## 🔹 Memory Management

### 31. 🧠 Cách Go quản lý memory và cấp phát?

Go sử dụng **garbage collector (GC)** để quản lý bộ nhớ. Bộ nhớ được cấp phát tự động khi tạo biến hoặc đối tượng (thông qua `new` hoặc `make`). GC của Go là **concurrent mark-and-sweep**, chạy song song với chương trình, giảm thời gian dừng (stop-the-world). Bộ nhớ được cấp phát trên **stack** (cho các biến cục bộ, nhỏ) hoặc **heap** (cho dữ liệu thoát ra ngoài phạm vi hàm).

### 32. 🔄 Escaping to heap: khi nào và tại sao điều này xảy ra?

"Escaping to heap" xảy ra khi một biến cục bộ được tham chiếu ngoài phạm vi hàm của nó (ví dụ: trả về con trỏ, lưu vào global variable, hoặc truyền vào goroutine). Trình biên dịch Go phân tích **escape analysis** để quyết định cấp phát trên heap thay vì stack nhằm đảm bảo an toàn bộ nhớ.

Ví dụ:

```go
func escape() *int {
    x := 42
    return &x // x "escapes" to heap
}
```

### 33. 🚨 Memory leaks phổ biến trong Go và cách tránh?

- **Goroutines bị treo**: Goroutine không kết thúc do channel bị chặn. Tránh bằng cách dùng context hoặc timeout.
- **Slice giữ tham chiếu**: Slice lớn bị cắt nhưng vẫn giữ tham chiếu đến array gốc. Dùng `copy` để tạo bản sao.
- **Map không được dọn dẹp**: Map tăng kích thước vô hạn. Xóa key không cần thiết bằng `delete`.
- **Listener không hủy**: Đăng ký listener mà không hủy. Dùng `defer` để unregister.

### 34. 🧰 Làm thế nào để phân tích memory usage của ứng dụng Go?

Sử dụng công cụ:

- **`runtime/pprof`**: Thu thập dữ liệu heap, phân tích qua `go tool pprof`.
- **`runtime.MemStats`**: Cung cấp thông tin runtime về memory (alloc, heap, GC cycles).
- **Tools bên thứ ba**: 如 `go-memviz` hoặc `pprof` visualizations.

Ví dụ:

```go
import "runtime/pprof"
pprof.WriteHeapProfile(file)
```

### 35. 🤔 Stack vs Heap allocation trong Go: làm thế nào trình biên dịch quyết định?

Trình biên dịch dùng **escape analysis**:

- **Stack**: Biến cục bộ, phạm vi rõ ràng, không thoát ra ngoài (nhanh, tự động giải phóng).
- **Heap**: Biến thoát ra ngoài phạm vi (trả về con trỏ, lưu global), được GC quản lý.

Ví dụ:

```go
func stack() int { x := 42; return x } // Stack
func heap() *int { x := 42; return &x } // Heap
```

### 36. 🔍 Cách sử dụng `runtime/pprof` để profiling memory?

- Thêm code để ghi profile:

```go
import "runtime/pprof"
f, _ := os.Create("mem.prof")
pprof.WriteHeapProfile(f)
f.Close()
```

- Chạy chương trình, sau đó phân tích:

```bash
go tool pprof mem.prof
```

- Dùng lệnh `top`, `list`, hoặc `web` để xem chi tiết.

### 37. 🚨 Vấn đề với việc sử dụng quá nhiều global variables?

- **Memory leaks**: Global variables không được GC giải phóng nếu không đặt lại giá trị.
- **Race conditions**: Dễ xảy ra khi nhiều goroutines truy cập cùng lúc.
- **Khó debug**: Tăng độ phức tạp và khó dự đoán trạng thái chương trình.

### 38. 🔄 GC tuning trong Go: options và best practices?

- **`GOGC`**: Điều chỉnh tần suất GC (mặc định 100, tăng để giảm GC, giảm để tiết kiệm RAM).
- **`runtime.GC()`**: Gọi GC thủ công (hiếm dùng).
- **Best practices**: Tránh cấp phát heap không cần thiết, tối ưu struct alignment, dùng pool (như `sync.Pool`).

Ví dụ:

```bash
GOGC=200 go run main.go
```

### 39. 🧠 Giải thích về memory alignment và padding trong structs?

**Memory alignment** đảm bảo các trường trong struct được căn chỉnh theo kích thước CPU (thường 8 bytes trên 64-bit). **Padding** là khoảng trống chèn vào để căn chỉnh, tránh truy cập không hiệu quả.

Ví dụ:

```go
type S struct {
    a byte // 1 byte
    b int  // 8 bytes, 7 bytes padding trước b
}
```

### 40. 🔍 Làm thế nào để tối ưu hóa memory footprint của một ứng dụng Go?

- Sử dụng kiểu dữ liệu nhỏ hơn (int32 thay vì int64 nếu đủ).
- Tái cấu trúc struct để giảm padding.
- Dùng `sync.Pool` cho đối tượng tái sử dụng.
- Tránh escaping không cần thiết qua escape analysis.

## 🔹 Reflection & Code Generation

### 41. 🧠 Reflection trong Go là gì và khi nào nên sử dụng?

Reflection cho phép kiểm tra và thay đổi dữ liệu runtime (kiểu, giá trị) mà không biết trước. Dùng khi:

- Xử lý dữ liệu động (JSON, database).
- Cần tính linh hoạt (frameworks, plugins).

### 42. 🔄 Giải thích cách sử dụng `reflect` package?

```go
import "reflect"
v := reflect.ValueOf(42)
t := reflect.TypeOf(42)
fmt.Println(v.Int(), t.Name()) // 42, int
```

### 43. 🔍 Performance implications của reflection?

Reflection chậm hơn code tĩnh vì:

- Truy cập runtime metadata.
- Không được tối ưu hóa bởi trình biên dịch.
  Dùng ít, hoặc thay bằng code generation nếu hiệu năng quan trọng.

### 44. 🧰 Go generate: mục đích và cách sử dụng?

`go generate` chạy lệnh tạo code trước khi biên dịch. Dùng cho:

- Tạo serializer, mock, bindings.
  Ví dụ:

```go
//go:generate stringer -type=Enum
type Enum int
```

### 45. 🔄 Cách implement các bộ serializers hiệu quả bằng reflection?

Dùng `reflect` để duyệt struct, ánh xạ field sang JSON:

```go
func serialize(v interface{}) string {
    rv := reflect.ValueOf(v)
    // Logic ánh xạ field
}
```

### 46. 🛠️ Khi nào nên sử dụng code generation thay vì reflection?

- Khi cần hiệu năng cao.
- Khi logic cố định, không cần runtime flexibility.

### 47. 🧩 Giải thích cách sử dụng tags trong struct?

Tags gắn metadata cho field, dùng với reflection:

```go
type User struct {
    Name string `json:"name"`
}
```

### 48. 🔍 Type switches so với reflection: trade-offs?

- **Type switch**: Nhanh, tĩnh, giới hạn số kiểu.
- **Reflection**: Linh hoạt, chậm, xử lý kiểu động.

### 49. 🔄 Cách implement generic-like behavior trong Go 1.17 và trước đó?

Dùng `interface{}` hoặc code generation:

```go
func Print[T any](v T) { fmt.Println(v) } // Trước 1.18: interface{}
```

### 50. 🧠 Generics trong Go 1.18+: thiết kế, giới hạn, và use cases?

- **Thiết kế**: Dùng `type parameters` (`[T any]`).
- **Giới hạn**: Không hỗ trợ method constraints phức tạp.
- **Use cases**: Collections, algorithms chung.

## 🔹 Context & Cancellation

### 51. 🧠 Context trong Go: mục đích và cách sử dụng?

`context` quản lý vòng đời request, hủy bỏ, timeout:

```go
ctx, cancel := context.WithTimeout(context.Background(), time.Second)
defer cancel()
```

### 52. 🔄 Cách truyền context qua call chain?

Truyền làm tham số đầu tiên:

```go
func doWork(ctx context.Context) error { ... }
```

### 53. 🚨 Context cancellation: patterns và best practices?

- Gọi `cancel()` qua `defer`.
- Kiểm tra `ctx.Done()` trong vòng lặp.

### 54. 🔍 Sự khác biệt giữa `context.WithCancel`, `context.WithTimeout`, và `context.WithDeadline`?

- `WithCancel`: Hủy thủ công.
- `WithTimeout`: Hủy sau khoảng thời gian.
- `WithDeadline`: Hủy tại thời điểm cụ thể.

### 55. 🧰 Giải thích cách implement graceful shutdown với contexts?

```go
ctx, cancel := context.WithCancel(context.Background())
go worker(ctx)
<-sigChan // Nhận tín hiệu
cancel()
```

### 56. 🤔 Những anti-patterns phổ biến khi sử dụng contexts?

- Lưu context trong struct.
- Dùng context values cho dữ liệu chính.

### 57. 🔄 Context values: khi nào nên sử dụng và khi nào nên tránh?

- **Dùng**: Metadata (request ID).
- **Tránh**: Dữ liệu logic chính (dùng tham số thay thế).

### 58. 🔍 Làm thế nào để thêm request-scoped data vào một context?

```go
ctx = context.WithValue(ctx, "userID", 123)
```

### 59. 🚨 Race conditions khi làm việc với contexts và cách tránh?

- Tránh đọc/ghi context values từ nhiều goroutines. Dùng mutex nếu cần.

### 60. 🧠 Tracing và request ID propagation với context values?

```go
ctx = context.WithValue(ctx, "requestID", uuid.New())
```

Truyền qua call chain để trace.

# Concurrency & Goroutines

## 🔹 Goroutines & Channels

### 61. 🧠 Goroutines là gì và chúng khác với threads như thế nào?

Goroutines là các đơn vị thực thi nhẹ của Go, được quản lý bởi runtime thay vì OS. Khác với threads:

- **Nhẹ hơn**: Goroutines chỉ chiếm vài KB stack (so với MB cho threads).
- **Quản lý bởi runtime**: Go scheduler xử lý multiplexing goroutines lên threads.
- **Chi phí thấp**: Tạo hàng nghìn goroutines dễ dàng, trong khi threads bị giới hạn bởi OS.

### 62. 🔄 Channels trong Go: cách hoạt động và use cases?

Channels là cơ chế giao tiếp giữa goroutines, hoạt động như ống dẫn dữ liệu:

- **Gửi/nhận**: `ch <- data` (gửi), `data := <-ch` (nhận).
- **Use cases**: Đồng bộ hóa, truyền dữ liệu, worker pools.
  Ví dụ:

```go
ch := make(chan int)
go func() { ch <- 42 }()
fmt.Println(<-ch)
```

### 63. 🚨 Buffered vs unbuffered channels: sự khác biệt và khi nào sử dụng?

- **Unbuffered**: Đồng bộ, chặn cho đến khi cả gửi và nhận sẵn sàng. Dùng khi cần phối hợp chặt chẽ.
- **Buffered**: Không đồng bộ, có dung lượng (buffer), không chặn nếu buffer chưa đầy. Dùng khi cần xử lý hàng đợi.
  Ví dụ:

```go
ch := make(chan int, 2) // Buffered
ch <- 1; ch <- 2 // Không chặn
```

### 64. 🔍 Cách xử lý nhiều channels với `select` statement?

`select` chọn giữa nhiều channel operations:

```go
select {
case v := <-ch1:
    fmt.Println(v)
case ch2 <- 42:
    fmt.Println("Sent")
default:
    fmt.Println("No action")
}
```

### 65. 🧰 Fan-in và fan-out patterns với goroutines?

- **Fan-out**: Phân chia công việc cho nhiều goroutines.
- **Fan-in**: Gom kết quả từ nhiều goroutines vào một channel.
  Ví dụ:

```go
func fanOut(in <-chan int, outs []chan int) {
    for v := range in {
        outs[v%len(outs)] <- v
    }
}
```

### 66. 🔄 Cách implement worker pools trong Go?

Tạo một nhóm goroutines xử lý công việc từ channel:

```go
func worker(id int, jobs <-chan int) {
    for j := range jobs {
        fmt.Printf("Worker %d processing %d\n", id, j)
    }
}

jobs := make(chan int, 100)
for w := 1; w <= 3; w++ {
    go worker(w, jobs)
}
```

### 67. 🔍 Cách detect và tránh goroutine leaks?

- **Detect**: Dùng `runtime.NumGoroutine()` hoặc `pprof`.
- **Tránh**: Đảm bảo goroutines kết thúc (đóng channel, dùng context).
  Ví dụ:

```go
ctx, cancel := context.WithCancel(context.Background())
go func() { <-ctx.Done() }()
cancel()
```

### 68. 🚨 Làm thế nào để kiểm soát số lượng goroutines trong ứng dụng?

- Dùng semaphore hoặc worker pool với số lượng cố định.
  Ví dụ:

```go
var sem = make(chan struct{}, 10) // Giới hạn 10 goroutines
go func() {
    sem <- struct{}{} // Acquire
    defer func() { <-sem }() // Release
}()
```

### 69. 🧠 Giải thích `sync.WaitGroup` và cách sử dụng?

`sync.WaitGroup` chờ nhiều goroutines hoàn thành:

```go
var wg sync.WaitGroup
wg.Add(2)
go func() { defer wg.Done(); fmt.Println("1") }()
go func() { defer wg.Done(); fmt.Println("2") }()
wg.Wait()
```

### 70. 🔄 Channel closing: best practices và patterns?

- Đóng channel khi không còn dữ liệu gửi.
- Chỉ người gửi đóng channel, người nhận kiểm tra bằng `v, ok := <-ch`.
  Ví dụ:

```go
ch := make(chan int)
go func() { ch <- 1; close(ch) }()
v, ok := <-ch // ok = false khi channel đóng
```

## 🔹 Đồng Bộ Hóa & Data Races

### 71. 🔍 Data races trong Go là gì và cách phát hiện?

Data race xảy ra khi nhiều goroutines truy cập cùng biến mà không đồng bộ, ít nhất một lần là ghi. Phát hiện bằng **race detector**:

```bash
go run -race main.go
```

### 72. 🚨 Các công cụ đồng bộ hóa trong package `sync`?

- `sync.Mutex`: Khóa độc quyền.
- `sync.RWMutex`: Khóa đọc/ghi.
- `sync.WaitGroup`: Chờ goroutines.
- `sync.Once`: Thực thi một lần.
- `sync.Pool`: Pool tái sử dụng.
- `sync.Cond`: Điều kiện chờ.

### 73. 🧠 Sự khác biệt giữa `sync.Mutex` và `sync.RWMutex`?

- **`sync.Mutex`**: Khóa toàn bộ (đọc/ghi).
- **`sync.RWMutex`**: Cho phép nhiều đọc cùng lúc, nhưng khóa ghi độc quyền. Dùng khi đọc nhiều hơn ghi.

### 74. 🔄 Giải thích `sync.Once` và use cases?

`sync.Once` đảm bảo một hàm chỉ chạy một lần:

```go
var once sync.Once
once.Do(func() { fmt.Println("Init") })
```

Use case: Khởi tạo singleton.

### 75. 🔍 Cách sử dụng `sync.Pool` để tái sử dụng objects?

```go
var pool = sync.Pool{New: func() interface{} { return &bytes.Buffer{} }}
buf := pool.Get().(*bytes.Buffer)
defer pool.Put(buf)
```

Dùng để giảm cấp phát heap.

### 76. 🧰 Sự khác biệt giữa atomic operations và mutexes?

- **Atomic**: Nhanh, thao tác đơn giản (như `atomic.AddInt32`).
- **Mutex**: Linh hoạt, bảo vệ khối code phức tạp.

### 77. 🤔 Deadlocks trong Go: nguyên nhân và cách tránh?

- **Nguyên nhân**: Goroutines khóa lẫn nhau (circular wait).
- **Tránh**: Sử dụng thứ tự khóa cố định, timeout, hoặc channel thay mutex.

### 78. 🚨 Race detector trong Go hoạt động như thế nào?

Race detector theo dõi truy cập bộ nhớ, báo lỗi nếu phát hiện xung đột. Bật bằng `-race`.

### 79. 🔄 Cách sử dụng `sync.Cond` cho coordination?

`sync.Cond` báo hiệu khi điều kiện thỏa:

```go
var mu sync.Mutex
cond := sync.NewCond(&mu)
go func() { mu.Lock(); cond.Wait(); fmt.Println("Go"); mu.Unlock() }()
cond.Signal()
```

### 80. 🧠 Memory barriers và memory ordering trong Go?

Go đảm bảo **sequential consistency** trong goroutine, dùng atomic/channel/mutex làm memory barrier để đồng bộ hóa giữa các goroutines.

## 🔹 Concurrency Patterns

### 81. 🧠 Generator pattern trong Go?

Tạo channel trả về dữ liệu tuần tự:

```go
func gen() <-chan int {
    ch := make(chan int)
    go func() { for i := 0; i < 5; i++ { ch <- i }; close(ch) }()
    return ch
}
```

### 82. 🔄 Pipeline pattern: thiết kế và implementation?

Xử lý dữ liệu qua các giai đoạn:

```go
func stage1(in <-chan int) <-chan int {
    out := make(chan int)
    go func() { for v := range in { out <- v * 2 }; close(out) }()
    return out
}
```

### 83. 🚨 Error handling trong concurrent code?

Dùng `errgroup` hoặc channel lỗi:

```go
var g errgroup.Group
g.Go(func() error { return errors.New("fail") })
if err := g.Wait(); err != nil { fmt.Println(err) }
```

### 84. 🔍 Cách implement fan-out, fan-in pattern?

Xem câu 65.

### 85. 🧰 Timeouts và cancellation trong concurrent operations?

Dùng context:

```go
ctx, cancel := context.WithTimeout(context.Background(), time.Second)
go func() { <-ctx.Done() }()
cancel()
```

### 86. 🔄 Semaphore pattern trong Go?

Xem câu 68.

### 87. 🚨 Context propagation trong concurrent operations?

Truyền context qua goroutines (xem câu 52).

### 88. 🤔 Rate limiting strategies trong Go?

Dùng `time.Ticker` hoặc `golang.org/x/time/rate`:

```go
limiter := rate.NewLimiter(1, 5) // 1 req/s, burst 5
limiter.Wait(context.Background())
```

### 89. 🔍 Pub-sub pattern implementation?

Dùng map[channel] để phát tín hiệu:

```go
type PubSub struct {
    subs map[chan string]struct{}
}
func (ps *PubSub) Publish(msg string) {
    for ch := range ps.subs { ch <- msg }
}
```

### 90. 🧠 Cách implement job queue với priority?

Dùng heap hoặc channel ưu tiên:

```go
type Job struct { Priority int }
jobs := make(chan Job)
go func() { heap.Push(&pq, Job{Priority: 1}) }()
```

# Gin Framework - Gin Basics

## 🔹 Gin Basics

### 91. 🧠 Gin là gì và tại sao nó phổ biến trong Go ecosystem?

Gin là một web framework nhẹ và hiệu năng cao được viết bằng Go, chủ yếu dùng để xây dựng RESTful API. Nó phổ biến trong Go ecosystem vì:

- **Hiệu năng**: Sử dụng router tối ưu hóa, nhanh hơn thư viện chuẩn (`http.ServeMux`).
- **Dễ sử dụng**: Cung cấp API trực quan, dễ học, phù hợp cho cả người mới và lập trình viên kỳ cựu.
- **Middleware**: Hỗ trợ hệ thống middleware linh hoạt, giúp mở rộng chức năng dễ dàng.
- **Cộng đồng mạnh mẽ**: Có tài liệu phong phú, nhiều package hỗ trợ và cộng đồng tích cực.

### 92. 🔄 Cài đặt và cấu trúc cơ bản của một ứng dụng Gin?

**Cài đặt**:

```bash
go get -u github.com/gin-gonic/gin
```

**Cấu trúc cơ bản**:

```go
package main

import "github.com/gin-gonic/gin"

func main() {
    // Khởi tạo Gin router với middleware mặc định (Logger, Recovery)
    r := gin.Default()

    // Định nghĩa route đơn giản
    r.GET("/ping", func(c *gin.Context) {
        c.JSON(200, gin.H{
            "message": "pong",
        })
    })

    // Chạy server trên port 8080
    r.Run(":8080")
}
```

### 93. 🔍 Routing trong Gin: patterns và best practices?

- **Patterns**:
  - **Static**: `/users`.
  - **Parameterized**: `/users/:id`.
  - **Wildcard**: `/files/*filepath`.
- **Best practices**:
  - Sử dụng `r.Group()` để nhóm các route liên quan (ví dụ: `/v1/users`).
  - Tránh lồng route quá sâu để giữ code dễ đọc.
  - Dùng prefix cho versioning (như `/v1/`).
    Ví dụ:

```go
v1 := r.Group("/v1")
v1.GET("/users/:id", getUserHandler)
```

### 94. 🚨 Middleware trong Gin: cách hoạt động và use cases?

- **Cách hoạt động**: Middleware là các hàm được thực thi trước/sau handler chính, theo chuỗi (chain). Gọi `c.Next()` để chuyển tiếp.
- **Use cases**:
  - Logging: Ghi log request.
  - Authentication: Kiểm tra token.
  - Rate limiting: Giới hạn số request.
    Ví dụ:

```go
r.Use(gin.Logger()) // Middleware mặc định ghi log
```

### 95. 🧰 Request và response handling trong Gin?

- **Request**: Lấy dữ liệu từ client qua `c.Bind()`, `c.Param()`, `c.Query()`.
- **Response**: Trả về JSON, string, hoặc status code qua `c.JSON()`, `c.String()`.
  Ví dụ:

```go
r.GET("/user", func(c *gin.Context) {
    c.JSON(200, gin.H{"name": "John"}) // Response
})
r.POST("/user", func(c *gin.Context) {
    var user struct { Name string }
    c.BindJSON(&user) // Request
})
```

### 96. 🔄 Cách xử lý parameters (path, query, form) trong Gin?

- **Path parameters**: `c.Param("key")`.
- **Query parameters**: `c.Query("key")` hoặc `c.DefaultQuery("key", "default")`.
- **Form parameters**: `c.PostForm("key")`.
  Ví dụ:

```go
r.GET("/users/:id", func(c *gin.Context) {
    id := c.Param("id")
    name := c.Query("name")
    c.JSON(200, gin.H{"id": id, "name": name})
})
```

### 97. 🚨 Giải thích về Gin Context và lifecycle?

- **`gin.Context`**: Đối tượng chứa thông tin request (header, body, params) và response (status, data). Được truyền qua middleware và handler.
- **Lifecycle**:
  1. Server nhận request.
  2. Middleware xử lý (logging, auth, etc.).
  3. Handler chính thực thi.
  4. Response được gửi về client.
     Ví dụ:

```go
func handler(c *gin.Context) {
    c.JSON(200, gin.H{"key": c.Request.Header.Get("User-Agent")})
}
```

### 98. 🤔 So sánh Gin với các frameworks Go khác (Echo, Fiber, etc.)?

- **Gin**: Nhanh, dễ dùng, dựa trên stdlib, cộng đồng lớn.
- **Echo**: Nhẹ hơn, ít phụ thuộc, API tương tự Gin, phù hợp dự án nhỏ.
- **Fiber**: Dùng `fasthttp` thay stdlib, nhanh hơn nhưng không hoàn toàn tương thích Go chuẩn, ít linh hoạt hơn với middleware.

### 99. 🔍 Performance considerations khi sử dụng Gin?

- **Release mode**: Chạy `gin.SetMode(gin.ReleaseMode)` để tắt debug logs.
- **Middleware**: Tránh dùng quá nhiều middleware không cần thiết.
- **JSON**: Tối ưu marshaling bằng cách dùng struct thay `gin.H` khi có thể.
- **Profiling**: Dùng `pprof` để tìm bottleneck.

### 100. 🧠 Cấu trúc dự án phổ biến với Gin?

```
project/
├── main.go              # Entry point
├── handlers/           # Xử lý logic route
│   └── user.go
├── models/            # Định nghĩa struct
│   └── user.go
├── routes/            # Định nghĩa route
│   └── routes.go
├── middleware/        # Custom middleware
│   └── auth.go
└── config/            # Cấu hình (DB, env)
    └── config.go
```

Ví dụ `main.go`:

```go
package main
import "project/routes"
func main() {
    r := gin.Default()
    routes.SetupRoutes(r)
    r.Run(":8080")
}
```

# Gin Framework - Middleware & Authentication

## 🔹 Middleware & Authentication

### 101. 🔄 Cách viết custom middleware trong Gin?

Custom middleware là một hàm trả về `gin.HandlerFunc`, được thêm vào chuỗi xử lý bằng `r.Use()`:

```go
func CustomMiddleware() gin.HandlerFunc {
    return func(c *gin.Context) {
        // Trước khi xử lý request
        fmt.Println("Before request")
        c.Next() // Gọi handler tiếp theo
        // Sau khi xử lý request
        fmt.Println("After request")
    }
}

func main() {
    r := gin.Default()
    r.Use(CustomMiddleware())
    r.GET("/", func(c *gin.Context) { c.String(200, "Hello") })
    r.Run()
}
```

### 102. 🚨 JWT authentication implementation trong Gin?

Sử dụng package `github.com/golang-jwt/jwt`:

```go
import "github.com/golang-jwt/jwt/v4"

func AuthMiddleware() gin.HandlerFunc {
    return func(c *gin.Context) {
        tokenStr := c.GetHeader("Authorization")
        token, err := jwt.Parse(tokenStr, func(token *jwt.Token) (interface{}, error) {
            return []byte("secret"), nil // Khóa bí mật
        })
        if err != nil || !token.Valid {
            c.AbortWithStatusJSON(401, gin.H{"error": "Unauthorized"})
            return
        }
        c.Next()
    }
}

r.Use(AuthMiddleware())
r.GET("/protected", func(c *gin.Context) { c.JSON(200, gin.H{"msg": "Protected"}) })
```

### 103. 🔍 CORS middleware configuration?

Sử dụng `github.com/gin-contrib/cors`:

```go
import "github.com/gin-contrib/cors"

func main() {
    r := gin.Default()
    r.Use(cors.New(cors.Config{
        AllowOrigins:     []string{"http://localhost:3000"},
        AllowMethods:     []string{"GET", "POST", "PUT"},
        AllowHeaders:     []string{"Origin", "Content-Type"},
        ExposeHeaders:    []string{"Content-Length"},
        AllowCredentials: true,
        MaxAge:           12 * time.Hour,
    }))
    r.GET("/api", func(c *gin.Context) { c.JSON(200, gin.H{"data": "CORS enabled"}) })
    r.Run()
}
```

### 104. 🧰 Rate limiting middleware trong Gin?

Sử dụng `golang.org/x/time/rate` hoặc custom:

```go
func RateLimitMiddleware() gin.HandlerFunc {
    limiter := rate.NewLimiter(1, 5) // 1 req/s, burst 5
    return func(c *gin.Context) {
        if !limiter.Allow() {
            c.AbortWithStatusJSON(429, gin.H{"error": "Too Many Requests"})
            return
        }
        c.Next()
    }
}

r.Use(RateLimitMiddleware())
```

### 105. 🤔 Logging và monitoring middleware?

Sử dụng middleware mặc định hoặc custom:

```go
r.Use(gin.LoggerWithFormatter(func(param gin.LogFormatterParams) string {
    return fmt.Sprintf("%s - [%s] %s %s %d\n",
        param.ClientIP,
        param.TimeStamp.Format(time.RFC1123),
        param.Method,
        param.Path,
        param.StatusCode,
    )
}))
// Hoặc dùng package như logrus để monitoring chi tiết
```

### 106. 🔄 Error handling middleware?

```go
func ErrorHandler() gin.HandlerFunc {
    return func(c *gin.Context) {
        c.Next() // Chạy các handler khác
        if len(c.Errors) > 0 {
            c.JSON(500, gin.H{
                "errors": c.Errors.ByType(gin.ErrorTypeAny).String(),
            })
        }
    }
}

r.Use(ErrorHandler())
r.GET("/error", func(c *gin.Context) { c.Error(errors.New("test error")) })
```

### 107. 🚨 Request validation middlewares?

Dùng `c.ShouldBind()` với struct tags:

```go
type User struct {
    Name string `form:"name" binding:"required"`
    Age  int    `form:"age" binding:"gte=18"`
}

r.POST("/user", func(c *gin.Context) {
    var user User
    if err := c.ShouldBind(&user); err != nil {
        c.JSON(400, gin.H{"error": err.Error()})
        return
    }
    c.JSON(200, user)
})
```

### 108. 🔍 OAuth2 integration với Gin?

Sử dụng `golang.org/x/oauth2`:

```go
import "golang.org/x/oauth2"

var oauthConfig = &oauth2.Config{
    ClientID:     "your-client-id",
    ClientSecret: "your-client-secret",
    RedirectURL:  "http://localhost:8080/callback",
    Scopes:       []string{"email"},
    Endpoint:     oauth2.Endpoint{ /* Google, GitHub, etc. */ },
}

r.GET("/login", func(c *gin.Context) {
    url := oauthConfig.AuthCodeURL("state")
    c.Redirect(302, url)
})

r.GET("/callback", func(c *gin.Context) {
    token, _ := oauthConfig.Exchange(context.Background(), c.Query("code"))
    c.JSON(200, token)
})
```

### 109. 🧠 Role-based access control implementation?

```go
func RoleMiddleware(requiredRole string) gin.HandlerFunc {
    return func(c *gin.Context) {
        role := c.GetString("role") // Giả sử role lấy từ JWT hoặc context
        if role != requiredRole {
            c.AbortWithStatusJSON(403, gin.H{"error": "Forbidden"})
            return
        }
        c.Next()
    }
}

r.GET("/admin", RoleMiddleware("admin"), func(c *gin.Context) {
    c.JSON(200, gin.H{"msg": "Admin area"})
})
```

### 110. 🔄 Session management trong Gin?

Sử dụng `github.com/gin-contrib/sessions`:

```go
import "github.com/gin-contrib/sessions"

func main() {
    r := gin.Default()
    store := sessions.NewCookieStore([]byte("secret"))
    r.Use(sessions.Sessions("mysession", store))

    r.GET("/set", func(c *gin.Context) {
        session := sessions.Default(c)
        session.Set("user", "john")
        session.Save()
        c.String(200, "Session set")
    })

    r.GET("/get", func(c *gin.Context) {
        session := sessions.Default(c)
        user := session.Get("user")
        c.JSON(200, gin.H{"user": user})
    })

    r.Run()
}
```

# Gin Framework - Advanced Gin

## 🔹 Advanced Gin

### 111. 🧠 File uploads và handling trong Gin?

Gin hỗ trợ upload file qua `c.FormFile()`:

```go
r.POST("/upload", func(c *gin.Context) {
    file, err := c.FormFile("file") // Lấy file từ form
    if err != nil {
        c.JSON(400, gin.H{"error": err.Error()})
        return
    }
    // Lưu file vào thư mục
    dst := fmt.Sprintf("uploads/%s", file.Filename)
    if err := c.SaveUploadedFile(file, dst); err != nil {
        c.JSON(500, gin.H{"error": "Failed to save file"})
        return
    }
    c.JSON(200, gin.H{"message": "File uploaded", "filename": file.Filename})
})
```

- Upload nhiều file: Dùng `c.MultipartForm()`.

### 112. 🔄 WebSocket implementation với Gin?

Dùng package `github.com/gorilla/websocket`:

```go
import "github.com/gorilla/websocket"

var upgrader = websocket.Upgrader{
    CheckOrigin: func(r *http.Request) bool { return true },
}

r.GET("/ws", func(c *gin.Context) {
    ws, err := upgrader.Upgrade(c.Writer, c.Request, nil)
    if err != nil {
        c.JSON(500, gin.H{"error": err.Error()})
        return
    }
    defer ws.Close()

    for {
        mt, msg, err := ws.ReadMessage()
        if err != nil { break }
        ws.WriteMessage(mt, append([]byte("Echo: "), msg...))
    }
})
```

### 113. 🚨 Streaming responses trong Gin?

Dùng `c.Stream()` để gửi dữ liệu theo luồng:

```go
r.GET("/stream", func(c *gin.Context) {
    c.Stream(func(w io.Writer) bool {
        // Gửi dữ liệu từng phần
        w.Write([]byte("Chunk of data\n"))
        time.Sleep(1 * time.Second)
        return true // Tiếp tục streaming
    })
})
```

### 114. 🔍 Graceful shutdown của Gin server?

Dùng `http.Server` với context:

```go
func main() {
    r := gin.Default()
    r.GET("/", func(c *gin.Context) { c.String(200, "Hello") })

    srv := &http.Server{
        Addr:    ":8080",
        Handler: r,
    }

    go func() {
        if err := srv.ListenAndServe(); err != nil && err != http.ErrServerClosed {
            log.Fatalf("Listen: %s\n", err)
        }
    }()

    // Chờ tín hiệu dừng
    quit := make(chan os.Signal, 1)
    signal.Notify(quit, os.Interrupt)
    <-quit

    ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
    defer cancel()
    if err := srv.Shutdown(ctx); err != nil {
        log.Fatal("Shutdown:", err)
    }
    log.Println("Server stopped")
}
```

### 115. 🧰 Testing Gin applications: approaches và tools?

Dùng `httptest`:

```go
func TestPing(t *testing.T) {
    r := gin.Default()
    r.GET("/ping", func(c *gin.Context) { c.JSON(200, gin.H{"message": "pong"}) })

    req, _ := http.NewRequest("GET", "/ping", nil)
    w := httptest.NewRecorder()
    r.ServeHTTP(w, req)

    assert.Equal(t, 200, w.Code)
    assert.Contains(t, w.Body.String(), "pong")
}
```

- Tools: `testing`, `github.com/stretchr/testify`.

### 116. 🔄 Custom validators trong Gin?

Đăng ký validator với `binding.Validator`:

```go
import "github.com/go-playground/validator/v10"

func init() {
    if v, ok := binding.Validator.Engine().(*validator.Validate); ok {
        v.RegisterValidation("custom", func(fl validator.FieldLevel) bool {
            return len(fl.Field().String()) > 3 // Ví dụ: độ dài > 3
        })
    }
}

type Data struct {
    Name string `binding:"custom"`
}

r.POST("/data", func(c *gin.Context) {
    var d Data
    if err := c.ShouldBind(&d); err != nil {
        c.JSON(400, gin.H{"error": err.Error()})
        return
    }
    c.JSON(200, d)
})
```

### 117. 🤔 Cách organize routes trong large applications?

Tách routes vào các file riêng:

```
routes/
├── user.go
└── product.go
```

Ví dụ `user.go`:

```go
package routes

import "github.com/gin-gonic/gin"

func SetupUserRoutes(r *gin.Engine) {
    user := r.Group("/users")
    user.GET("/:id", getUser)
}

func getUser(c *gin.Context) { c.JSON(200, gin.H{"id": c.Param("id")}) }
```

Trong `main.go`:

```go
routes.SetupUserRoutes(r)
```

### 118. 🔍 Gin và internationalization (i18n)?

Dùng `github.com/gin-contrib/i18n`:

```go
import "github.com/gin-contrib/i18n"

func main() {
    r := gin.Default()
    r.Use(i18n.Localize(i18n.WithBundle(&i18n.BundleCfg{
        RootPath:      "./locales",
        AcceptLanguage: []language.Tag{language.English, language.Vietnamese},
        DefaultLanguage: language.English,
    })))
    r.GET("/hello", func(c *gin.Context) {
        c.String(200, i18n.MustGetMessage("hello"))
    })
    r.Run()
}
```

File `locales/en.toml`:

```toml
[hello]
other = "Hello, world!"
```

### 119. 🚨 Performance profiling của Gin applications?

Dùng `net/http/pprof`:

```go
import _ "net/http/pprof"

func main() {
    r := gin.Default()
    r.GET("/debug/pprof/*any", gin.WrapH(http.DefaultServeMux)) // Expose pprof
    r.Run()
}
```

Truy cập `http://localhost:8080/debug/pprof/` và dùng `go tool pprof`.

### 120. 🧠 Auto-documentation với Gin và Swagger?

Dùng `github.com/swaggo/swag` và `github.com/swaggo/gin-swagger`:

```go
import "github.com/swaggo/gin-swagger" // gin-swagger
import "github.com/swaggo/swag/example/basic/docs"

// @Summary Get pong
// @Produce json
// @Success 200 {object} map[string]string
// @Router /ping [get]
func ping(c *gin.Context) { c.JSON(200, gin.H{"message": "pong"}) }

func main() {
    r := gin.Default()
    r.GET("/ping", ping)
    r.GET("/swagger/*any", ginSwagger.WrapHandler(swaggerFiles.Handler))
    r.Run()
}
```

Chạy `swag init` để sinh `docs/`, sau đó truy cập `/swagger/index.html`.

# RESTful API & Microservices - RESTful API Design

## 🔹 RESTful API Design

### 121. 🧠 Principles của RESTful API design?

REST (Representational State Transfer) dựa trên các nguyên tắc:

- **Tài nguyên (Resources)**: Mọi thứ được biểu diễn dưới dạng tài nguyên (users, orders), truy cập qua URI (như `/users`).
- **HTTP Methods**: Sử dụng các phương thức chuẩn: `GET` (lấy), `POST` (tạo), `PUT` (cập nhật), `DELETE` (xóa).
- **Stateless**: Mỗi request độc lập, server không lưu trạng thái client.
- **Uniform Interface**: Giao diện nhất quán (URI, HATEOAS).
- **Client-Server**: Tách biệt logic client và server.
- **Cacheable**: Hỗ trợ caching để tăng hiệu suất.

### 122. 🔄 Versioning strategies cho APIs?

- **URL Versioning**: Thêm version vào URI (e.g., `/v1/users`).
  - Ưu: Đơn giản, rõ ràng.
  - Nhược: URI thay đổi khi version mới ra đời.
- **Header Versioning**: Dùng header (e.g., `Accept: application/vnd.api.v1+json`).
  - Ưu: Giữ URI sạch.
  - Nhược: Phức tạp hơn cho client.
- **Query Parameter**: `/users?version=1`.
  - Ưu: Linh hoạt.
  - Nhược: Không chuẩn REST.
- **Best practice**: Dùng URL versioning cho đơn giản.

### 123. 🔍 Status codes: khi nào sử dụng cái nào?

- **200 OK**: Thành công (GET, PUT).
- **201 Created**: Tạo tài nguyên thành công (POST).
- **204 No Content**: Thành công, không trả dữ liệu (DELETE).
- **400 Bad Request**: Lỗi request (dữ liệu không hợp lệ).
- **401 Unauthorized**: Chưa xác thực.
- **403 Forbidden**: Không có quyền.
- **404 Not Found**: Tài nguyên không tồn tại.
- **500 Internal Server Error**: Lỗi server.

### 124. 🚨 Pagination strategies trong REST APIs?

- **Offset-based**: `/users?offset=10&limit=10`.
  - Ưu: Dễ hiểu.
  - Nhược: Hiệu suất kém với tập dữ liệu lớn.
- **Cursor-based**: `/users?cursor=abc123&limit=10`.
  - Ưu: Hiệu quả, không phụ thuộc vị trí tuyệt đối.
  - Nhược: Phức tạp hơn cho client.
- **Page-based**: `/users?page=2&per_page=10`.
  - Ưu: Thân thiện với người dùng.
  - Nhược: Tương tự offset-based.

### 125. 🧰 HATEOAS và hypermedia trong API responses?

HATEOAS (Hypermedia as the Engine of Application State) thêm liên kết để điều hướng:

```json
{
  "id": 1,
  "name": "John",
  "links": {
    "self": "/users/1",
    "orders": "/users/1/orders"
  }
}
```

- **Ưu**: Client tự khám phá API.
- **Nhược**: Tăng kích thước response.

### 126. 🔄 Caching strategies cho REST APIs?

- **ETag**: Server gửi hash của tài nguyên, client kiểm tra `If-None-Match`.
- **Last-Modified**: Dùng header `If-Modified-Since`.
- **Cache-Control**: `Cache-Control: max-age=3600` để client lưu cache.
- **CDN**: Dùng CDN như Cloudflare để cache response.
  Ví dụ:

```go
c.Header("Cache-Control", "public, max-age=3600")
c.JSON(200, data)
```

### 127. 🚨 Idempotent operations và importance trong APIs?

- **Idempotent**: Lặp lại request không thay đổi kết quả (e.g., `PUT`, `DELETE`).
- **Importance**: Đảm bảo tính nhất quán khi request thất bại/lặp lại (e.g., mạng không ổn định).
- **Ví dụ**: `PUT /users/1` cập nhật cùng dữ liệu nhiều lần → kết quả không đổi.

### 128. 🤔 Error handling và error responses trong APIs?

Trả về JSON chi tiết lỗi:

```json
{
  "error": "Invalid input",
  "details": {
    "field": "email",
    "message": "Email is required"
  },
  "code": 400
}
```

- **Best practice**: Dùng status code phù hợp, thêm thông tin cụ thể, tránh lộ chi tiết server.

### 129. 🔍 API documentation best practices?

- **OpenAPI/Swagger**: Tự động sinh tài liệu từ code.
- **Examples**: Cung cấp request/response mẫu.
- **Consistency**: Mô tả rõ ràng endpoint, params, status codes.
- **Versioning**: Ghi chú version hiện tại.
- **Tools**: Swagger UI, Postman collection.

### 130. 🧠 Cross-Origin Resource Sharing (CORS) trong REST APIs?

CORS cho phép client từ domain khác truy cập API:

- **Headers**:
  - `Access-Control-Allow-Origin: *` (hoặc domain cụ thể).
  - `Access-Control-Allow-Methods: GET, POST`.
- **Preflight**: Xử lý request `OPTIONS` cho các phương thức phức tạp.
  Ví dụ với Gin:

```go
r.Use(cors.New(cors.Config{
    AllowOrigins: []string{"http://example.com"},
    AllowMethods: []string{"GET", "POST"},
}))
```

# RESTful API & Microservices - Microservices Architecture

## 🔹 Microservices Architecture

### 131. 🔄 Microservices vs monolithic architecture: trade-offs?

- **Monolithic**:
  - **Ưu**: Đơn giản triển khai, debug dễ, hiệu năng cao trong hệ thống nhỏ.
  - **Nhược**: Khó mở rộng, bảo trì phức tạp khi lớn.
- **Microservices**:
  - **Ưu**: Linh hoạt, dễ mở rộng từng dịch vụ, công nghệ đa dạng.
  - **Nhược**: Phức tạp (quản lý nhiều dịch vụ), độ trễ mạng, khó đảm bảo tính nhất quán.

### 132. 🚨 Service discovery trong microservices?

Service discovery giúp các dịch vụ tìm nhau mà không cần hardcode địa chỉ:

- **Cơ chế**:
  - **Client-side**: Dùng thư viện như `Consul` hoặc `etcd`.
  - **Server-side**: Proxy như `Envoy`.
- **Ví dụ với Consul**:

```go
import "github.com/hashicorp/consul/api"

client, _ := api.NewClient(api.DefaultConfig())
service, _, _ := client.Agent().ServiceRegister(&api.AgentServiceRegistration{
    Name: "user-service",
    Port: 8080,
})
```

- **Tools**: Consul, Eureka, Kubernetes DNS.

### 133. 🔍 Inter-service communication patterns?

- **Synchronous**:
  - **REST**: Dùng HTTP, đơn giản nhưng chậm.
  - **gRPC**: Nhanh, type-safe, dùng Protocol Buffers.
- **Asynchronous**:
  - **Message Queues**: RabbitMQ, Kafka (gửi message, xử lý sau).
  - **Pub/Sub**: Một dịch vụ phát, nhiều dịch vụ nhận (e.g., NATS).

### 134. 🧰 API Gateway pattern: mục đích và implementation?

- **Mục đích**: Điểm vào duy nhất cho client, xử lý routing, auth, rate limiting.
- **Implementation**:
  - Dùng tools như Kong, Traefik.
  - Tự viết với Gin:

```go
r := gin.Default()
r.GET("/users/:id", proxyTo("http://user-service:8080"))
func proxyTo(target string) gin.HandlerFunc {
    return func(c *gin.Context) {
        // Proxy request đến dịch vụ backend
    }
}
```

### 135. 🤔 Circuit breaker pattern trong Go?

Ngăn chặn gọi lặp lại dịch vụ lỗi bằng `github.com/sony/gobreaker`:

```go
import "github.com/sony/gobreaker"

var cb *gobreaker.CircuitBreaker

func init() {
    cb = gobreaker.NewCircuitBreaker(gobreaker.Settings{
        Name:    "user-service",
        MaxRequests: 2,
        Timeout: 5 * time.Second,
    })
}

func callService() (string, error) {
    result, err := cb.Execute(func() (interface{}, error) {
        resp, err := http.Get("http://user-service")
        return resp, err
    })
    if err != nil { return "", err }
    return result.(*http.Response).Status, nil
}
```

### 136. 🔄 Distributed tracing trong microservices?

Theo dõi request qua nhiều dịch vụ:

- **Tools**: Jaeger, Zipkin.
- **Ví dụ với Jaeger**:

```go
import "github.com/opentracing/opentracing-go"

tracer, closer := jaeger.NewTracer("my-service", sampler, reporter)
defer closer.Close()
span := tracer.StartSpan("operation")
defer span.Finish()
```

### 137. 🚨 Event-driven architecture trong microservices?

Dịch vụ phản ứng với sự kiện:

- **Tools**: Kafka, NATS.
- **Ví dụ với NATS**:

```go
nc, _ := nats.Connect("nats://localhost:4222")
nc.Subscribe("order.created", func(m *nats.Msg) {
    fmt.Printf("Received: %s\n", string(m.Data))
})
nc.Publish("order.created", []byte("New order"))
```

### 138. 🔍 Saga pattern for distributed transactions?

Phối hợp giao dịch qua nhiều dịch vụ:

- **Choreography**: Dịch vụ phát sự kiện, các dịch vụ khác phản ứng.
- **Orchestration**: Một dịch vụ trung tâm điều phối.
  Ví dụ Choreography:

```go
nc.Publish("payment.failed", []byte("Rollback order"))
```

### 139. 🧠 CQRS và Event Sourcing trong microservices?

- **CQRS (Command Query Responsibility Segregation)**: Tách đọc (Query) và ghi (Command).
- **Event Sourcing**: Lưu trạng thái dưới dạng chuỗi sự kiện thay vì snapshot.
  Ví dụ:

```go
type Event struct { Type string; Data interface{} }
var events []Event

func applyEvent(e Event) { events = append(events, e) }
func getState() { /* Tái tạo từ events */ }
```

### 140. 🔄 Monitoring và health checking trong microservices?

- **Monitoring**: Dùng Prometheus để thu thập metrics.
- **Health checking**: Thêm endpoint `/health`.
  Ví dụ với Gin:

```go
r.GET("/health", func(c *gin.Context) {
    c.JSON(200, gin.H{"status": "healthy"})
})
```

- **Tools**: Prometheus, Grafana, ELK Stack.

# RESTful API & Microservices - gRPC & Protocol Buffers

## 🔹 gRPC & Protocol Buffers

### 141. 🧠 gRPC là gì và khi nào nên sử dụng so với REST?

- **gRPC**: Là một framework RPC (Remote Procedure Call) hiệu năng cao, dùng Protocol Buffers làm giao thức truyền dữ liệu, dựa trên HTTP/2.
- **So với REST**:
  - **Ưu**: Nhanh hơn (nhờ binary serialization và HTTP/2), hỗ trợ streaming, type-safe.
  - **Nhược**: Phức tạp hơn, ít thân thiện với browser.
- **Khi nào dùng**: Microservices nội bộ, hệ thống yêu cầu hiệu suất cao, real-time (chat, streaming).

### 142. 🔄 Protocol Buffers: benefits và limitations?

- **Benefits**:
  - Nhỏ gọn: Binary format tiết kiệm băng thông hơn JSON.
  - Nhanh: Serialization/deserialization hiệu quả.
  - Type-safe: Định nghĩa schema rõ ràng.
  - Multi-language: Hỗ trợ nhiều ngôn ngữ.
- **Limitations**:
  - Khó đọc: Không thân thiện với người dùng như JSON.
  - Phụ thuộc tool: Cần `protoc` để sinh code.
  - Ít linh hoạt: Thay đổi schema cần cẩn thận để tương thích ngược.

### 143. 🚨 Implementing gRPC services trong Go?

1. Định nghĩa `.proto` file:

```proto
syntax = "proto3";
package pb;
service Greeter {
    rpc SayHello (HelloRequest) returns (HelloReply) {}
}
message HelloRequest { string name = 1; }
message HelloReply { string message = 1; }
```

2. Sinh code:

```bash
protoc --go_out=. --go-grpc_out=. *.proto
```

3. Implement server:

```go
type server struct { pb.UnimplementedGreeterServer }

func (s *server) SayHello(ctx context.Context, req *pb.HelloRequest) (*pb.HelloReply, error) {
    return &pb.HelloReply{Message: "Hello " + req.Name}, nil
}

func main() {
    lis, _ := net.Listen("tcp", ":50051")
    s := grpc.NewServer()
    pb.RegisterGreeterServer(s, &server{})
    s.Serve(lis)
}
```

4. Client:

```go
conn, _ := grpc.Dial("localhost:50051", grpc.WithInsecure())
client := pb.NewGreeterClient(conn)
resp, _ := client.SayHello(context.Background(), &pb.HelloRequest{Name: "World"})
fmt.Println(resp.Message)
```

### 144. 🔍 gRPC error handling?

Dùng `google.golang.org/grpc/status`:

```go
func (s *server) SayHello(ctx context.Context, req *pb.HelloRequest) (*pb.HelloReply, error) {
    if req.Name == "" {
        return nil, status.Error(codes.InvalidArgument, "Name is required")
    }
    return &pb.HelloReply{Message: "Hello " + req.Name}, nil
}
```

Client kiểm tra:

```go
if err != nil {
    if s, ok := status.FromError(err); ok {
        fmt.Printf("gRPC error: %v\n", s.Message())
    }
}
```

### 145. 🧰 Bidirectional streaming trong gRPC?

Định nghĩa trong `.proto`:

```proto
rpc Chat (stream ChatMessage) returns (stream ChatMessage) {}
```

Server:

```go
func (s *server) Chat(stream pb.Greeter_ChatServer) error {
    for {
        msg, err := stream.Recv()
        if err == io.EOF { return nil }
        if err != nil { return err }
        stream.Send(&pb.ChatMessage{Content: "Echo: " + msg.Content})
    }
}
```

Client:

```go
stream, _ := client.Chat(context.Background())
stream.Send(&pb.ChatMessage{Content: "Hi"})
resp, _ := stream.Recv()
fmt.Println(resp.Content)
```

### 146. 🔄 gRPC authentication và authorization?

- **TLS**: Dùng chứng chỉ cho kết nối an toàn.
- **Token-based (JWT)**:

```go
func unaryInterceptor(ctx context.Context, req interface{}, info *grpc.UnaryServerInfo, handler grpc.UnaryHandler) (interface{}, error) {
    if md, ok := metadata.FromIncomingContext(ctx); ok {
        token := md["authorization"]
        // Validate token
    }
    return handler(ctx, req)
}

s := grpc.NewServer(grpc.UnaryInterceptor(unaryInterceptor))
```

### 147. 🤔 gRPC gateway for REST compatibility?

Dùng `github.com/grpc-ecosystem/grpc-gateway`:

1. Cập nhật `.proto`:

```proto
import "google/api/annotations.proto";
rpc SayHello (HelloRequest) returns (HelloReply) {
    option (google.api.http) = { get: "/v1/hello/{name}" };
}
```

2. Sinh code:

```bash
protoc --grpc-gateway_out=. *.proto
```

3. Implement:

```go
mux := runtime.NewServeMux()
grpcgateway.RegisterGreeterHandlerFromEndpoint(context.Background(), mux, "localhost:50051", []grpc.DialOption{grpc.WithInsecure()})
http.ListenAndServe(":8080", mux)
```

### 148. 🔍 Load balancing với gRPC?

- **Client-side**: Dùng `grpc.WithBalancerName("round_robin")`.
- **Server-side**: Proxy như Envoy hoặc Kubernetes load balancer.
  Ví dụ:

```go
conn, _ := grpc.Dial("dns:///service.example.com", grpc.WithBalancerName("round_robin"))
```

### 149. 🚨 gRPC interceptors: usage và patterns?

- **Unary Interceptor**: Xử lý từng request.

```go
func loggingInterceptor(ctx context.Context, req interface{}, info *grpc.UnaryServerInfo, handler grpc.UnaryHandler) (interface{}, error) {
    log.Printf("Request: %v", req)
    return handler(ctx, req)
}
```

- **Stream Interceptor**: Xử lý stream.
- **Patterns**: Logging, auth, metrics.

### 150. 🧠 gRPC reflection và tools?

- **Reflection**: Cho phép client khám phá service mà không cần `.proto`.

```go
import "google.golang.org/grpc/reflection"

s := grpc.NewServer()
reflection.Register(s)
```

- **Tools**: `grpcurl`, `BloomRPC` để test/debug.

## 🔹 Database Interactions

### 151. 🧠 Cách làm việc với SQL databases trong Go?

Go sử dụng package `database/sql` cùng driver cụ thể (như `github.com/lib/pq` cho PostgreSQL):

```go
import (
    "database/sql"
    _ "github.com/lib/pq" // Driver PostgreSQL
)

func main() {
    db, err := sql.Open("postgres", "user=pg password=pass dbname=test sslmode=disable")
    if err != nil { panic(err) }
    defer db.Close()

    rows, err := db.Query("SELECT id, name FROM users")
    if err != nil { panic(err) }
    defer rows.Close()

    for rows.Next() {
        var id int
        var name string
        rows.Scan(&id, &name)
        fmt.Println(id, name)
    }
}
```

### 152. 🔄 SQL vs NoSQL trong Go applications?

- **SQL**:
  - **Ưu**: Cấu trúc rõ ràng, hỗ trợ join, ACID.
  - **Nhược**: Ít linh hoạt khi mở rộng ngang.
  - **Dùng**: Ứng dụng cần tính nhất quán (e.g., tài chính).
- **NoSQL** (như MongoDB với `go.mongodb.org/mongo-driver`):
  - **Ưu**: Linh hoạt, dễ mở rộng, tốt cho dữ liệu không cấu trúc.
  - **Nhược**: Không đảm bảo ACID đầy đủ.
  - **Dùng**: Ứng dụng big data, real-time (e.g., analytics).
- **Go**: Hỗ trợ cả hai qua driver (SQL: `database/sql`, NoSQL: driver riêng).

### 153. 🔍 Cách sử dụng `database/sql` package?

- **Kết nối**: `sql.Open(driver, dataSourceName)`.
- **Query**: `db.Query()` (nhiều row), `db.QueryRow()` (một row).
- **Exec**: `db.Exec()` cho INSERT/UPDATE/DELETE.
  Ví dụ:

```go
db, _ := sql.Open("mysql", "user:pass@/dbname")
row := db.QueryRow("SELECT name FROM users WHERE id = ?", 1)
var name string
row.Scan(&name)
fmt.Println(name)
```

### 154. 🚨 Connection pooling best practices?

- **Mặc định**: `database/sql` tự quản lý pool.
- **Tuning**:
  - `db.SetMaxOpenConns(n)`: Giới hạn số kết nối mở.
  - `db.SetMaxIdleConns(n)`: Số kết nối nhàn rỗi.
  - `db.SetConnMaxLifetime(t)`: Thời gian sống tối đa của kết nối.
    Ví dụ:

```go
db.SetMaxOpenConns(25)
db.SetMaxIdleConns(10)
db.SetConnMaxLifetime(time.Hour)
```

### 155. 🧰 Prepared statements và query parameters?

Prepared statements tăng bảo mật (tránh SQL injection) và hiệu suất:

```go
stmt, _ := db.Prepare("INSERT INTO users(name) VALUES(?)")
defer stmt.Close()
stmt.Exec("John")
```

- Query parameters dùng `?` hoặc `$1` (tùy driver).

### 156. 🔄 Transaction management trong Go?

Dùng `db.Begin()` để khởi tạo transaction:

```go
tx, _ := db.Begin()
_, err := tx.Exec("UPDATE users SET name = ? WHERE id = ?", "Jane", 1)
if err != nil {
    tx.Rollback()
    return
}
tx.Commit()
```

### 157. 🤔 Cách xử lý NULL values trong SQL databases?

Dùng các kiểu nullable từ `database/sql`:

- `sql.NullString`, `sql.NullInt64`, v.v.
  Ví dụ:

```go
var s sql.NullString
row := db.QueryRow("SELECT name FROM users WHERE id = ?", 1)
row.Scan(&s)
if s.Valid {
    fmt.Println(s.String)
} else {
    fmt.Println("NULL")
}
```

### 158. 🔍 Performance tuning cho database operations?

- **Index**: Tạo index cho cột hay tìm kiếm.
- **Connection Pool**: Tối ưu số kết nối (xem câu 154).
- **Batch Processing**: Gộp nhiều INSERT/UPDATE:

```go
tx, _ := db.Begin()
stmt, _ := tx.Prepare("INSERT INTO users(name) VALUES(?)")
for _, name := range names { stmt.Exec(name) }
tx.Commit()
```

- **Profiling**: Dùng `EXPLAIN` để phân tích query.

### 159. 🚨 Migrations strategies trong Go applications?

- **Tools**: `github.com/golang-migrate/migrate`, `sql-migrate`.
- **Cách làm**:
  1. Tạo file migration (e.g., `001_create_users.up.sql`):

```sql
CREATE TABLE users (id SERIAL PRIMARY KEY, name TEXT);
```

2. Áp dụng:

```go
import "github.com/golang-migrate/migrate/v4"

m, _ := migrate.New("file://migrations", "postgres://user:pass@localhost/db")
m.Up()
```

- **Best practice**: Version migration, rollback hỗ trợ.

### 160. 🧠 Cách làm việc với stored procedures?

Gọi stored procedure bằng `db.Exec` hoặc `db.Query`:

```go
// Stored procedure: CREATE PROCEDURE add_user(IN name TEXT) ...
_, err := db.Exec("CALL add_user(?)", "John")
if err != nil { panic(err) }

// Với output
row := db.QueryRow("CALL get_user(?)", 1)
var name string
row.Scan(&name)
```

## 🔹 ORM & Query Builders

### 161. 🔄 GORM: features và limitations?

- **Features**:
  - ORM đầy đủ: Ánh xạ struct sang table.
  - Hỗ trợ associations (has one, has many).
  - Hooks (before/after create, update).
  - Auto-migration, transactions, raw SQL.
- **Limitations**:
  - Hiệu suất: Chậm hơn raw SQL trong trường hợp phức tạp.
  - Phức tạp: Khó tùy chỉnh query nâng cao.
  - Overhead: Thêm layer abstraction không cần thiết cho dự án nhỏ.
    Ví dụ:

```go
import "gorm.io/gorm"

type User struct { gorm.Model; Name string }
db.AutoMigrate(&User{})
db.Create(&User{Name: "John"})
```

### 162. 🚨 Relationships trong GORM?

- **Has One**: `gorm:"foreignKey:UserID"`.
- **Has Many**: Slice với foreign key.
- **Many to Many**: Bảng trung gian.
  Ví dụ:

```go
type User struct { gorm.Model; Name string; Profile Profile }
type Profile struct { gorm.Model; UserID uint; Bio string }

db.AutoMigrate(&User{}, &Profile{})
db.Create(&User{Name: "John", Profile: Profile{Bio: "Developer"}})
```

### 163. 🔍 GORM hooks và callbacks?

Hooks chạy trước/sau thao tác CRUD:

```go
type User struct {
    gorm.Model
    Name string
}

func (u *User) BeforeCreate(tx *gorm.DB) error {
    fmt.Println("Creating:", u.Name)
    return nil
}

db.Create(&User{Name: "John"}) // In "Creating: John"
```

- Các hook: `BeforeCreate`, `AfterUpdate`, v.v.

### 164. 🧰 GORM transactions và locking?

- **Transactions**:

```go
db.Transaction(func(tx *gorm.DB) error {
    tx.Create(&User{Name: "John"})
    if err := tx.Create(&User{Name: "Jane"}).Error; err != nil {
        return err // Rollback
    }
    return nil // Commit
})
```

- **Locking**:

```go
db.Clauses(clause.Locking{Strength: "UPDATE"}).First(&user) // FOR UPDATE
```

### 165. 🤔 Query optimization với GORM?

- **Preload**: Tải trước associations để tránh N+1:

```go
db.Preload("Profile").Find(&users)
```

- **Select**: Giới hạn cột:

```go
db.Select("id, name").Find(&users)
```

- **Raw SQL**: Dùng khi query phức tạp:

```go
db.Raw("SELECT * FROM users WHERE age > ?", 18).Scan(&users)
```

### 166. 🔄 SQLx so với standard `database/sql`?

- **SQLx** (`github.com/jmoiron/sqlx`):
  - Mở rộng `database/sql`.
  - Hỗ trợ struct mapping, named queries.
  - Ví dụ:

```go
type User struct { ID int; Name string }
var user User
sqlx.Get(db, &user, "SELECT * FROM users WHERE id = ?", 1)
```

- **So với `database/sql`**:
  - **Ưu**: Dễ dùng hơn, ít boilerplate.
  - **Nhược**: Phụ thuộc thư viện bên thứ ba.

### 167. 🚨 Các alternatives cho GORM trong Go ecosystem?

- **SQLx**: Nhẹ, gần với raw SQL.
- **Ent** (`entgo.io`): Type-safe, code-first, hiệu suất cao.
- **Bun** (`github.com/uptrace/bun`): Nhẹ, hỗ trợ PostgreSQL/MySQL.
- **XORM**: Tương tự GORM, ít phổ biến hơn.

### 168. 🔍 N+1 query problem và cách tránh?

- **N+1**: Query riêng cho mỗi liên kết (e.g., lấy users rồi query profile từng user).
- **Tránh**:
  - **Preload** (GORM): `db.Preload("Profile").Find(&users)`.
  - **JOIN**: Dùng raw SQL hoặc builder:

```go
db.Joins("JOIN profiles ON profiles.user_id = users.id").Find(&users)
```

### 169. 🧠 Cách implement soft deletes?

Thêm trường `DeletedAt`:

```go
type User struct {
    gorm.Model
    Name      string
    DeletedAt gorm.DeletedAt `gorm:"index"`
}

db.Delete(&User{ID: 1}) // Chỉ cập nhật DeletedAt
db.Unscoped().Delete(&User{ID: 1}) // Xóa thật
```

- Lấy dữ liệu: GORM tự lọc bỏ soft-deleted record.

### 170. 🔄 Batch operations và bulk inserts?

- **Batch Insert** (GORM):

```go
users := []User{{Name: "John"}, {Name: "Jane"}}
db.Create(&users) // Hoặc db.CreateInBatches(&users, 100)
```

- **Bulk Insert** (SQLx):

```go
sqlx.NamedExec(db, "INSERT INTO users (name) VALUES (:name)", users)
```

- **Best practice**: Chia nhỏ batch (e.g., 1000 rows) để tránh timeout.

## 🔹 NoSQL & Caching

### 171. 🧠 Working với MongoDB trong Go?

Sử dụng driver `go.mongodb.org/mongo-driver`:

```go
import "go.mongodb.org/mongo-driver/mongo"

client, _ := mongo.Connect(context.Background(), options.Client().ApplyURI("mongodb://localhost:27017"))
coll := client.Database("test").Collection("users")
res, _ := coll.InsertOne(context.Background(), bson.D{{"name", "John"}})
fmt.Println(res.InsertedID)
```

- **CRUD**: `InsertOne`, `Find`, `UpdateOne`, `DeleteOne`.

### 172. 🔄 Redis integration trong Go applications?

Sử dụng `github.com/go-redis/redis`:

```go
import "github.com/go-redis/redis/v8"

client := redis.NewClient(&redis.Options{Addr: "localhost:6379"})
ctx := context.Background()
client.Set(ctx, "key", "value", 0)
val, _ := client.Get(ctx, "key").Result()
fmt.Println(val)
```

- **Dùng**: Cache, session, queue.

### 173. 🚨 Caching strategies và patterns?

- **Cache-Aside**: Ứng dụng tự quản lý cache:

```go
val, err := cache.Get("key")
if err != nil {
    val = db.Query("...")
    cache.Set("key", val, time.Hour)
}
```

- **Write-Through**: Ghi đồng thời vào cache và DB.
- **Read-Through**: Cache tự lấy từ DB nếu miss.
- **TTL**: Đặt thời gian sống (e.g., `time.Hour`).

### 174. 🔍 Distributed caching với Go?

Dùng Redis hoặc Memcached:

- **Redis Cluster**:

```go
client := redis.NewClusterClient(&redis.ClusterOptions{
    Addrs: []string{"node1:6379", "node2:6379"},
})
client.Set(context.Background(), "key", "value", 0)
```

- **Consistent Hashing**: Phân phối key qua nhiều node.

### 175. 🧰 Elasticsearch integration trong Go?

Sử dụng `github.com/elastic/go-elasticsearch`:

```go
import "github.com/elastic/go-elasticsearch/v8"

es, _ := elasticsearch.NewClient(elasticsearch.Config{Addresses: []string{"http://localhost:9200"}})
res, _ := es.Index("users", strings.NewReader(`{"name": "John"}`), es.Index.WithDocumentID("1"))
fmt.Println(res.Status())
```

- **Dùng**: Tìm kiếm full-text, analytics.

### 176. 🔄 Time-series databases với Go?

Dùng InfluxDB (`github.com/influxdata/influxdb-client-go`):

```go
client := influxdb2.NewClient("http://localhost:8086", "token")
writeAPI := client.WriteAPIBlocking("org", "bucket")
p := influxdb2.NewPointWithMeasurement("stat").AddTag("unit", "temp").AddField("avg", 23.5).SetTime(time.Now())
writeAPI.WritePoint(context.Background(), p)
```

- **Dùng**: Metrics, IoT.

### 177. 🤔 Cache invalidation strategies?

- **TTL**: Xóa tự động sau thời gian sống.
- **Write-Update**: Cập nhật cache khi DB thay đổi.
- **Write-Invalidate**: Xóa cache khi DB thay đổi:

```go
db.Update("user", 1, "Jane")
cache.Del("user:1")
```

- **Event-based**: Dùng message queue để thông báo invalidation.

### 178. 🔍 DynamoDB với Go?

Sử dụng `github.com/aws/aws-sdk-go-v2/service/dynamodb`:

```go
import "github.com/aws/aws-sdk-go-v2/service/dynamodb"

cfg, _ := config.LoadDefaultConfig(context.Background())
svc := dynamodb.NewFromConfig(cfg)
svc.PutItem(context.Background(), &dynamodb.PutItemInput{
    TableName: aws.String("users"),
    Item: map[string]types.AttributeValue{
        "id":   &types.AttributeValueMemberS{Value: "1"},
        "name": &types.AttributeValueMemberS{Value: "John"},
    },
})
```

- **Dùng**: Ứng dụng serverless.

### 179. 🚨 Cassandra integration trong Go?

Sử dụng `github.com/gocql/gocql`:

```go
import "github.com/gocql/gocql"

cluster := gocql.NewCluster("localhost:9042")
session, _ := cluster.CreateSession()
defer session.Close()
session.Query("INSERT INTO users (id, name) VALUES (?, ?)", 1, "John").Exec()
```

- **Dùng**: Dữ liệu phân tán, write-heavy.

### 180. 🧠 Graph databases trong Go applications?

Dùng Neo4j với `github.com/neo4j/neo4j-go-driver`:

```go
driver, _ := neo4j.NewDriver("bolt://localhost:7687", neo4j.BasicAuth("neo4j", "password", ""))
defer driver.Close()
session := driver.NewSession(context.Background(), neo4j.SessionConfig{})
defer session.Close()
session.Run("CREATE (p:Person {name: $name})", map[string]interface{}{"name": "John"})
```

- **Dùng**: Quan hệ phức tạp (social network, recommendation).

## Testing & Debugging

### 🔹 Testing Strategies

### 181. 🧠 Unit testing trong Go: best practices?

- **Isolate**: Test từng hàm độc lập, tránh phụ thuộc.
- **Naming**: Dùng `Test<FunctionName>` (e.g., `TestAdd`).
- **Small Scope**: Test nhỏ, tập trung vào một logic.
- **No External**: Không gọi DB/network thật.
- **Assertions**: Dùng `t.Errorf` hoặc `assert` từ `github.com/stretchr/testify`.
  Ví dụ:

```go
func Add(a, b int) int { return a + b }
func TestAdd(t *testing.T) {
    if got := Add(2, 3); got != 5 {
        t.Errorf("Add(2, 3) = %d; want 5", got)
    }
}
```

### 182. 🔄 Cách sử dụng Go's built-in testing framework?

- File: `<name>_test.go`.
- Hàm: `func TestXxx(t *testing.T)`.
- Chạy: `go test`.
  Ví dụ:

```go
package main

import "testing"

func TestHello(t *testing.T) {
    want := "Hello"
    if got := "Hello"; got != want {
        t.Errorf("got %q, want %q", got, want)
    }
}
```

- Flags: `go test -v` (verbose), `-run TestXxx` (chạy test cụ thể).

### 183. 🚨 Mocking trong Go tests?

Dùng interface để mock phụ thuộc:

```go
type DB interface { Get(id int) string }
type MockDB struct {}
func (m *MockDB) Get(id int) string { return "mock" }

func GetName(db DB, id int) string { return db.Get(id) }

func TestGetName(t *testing.T) {
    mockDB := &MockDB{}
    if got := GetName(mockDB, 1); got != "mock" {
        t.Errorf("got %q, want %q", got, "mock")
    }
}
```

- Tool: `github.com/golang/mock`.

### 184. 🔍 Table-driven tests trong Go?

Dùng slice/map để test nhiều trường hợp:

```go
func TestAdd(t *testing.T) {
    tests := []struct {
        a, b, want int
    }{
        {2, 3, 5},
        {0, 0, 0},
        {-1, 1, 0},
    }
    for _, tt := range tests {
        if got := Add(tt.a, tt.b); got != tt.want {
            t.Errorf("Add(%d, %d) = %d; want %d", tt.a, tt.b, got, tt.want)
        }
    }
}
```

### 185. 🧰 Benchmarking trong Go?

Dùng `func BenchmarkXxx(b *testing.B)`:

```go
func BenchmarkAdd(b *testing.B) {
    for i := 0; i < b.N; i++ {
        Add(2, 3)
    }
}
```

- Chạy: `go test -bench=.`.
- Flags: `-benchtime=5s`, `-benchmem`.

### 186. 🔄 Integration testing cho Go applications?

Test với DB/network thật:

```go
func TestUserRepo(t *testing.T) {
    db, _ := sql.Open("sqlite3", ":memory:")
    db.Exec("CREATE TABLE users (id INTEGER, name TEXT)")
    repo := NewUserRepo(db)
    repo.Save(User{ID: 1, Name: "John"})
    user, _ := repo.Get(1)
    if user.Name != "John" {
        t.Errorf("got %q, want %q", user.Name, "John")
    }
}
```

- Dùng DB test hoặc Docker container.

### 187. 🤔 End-to-end testing approaches?

Test toàn bộ hệ thống (API, DB):

- **Tool**: `net/http/httptest` hoặc `github.com/stretchr/testify`.
- Ví dụ với Gin:

```go
func TestAPI(t *testing.T) {
    r := gin.Default()
    r.GET("/user", func(c *gin.Context) { c.JSON(200, gin.H{"name": "John"}) })
    req, _ := http.NewRequest("GET", "/user", nil)
    w := httptest.NewRecorder()
    r.ServeHTTP(w, req)
    assert.Equal(t, 200, w.Code)
    assert.Contains(t, w.Body.String(), "John")
}
```

### 188. 🔍 Test coverage tools và reporting?

- Chạy: `go test -cover`.
- Báo cáo chi tiết: `go test -coverprofile=cover.out && go tool cover -html=cover.out`.
- Tool bên thứ ba: `gocov`, `coveralls`.

### 189. 🚨 Property-based testing trong Go?

Kiểm tra thuộc tính với dữ liệu ngẫu nhiên, dùng `github.com/leanovate/gopter`:

```go
import "github.com/leanovate/gopter"

func TestAddProperties(t *testing.T) {
    properties := gopter.NewProperties(nil)
    properties.Property("commutative", prop.ForAll(
        func(a, b int) bool { return Add(a, b) == Add(b, a) },
        gen.Int(), gen.Int(),
    ))
    properties.TestingRun(t)
}
```

### 190. 🧠 Behavior-driven development (BDD) trong Go?

Dùng `github.com/onsi/ginkgo` và `github.com/onsi/gomega`:

```go
import (
    . "github.com/onsi/ginkgo/v2"
    . "github.com/onsi/gomega"
)

var _ = Describe("Add", func() {
    It("should add two numbers correctly", func() {
        Expect(Add(2, 3)).To(Equal(5))
    })
})
```

- Chạy: `ginkgo`.
- **Ưu**: Test mô tả hành vi, dễ đọc.

## 🔹 Debugging & Profiling

### 191. 🔄 Debugging tools cho Go applications?

- **Delve**: Debugger mạnh mẽ cho Go (xem câu 192).
- **VS Code**: Tích hợp Delve qua extension Go.
- **fmt.Printf**: Debugging cơ bản bằng log.
- **pprof**: Phân tích hiệu suất (CPU, memory).
- **GDB**: Ít phổ biến hơn Delve do hạn chế với goroutines.

### 192. 🚨 Cách sử dụng Delve debugger?

Cài đặt: `go install github.com/go-delve/delve/cmd/dlv@latest`

- **Debug chương trình**:

```bash
dlv debug main.go
```

- **Commands**:
  - `break main.main` (đặt breakpoint).
  - `continue` (chạy đến breakpoint).
  - `print x` (xem giá trị biến).
  - `step` (bước vào hàm).
- **Attach vào process**:

```bash
dlv attach <pid>
```

### 193. 🔍 Logging best practices trong Go?

- **Dùng `log` chuẩn**: Đơn giản, thread-safe.

```go
import "log"
log.Printf("User %d logged in", userID)
```

- **Structured Logging**: Dùng `github.com/sirupsen/logrus`:

```go
import "github.com/sirupsen/logrus"
log.WithFields(logrus.Fields{"user": userID}).Info("Logged in")
```

- **Best practices**:
  - Log level (debug, info, error).
  - Tránh log nhạy cảm (password).
  - Ghi vào file/stdout tùy môi trường.

### 194. 🧰 Tracing trong Go applications?

Dùng `runtime/trace`:

```go
f, _ := os.Create("trace.out")
trace.Start(f)
defer trace.Stop()
// Code cần trace
```

Xem: `go tool trace trace.out`.

- **Thư viện**: OpenTelemetry, Jaeger.

### 195. 🤔 CPU và memory profiling?

- **CPU**:

```go
f, _ := os.Create("cpu.prof")
pprof.StartCPUProfile(f)
defer pprof.StopCPUProfile()
```

- **Memory**:

```go
f, _ := os.Create("mem.prof")
pprof.WriteHeapProfile(f)
```

Xem: `go tool pprof cpu.prof` hoặc `mem.prof`.

### 196. 🔄 Sử dụng pprof cho performance analysis?

Expose qua HTTP:

```go
import _ "net/http/pprof"

http.HandleFunc("/debug/pprof/", pprof.Index)
go http.ListenAndServe(":6060", nil)
```

- Truy cập: `http://localhost:6060/debug/pprof/`.
- Lấy profile: `go tool pprof http://localhost:6060/debug/pprof/profile`.
- Phân tích: `top`, `list`, `web`.

### 197. 🚨 Cách phân tích goroutine dumps?

- Lấy dump:

```go
pprof.Lookup("goroutine").WriteTo(os.Stdout, 1)
```

- Hoặc qua HTTP: `/debug/pprof/goroutine?debug=1`.
- Phân tích: Xem stack trace để tìm goroutine bị chặn.

### 198. 🔍 Flame graphs cho profiling?

1. Lấy CPU profile:

```bash
go tool pprof -http=:8080 cpu.prof
```

2. Dùng tool như `github.com/uber/go-torch`:

```bash
go-torch cpu.prof
```

- Flame graph hiển thị thời gian CPU theo stack.

### 199. 🧠 Runtime metrics collection và visualization?

- **Thu thập**:

```go
var m runtime.MemStats
runtime.ReadMemStats(&m)
fmt.Printf("Alloc = %v MiB", m.Alloc/1024/1024)
```

- **Expose qua Prometheus**:

```go
import "github.com/prometheus/client_golang/prometheus/promhttp"
http.Handle("/metrics", promhttp.Handler())
```

- **Visualization**: Dùng Grafana.

### 200. 🔄 Distributed tracing implementation?

Dùng OpenTelemetry với Jaeger:

```go
import "go.opentelemetry.io/otel"

tp, _ := jaeger.NewJaegerProvider("http://localhost:14268/api/traces")
otel.SetTracerProvider(tp)
tracer := otel.Tracer("my-app")
ctx, span := tracer.Start(context.Background(), "operation")
defer span.End()
```

- Gửi trace qua Jaeger, xem trên UI.

# Monitoring & Observability

## 🔹 Monitoring & Observability

### 201. 🧠 Monitoring strategies cho Go applications?

- **Metrics**: Thu thập CPU, memory, request latency (Prometheus).
- **Logging**: Ghi log chi tiết (structured logging).
- **Tracing**: Theo dõi request qua các dịch vụ (Jaeger, Zipkin).
- **Healthchecks**: Endpoint kiểm tra trạng thái.
- **Alerts**: Thiết lập cảnh báo khi vượt ngưỡng (Grafana, Alertmanager).
- **Best practice**: Kết hợp cả ba (metrics, logs, traces) để quan sát toàn diện.

### 202. 🔄 Prometheus integration trong Go?

Dùng `github.com/prometheus/client_golang`:

```go
import (
    "github.com/prometheus/client_golang/prometheus"
    "github.com/prometheus/client_golang/prometheus/promhttp"
)

var requests = prometheus.NewCounter(prometheus.CounterOpts{
    Name: "http_requests_total",
    Help: "Total number of HTTP requests",
})

func init() { prometheus.MustRegister(requests) }

func main() {
    http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        requests.Inc()
        w.Write([]byte("Hello"))
    })
    http.Handle("/metrics", promhttp.Handler())
    http.ListenAndServe(":8080", nil)
}
```

- Truy cập: `http://localhost:8080/metrics`.

### 203. 🚨 Healthchecks implementation?

Thêm endpoint `/health`:

```go
r := gin.Default()
r.GET("/health", func(c *gin.Context) {
    // Kiểm tra DB, service dependencies nếu cần
    c.JSON(200, gin.H{"status": "healthy"})
})
```

- **Best practice**: Trả về 503 nếu có lỗi (e.g., DB down).

### 204. 🔍 Structured logging trong Go?

Dùng `github.com/sirupsen/logrus`:

```go
import "github.com/sirupsen/logrus"

func main() {
    log := logrus.New()
    log.SetFormatter(&logrus.JSONFormatter{})
    log.WithFields(logrus.Fields{
        "user_id": 123,
        "action":  "login",
    }).Info("User logged in")
}
```

- **Output**: `{"action":"login","level":"info","user_id":123,...}`.

### 205. 🧰 Cách sử dụng OpenTelemetry với Go?

```go
import (
    "go.opentelemetry.io/otel"
    "go.opentelemetry.io/otel/exporters/jaeger"
    "go.opentelemetry.io/otel/sdk/trace"
)

func initTracer() *trace.TracerProvider {
    exporter, _ := jaeger.NewJaegerExporter(jaeger.WithCollectorEndpoint("http://localhost:14268/api/traces"))
    tp := trace.NewTracerProvider(trace.WithBatcher(exporter))
    otel.SetTracerProvider(tp)
    return tp
}

func main() {
    tp := initTracer()
    defer tp.Shutdown(context.Background())
    tracer := otel.Tracer("my-app")
    ctx, span := tracer.Start(context.Background(), "main")
    defer span.End()
}
```

### 206. 🔄 Metrics collection và reporting?

Dùng Prometheus client (xem câu 202) hoặc runtime metrics:

```go
import "runtime"

func collectMetrics() {
    var m runtime.MemStats
    runtime.ReadMemStats(&m)
    fmt.Printf("HeapAlloc: %v bytes\n", m.HeapAlloc)
}
```

- Báo cáo: Expose qua `/metrics`, scrape bằng Prometheus.

### 207. 🤔 Log aggregation và analysis?

- **Aggregation**: Dùng ELK Stack (Elasticsearch, Logstash, Kibana).
- **Go setup**:

```go
logrus.SetOutput(&lumberjack.Logger{
    Filename:   "app.log",
    MaxSize:    100, // MB
    MaxBackups: 3,
})
```

- **Analysis**: Đẩy log vào Elasticsearch, xem qua Kibana.

### 208. 🔍 Alerting systems integration?

Dùng Prometheus + Alertmanager:

1. Thêm rule trong `prometheus.yml`:

```yaml
alerting:
  alertmanagers:
    - static_configs:
        - targets: ["localhost:9093"]
```

2. Định nghĩa alert:

```yaml
groups:
  - name: example
    rules:
      - alert: HighRequestLatency
        expr: http_request_duration_seconds > 1
        for: 5m
```

3. Alertmanager gửi thông báo (email, Slack).

### 209. 🚨 Distributed tracing với Jaeger/Zipkin?

Dùng Jaeger (xem câu 205) hoặc Zipkin:

```go
import "github.com/openzipkin/zipkin-go"

tracer, _ := zipkin.NewTracer(zipkin.NewRecorder("http://localhost:9411/api/v2/spans", "my-app"))
span := tracer.StartSpan("operation")
defer span.Finish()
```

- Xem trace trên UI Zipkin/Jaeger.

### 210. 🧠 Giám sát performance của database queries?

- **Metrics**: Đo thời gian query:

```go
start := time.Now()
rows, _ := db.Query("SELECT * FROM users")
duration := time.Since(start)
prometheus.NewHistogram(prometheus.HistogramOpts{
    Name: "db_query_duration_seconds",
}).Observe(duration.Seconds())
```

- **Tracing**: Thêm span cho query:

```go
ctx, span := tracer.Start(ctx, "db-query")
defer span.End()
db.QueryContext(ctx, "SELECT * FROM users")
```

- **Tool**: Prometheus, pprof, hoặc DB-specific profiling.

## Performance & Optimization

# Application Performance

## 🔹 Application Performance

### 211. 🧠 Common bottlenecks trong Go applications?

- **Memory**: Cấp phát quá nhiều (allocations) trên heap.
- **CPU**: Vòng lặp không tối ưu, string concatenation nặng.
- **I/O**: Chặn goroutines khi đọc/ghi file, DB, network.
- **Concurrency**: Quá nhiều goroutines hoặc data race.
- **Garbage Collection**: GC chạy thường xuyên do allocations lớn.

### 212. 🔄 Memory optimization techniques?

- **Tránh escaping**: Giữ biến trên stack:

```go
func stack() int { x := 42; return x } // Stack
func heap() *int { x := 42; return &x } // Heap
```

- **Dùng `sync.Pool`**: Tái sử dụng object:

```go
var pool = sync.Pool{New: func() interface{} { return &bytes.Buffer{} }}
```

- **Giảm allocations**: Dùng slice với capacity:

```go
s := make([]int, 0, 100) // Pre-allocate
```

### 213. 🚨 CPU optimization trong Go?

- **Tránh reflection**: Thay bằng code tĩnh.
- **Tối ưu vòng lặp**: Giảm công việc dư thừa:

```go
for i := 0; i < len(slice); i++ { /* Cache len */ }
```

- **Dùng `runtime.GOMAXPROCS`**: Tối ưu số thread:

```go
runtime.GOMAXPROCS(runtime.NumCPU())
```

### 214. 🔍 Caching strategies cho performance?

- **In-memory**: Dùng map hoặc `sync.Map`:

```go
cache := make(map[string]string)
cache["key"] = "value"
```

- **Redis**: Cache phân tán (xem câu 172).
- **Memoization**: Lưu kết quả hàm:

```go
var memo = map[int]int{}
func fib(n int) int {
    if v, ok := memo[n]; ok { return v }
    v := fib(n-1) + fib(n-2)
    memo[n] = v
    return v
}
```

### 215. 🧰 I/O bound vs CPU bound optimizations?

- **I/O Bound** (chờ network, DB):
  - Dùng goroutines để xử lý đồng thời.
  - Buffering: `bufio.Reader`, `bufio.Writer`.
- **CPU Bound** (tính toán nặng):
  - Song tuyến hóa với `runtime.GOMAXPROCS`.
  - Tối ưu thuật toán (e.g., dùng binary search thay linear).

### 216. 🔄 Concurrency tuning cho performance?

- **Giới hạn goroutines**: Dùng semaphore:

```go
sem := make(chan struct{}, 10) // Max 10 goroutines
for i := 0; i < 100; i++ {
    sem <- struct{}{}
    go func() { defer func() { <-sem }(); /* work */ }()
}
```

- **Worker Pool**: Xem câu 66.
- **Tối ưu channel**: Buffered nếu cần.

### 217. 🤔 String manipulation efficiency trong Go?

- **Dùng `strings.Builder`**:

```go
var b strings.Builder
for _, s := range []string{"a", "b"} {
    b.WriteString(s) // Ít allocation hơn +
}
```

- **Pre-allocate**: `b.Grow(n)` để tránh resize.
- **Tránh `string` → `[]byte` conversion**: Dùng `[]byte` nếu có thể.

### 218. 🔍 Struct layout optimization?

- **Memory alignment**: Sắp xếp field theo kích thước giảm dần:

```go
type Good struct {
    a int64  // 8 bytes
    b int32  // 4 bytes
    c byte   // 1 byte, padding tối thiểu
}
type Bad struct {
    a byte   // 1 byte + 7 bytes padding
    b int64  // 8 bytes
    c int32  // 4 bytes + 4 bytes padding
}
```

- Kiểm tra: `unsafe.Sizeof`.

### 219. 🚨 Giảm allocations trong hot paths?

- **Dùng buffer tái sử dụng**:

```go
buf := bytes.NewBuffer(make([]byte, 0, 1024))
buf.Reset() // Tái sử dụng
```

- **Tránh slice append không cần thiết**:

```go
s := make([]int, len(data))
copy(s, data) // Thay vì append
```

- **Profiling**: Dùng `pprof` để tìm allocation.

### 220. 🧠 Performance implications của interface usage?

- **Overhead**: Interface gây allocation khi boxing giá trị:

```go
var i interface{} = 42 // Allocation
```

- **Type assertion**: Chậm hơn truy cập trực tiếp.
- **Giải pháp**:
  - Dùng concrete type khi có thể.
  - Tránh interface{} trong hot paths.
- **Kiểm tra**: Dùng benchmark để đo.

### 🔹 Benchmarking & Profiling

221. 🔄 Cách viết benchmarks trong Go?
222. 🚨 Benchmarking best practices?
223. 🔍 Profiling với go tool pprof?
224. 🧰 CPU profiling interpretation?
225. 🤔 Memory profiling và analysis?
226. 🔄 Contention profiling?
227. 🚨 Trace analysis với go tool trace?
228. 🔍 Flame graphs interpretation?
229. 🧠 Continuous performance testing?
230. 🔄 Performance comparison giữa các iterations của code?

### 🔹 Scaling & Load Handling

231. 🧠 Horizontal vs vertical scaling trong Go applications?
232. 🔄 Load balancing strategies?
233. 🚨 Back pressure mechanisms?
234. 🔍 Rate limiting implementations?
235. 🧰 Connection pooling optimization?
236. 🔄 Cách handle thousands of concurrent connections?
237. 🤔 Long-polling vs WebSockets vs Server-Sent Events?
238. 🔍 Database scaling strategies?
239. 🚨 Caching layers và CDN integration?
240. 🧠 Stateless design cho scalability?

## Design Patterns & Architecture

### 🔹 Common Design Patterns

241. 🧠 Dependency injection trong Go?
242. 🔄 Repository pattern implementation?
243. 🚨 Factory pattern trong Go?
244. 🔍 Singleton trong Go context?
245. 🧰 Strategy pattern trong Go?
246. 🔄 Observer pattern và event emitters?
247. 🤔 Decorator pattern trong Go?
248. 🔍 Adapter pattern implementation?
249. 🚨 Command pattern trong Go?
250. 🧠 Builder pattern cho complex objects?

### 🔹 Application Architecture

251. 🔄 Clean Architecture trong Go applications?
252. 🚨 Hexagonal/Ports and Adapters architecture?
253. 🔍 Domain-Driven Design (DDD) trong Go?
254. 🧰 CQRS pattern implementation?
255. 🤔 Event Sourcing trong Go?
256. 🔄 Service-oriented architecture (SOA) principles?
257. 🚨 Layered architecture implementation?
258. 🔍 Functional Core, Imperative Shell pattern?
259. 🧠 Microservices vs modular monolith trade-offs?
260. 🔄 API-first design approach?

### 🔹 Code Organization

261. 🧠 Project layout best practices trong Go?
262. 🔄 Package design principles?
263. 🚨 Cách tổ chức code trong large Go applications?
264. 🔍 Interface design best practices?
265. 🧰 Go modules organization cho multiple services?
266. 🔄 Monorepo vs multi-repo approaches với Go?
267. 🤔 When to use internal packages?
268. 🔍 Managing dependencies giữa modules?
269. 🚨 Versioning strategies cho libraries?
270. 🧠 Code duplication vs abstraction trade-offs?

## DevOps & Deployment

### 🔹 Containerization & Orchestration

271. 🧠 Containerizing Go applications?
272. 🔄 Multi-stage Docker builds cho Go?
273. 🚨 Docker image optimization cho Go apps?
274. 🔍 Container security best practices?
275. 🧰 Kubernetes deployment strategies?
276. 🔄 Health checks và readiness probes?
277. 🤔 Zero-downtime deployments?
278. 🔍 Horizontal Pod Autoscaling với Go metrics?
279. 🚨 Stateful applications trong Kubernetes?
280. 🧠 Sidecar pattern implementations?

### 🔹 CI/CD & Deployment

281. 🔄 CI/CD pipeline cho Go applications?
282. 🚨 Automated testing trong pipelines?
283. 🔍 Deployment strategies (blue-green, canary, etc.)?
284. 🧰 Infrastructure as Code với Go applications?
285. 🤔 Secrets management trong deployments?
286. 🔄 Configuration management best practices?
287. 🚨 Feature flags implementation?
288. 🔍 Rolling back deployments?
289. 🧠 Environment parity issues?
290. 🔄 Deployment artifacts và versioning?

## Security

### 🔹 Application Security

291. 🧠 Common security vulnerabilities trong Go applications?
292. 🔄 Input validation best practices?
293. 🚨 SQL injection prevention?
294. 🔍 XSS prevention trong Go web applications?
295. 🧰 CSRF protection strategies?
296. 🔄 Secure password hashing trong Go?
297. 🤔 Rate limiting for security?
298. 🔍 Dependency vulnerability scanning?
299. 🚨 Secure coding practices trong Go?
300. 🧠 Security headers trong Go web applications?
