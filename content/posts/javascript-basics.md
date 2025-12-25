---
title: "JavaScript cơ bản - Biến, Kiểu dữ liệu và Toán tử"
date: 2025-12-15
draft: false
tags: ["JavaScript", "Cơ bản", "ES6"]
categories: ["JavaScript"]
---

# JavaScript cơ bản - Nền tảng lập trình web

JavaScript là ngôn ngữ lập trình phổ biến nhất cho web development. Hãy cùng tìm hiểu các khái niệm cơ bản!

## Khai báo biến

### var, let, const

```javascript
// var - function scope (cũ, không khuyến nghị)
var name = "An";
var age = 25;

// let - block scope (có thể thay đổi)
let email = "an@example.com";
email = "newemail@example.com"; // OK

// const - block scope (không thay đổi)
const PI = 3.14159;
// PI = 3.14; // Lỗi!

const person = {
    name: "Bình",
    age: 30
};
person.age = 31; // OK - thay đổi thuộc tính
// person = {}; // Lỗi - không thể gán lại
```

**Best Practice:**
- Ưu tiên dùng `const`
- Dùng `let` khi cần thay đổi giá trị
- Tránh dùng `var`

## Kiểu dữ liệu

### Primitive Types

```javascript
// String
let firstName = "Nguyễn";
let lastName = 'Văn An';
let fullName = `${firstName} ${lastName}`; // Template literal

// Number
let age = 25;
let price = 99.99;
let negative = -10;
let infinity = Infinity;

// Boolean
let isStudent = true;
let hasGraduated = false;

// Null và Undefined
let emptyValue = null;      // Cố ý để trống
let notDefined;             // undefined (chưa gán giá trị)

// Symbol (ES6)
let id = Symbol('id');

// BigInt (ES2020)
let bigNumber = 9007199254740991n;
```

### Reference Types

```javascript
// Array
let fruits = ["Táo", "Chuối", "Cam"];
let numbers = [1, 2, 3, 4, 5];
let mixed = [1, "text", true, null, {name: "An"}];

// Object
let student = {
    name: "Nguyễn Văn An",
    age: 20,
    grade: "A",
    subjects: ["Math", "Physics", "Chemistry"]
};

// Function
function greet(name) {
    return `Xin chào, ${name}!`;
}
```

## Kiểm tra kiểu dữ liệu

```javascript
console.log(typeof "Hello");        // "string"
console.log(typeof 42);             // "number"
console.log(typeof true);           // "boolean"
console.log(typeof undefined);      // "undefined"
console.log(typeof null);           // "object" (bug lịch sử)
console.log(typeof {});             // "object"
console.log(typeof []);             // "object"
console.log(typeof function(){});   // "function"

// Kiểm tra Array
console.log(Array.isArray([]));     // true
console.log(Array.isArray({}));     // false
```

## Toán tử

### Toán tử số học

```javascript
let a = 10;
let b = 3;

console.log(a + b);  // 13 - Cộng
console.log(a - b);  // 7  - Trừ
console.log(a * b);  // 30 - Nhân
console.log(a / b);  // 3.333... - Chia
console.log(a % b);  // 1  - Chia lấy dư
console.log(a ** b); // 1000 - Lũy thừa (ES2016)

// Increment/Decrement
let count = 0;
count++;        // count = 1 (post-increment)
++count;        // count = 2 (pre-increment)
count--;        // count = 1 (post-decrement)
--count;        // count = 0 (pre-decrement)
```

### Toán tử so sánh

```javascript
console.log(5 == "5");   // true  - So sánh giá trị (type coercion)
console.log(5 === "5");  // false - So sánh giá trị và kiểu

console.log(5 != "5");   // false
console.log(5 !== "5");  // true

console.log(10 > 5);     // true
console.log(10 >= 10);   // true
console.log(5 < 10);     // true
console.log(5 <= 5);     // true

// Best Practice: Luôn dùng === và !==
```

### Toán tử logic

```javascript
let isAdult = true;
let hasLicense = false;

console.log(isAdult && hasLicense);  // false - AND
console.log(isAdult || hasLicense);  // true  - OR
console.log(!isAdult);               // false - NOT

// Short-circuit evaluation
let name = "" || "Anonymous";        // "Anonymous"
let user = null || {name: "An"};    // {name: "An"}

// Nullish coalescing (ES2020)
let count = 0;
console.log(count || 10);   // 10 (0 là falsy)
console.log(count ?? 10);   // 0  (chỉ null/undefined thì lấy 10)
```

## String Methods

```javascript
let text = "  JavaScript Tutorial  ";

// Length
console.log(text.length);           // 23

// Trim
console.log(text.trim());           // "JavaScript Tutorial"

// Case conversion
console.log(text.toUpperCase());    // "  JAVASCRIPT TUTORIAL  "
console.log(text.toLowerCase());    // "  javascript tutorial  "

// Substring
let str = "JavaScript";
console.log(str.substring(0, 4));   // "Java"
console.log(str.slice(0, 4));       // "Java"
console.log(str.slice(-6));         // "Script"

// Replace
let message = "Hello World";
console.log(message.replace("World", "JavaScript")); // "Hello JavaScript"

// Split
let csv = "An,20,Student";
console.log(csv.split(","));        // ["An", "20", "Student"]

// Includes, startsWith, endsWith
console.log(str.includes("Script"));     // true
console.log(str.startsWith("Java"));     // true
console.log(str.endsWith("Script"));     // true
```

## Array Methods cơ bản

```javascript
let fruits = ["Táo", "Chuối", "Cam"];

// Thêm/Xóa
fruits.push("Xoài");         // Thêm cuối
fruits.unshift("Dâu");       // Thêm đầu
fruits.pop();                // Xóa cuối
fruits.shift();              // Xóa đầu

// Truy cập
console.log(fruits[0]);      // "Táo"
console.log(fruits.length);  // 3

// Tìm kiếm
console.log(fruits.indexOf("Cam"));      // 2
console.log(fruits.includes("Chuối"));   // true

// Join
console.log(fruits.join(", "));          // "Táo, Chuối, Cam"

// Slice
let citrus = fruits.slice(1, 3);         // ["Chuối", "Cam"]

// Concat
let moreFruits = fruits.concat(["Dưa", "Ổi"]);
```

## Template Literals

```javascript
let name = "An";
let age = 25;

// Multiline string
let message = `
    Xin chào,
    Tên tôi là ${name}.
    Tôi ${age} tuổi.
`;

// Expression
let price = 100;
let tax = 10;
console.log(`Tổng: ${price + tax}đ`);    // "Tổng: 110đ"

// Function call
function upper(str) {
    return str.toUpperCase();
}
console.log(`Tên: ${upper(name)}`);      // "Tên: AN"
```

## Destructuring

```javascript
// Array destructuring
let colors = ["red", "green", "blue"];
let [first, second] = colors;
console.log(first);   // "red"
console.log(second);  // "green"

// Object destructuring
let person = {
    name: "An",
    age: 25,
    city: "Hà Nội"
};

let {name, age} = person;
console.log(name);    // "An"
console.log(age);     // 25

// Với alias
let {name: fullName, age: years} = person;
console.log(fullName); // "An"

// Default values
let {country = "Việt Nam"} = person;
console.log(country);  // "Việt Nam"
```

## Spread và Rest Operators

```javascript
// Spread operator (...)
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];
let combined = [...arr1, ...arr2];  // [1, 2, 3, 4, 5, 6]

let original = {name: "An", age: 25};
let copy = {...original, city: "Hà Nội"};
// {name: "An", age: 25, city: "Hà Nội"}

// Rest operator
function sum(...numbers) {
    return numbers.reduce((total, num) => total + num, 0);
}
console.log(sum(1, 2, 3, 4, 5));  // 15
```

## Kết luận

JavaScript cơ bản bao gồm:
- Biến: `const`, `let` (tránh `var`)
- Kiểu dữ liệu: Primitive và Reference types
- Toán tử: Số học, so sánh, logic
- String và Array methods
- Modern syntax: Template literals, destructuring, spread/rest

Đây là nền tảng để học các khái niệm nâng cao! 🚀
