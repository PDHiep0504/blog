---
title: "JavaScript Functions - Arrow Functions và Higher-Order Functions"
date: 2025-12-14
draft: false
tags: ["JavaScript", "Functions", "ES6", "Functional Programming"]
categories: ["JavaScript"]
---

# JavaScript Functions - Từ cơ bản đến nâng cao

Functions là building blocks của JavaScript. Hãy cùng khám phá các cách định nghĩa và sử dụng functions!

## Các cách định nghĩa Function

### 1. Function Declaration

```javascript
// Function declaration - Có hoisting
function greet(name) {
    return `Xin chào, ${name}!`;
}

console.log(greet("An"));  // "Xin chào, An!"

// Có thể gọi trước khi khai báo
sayHello("Bình");

function sayHello(name) {
    console.log(`Hello, ${name}!`);
}
```

### 2. Function Expression

```javascript
// Function expression - Không có hoisting
const multiply = function(a, b) {
    return a * b;
};

console.log(multiply(5, 3));  // 15

// Không thể gọi trước khi khai báo
// divide(10, 2);  // Lỗi!

const divide = function(a, b) {
    return a / b;
};
```

### 3. Arrow Function (ES6)

```javascript
// Cú pháp đơn giản
const add = (a, b) => a + b;
console.log(add(2, 3));  // 5

// Với một tham số, bỏ được ()
const square = x => x * x;
console.log(square(4));  // 16

// Nhiều dòng code
const calculate = (a, b) => {
    const sum = a + b;
    const product = a * b;
    return {sum, product};
};

console.log(calculate(3, 4));
// {sum: 7, product: 12}

// Return object literal
const createPerson = (name, age) => ({
    name: name,
    age: age
});

// Hoặc dùng shorthand
const createUser = (name, age) => ({name, age});
```

### So sánh Regular Function vs Arrow Function

```javascript
// Regular function - có 'this' riêng
const obj1 = {
    name: "Object 1",
    regularFunc: function() {
        console.log(this.name);
    }
};
obj1.regularFunc();  // "Object 1"

// Arrow function - kế thừa 'this' từ scope cha
const obj2 = {
    name: "Object 2",
    arrowFunc: () => {
        console.log(this.name);  // undefined (this = window)
    }
};

// Arrow function trong method
const counter = {
    count: 0,
    increment: function() {
        // Arrow function kế thừa 'this' từ increment
        setTimeout(() => {
            this.count++;
            console.log(this.count);
        }, 100);
    }
};
counter.increment();  // 1
```

## Parameters và Arguments

### Default Parameters

```javascript
function greet(name = "Khách", greeting = "Xin chào") {
    return `${greeting}, ${name}!`;
}

console.log(greet());              // "Xin chào, Khách!"
console.log(greet("An"));          // "Xin chào, An!"
console.log(greet("Bình", "Hi"));  // "Hi, Bình!"
```

### Rest Parameters

```javascript
// Thu thập tất cả arguments vào array
function sum(...numbers) {
    return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3));           // 6
console.log(sum(1, 2, 3, 4, 5));     // 15

// Kết hợp với tham số thường
function introduce(greeting, ...names) {
    return `${greeting} ${names.join(", ")}!`;
}

console.log(introduce("Xin chào", "An", "Bình", "Chi"));
// "Xin chào An, Bình, Chi!"
```

### Destructuring Parameters

```javascript
// Object destructuring
function createUser({name, age, email}) {
    return {
        name: name,
        age: age,
        email: email,
        createdAt: new Date()
    };
}

const user = createUser({
    name: "An",
    age: 25,
    email: "an@example.com"
});

// Với default values
function login({username, password, remember = false}) {
    console.log(`User: ${username}, Remember: ${remember}`);
}

login({username: "an123", password: "pass123"});
// "User: an123, Remember: false"
```

## Higher-Order Functions

Functions nhận function làm tham số hoặc return function.

### Callback Functions

```javascript
// Function nhận callback
function processArray(arr, callback) {
    const result = [];
    for (let item of arr) {
        result.push(callback(item));
    }
    return result;
}

const numbers = [1, 2, 3, 4, 5];

const doubled = processArray(numbers, x => x * 2);
console.log(doubled);  // [2, 4, 6, 8, 10]

const squared = processArray(numbers, x => x ** 2);
console.log(squared);  // [1, 4, 9, 16, 25]
```

### Functions returning Functions

```javascript
function multiplier(factor) {
    return function(number) {
        return number * factor;
    };
}

const double = multiplier(2);
const triple = multiplier(3);

console.log(double(5));   // 10
console.log(triple(5));   // 15

// Với arrow function
const adder = x => y => x + y;

const add5 = adder(5);
console.log(add5(3));     // 8
console.log(adder(10)(20)); // 30
```

## Array Higher-Order Methods

### map() - Transform mỗi phần tử

```javascript
const numbers = [1, 2, 3, 4, 5];

const doubled = numbers.map(num => num * 2);
console.log(doubled);  // [2, 4, 6, 8, 10]

const students = [
    {name: "An", score: 85},
    {name: "Bình", score: 92},
    {name: "Chi", score: 78}
];

const names = students.map(student => student.name);
console.log(names);  // ["An", "Bình", "Chi"]

const grades = students.map(s => ({
    name: s.name,
    grade: s.score >= 90 ? 'A' : s.score >= 80 ? 'B' : 'C'
}));
```

### filter() - Lọc phần tử

```javascript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

const evens = numbers.filter(num => num % 2 === 0);
console.log(evens);  // [2, 4, 6, 8, 10]

const students = [
    {name: "An", score: 85, age: 20},
    {name: "Bình", score: 92, age: 22},
    {name: "Chi", score: 78, age: 19},
    {name: "Dũng", score: 95, age: 21}
];

const topStudents = students.filter(s => s.score >= 90);
console.log(topStudents);
// [{name: "Bình", ...}, {name: "Dũng", ...}]

const adults = students.filter(s => s.age >= 21);
```

### reduce() - Giảm về một giá trị

```javascript
const numbers = [1, 2, 3, 4, 5];

// Tổng
const sum = numbers.reduce((total, num) => total + num, 0);
console.log(sum);  // 15

// Tích
const product = numbers.reduce((result, num) => result * num, 1);
console.log(product);  // 120

// Tìm max
const max = numbers.reduce((max, num) => num > max ? num : max);
console.log(max);  // 5

// Đếm phần tử
const fruits = ["táo", "chuối", "táo", "cam", "chuối", "táo"];
const count = fruits.reduce((acc, fruit) => {
    acc[fruit] = (acc[fruit] || 0) + 1;
    return acc;
}, {});
console.log(count);
// {táo: 3, chuối: 2, cam: 1}

// Group by
const students = [
    {name: "An", grade: "A"},
    {name: "Bình", grade: "B"},
    {name: "Chi", grade: "A"}
];

const grouped = students.reduce((acc, student) => {
    const grade = student.grade;
    if (!acc[grade]) {
        acc[grade] = [];
    }
    acc[grade].push(student.name);
    return acc;
}, {});
console.log(grouped);
// {A: ["An", "Chi"], B: ["Bình"]}
```

### find() và findIndex()

```javascript
const users = [
    {id: 1, name: "An", active: true},
    {id: 2, name: "Bình", active: false},
    {id: 3, name: "Chi", active: true}
];

const user = users.find(u => u.id === 2);
console.log(user);  // {id: 2, name: "Bình", active: false}

const index = users.findIndex(u => u.name === "Chi");
console.log(index);  // 2

const activeUser = users.find(u => u.active);
console.log(activeUser);  // {id: 1, name: "An", ...}
```

### every() và some()

```javascript
const numbers = [2, 4, 6, 8, 10];

// every() - tất cả phải thỏa mãn
const allEven = numbers.every(num => num % 2 === 0);
console.log(allEven);  // true

// some() - ít nhất một phải thỏa mãn
const hasLarge = numbers.some(num => num > 5);
console.log(hasLarge);  // true

const students = [
    {name: "An", score: 85},
    {name: "Bình", score: 92},
    {name: "Chi", score: 78}
];

const allPassed = students.every(s => s.score >= 50);
console.log(allPassed);  // true

const hasTopScore = students.some(s => s.score >= 90);
console.log(hasTopScore);  // true
```

## Method Chaining

Kết hợp nhiều methods lại với nhau.

```javascript
const students = [
    {name: "An", score: 85, age: 20},
    {name: "Bình", score: 92, age: 22},
    {name: "Chi", score: 78, age: 19},
    {name: "Dũng", score: 95, age: 21},
    {name: "Em", score: 88, age: 20}
];

// Lấy tên sinh viên điểm >= 85, sắp xếp theo tên
const topStudentNames = students
    .filter(s => s.score >= 85)
    .map(s => s.name)
    .sort();

console.log(topStudentNames);
// ["An", "Bình", "Dũng", "Em"]

// Tính điểm trung bình của sinh viên >= 20 tuổi
const avgScore = students
    .filter(s => s.age >= 20)
    .map(s => s.score)
    .reduce((sum, score, _, arr) => sum + score / arr.length, 0);

console.log(avgScore);  // 90
```

## Closure

Function có thể truy cập biến từ scope cha.

```javascript
function createCounter() {
    let count = 0;  // Private variable
    
    return {
        increment: function() {
            count++;
            return count;
        },
        decrement: function() {
            count--;
            return count;
        },
        getCount: function() {
            return count;
        }
    };
}

const counter = createCounter();
console.log(counter.increment());  // 1
console.log(counter.increment());  // 2
console.log(counter.decrement());  // 1
console.log(counter.getCount());   // 1
// console.log(counter.count);     // undefined - private!
```

## Kết luận

JavaScript Functions:
- Nhiều cách định nghĩa: Declaration, Expression, Arrow
- Arrow functions ngắn gọn, `this` lexical
- Higher-order functions mạnh mẽ
- Array methods: `map`, `filter`, `reduce`, `find`, `some`, `every`
- Closure cho private variables

Master functions để viết code clean và functional! 💪
