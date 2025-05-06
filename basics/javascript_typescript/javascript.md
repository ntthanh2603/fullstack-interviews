# Danh sách những câu hỏi trong phỏng vấn Javascript



<p align="center">
  <img src="https://github.com/tuantvk/javascript-interview-questions-developer/blob/master/assets/background.png" alt="background" />
<p>

Những lý thuyết này cung cấp cho bạn danh sách các câu hỏi phỏng vấn JavaScript thường được hỏi với câu trả lời cho người mới bắt đầu. Các câu hỏi không sắp xếp theo thứ tự khó dần lên bạn có thể lướt hết qua mọi thứ để thử thách bản thân mình.

Nếu bạn có câu hỏi phỏng vấn JavaScript nào hay thì chia sẻ cho mọi người bằng cách bằng cách tạo **issue** hoặc **pull request** cho mình :rocket:.

Danh sách các ngôn ngữ khác:

- [English](./README_EN.md)

Let's go !!!

## Câu 0: Javascript là gì?

**JavaScript**, theo phiên bản hiện hành, là một ngôn ngữ lập trình thông dịch được phát triển từ các ý niệm nguyên mẫu. Ngôn ngữ này được dùng rộng rãi cho các trang web (phía người dùng) cũng như phía máy chủ (với **Nodejs**).

---

## Câu 1: Sự khác nhau giữa JavaScript và JScript là gì?

Đơn giản bạn có thể nói JScript giống như JavaScript, nhưng nó được cung cấp bởi Microsoft.

---

## Câu 2: Trong javascript đối tượng window được sử dụng để làm gì?

Đối tượng window được tạo tự động bởi trình duyệt đại diện cho một cửa sổ trình duyệt. Nó được sử dụng để hiển thị hộp thoại bật lên như hộp thoại alert, confirm, v.v. và mọi thứ trong Javascript như **object, functions, variables** đều có thể trở thành **window object** bao gồm cả **HTML DOM**.

<details><summary><b>Ví dụ:</b></summary>
<p>
<code>
window.document.getElementById("header");
</code><br />
hoặc:
<br />
<code>
document.getElementById("header");
</code>

<p>
</details>

---

## Câu 3: Sự khác nhau giữa == và === là gì?

Toán tử **==** chỉ kiểm tra tính bằng nhau, còn **===** kiểm tra tính bằng nhau và giá trị kiểu dữ liệu tức là phải cùng kiểu dữ liệu.

#### Ví dụ:

```javascript
var number1 = 12;
var number2 = "12";

console.log("log 1: ", number1 == number2);
console.log("log 2: ", number1 === number2);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>log 1: true</code><br />
<code>log 2: false</code>
<p>
</details>

---

## Câu 4: Negative Infinity là gì?

**Negative Infinity** là một số trong JavaScript có thể được bắt nguồn bằng cách chia số âm cho 0. Khi nào sử dụng nó, khi nó là **một số** hoặc **Number object** và nó sẽ return về **undefined**.

#### Ví dụ:

```javascript
var my_number = 100;
my_number.NEGATIVE_INFINITY;

console.log(my_number);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>undefined</code>
<p>
</details>

---

## Câu 5: Cách để xử lý các ngoại lệ trong JavaScript?

Sử dụng khối **try/catch**, chúng ta có thể xử lý các ngoại lệ trong JavaScript. JavaScript hỗ trợ các từ khóa try, catch, finally, throw để xử lý ngoại lệ.

#### Ví dụ:

```javascript
function check(x) {
  try {
    if (x == "") throw "empty";
    if (isNaN(x)) throw "not a number";
    x = Number(x);
    if (x < 5) throw "too low";
    if (x > 10) throw "too high";
  } catch (err) {
    console.log("catch ", err);
  }
}

console.log(check(""));
console.log(check("test"));
console.log(check(55));
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>catch  empty</code><br />
<code>undefined</code><br />
<code>catch  not a number</code><br />
<code>undefined</code><br />
<code>catch  too high</code><br />
<code>undefined</code><br />
<p>
</details>

---

## Câu 6: Hàm isNaN() là gì?

Hàm **isNaN()** trả về true nếu giá trị của biến **không phải** là một số.

#### Ví dụ:

```javascript
console.log(isNaN(123));
console.log(isNaN(0));
console.log(isNaN("Hello"));
console.log(isNaN("2005/12/12"));
console.log(isNaN(""));
console.log(isNaN(true));
console.log(isNaN(undefined));
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>false</code><br />
<code>false</code><br />
<code>true</code><br />
<code>true</code><br />
<code>false</code><br />
<code>false</code><br />
<p>
</details>

---

## Câu 7: Cách để comment trong Javascript?

Sử dụng **//** cho một dòng

hoặc

sử dụng **/_ nội dung _/** cho nhiều dòng

#### Ví dụ:

```javascript
// Khai báo biến
var number = 2;

/*
  Đây là cách
  để comment
  nhiều dòng
*/
var girl_friend = null;
```

---

## Câu 8: Tại sao 0.1 + 0.2 không bằng 0.3 ?

Vấn đề này liên quan đến việc Javascript lưu trữ dữ liệu float ở dạng nhị phân chính xác tới từng con số sau dấu phẩy.

**Giải pháp:**

- Sử dụng hàm **toFixed()**
- **Mẹo nhỏ** là nhân với 10 và chia cho 10
- Tham khảo các hàm làm tròn như **round()**, v.v.

#### Ví dụ:

```javascript
console.log(0.1 + 0.2);

// Amazing !
var x = (0.2 * 10 + 0.1 * 10) / 10;
console.log(x);

// Sử dụng toFixed()
var number = 0.1 + 0.2;
console.log(number.toFixed(2));
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>0.30000000000000004</code><br />
<code>0.3</code><br />
<code>0.3</code>
<p>
</details>

---

## Câu 9: Phân biệt giữa Function Declaration và Function Expression.

**Function declaration** sử dụng từ khóa function rồi đến tên hàm. Còn **Function expression** bắt đầu bằng **var**, **let** hoặc **const**, theo sau là tên của hàm và toán tử **=**.

#### Ví dụ:

```javascript
// Function Declaration
function sum(x, y) {
  return x + y;
}

// Function Expression: ES5
var sum = function (x, y) {
  return x + y;
};
// Function Expression: ES6+
const sum = (x, y) => {
  return x + y;
};
```

---

## Câu 10: Tại sao Math.max() lại nhỏ hơn Math.min().

Khi chạy code **Math.max() > Math.min()**, giá trị trả về là **False**, nghe có vẻ không hợp lý. Tuy nhiên, nếu không có tham số nào được truyền vào, **Math.min()** trả về **Infinity** và **Math.max()** trả về **-Infinity**. Vậy nên **Math.max() < Math.min()**.

Nếu tham số xuất hiện là infinity và một số nào khác, kết quả trả về sẽ là số có giá trị đó.

#### Ví dụ:

```javascript
var infinity = 5;

var value1 = Math.min(1);
var value2 = Math.min(1, infinity);
var value3 = Math.min(1, -infinity);

console.log(value1);
console.log(value2);
console.log(value3);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>1</code><br />
<code>1</code><br />
<code>-5</code><br />
<p>
</details>

---

## Câu 11: Closure trong Javascript là gì?

Dể hiểu, closure là 1 hàm nội truy cập đến các biến bên ngoài phạm vi của nó. Closure có thể được sử dụng để **implement privacy** và tạo ra các **function factory**.

#### Ví dụ:

```javascript
const arr = [1, 2, 3, 4];

for (var i = 0; i < arr.length; i++) {
  setTimeout(function () {
    console.log(i);
  }, 10);
}
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>4</code><br />
<code>4</code><br />
<code>4</code><br />
<code>4</code><br />

Lý do là bởi vì hàm setTimeout sẽ tạo ra 1 function (closure) có thể truy cập phạm vi bên ngoài nó, vòng loop sẽ chứa index i. Sau 10ms, hàm được thực thi và nó sẽ log ra giá trị của i, là giá trị cuối cùng của vòng lặp (4).

<p>
</details>

---

## Câu 12: Hàm encodeURI() là gì?

Hàm này mã hóa các ký tự đặc biệt, ngoại trừ :, /? : @ & = + $ #

Để mã hóa ngược chuỗi đó lại mình sử dụng hàm **decodeURI()**.

#### Ví dụ:

```javascript
var uri = "my test.asp?name=ståle&car=saab";
var res = encodeURI(uri);

console.log(res);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>my%20test.asp?name=st%C3%A5le&car=saab</code><br />
<p>
</details>

---

## Câu 13: Array() khác với [] như nào trong khi tạo ra một array trong JavaScript?

Nếu sử dụng cách tạo array initializer nó sẽ tạo ra danh sách các phần tử trong mảng và được ngăn cách bởi dấu phẩy.

#### Ví dụ:

```javascript
var arr1 = [5];
var arr2 = new Array(5);

console.log(arr1);
console.log(arr2);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>[ 5 ]</code><br />
<code>[ <5 empty items> ]</code>
<p>
</details>

---

## Câu 14: Strict mode trong JavaScript là gì?

Strict theo nghĩa tiếng Việt là "nghiêm khắc". Strict Mode là một quy mẫu nghiêm khắc trong Javascript. Nếu như việc viết code bình thường là Normal mode, thì Strict Mode sẽ có thêm các quy định khác so với Normal mode.

#### Ví dụ:

```javascript
"use strict";

function foo() {
  var bar = 0;
  return bar;
}

bar = 1;
```

<details><summary><b>Đáp án:</b></summary>
<p>
Xảy ra lỗi:
<code>ReferenceError: bar is not defined</code><br />
<p>
</details>

---

## Câu 15: Variable typing trong JavaScript là gì?

JavaScript là một ngôn ngữ rất lỏng lẻo. Biến chỉ được xác định khi giá trị được gán và có thể thay đổi khi biến xuất hiện trong các ngữ cảnh khác nhau. Lên đơn giản nó là kiểu dữ liệu của biến đó.

Để kiểm tra kiểu dữ liệu của biến đó ta dùng **typeof** trong Javascript.

#### Ví dụ:

```javascript
var length = 16;
var lastName = "Johnson";
var x = { firstName: "John", lastName: "Doe" };

console.log(typeof length);
console.log(typeof lastName);
console.log(typeof x);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>number</code><br />
<code>string</code><br />
<code>object</code>
<p>
</details>

---

## Câu 16: Các kiểu dữ liệu trong Javascript là gì?

Javascript có những kiểu dữ liệu sau:

- Number
- String
- Boolean
- Object
- Undefined

#### Ví dụ:

```javascript
var a = 5;
var b = "Hello";
var c = true;
var d = { id: 1, name: "Lyly" };

console.log(typeof a);
console.log(typeof b);
console.log(typeof c);
console.log(typeof d);
console.log(typeof e);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>number</code><br />
<code>string</code><br />
<code>boolean</code><br />
<code>object</code><br />
<code>undefined</code>
<p>
</details>

---

## Câu 17: `this` trong Javascript là gì?

Từ khóa **this** dùng để chỉ đối tượng từ nơi nó được gọi.

#### Ví dụ:

```javascript
var Student = {
  name: "Lyly",
  age: 20,
  getName: function () {
    return this.name;
  },
};

console.log(Student.getName());
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>Lyly</code><br />
<p>
</details>

---

## Câu 18: Khác nhau giữa ViewState và SessionState là gì?

- **ViewState** là dành riêng cho một trang trong phiên.

- **SessionState** dành riêng cho dữ liệu cụ thể của người dùng có thể được truy cập trên tất cả các trang trong ứng dụng web.

---

## Câu 19: Làm sao để thay đổi style/class của element?

Có thể sử dụng document để thay đổi style/class.

#### Ví dụ:

```javascript
document.getElementById("myId").style.fontSize = "20px";

// or

document.getElementById("myId").className = "newclass";
```

---

## Câu 20: Các cấu trúc lặp trong Javascript là gì?

Có các vòng lặp sau:

- for
- while
- do-while loops

#### Ví dụ:

```javascript
var arr = ["apple", "banana", "mango", "cherry"];

// Sử dụng for
for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]);
}

// Sử dụng while
let j = 0;
while (j < arr.length) {
  console.log(arr[j]);
  j++;
}

// Sử dụng do-while loops
let k = 0;
do {
  console.log(arr[k]);
  k++;
} while (k < arr.length);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>cherry</code><br />
<code>apple</code><br />
<code>banana</code><br />
<code>mango</code><br />
<p>
</details>

---

## Câu 21: Kết quả của `5 + 2 + "7"` là gì?

Vì 3 và 2 là số nguyên, chúng sẽ cộng vào với nhau và kết quả là số. Còn 7 là một chuỗi, nên Javascipt sẽ hiểu thành nối chuỗi. Vì vậy, kết quả sẽ là 77.

#### Ví dụ:

```javascript
console.log(5 + 2 + "7");
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>77</code><br />
<p>
</details>

---

## Câu 22: Chức năng của `delete` là gì?

**delete** dùng để xóa các property cũng như các giá trị.

#### Ví dụ:

```javascript
var student = { name: "Lyly", age: 20 };

delete student.age;

console.log(student);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>{ name: 'Lyly' }</code><br />
<p>
</details>

---

## Câu 23: Hàm `pop()` trong Javascript để làm gì?

**pop()** trong Javascript dùng để lấy phần tử cuối cùng trong mảng. Điều này thì trái ngược với hàm **shift()**.

#### Ví dụ:

```javascript
var number = ["one", "two", "three", "four"];

console.log(number.pop());
console.log(number.shift());
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>four</code><br />
<code>one</code>
<p>
</details>

---

## Câu 24: Kết quả in ra là gì?

#### Ví dụ:

```javascript
var myArray = [[[]]];

console.log(myArray);
```

<details><summary><b>Đáp án:</b></summary>
<p>
Là một mảng 3 chiều
<code>[ [ [] ] ]</code><br />
<p>
</details>

---

## Câu 25: `let` và `const` trong Javascript là gì?

Từ khóa **let** & **const** được giới thiệu trong phiên bản ES6 với tầm nhìn tạo ra hai loại biến khác nhau trong javascript, một loại là bất biến và loại khác là có thể thay đổi.

- **const**: Nó được sử dụng để tạo ra một biến bất biến. Biến không thay đổi là các biến có giá trị không bao giờ thay đổi trong vòng đời hoàn chỉnh của chương trình.

- **let**: let được sử dụng để tạo một biến có thể thay đổi. Các biến có thể thay đổi là các biến bình thường như var có thể thay đổi bất kỳ số lượng thời gian nào.

#### Ví dụ:

```javascript
let name = "Lyly";

const age = 18;

name = "John";
age = 20;

console.log(name);
console.log(age);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>John</code><br />
<code>TypeError: Assignment to constant variable.</code>
<p>
</details>

---

## Câu 26: Làm sao để thêm hoặc xóa sửa trong object Javascript?

Ta có thể thêm một thuộc tính vào một đối tượng bằng **object.property_name = value**, **delete object.property_name** để xóa một thuộc tính.

#### Ví dụ:

```javascript
let user = new Object();

user.name = "Lyly";
user.age = 20;

console.log(user);

delete user.age;

console.log(user);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>{ name: 'Lyly', age: 20 }</code><br />
<code>{ name: 'Lyly' }</code>
<p>
</details>

---

## Câu 27: Cách để xóa các phần tử giống nhau trong mảng sử dụng ES6?

Dưới đây là một số cách:

#### Ví dụ:

```javascript
var array = [1, 2, 6, 5, 3, 2, 6];

// Sử dụng Set
console.log(...new Set(array));

// Sử dụng filter
console.log(array.filter((item, index) => array.indexOf(item) === index));

// Sử dụng reduce
console.log(
  array.reduce(
    (uniq, item) => (uniq.includes(item) ? uniq : [...uniq, item]),
    []
  )
);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code></code><br />
<code>1 2 6 5 3</code><br />
<code>[ 1, 2, 6, 5, 3 ]</code><br />
<code>[ 1, 2, 6, 5, 3 ]</code>
<p>
</details>

---

## Câu 28: Khác nhau giữa từ khóa `undefined` và `null` là gì?

Khi bạn khởi tạo ra một biến nhưng không gán giá trị cho nó thì sẽ là **undefined**. Còn **null** là một **object**.

#### Ví dụ:

```javascript
var a;

console.log(typeof a);
console.log(typeof null);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>undefined</code><br />
<code>object</code>
<p>
</details>

---

## Câu 29: Một số Framework để test Javascript là gì?

Các framework phổ biến nhất hiện nay:

- Unit.js
- Jasmine
- Karma
- Chai
- AVA
- Mocha
- JSUnit
- QUnit
- Jest

#### Ví dụ:

```javascript
// Sử dụng Chai
var answer = 43;

expect(answer).to.equal(42);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>AssertionError: expected 43 to equal 42.</code><br />
<p>
</details>

---

## Câu 30: `export` và `import` là gì?

**export** hay **import** là cách để ta tạo ra các module trong Javascript. Bằng cách đó, ta có thể chia các phần nhỏ trong dự án để dễ quản lý. **import** cho phép ta lấy một số biến hoặc một phương thức nào đó của file. Còn **export** là biến một file thành một module. Xem ví dụ để hiểu hơn.

#### Ví dụ:

```javascript
// person.js

let name = "Lyly",
  occupation = "developer",
  age = 20;

export { name, age };

//index.js

import { name, age } from "./person";

console.log(name);
console.log(age);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>Lyly</code><br />
<code>20</code>
<p>
</details>

---

## Câu 31: Làm sao để chuyển đổi ngày trong Javascript thành tiêu chuẩn ISO?

Hàm **toISOString()** được sử dụng để chuyển đổi ngày javascript thành tiêu chuẩn ISO. Nó chuyển đổi đối tượng Ngày JavaScript thành một chuỗi, sử dụng tiêu chuẩn ISO.

#### Ví dụ:

```javascript
var date = new Date();
var n = date.toISOString();

console.log(n);
// YYYY-MM-DDTHH:mm:ss.sssZ
```

---

## Câu 32: Cách để clone một object trong Javascript?

Hàm **Object.assign()** được sử dụng để clone một đối tượng trong Javascript. Ngoài ra bạn cũng có thể sử dụng clone của **lodash**. Lodash là một framework sử lý mạnh mẽ mảng và object. Xem thêm tại [Lodash](https://lodash.com/)

#### Ví dụ:

```javascript
var x = { name: "Lyly" };
var y = Object.assign({}, x);

console.log(y);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>{ name: 'Lyly' }</code><br />
<p>
</details>

---

## Câu 33: Cách để tạo mảng trong Javascript?

Có 3 cách khác nhau để tạo mảng trong Javascript. Xem ví dụ

#### Ví dụ:

```javascript
var arr1 = [1, 2, 3, 4];

var arr2 = new Array();

var arr3 = new Array(1, 2, 3, 4);

console.log(arr1);
console.log(arr2);
console.log(arr3);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>[ 1, 2, 3, 4 ]</code><br />
<code>[]</code><br />
<code>[ 1, 2, 3, 4 ]</code>
<p>
</details>

---

## Câu 34: Các sự kiện chuột HTML DOM là gì?

Một số sự kiện chuột trong DOM như:

- onclick
- ondblclick
- mousemove
- mousedown
- mouseover
- mouseout
- mouseup

---

## Câu 35: Giá trị in ra màn hình là gì?

#### Ví dụ:

```javascript
console.log(undefined * 2);

console.log(null * 2);

console.log("" * 2);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>NaN</code><br />
<code>0</code><br />
<code>0</code>
<p>
</details>

---

## Câu 36: Tại sao `018 - 017 = 3` trong Javascript?

Việc 018 - 017 trả về 3 là kết quả của chuyển đổi loại im lặng. Trong trường hợp này, ta nói về số bát phân.

#### Ví dụ:

```javascript
console.log(018 - 017);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>3</code><br />
<p>
</details>

---

## Câu 37: Phân biệt giữa test() và exec()?

Cả **test()** và **exec()** đều là biểu thức **RegExp**. [Xem chi tiết](https://developer.mozilla.org/vi/docs/Web/JavaScript/Reference/Global_Objects/RegExp)

- Sử dụng **test()** sẽ search chuỗi trong theo giá trị ta truyền vô, nếu chuỗi đó tồn tại thì sẽ return về Boolean giá trị là 'true' hoặc 'false'.

- Sử dụng **exec()** sẽ search chuỗi trong theo giá trị ta truyền vô, nếu chuỗi đó tồn tại thì sẽ return về chuỗi đó, nếu không sẽ return về giá trị 'null'.

#### Ví dụ:

```javascript
var str = "The best things in life are free";
var patt = new RegExp("b");

var res_test = patt.test(str);
var res_exec = patt.exec(str);

console.log(res_test);
console.log(res_exec);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>true</code><br />
<code>[ 'b', index: 4, input: 'The best things in life are free' ]</code>
<p>
</details>

---

## Câu 38: Kết quả in ra là gì?

#### Ví dụ:

```javascript
setTimeout(function () {
  console.log("first line");
}, 0);

console.log("second line");

console.log("third line");
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>second line</code><br />
<code>third line</code><br />
<code>first line</code>

Khi có setTimeout() tiến trình trở thành bất đồng bộ. Ta cần chờ mọi thứ trong stack hoàn thành trước.

<p>
</details>

---

## Câu 39: Các cách để accessed vào HTML element trong Javascript?

Có những cách sau:

- **getElementById()** lấy một element bằng tên **id**.
- **getElementsByClass()** lấy một element bằng tên **class**.
- **getElementsByTagName()** lấy một element bằng tên của **tag name**.
- **querySelector()** đây là function css style selector và sẽ return về giá trị đầu tiên.

#### Ví dụ:

```html
<!DOCTYPE html>
<html>
  <body>
    <p id="demo">Click the button to change the text in this paragraph.</p>
    <p class="demo">Click the button to change the text in this paragraph.</p>
    <h5>Click the button to change the text in this paragraph.</h5>
    <p class="example">
      Click the button to change the text in this paragraph.
    </p>

    <script>
      // Sử dụng getElementById()
      document.getElementById("demo").innerHTML = "Hello World";

      // Sử dụng getElementsByClassName()
      document.getElementsByClassName("demo")[0].innerHTML = "Hello World!";

      // Sử dụng getElementsByTagName()
      document.getElementsByTagName("h5")[0].innerHTML = "Hello World!";

      // Sử dụng querySelector()
      document.querySelector(".example").style.backgroundColor = "red";
    </script>
  </body>
</html>
```

---

## Câu 40: Có mấy cách sử dụng Javascript trong HTML?

Có 3 cách sau:

- Inline
- Internal
- External

#### Ví dụ:

```html
<!-- Inline -->
<a href="#" onclick="(function(){alert('Hello world');})()">Click Me</a>

<!-- Internal -->
<script>
console.log('Hello world')
</script>

<!-- External -->
<script type="text/javascript" src="external.js">

// file external.js
console.log('Hello world')
```

---

## Câu 41: Một số framework UI của Javascript là gì?

Một số framework UI nổi tiếng của Javascript hiện nay là:

- React
- Angular
- Vue
- Meteor
- Ember

#### Ví dụ:

```javascript
// React
class HelloMessage extends React.Component {
  render() {
    return <div>Hello {this.props.name}</div>;
  }
}

ReactDOM.render(
  <HelloMessage name="Taylor" />,
  document.getElementById("hello-example")
);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>Hello Taylor</code><br />
<p>
</details>

---

## Câu 42: Kết quả in ra là gì?

#### Ví dụ:

```javascript
var a = !+[] + [] + ![];

console.log(a.length);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>9</code><br />
Đây là điều thú vị của Javascript.
<p>
</details>

---

## Câu 43: Sự khác nhau giữa .forEach và .map trong Javascript là gì?

`.forEach`

- Vòng lặp dựa vào các phần tử có trong mảng.
- Thực hiện callback cho mỗi vòng lặp.
- Không trả về giá trị.

`.map`

- Vòng lặp dựa vào các phần tử có trong mảng.
- Hàm map sẽ lặp qua từng phần tử nhưng sẽ tạo ra một mảng mới dựa trên các giá trị trong vòng lặp.

#### Ví dụ:

```javascript
const a = [1, 2, 3];

const ex1 = a.forEach((num, index) => {
  // Làm 1 điều gì đó
});

const ex2 = a.map((num) => {
  return num * 2;
});

console.log(ex1);
console.log(ex2);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>undefined</code><br />
<code>[ 2, 4, 6 ]</code>
<p>
</details>

---

## Câu 44: JSON là gì và cách sử dụng?

JSON là một định dạng dữ liệu dựa trên văn bản theo cú pháp đối tượng JavaScript.

#### Ví dụ:

```javascript
// Chuyển object qua JSON
var obj1 = [
  { id: 1, name: "Lyly" },
  { id: 2, name: "May" },
];

console.log(JSON.stringify(obj1));

// Chuyển JSON về object
var obj2 = '{ "id": 9, "name": "Lyly", "age": "20", "city": "New York" }';

console.log(JSON.parse(obj2));
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>[{"id":1,"name":"Lyly"},{"id":2,"name":"May"}]</code><br />
<code>{ id: 9, name: 'Lyly', age: '20', city: 'New York' }</code>
<p>
</details>

---

## Câu 45: Sự khác nhau giữa `slice` và `splice` là gì?

| slice                                     | splice                                      |
| ----------------------------------------- | ------------------------------------------- |
| Không làm thay đổi mảng ban đầu           | Có thể bị thay đổi mảng ban đầu             |
| Trả về tập hợp con của mảng ban đầu       | Trả về các phần tử bị xóa khỏi mảng ban đầu |
| Sử dụng để lấy các phần tử con trong mảng | Sử dụng để thêm hoặc xóa phần tử của mảng   |

#### Ví dụ:

```javascript
// Sử dụng slice
var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];

var citrus = fruits.slice(1, 3);

console.log(fruits);
console.log(citrus);

// Sử dụng splice
var fruits = ["Banana", "Orange", "Apple", "Mango"];

fruits.splice(2, 0, "Lemon", "Kiwi");

console.log(fruits);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>[ 'Banana', 'Orange', 'Lemon', 'Apple', 'Mango' ]</code><br />
<code>[ 'Orange', 'Lemon' ]</code><br /><br />
<code>[ 'Banana', 'Orange', 'Lemon', 'Kiwi', 'Apple', 'Mango' ]</code>
<p>
</details>

---

## Câu 46: Higher order function trong Javascript là gì?

Higher order function là hàm chấp nhận hàm khác làm đối số hoặc trả về hàm dưới dạng giá trị trả về.

#### Ví dụ:

```javascript
const higherOrderFunc = () => console.log("Hello world !");

const higherOrder = (ReturnHigherOrderFunc) => ReturnHigherOrderFunc();

higherOrder(higherOrderFunc);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>Hello world !</code><br />
<p>
</details>

---

## Câu 47: Hàm `unary` trong Javascript là gì?

Hàm **unary** (monadic) là một hàm chấp nhận chính xác một đối số. Nó là viết tắt của một đối số được chấp nhận bởi một hàm.

#### Ví dụ:

```javascript
const unaryFunction = (a) => console.log(a + 10);

unaryFunction(5);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>15</code><br />
<p>
</details>

---

## Câu 48: IIFE (Immediately Invoked Function Expression) trong Javascript là gì?

**IIFE (Immediately Invoked Function Expression)** là một hàm JavaScript chạy ngay khi được định nghĩa.

Lý do chính để sử dụng IIFE là để có được quyền riêng tư dữ liệu vì bất kỳ biến nào được khai báo trong IIFE đều không thể được truy cập bởi bên ngoài. Tức là, nếu bạn cố gắng truy cập các biến bằng IIFE thì nó sẽ xuất hiện một lỗi như dưới đây:

#### Ví dụ:

```javascript
// Ví dụ IIFE
(function () {
  // logic here
})();

// Báo lỗi khi chạy
(function () {
  var message = "IIFE";
  console.log(message);
})();

console.log(message);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>ReferenceError: message is not defined</code><br />
<p>
</details>

---

## Câu 49: Làm thể nào để nhận notification từ server-send?

Ta có thể sử dụng **EventSource** để nhận thông báo sự kiện do máy chủ gửi. [Xem chi tiết EventSource](https://developer.mozilla.org/en-US/docs/Web/API/EventSource)

#### Ví dụ:

```javascript
// Ví dụ mẫu
if (typeof EventSource !== "undefined") {
  var source = new EventSource("sse_generator.js");
  source.onmessage = function (event) {
    document.getElementById("output").innerHTML += event.data + "<br>";
  };
}
```

---

## Câu 50: `Promise.all` trong Javascript là gì?

**Promise.all** là một hàm sẽ lấy một loạt các Promises làm đầu vào (có thể lặp lại). Phương thức này nhận vào một mảng các promises và chỉ resolve khi tất cả các promises này hoàn thành, hoặc reject khi một trong số chúng xảy ra lỗi.

#### Ví dụ:

```javascript
Promise.all([Promise1, Promise2, Promise3])
  .then((result) => {
    console.log(result);
  })
  .catch((error) => {
    console.log(`Error in promises ${error}`);
  });
```

---

## Câu 51: `Promise.race` trong Javascript là gì?

Promise.race nghĩa là hàm promise chạy đua (LOL). Phương thức này nhận vào một mảng các promises và sẽ resolve/reject ngay khi một trong số các promises này hoàn thành/xảy ra lỗi.

#### Ví dụ:

```javascript
var promise1 = new Promise(function (resolve, reject) {
  setTimeout(resolve, 500, "one");
});

var promise2 = new Promise(function (resolve, reject) {
  setTimeout(resolve, 100, "two");
});

Promise.race([promise1, promise2]).then(function (value) {
  console.log(value);
});
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>two</code><br />
<p>
</details>

---

## Câu 52: Hàm eval() trong Javascript là gì?

Hàm **eval()** dùng để tính toán một chuỗi trong Javascript. Nó sẽ nhận vào một chuỗi và biến nó qua phép tính. Chuỗi có thể là biểu thức JavaScript, biến, câu lệnh hoặc chuỗi câu lệnh.

#### Ví dụ:

```javascript
var a = "1 + 5 - 3";
var b = "10 / 2" + 6;

console.log(eval(a));
console.log(eval(b));
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>3</code><br />
<code>0.38461538461538464</code>
<p>
</details>

---

## Câu 53: Ví dụ đơn giản để so sánh 2 object với nhau?

Để so sánh 2 object có khá nhiều cách khác nhau. Tuy nhiên có một cách rất đơn giản đó là parse qua JSON bằng cách sử dụng **JSON.sstringify()**.

#### Ví dụ:

```javascript
var user1 = { name: "Lyly", org: "dev" };
var user2 = { name: "Lyly", org: "dev" };

var dog = { name: "dog", age: 10 };
var cat = { name: "cat", age: 10 };

var compare_user = JSON.stringify(user1) === JSON.stringify(user2);
var compare_animal = JSON.stringify(dog) === JSON.stringify(cat);

console.log(compare_user);
console.log(compare_animal);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>true</code><br />
<code>false</code>
<p>
</details>

---

## Câu 54: Khác nhau giữa parameter và argument là gì?

Các **parameter** là tên biến của định nghĩa hàm, trong khi các **argument** là các giá trị được cung cấp cho hàm khi nó được gọi.

#### Ví dụ:

```javascript
function myFunction(parameter1, parameter2) {
  console.log(arguments[0]);
}
myFunction("argument1", "argument2");
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>argument1</code><br />
<p>
</details>

---

## Câu 55: Kết quả trả về là gì?

#### Ví dụ:

```javascript
const a = [1, 2, 3];
const b = [1, 2, 3];
const c = "1,2,3";

console.log(a == c);
console.log(a == b);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>true</code><br />
<code>false</code><br /><br />
console.log đầu tiên sẽ trả về là đúng vì trình biên dịch của JavaScript thực hiện chuyển đổi type của biến và do đó, nó so sánh với các chuỗi theo giá trị của chúng. Mặt khác, console.log thứ hai sẽ trả về sai vì Mảng là object và object được so sánh bằng tham chiếu.
<p>
</details>

---

## Câu 56: Kết quả trả về là gì?

#### Ví dụ:

```javascript
function greet() {
  return;
  {
    message: "hello";
  }
}

var a = greet();

console.log(a);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>undefined</code><br /><br />
Do tính năng chèn dấu chấm phẩy tự động (ASI) của JavaScript, trình biên dịch đặt dấu chấm phẩy sau từ khóa trả về và do đó, nó trả về undefined mà không bị lỗi.
<p>
</details>

---

## Câu 57: Kết quả trả về là gì?

#### Ví dụ:

```javascript
console.log(typeof typeof 0);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>string</code><br /><br />
Do typeof 0 là "number" nên typeof của "number" sẽ là chuỗi. Javascript thật đáng sợ !!!
<p>
</details>

---

## Câu 58: Kể tên một số thư viện Javascript xử lí array và object?

Có 2 thư viện xử lí array và object nổi tiếng nhất hiện nay là:

- lodash [Xem chi tiết](https://lodash.com/)
- underscore.js [Xem chi tiết](https://underscorejs.org/)

#### Ví dụ:

```javascript
// underscore.js
_.map([1, 2, 3], (num) => num * 3);

// lodash
_.map([4, 8], (x) => x * 2);

// Cách sử dụng khá giống nhau
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>[3, 6, 9]</code><br />
<code>[8, 16]</code>
<p>
</details>

---

## Câu 59: Kết quả trả về là gì?

Khi xét thuộc tính cho object, JavaScript sẽ ngầm định **stringify** parameter. Trong trường hợp này, vì b và c là cả hai là object, nên sẽ được chuyển đổi thành "[Object Object]". Kết quả là, cả [b] và [c] đều tương đương với ["[Object Object]"] và có thể được sử dụng thay thế cho nhau. Do đó, khi ta tham chiếu [c] cũng giống như là tham chiếu [b].

#### Ví dụ:

```javascript
var a = {},
  b = { key: "b" },
  c = { key: "c" };

a[b] = 123;
a[c] = 456;

console.log(a[b]);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>456</code><br />
<p>
</details>

---

## Câu 60: Kết quả trả về là gì?

#### Ví dụ:

```javascript
var a = [1, 2, 3];

a[10] = 99;

console.log(a);
console.log(a[6]);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>[ 1, 2, 3, <7 empty items>, 99 ]</code><br />
<code>undefined</code>
<p>
</details>

---

## Câu 61: Kết quả trả về là gì?

#### Ví dụ:

```javascript
var output = (function (x) {
  delete x;
  return x;
})(0);

console.log(output);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>0</code><br />
Kết quả trả về là 0. Bởi vì delete dùng để xóa property của object. Nhưng ở đây x không phải là object, nó là biến local variable nên delete không thể xóa được.
<p>
</details>

---

## Câu 62: Kết quả trả về là gì?

#### Ví dụ:

```javascript
var x = 1;
var output = (function () {
  delete x;
  return x;
})();

console.log(output);
```

<details><summary><b>Đáp án:</b></summary>
<p>
<code>1</code><br />
Kết quả trả về là 1. Bởi vì delete dùng để xóa property của object. Nhưng ở đây x không phải là object, nó là biến global variable có typeof number.
<p>
</details>

# Tổng hợp câu hỏi phỏng vấn Javascript về Front-end lẫn Back-end

![enter image description here](https://i.imgur.com/9P7ipCX.png)
Các câu hỏi bên dưới chủ yếu về Javascript bao gồm cả Front-end lẫn Back-end. Rất mong sẽ giúp ích cho các bạn trong quá trình ôn luyện Javascript cũng như chuẩn bị phỏng vấn .

Nếu thích hãy để lại cho mình **1 STAR** nhé . Nếu bạn nào có câu hỏi hay và muốn đóng góp để repository này ngày càng chất lượng đừng ngần ngại **Create Pull Request** để đăng câu mới nhé !

## Câu 1. Sự khác biệt giữa `undefined` và ` not defined` trong JavaScript

### Câu trả lời

Trong JavaScript nếu bạn sử dụng một biến không tồn tại và chưa được khai báo, thì JavaScript sẽ đưa ra một lỗi `var name is not defined` và các lệnh sẽ ngừng thực thi sau đó. Nhưng nếu bạn sử dụng `typeof undeclared_variable` thì nó sẽ trả về `undefined`.

Tiếp theo, hãy hiểu sự khác biệt giữa khai báo và định nghĩa.

`var x` là một khai báo vì bạn chưa xác định giá trị của nó, nhưng bạn đang khai báo sự tồn tại của nó và nhu cầu cấp phát bộ nhớ cho chính nó

```javascript
var x; // khai báo x
console.log(x); // kết quả : undefined
```

`var x = 1` bao gồm cả khai báo và định nghĩa (chúng ta cũng có thể nói rằng chúng ta đang thực hiện khởi tạo), Ở đây xảy ra 2 việc là khai báo và gán giá trị cho biến x . Trong JavaScript, mọi khai báo biến và khai báo hàm đều được đưa lên đầu phạm vi hiện tại của nó sau đó mới được sử dụng và thuật ngữ này gọi là `hoising`.

Một biến có thể được khai báo nhưng không được xác định giá trị . Và dĩ nhiên giá trị của nó là `undefined`.

```javascript
var x; // khai báo biến x
typeof x === "undefined"; // kết quả : true
```

Một biến có thể không được khai báo cũng không được định nghĩa. Khi chúng ta tham chiếu đên biến đó thì kết quả sẽ là `undefined`.

```
console.log (y); // Kết quả:  ReferenceError: y is not defined
```

### Tham khảo :

[http://stackoverflow.com/questions/20822022/javascript-variable-definition-declaration](http://stackoverflow.com/questions/20822022/javascript-variable-definition-declaration)

## Câu 2. Với giá trị nào của `x` thì kết quả của các câu lệnh sau không giống nhau?

```javascript
//  if( x <= 100 ) {...}
//  if( !(x > 100) ) {...}
```

### Câu trả lời

`NaN <= 100` là` false` và` NaN> 100` cũng là` false`, vì vậy nếu
giá trị của `x` là `NaN`, các câu lệnh không giống nhau.

Điều tương tự cũng xảy ra với bất kỳ giá trị nào của x khi trả về **NaN**, ví dụ: `undefined`,`[1,2,5]`,`{a: 22}`, v.v.

Đây là lý do tại sao bạn cần chú ý khi xử lý các biến . `NaN` có thể bằng nhau, nhỏ hơn hoặc nhiều hơn bất kỳ giá trị số nào khác, vì vậy cách tốt nhất để kiểm tra xem giá trị đó có phải là `NaN` hay không, là sử dụng hàm `isNaN ()`.

## Câu 3. Hạn chế của việc khai báo hàm trực tiếp bằng các đối tượng JavaScript là gì?

### Câu trả lời

Một trong những nhược điểm của việc khai báo các hàm trực tiếp bằng các đối tượng JavaScript là chúng quản lý bộ nhớ rất kém . Song song đó, một bản sao mới của hàm được tạo cho mỗi phiên bản của một đối tượng (biến). Hãy xem ví dụ bên dưới :

```javascript
var Employee = function (name, company, salary) {
  this.name = name || "";
  this.company = company || "";
  this.salary = salary || 5000;

  // Tạo một hàm/phương thức mới bằng cách này :
  this.formatSalary = function () {
    return "$ " + this.salary;
  };
};

// Chúng ta cũng có thể tạo hàm/phương thức mới bằng prototype:
Employee.prototype.formatSalary2 = function () {
  return "$ " + this.salary;
};

// Tạo mới các đối tượng với các đối tượng khác nhau
var emp1 = new Employee("Yuri Garagin", "Company 1", 1000000);
var emp2 = new Employee("Dinesh Gupta", "Company 2", 1039999);
var emp3 = new Employee("Erich Fromm", "Company 3", 1299483);
```

Ở đây, mỗi biến đối tượng `emp1`,` emp2`, `emp3` có bản sao riêng của phương thức` formatSalary`. Tuy nhiên, `formatSalary2` sẽ chỉ được thêm một lần vào một đối tượng `Employee.prototype`.

## Câu 4. Thế nào là closure trong javascript? bạn có thể cho một ví dụ?

### Câu trả lời

Closure là một hàm được định nghĩa bên trong một hàm khác (được gọi là hàm cha) và có quyền truy cập vào biến được khai báo và định nghĩa trong phạm vi hàm cha.

Closure có quyền truy cập vào biến trong ba phạm vi:

- Biến được khai báo trong phạm vi của chính mình
- Biến được khai báo trong phạm vi hàm cha
- Biến toàn cục

```javascript
var globalVar = "abc";

(function outerFunction(outerArg) {
  // bắt đầu phạm vi của  outerFunction
  // Biến bên trong phạm vi của outerFunction
  var outerFuncVar = "x";
  //  Hàm close tự gọi
  (function innerFunction(innerArg) {
    // bắt đầu phạm vi của innerFunction
    // Biến được khai báo và định nghĩa bên trong pham vi của innerFunction
    var innerFuncVar = "y";
    console.log(
      "outerArg = " +
        outerArg +
        "\n" +
        "outerFuncVar = " +
        outerFuncVar +
        "\n" +
        "innerArg = " +
        innerArg +
        "\n" +
        "innerFuncVar = " +
        innerFuncVar +
        "\n" +
        "globalVar = " +
        globalVar
    );
    //kết thúc phạm vi của  innerFunction
  })(5); // Truyền tham số 5
  // kết thúc phạm vi của  outerFunction
})(7); //Truyền tham số 7
```

innerFunction là một Closure được định nghĩa bên trong outerFunction và có quyền truy cập vào tất cả các biến được khai báo và định nghĩa trong phạm vi hàm cha. Ngoài chức năng này thì Closure có quyền truy cập vào biến toàn cục .

## Câu 5. Viết hàm `mul` để được kết quả như bên dưới

`` `javascript
console.log (mul (2) (3) (4)); // kết quả: 24
console.log (mul (4) (3) (4)); // kết quả: 48 `` `

### Câu trả lời

Dưới đây là code để giải đề trên :

```javascript
function mul(x) {
  return function (y) {
    // hàm ẩn danh
    return function (z) {
      // hàm ẩn danh
      return x * y * z;
    };
  };
}
```

Ở đây, hàm `mul` nhận đối số thứ nhất và trả về hàm ẩn danh nhận tham số thứ hai và trả về hàm ẩn danh nhận tham số thứ ba và trả về phép nhân của các đối số được truyền liên tiếp

Trong hàm Javascript được định nghĩa bên trong có quyền truy cập vào biến hàm ngoài và hàm là đối tượng lớp đầu tiên để nó cũng có thể được trả về bởi hàm và được truyền dưới dạng đối số trong hàm khác.
Một số điểm cần lưu ý :

- Hàm là một hình thức khác của loại Đối tượng
- Một hàm có thể có các thuộc tính và có một liên kết trả về hàm constructor của nó
- Một hàm có thể được lưu trữ dưới dạng biến
- Một hàm có thể được truyền dưới dạng tham số cho hàm khác
- Một hàm có thể được trả về từ một hàm khác

## Câu 6. Làm thế nào để làm trống một mảng trong JavaScript?

Ví dụ:

```javascript
var arrayList = ["a", "b", "c", "d", "e", "f"];
```

Làm thế nào chúng ta có thể làm trống mảng trên?

### Câu trả lời

Chúng ta hãy thử qua các cách sau đây

#### Cách 1

```
javascript
arrayList = [];
```

Đoạn mã trên sẽ làm cho biến ArrayList thành một mảng trống mới. Cách này được khuyến nghị nếu bạn không có **tham chiếu đến mảng ban đầu** Ví dụ:

```javascript
var arrayList = ["a", "b", "c", "d", "e", "f"]; // Tạo mảng
var anotherArrayList = arrayList; // Tham chiếu đến arrayList từ một biến khác
arrayList = []; // Làm trống mảng đó
console.log(anotherArrayList); // Kết quả  ['a', 'b', 'c', 'd', 'e', 'f']
```

#### Cách 2

```javascript
arrayList.length = 0;
```

Code trên sẽ xóa mảng hiện có bằng cách đặt độ dài của nó thành 0. Cách làm này cũng sẽ cập nhật tất cả các biến tham chiếu trỏ đến mảng ban đầu.

Ví dụ:

`` `javascript
var arrayList = ['a', 'b', 'c', 'd', 'e', 'f']; // Tạo mảng 
var anotherArrayList = arrayList;  // Tham chiếu đến  arrayList từ một biến khác 
arrayList.length = 0; // Làm rỗng mảng 
console.log(anotherArrayList); // Kết quả :  [] `` `

#### Cách 3

```javascript
while (arrayList.length) {
  arrayList.pop();
}
```

Cách mình vừa thực hiện cũng có thể làm trống mảng. Nhưng không nên sử dụng thường xuyên.

## Câu 7 . Kết quả của đoạn code sau đây?

```javascript
var output = (function (x) {
  delete x;
  return x;
})(0);

console.log(output);
```

### Câu trả lời

Đoạn mã trên sẽ xuất ra `0` . Toán tử `delete` được sử dụng để xóa một thuộc tính khỏi một đối tượng. Ở đây `x` không phải là một đối tượng, nó là ** biến cục bộ **. Toán tử `delete` không ảnh hưởng đến các biến cục bộ.

## Câu 8. Kết quả của đoạn code sau đây ?

```javascript
var x = 1;
var output = (function () {
  delete x;
  return x;
})();

console.log(output);
```

### Câu trả lời

Đoạn mã trên sẽ xuất ra `1` . Toán tử `delete` được sử dụng để xóa một thuộc tính khỏi một đối tượng. Ở đây `x` không phải là một đối tượng, nó là ** biến toàn cục ** thuộc loại` number`.

## Câu 9. Kết quả của đoạn code sau đây ?

```javascript
var x = { foo: 1 };
var output = (function () {
  delete x.foo;
  return x.foo;
})();

console.log(output);
```

### Câu trả lời

Đoạn mã trên sẽ xuất ra `undefined` . Toán tử `delete` được sử dụng để xóa một thuộc tính khỏi một đối tượng. Ở đây `x` là một đối tượng có foo là một thuộc tính và từ một hàm IIFE, mình đang xóa thuộc tính`foo` của đối tượng` x` và sau khi xóa, mình đang tham chiếu thuộc tính đã xóa **foo** nên kết quả sẽ là `undefined`.

## Câu 10. Kết quả của đoạn code sau đây ?

```javascript
var Employee = {
  company: "xyz",
};
var emp1 = Object.create(Employee);
delete emp1.company;
console.log(emp1.company);
```

### Câu trả lời

Đoạn mã trên sẽ xuất ra `xyz` . Ở đây, đối tượng `emp1` có company là **prototype**, xóa toán tử không xóa thuộc tính **prototype**

Đối tượng `emp1` không có ** company** làm thuộc tính của mình. bạn có thể kiểm tra bằng cách `console.log (emp1.hasOwnProperty ('company')); // Kết quả : false` Tuy nhiên, chúng ta có thể xóa thuộc tính **company** khỏi đối tượng `Employee` bằng cách sử dụng `delete Employee.company` hoặc chúng ta cũng có thể xóa khỏi đối tượng `emp1` bằng cách sử dụng thuộc tính `__proto__` và `delete emp1.__proto__.company`.

## Câu 11. `undefined x 1` trong JavaScript là gì?

```javascript
var trees = ["redwood", "bay", "cedar", "oak", "maple"];
delete trees[3];
```

### Câu trả lời

- Khi bạn chạy code trên và `console.log(trees);` và bạn sẽ nhận được kết quả `["redwood", "bay", "cedar", undefined × 1, "maple"]`
- Trong các phiên bản gần đây của Chrome, bạn sẽ thấy từ `empty` thay vì `undefined x 1`
- Khi bạn chạy trên trình duyệt Firefox thì bạn sẽ nhận được kết quả `["redwood", "bay", "cedar", undefined, "maple"]`

Rõ ràng, Chrome có cách hiển thị cho các index chưa được khởi tạo trong các mảng. Tuy nhiên, khi bạn kiểm tra `trees[3] === undefined` trong bất kỳ trình duyệt nào, bạn sẽ nhận được kết quả là ` true`. Rõ ràng `undefined x 1` đại diện cho các phần tử hay index chưa được khởi tạo.

## Câu 12. Kết quả của đoạn code bên dưới là gì?

```javascript
var trees = ["xyz", "xxxx", "test", "ryan", "apple"];
delete trees[3];
console.log(trees.length);
```

### Câu trả lời

Kết qủa của đoạn code bên trên là `5`. Khi chúng ta sử dụng toán tử `delete` để xoá mảng các phần tử , thì độ dài mảng sẽ không ảnh hưởng. Điều này luôn đúng nếu bạn dùng `delete` cho từng phần tử của mảng.

Bởi vì khi ta dùng toán tử `delete` để xoá các phần tử của mảng thì các phần từ đó sẽ được chuyển thành `undefined x 1` nên độ dài mảng không thay đổi là tất nhiên . Còn `undefined x 1` là gì, mời xem

## Câu 13. Kết quả của đoạn code bên dưới là gì?

```javascript
var bar = true;
console.log(bar + 0);
console.log(bar + "xyz");
console.log(bar + true);
console.log(bar + false);
```

### Câu trả lời

Kết quả của đoạn code sẽ là `1, "truexyz", 2, 1` , bởi vì ta có quy luật sau :

- Số + Số -> Phép cộng
- Biến Boolean + Số -> Phép cộng
- Biến Boolean + Biến Boolean --> Phép cộng
- Số + Chuỗi -> Phép nối chuỗi
- Chuỗi + Biến Boolean -> Phép nối chuỗi
- Chuỗi + Chuỗi -> Phép nối chuỗi

## Câu 14. Kết quả của đoạn code bên dưới là gì?

```javascript
var z = 1,
  y = (z = typeof y);
console.log(y);
```

### Câu trả lời

Kết quả của code bên trên sẽ là `"undefined"` .Theo như quy luật toán tử kết hợp với mức độ ưu tiên như nhau. Quy luật của toán tử gán là `Phải sang trái ` nên `typeof y` sẽ được xem xét đầu tiên và giá trị hiện tại của nó là `"undefined"` và gán cho `z` sau đó gán cho `y` nên ta được kết quả như trên.
Code sẽ được thực hiện theo thứ tự bên dưới

````javascript
var z;
z = 1;
var y;
z = typeof y;
y = z;



## Câu 15. Kết quả của đoạn code bên dưới là gì?


```javascript
// NFE (Named Function Expression)
var foo = function bar() { return 12; };
typeof bar();
````

### Câu trả lời

Kết quả sẽ là `Reference Error`. Để khắc phục lỗi trên ta sẽ thực hiện như sau :

**Cách 1**

```javascript
var bar = function () {
  return 12;
};
typeof bar();
```

**Cách 2**

```javascript
function bar() {
  return 12;
}
typeof bar();
```

Định nghĩa hàm chỉ có thể có một biến tham chiếu đó là tên hàm , Ở Cách 1 `bar` tham chiếu đến một `anonymous function` và trong Cách 2 chúng ta có định nghĩa `bar` là một tên hàm

```javascript
var foo = function bar() {
  // foo is visible here
  // bar is visible here
  console.log(typeof bar()); // Works here :)
};
// foo is visible here
// bar is undefined here
```

**CẬP NHẬT MỖI TUẦN - NHỚ STAR ĐỂ THEO DÕI CÂU HỎI MỚI NHÉ**
