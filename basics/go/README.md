# Go Programming Knowledge Guide

## Mục lục
- [Kiến thức cơ bản](#kiến-thức-cơ-bản)
- [Kiến thức trung cấp](#kiến-thức-trung-cấp) 
- [Kiến thức nâng cao](#kiến-thức-nâng-cao)
- [Standard Library quan trọng](#standard-library-quan-trọng)
- [Tài nguyên học tập](#tài-nguyên-học-tập)

## Kiến thức cơ bản

### Cú pháp và kiểu dữ liệu

#### Khai báo biến và hằng số
```go
// Khai báo biến
var name string = "Go"
var age int = 15
var isActive bool = true

// Short declaration
name := "Go"
age := 15

// Khai báo hằng số
const PI = 3.14159
const MaxUsers = 100
```

#### Các kiểu dữ liệu cơ bản
- **Integers**: `int`, `int8`, `int16`, `int32`, `int64`, `uint`, `uint8`, `uint16`, `uint32`, `uint64`
- **Floating point**: `float32`, `float64`
- **String**: `string`
- **Boolean**: `bool`
- **Byte**: `byte` (alias cho uint8)
- **Rune**: `rune` (alias cho int32, đại diện Unicode code point)

#### Collections

**Arrays**
```go
var arr [5]int = [5]int{1, 2, 3, 4, 5}
arr := [...]int{1, 2, 3, 4, 5} // Compiler tự tính size
```

**Slices**
```go
slice := []int{1, 2, 3}
slice = append(slice, 4, 5)
subSlice := slice[1:3] // [2, 3]
```

**Maps**
```go
m := make(map[string]int)
m["key1"] = 10

// Hoặc literal syntax
m := map[string]int{
    "apple":  5,
    "banana": 3,
}
```

**Structs**
```go
type Person struct {
    Name string
    Age  int
}

p := Person{Name: "Alice", Age: 30}
```

#### Pointers
```go
var x int = 10
var p *int = &x  // p trỏ đến địa chỉ của x
fmt.Println(*p)  // Dereference pointer, in ra 10
```

### Control Flow

#### Conditional statements
```go
// if/else
if age >= 18 {
    fmt.Println("Adult")
} else if age >= 13 {
    fmt.Println("Teenager") 
} else {
    fmt.Println("Child")
}

// switch
switch day := time.Now().Weekday(); day {
case time.Monday:
    fmt.Println("Monday")
case time.Tuesday, time.Wednesday:
    fmt.Println("Tuesday or Wednesday")
default:
    fmt.Println("Other day")
}
```

#### Loops
```go
// for loop cơ bản
for i := 0; i < 10; i++ {
    fmt.Println(i)
}

// while-like loop
for condition {
    // code
}

// infinite loop
for {
    // code
    if someCondition {
        break
    }
}

// range loop
slice := []int{1, 2, 3, 4, 5}
for index, value := range slice {
    fmt.Printf("Index: %d, Value: %d\n", index, value)
}
```

#### Defer, Panic, Recover
```go
func example() {
    defer fmt.Println("This runs at the end")
    
    if someError {
        panic("Something went wrong")
    }
}

func safeFunction() {
    defer func() {
        if r := recover(); r != nil {
            fmt.Println("Recovered from:", r)
        }
    }()
    
    // Potentially panicking code
}
```

## Kiến thức trung cấp

### Functions

#### Khai báo và sử dụng functions
```go
// Function cơ bản
func add(a, b int) int {
    return a + b
}

// Multiple return values
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, errors.New("division by zero")
    }
    return a / b, nil
}

// Named return values
func calculate(a, b int) (sum, product int) {
    sum = a + b
    product = a * b
    return // Naked return
}
```

#### Variadic functions
```go
func sum(numbers ...int) int {
    total := 0
    for _, num := range numbers {
        total += num
    }
    return total
}

result := sum(1, 2, 3, 4, 5)
```

#### Anonymous functions và Closures
```go
// Anonymous function
func() {
    fmt.Println("Anonymous function")
}()

// Closure
func makeCounter() func() int {
    count := 0
    return func() int {
        count++
        return count
    }
}

counter := makeCounter()
fmt.Println(counter()) // 1
fmt.Println(counter()) // 2
```

#### Methods và Receivers
```go
type Rectangle struct {
    Width, Height float64
}

// Value receiver
func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

// Pointer receiver
func (r *Rectangle) Scale(factor float64) {
    r.Width *= factor
    r.Height *= factor
}
```

### Structs và Interfaces

#### Embedded Structs
```go
type Person struct {
    Name string
    Age  int
}

type Employee struct {
    Person    // Embedded struct
    ID       int
    Salary   float64
}

emp := Employee{
    Person: Person{Name: "John", Age: 30},
    ID:     123,
    Salary: 50000,
}
```

#### Interfaces
```go
type Shape interface {
    Area() float64
    Perimeter() float64
}

type Circle struct {
    Radius float64
}

func (c Circle) Area() float64 {
    return math.Pi * c.Radius * c.Radius
}

func (c Circle) Perimeter() float64 {
    return 2 * math.Pi * c.Radius
}

// Circle implement Shape interface
var s Shape = Circle{Radius: 5}
```

#### Empty Interface
```go
func printAnything(v interface{}) {
    fmt.Println(v)
}

// Type assertion
func processValue(v interface{}) {
    switch val := v.(type) {
    case string:
        fmt.Println("String:", val)
    case int:
        fmt.Println("Integer:", val)
    default:
        fmt.Println("Unknown type")
    }
}
```

### Package System

#### Tạo và Import Packages
```go
// File: math/calculator.go
package math

// Exported function (starts with capital letter)
func Add(a, b int) int {
    return a + b
}

// Unexported function
func subtract(a, b int) int {
    return a - b
}
```

```go
// File: main.go
package main

import (
    "fmt"
    "yourmodule/math"
)

func main() {
    result := math.Add(5, 3)
    fmt.Println(result)
}
```

#### Module System
```bash
# Initialize module
go mod init mymodule

# Add dependencies
go get github.com/gorilla/mux

# Update dependencies
go mod tidy
```

## Kiến thức nâng cao

### Concurrency

#### Goroutines
```go
func sayHello() {
    fmt.Println("Hello from goroutine")
}

func main() {
    go sayHello() // Start goroutine
    
    // Anonymous goroutine
    go func() {
        fmt.Println("Anonymous goroutine")
    }()
    
    time.Sleep(time.Second) // Wait for goroutines
}
```

#### Channels
```go
// Unbuffered channel
ch := make(chan int)

// Buffered channel
bufferedCh := make(chan int, 5)

// Sending and receiving
go func() {
    ch <- 42 // Send value
}()

value := <-ch // Receive value

// Channel direction
func sender(ch chan<- int) {  // Send-only
    ch <- 42
}

func receiver(ch <-chan int) { // Receive-only
    value := <-ch
    fmt.Println(value)
}
```

#### Select Statement
```go
func main() {
    ch1 := make(chan string)
    ch2 := make(chan string)
    
    go func() {
        time.Sleep(1 * time.Second)
        ch1 <- "Channel 1"
    }()
    
    go func() {
        time.Sleep(2 * time.Second)
        ch2 <- "Channel 2"
    }()
    
    select {
    case msg1 := <-ch1:
        fmt.Println(msg1)
    case msg2 := <-ch2:
        fmt.Println(msg2)
    case <-time.After(3 * time.Second):
        fmt.Println("Timeout")
    }
}
```

#### Mutex và Sync Package
```go
import "sync"

var (
    counter int
    mutex   sync.Mutex
    wg      sync.WaitGroup
)

func increment() {
    defer wg.Done()
    mutex.Lock()
    counter++
    mutex.Unlock()
}

func main() {
    for i := 0; i < 100; i++ {
        wg.Add(1)
        go increment()
    }
    wg.Wait()
    fmt.Println("Counter:", counter)
}
```

#### Context Package
```go
import "context"

func worker(ctx context.Context) {
    for {
        select {
        case <-ctx.Done():
            fmt.Println("Worker cancelled")
            return
        default:
            // Do work
            time.Sleep(100 * time.Millisecond)
        }
    }
}

func main() {
    ctx, cancel := context.WithTimeout(context.Background(), 2*time.Second)
    defer cancel()
    
    go worker(ctx)
    
    time.Sleep(3 * time.Second)
}
```

### Error Handling

#### Error Interface
```go
type error interface {
    Error() string
}

// Custom error
type MyError struct {
    Code    int
    Message string
}

func (e MyError) Error() string {
    return fmt.Sprintf("Error %d: %s", e.Code, e.Message)
}

func doSomething() error {
    return MyError{Code: 404, Message: "Not found"}
}
```

#### Error Wrapping (Go 1.13+)
```go
import "errors"

func processFile(filename string) error {
    err := openFile(filename)
    if err != nil {
        return fmt.Errorf("failed to process file %s: %w", filename, err)
    }
    return nil
}

// Check for specific error
var pathError *os.PathError
if errors.As(err, &pathError) {
    fmt.Println("Path error:", pathError.Path)
}

// Check if error is a specific type
if errors.Is(err, os.ErrNotExist) {
    fmt.Println("File does not exist")
}
```

### Testing

#### Unit Testing
```go
// File: math_test.go
package math

import "testing"

func TestAdd(t *testing.T) {
    result := Add(2, 3)
    expected := 5
    
    if result != expected {
        t.Errorf("Add(2, 3) = %d; want %d", result, expected)
    }
}

func TestAddWithSubtests(t *testing.T) {
    tests := []struct {
        name string
        a, b int
        want int
    }{
        {"positive", 2, 3, 5},
        {"negative", -2, -3, -5},
        {"zero", 0, 5, 5},
    }
    
    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            if got := Add(tt.a, tt.b); got != tt.want {
                t.Errorf("Add(%d, %d) = %d, want %d", tt.a, tt.b, got, tt.want)
            }
        })
    }
}
```

#### Benchmarks
```go
func BenchmarkAdd(b *testing.B) {
    for i := 0; i < b.N; i++ {
        Add(2, 3)
    }
}
```

#### Test Coverage
```bash
# Run tests with coverage
go test -cover

# Generate coverage report
go test -coverprofile=coverage.out
go tool cover -html=coverage.out
```

## Standard Library quan trọng

### fmt Package
```go
import "fmt"

// Print functions
fmt.Print("Hello")
fmt.Println("Hello")
fmt.Printf("Hello %s, age %d\n", "Alice", 30)

// Sprintf for string formatting
message := fmt.Sprintf("User: %s", username)

// Scan functions
var name string
var age int
fmt.Scanf("%s %d", &name, &age)
```

### strings Package
```go
import "strings"

// String operations
s := "Hello World"
fmt.Println(strings.Contains(s, "World"))  // true
fmt.Println(strings.ToUpper(s))            // "HELLO WORLD"
fmt.Println(strings.Split(s, " "))         // ["Hello", "World"]
fmt.Println(strings.Join([]string{"a", "b", "c"}, "-")) // "a-b-c"
```

### strconv Package
```go
import "strconv"

// String to number
i, err := strconv.Atoi("42")
f, err := strconv.ParseFloat("3.14", 64)

// Number to string
s := strconv.Itoa(42)
s := strconv.FormatFloat(3.14, 'f', 2, 64)
```

### io và os Packages
```go
import (
    "io"
    "os"
)

// File operations
file, err := os.Open("data.txt")
if err != nil {
    log.Fatal(err)
}
defer file.Close()

data, err := io.ReadAll(file)
if err != nil {
    log.Fatal(err)
}

// Write to file
err = os.WriteFile("output.txt", []byte("Hello World"), 0644)
```

### net/http Package
```go
import (
    "net/http"
    "fmt"
    "log"
)

// Simple HTTP server
func handler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Hello, %s!", r.URL.Path[1:])
}

func main() {
    http.HandleFunc("/", handler)
    log.Fatal(http.ListenAndServe(":8080", nil))
}

// HTTP client
resp, err := http.Get("https://api.example.com/data")
if err != nil {
    log.Fatal(err)
}
defer resp.Body.Close()

body, err := io.ReadAll(resp.Body)
```

### encoding/json Package
```go
import "encoding/json"

type Person struct {
    Name string `json:"name"`
    Age  int    `json:"age"`
}

// Marshal (Go to JSON)
p := Person{Name: "Alice", Age: 30}
jsonData, err := json.Marshal(p)

// Unmarshal (JSON to Go)
jsonStr := `{"name":"Bob","age":25}`
var person Person
err := json.Unmarshal([]byte(jsonStr), &person)
```

### database/sql Package
```go
import (
    "database/sql"
    _ "github.com/lib/pq" // PostgreSQL driver
)

db, err := sql.Open("postgres", "user=username dbname=mydb")
if err != nil {
    log.Fatal(err)
}
defer db.Close()

// Query
rows, err := db.Query("SELECT name, age FROM users WHERE age > $1", 18)
if err != nil {
    log.Fatal(err)
}
defer rows.Close()

for rows.Next() {
    var name string
    var age int
    err := rows.Scan(&name, &age)
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("Name: %s, Age: %d\n", name, age)
}
```

## Tài nguyên học tập

### Documentation chính thức
- [Go Documentation](https://golang.org/doc/)
- [Go by Example](https://gobyexample.com/)
- [Effective Go](https://golang.org/doc/effective_go.html)

### Tools hữu ích
```bash
# Format code
go fmt

# Vet code for potential issues
go vet

# Run tests
go test

# Build executable
go build

# Install dependencies
go mod tidy

# Check for vulnerabilities
go list -json -deps | nancy sleuth
```

### Best Practices
- Luôn handle errors properly
- Sử dụng gofmt để format code
- Viết tests cho code của bạn
- Sử dụng interfaces để design loose coupling
- Prefer composition over inheritance
- Keep functions small và focused
- Sử dụng meaningful var