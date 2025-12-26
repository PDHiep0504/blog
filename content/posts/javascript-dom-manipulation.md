---
title: "DOM Manipulation với JavaScript - Tương tác với HTML"
date: 2025-12-12
draft: false
tags: ["JavaScript", "DOM", "Web Development", "Frontend"]
categories: ["JavaScript"]
description: "Hướng dẫn cách chọn, tạo, chỉnh sửa và xoá element bằng JavaScript, lắng nghe event và tối ưu hiệu năng khi thao tác DOM trên trang web."
image: "images/posts/javascript-dom-manipulation.jpg"
---

# DOM Manipulation với JavaScript

DOM (Document Object Model) là cách JavaScript tương tác với HTML. Hãy cùng học cách thao tác với trang web!

## Lý thuyết: DOM, Render Tree và Event

### DOM khác gì HTML?

- **HTML** là văn bản markup.
- **DOM** là cấu trúc đối tượng (tree) được browser tạo ra sau khi parse HTML.

Nên nhớ: DOM có thể thay đổi trong runtime (bạn add/remove element), còn HTML source ban đầu thì không tự đổi.

### Render pipeline (vì sao DOM thao tác nhiều sẽ chậm)

Thông thường browser đi theo chuỗi:

1) Parse HTML → DOM
2) Parse CSS → CSSOM
3) Combine → Render Tree
4) Layout (tính toán vị trí/kích thước)
5) Paint/Composite

Một số thao tác DOM/CSS có thể kích hoạt **reflow/layout** (đắt). Vì vậy nên:

- Gom nhiều thay đổi vào một lần (batch)
- Tránh đọc layout (`offsetWidth`, `getBoundingClientRect`) xen kẽ với ghi style liên tục

### Live collection vs static collection

- `getElementsByClassName/getElementsByTagName` trả về **HTMLCollection** (thường là live).
- `querySelectorAll` trả về **NodeList** (thường là static snapshot).

Khi bạn mutate DOM, live collection có thể thay đổi theo, dễ gây bug nếu đang loop.

### Event propagation: capture → target → bubble

Event trong browser thường đi qua 3 pha:

- Capture (từ ngoài vào trong)
- Target
- Bubble (từ trong ra ngoài)

Điều này dẫn tới kỹ thuật **event delegation**: gắn 1 listener ở container thay vì gắn cho từng item.

### innerHTML và rủi ro XSS

`innerHTML` tiện, nhưng nếu nhét dữ liệu user vào mà không sanitize sẽ có nguy cơ XSS. Khi chỉ cần text, ưu tiên `textContent`.

## DOM là gì?

DOM là tree structure đại diện cho HTML document. Mỗi HTML element là một node trong tree.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>My Page</title>
  </head>
  <body>
    <h1 id="title">Hello World</h1>
    <p class="text">This is a paragraph</p>
  </body>
</html>
```

## Selecting Elements

### getElementById()

```javascript
// Lấy element theo ID
const title = document.getElementById('title');
console.log(title);  // <h1 id="title">Hello World</h1>
console.log(title.textContent);  // "Hello World"
```

### getElementsByClassName()

```javascript
// Lấy elements theo class (HTMLCollection)
const textElements = document.getElementsByClassName('text');
console.log(textElements.length);  // Số lượng elements
console.log(textElements[0]);      // Element đầu tiên

// Convert sang Array để dùng array methods
const textArray = Array.from(textElements);
textArray.forEach(el => console.log(el.textContent));
```

### getElementsByTagName()

```javascript
// Lấy elements theo tag name
const paragraphs = document.getElementsByTagName('p');
const allDivs = document.getElementsByTagName('div');
```

### querySelector() - Modern & Flexible

```javascript
// Lấy element đầu tiên match CSS selector
const title = document.querySelector('#title');
const firstPara = document.querySelector('.text');
const firstLink = document.querySelector('a');
const navLink = document.querySelector('nav a.active');

// Kết hợp selectors
const specialDiv = document.querySelector('div.container > p.important');
```

### querySelectorAll() - Get All Matches

```javascript
// Lấy tất cả elements match selector (NodeList)
const allLinks = document.querySelectorAll('a');
const navItems = document.querySelectorAll('nav li');

// NodeList có forEach (khác HTMLCollection)
allLinks.forEach(link => {
    console.log(link.href);
});

// Convert sang Array để dùng map, filter...
const linkTexts = Array.from(allLinks).map(link => link.textContent);
```

## Manipulating Content

### textContent vs innerHTML

```html
<div id="content">
    <p>Hello <strong>World</strong></p>
</div>
```

```javascript
const content = document.querySelector('#content');

// textContent - chỉ text, không HTML
console.log(content.textContent);  // "Hello World"
content.textContent = 'New text';  // <div id="content">New text</div>

// innerHTML - bao gồm HTML tags
console.log(content.innerHTML);    // "<p>Hello <strong>World</strong></p>"
content.innerHTML = '<p>New <em>HTML</em></p>';

// innerText - giống textContent nhưng tính đến CSS styling
console.log(content.innerText);
```

### Changing Attributes

```javascript
const img = document.querySelector('img');

// Get attribute
console.log(img.src);
console.log(img.alt);
console.log(img.getAttribute('data-id'));

// Set attribute
img.src = 'new-image.jpg';
img.alt = 'New description';
img.setAttribute('data-id', '123');

// Remove attribute
img.removeAttribute('data-id');

// Check if has attribute
if (img.hasAttribute('alt')) {
    console.log('Image has alt text');
}
```

## Manipulating Styles

### Inline Styles

```javascript
const box = document.querySelector('.box');

// Đặt style trực tiếp
box.style.backgroundColor = 'blue';
box.style.color = 'white';
box.style.padding = '20px';
box.style.borderRadius = '10px';

// Camel case cho CSS properties
box.style.fontSize = '18px';       // font-size
box.style.marginTop = '10px';      // margin-top

// Get computed style
const styles = window.getComputedStyle(box);
console.log(styles.backgroundColor);
console.log(styles.fontSize);
```

### Class Manipulation

```javascript
const element = document.querySelector('.item');

// className - thay thế toàn bộ classes
element.className = 'new-class another-class';

// classList - modern way (recommended)
element.classList.add('active');          // Thêm class
element.classList.remove('inactive');     // Xóa class
element.classList.toggle('highlight');    // Bật/tắt class
element.classList.replace('old', 'new'); // Thay thế

// Check if has class
if (element.classList.contains('active')) {
    console.log('Element is active');
}

// Multiple classes
element.classList.add('class1', 'class2', 'class3');
element.classList.remove('class1', 'class2');
```

## Creating and Modifying Elements

### createElement()

```javascript
// Tạo element mới
const div = document.createElement('div');
const p = document.createElement('p');
const button = document.createElement('button');

// Thêm content
p.textContent = 'This is a paragraph';
button.textContent = 'Click me';
button.className = 'btn btn-primary';

// Thêm vào DOM
div.appendChild(p);
div.appendChild(button);
document.body.appendChild(div);
```

### insertAdjacentHTML()

```javascript
const container = document.querySelector('.container');

// beforebegin: trước element
// afterbegin: đầu element (first child)
// beforeend: cuối element (last child)
// afterend: sau element

container.insertAdjacentHTML('beforeend', '<p>New paragraph</p>');
container.insertAdjacentHTML('afterbegin', '<h2>Title</h2>');
```

### Removing Elements

```javascript
const element = document.querySelector('.to-remove');

// Modern way
element.remove();

// Old way
element.parentNode.removeChild(element);

// Remove all children
const container = document.querySelector('.container');
while (container.firstChild) {
    container.removeChild(container.firstChild);
}

// Hoặc
container.innerHTML = '';
```

## Event Handling

### addEventListener()

```javascript
const button = document.querySelector('#myButton');

// Click event
button.addEventListener('click', function(event) {
    console.log('Button clicked!');
    console.log('Event:', event);
    console.log('Target:', event.target);
});

// Với arrow function
button.addEventListener('click', (e) => {
    console.log('Clicked!');
});

// Named function
function handleClick(event) {
    console.log('Button was clicked');
}
button.addEventListener('click', handleClick);

// Remove event listener
button.removeEventListener('click', handleClick);
```

### Common Events

```javascript
// Mouse events
element.addEventListener('click', e => {});
element.addEventListener('dblclick', e => {});
element.addEventListener('mouseenter', e => {});
element.addEventListener('mouseleave', e => {});
element.addEventListener('mousemove', e => {});

// Keyboard events
document.addEventListener('keydown', e => {
    console.log('Key pressed:', e.key);
    console.log('Key code:', e.code);
});

document.addEventListener('keyup', e => {});

// Form events
const form = document.querySelector('form');
const input = document.querySelector('input');

form.addEventListener('submit', e => {
    e.preventDefault();  // Ngăn form submit
    console.log('Form submitted');
});

input.addEventListener('input', e => {
    console.log('Input value:', e.target.value);
});

input.addEventListener('change', e => {
    console.log('Input changed');
});

input.addEventListener('focus', e => {
    console.log('Input focused');
});

input.addEventListener('blur', e => {
    console.log('Input lost focus');
});
```

### Event Delegation

Sử dụng event bubbling để handle events hiệu quả.

```javascript
// ❌ Không hiệu quả - attach event cho mỗi item
const items = document.querySelectorAll('.item');
items.forEach(item => {
    item.addEventListener('click', e => {
        console.log('Item clicked');
    });
});

// ✅ Hiệu quả - attach event cho parent
const list = document.querySelector('.list');
list.addEventListener('click', e => {
    if (e.target.classList.contains('item')) {
        console.log('Item clicked:', e.target);
    }
});
```

## Ví dụ thực tế: Todo List

```html
<!DOCTYPE html>
<html>
<head>
    <title>Todo List</title>
    <style>
        .completed {
            text-decoration: line-through;
            color: gray;
        }
        .todo-item {
            padding: 10px;
            margin: 5px 0;
            background: #f0f0f0;
            border-radius: 5px;
        }
    </style>
</head>
<body>
    <h1>Todo List</h1>
    
    <div id="app">
        <input type="text" id="todoInput" placeholder="Nhập công việc...">
        <button id="addBtn">Thêm</button>
        <ul id="todoList"></ul>
    </div>

    <script src="app.js"></script>
</body>
</html>
```

```javascript
// app.js
const todoInput = document.querySelector('#todoInput');
const addBtn = document.querySelector('#addBtn');
const todoList = document.querySelector('#todoList');

// Thêm todo
function addTodo() {
    const text = todoInput.value.trim();
    
    if (text === '') {
        alert('Vui lòng nhập công việc!');
        return;
    }
    
    // Tạo list item
    const li = document.createElement('li');
    li.className = 'todo-item';
    
    // Tạo checkbox
    const checkbox = document.createElement('input');
    checkbox.type = 'checkbox';
    checkbox.addEventListener('change', function() {
        li.classList.toggle('completed');
    });
    
    // Tạo text span
    const span = document.createElement('span');
    span.textContent = text;
    span.style.marginLeft = '10px';
    
    // Tạo delete button
    const deleteBtn = document.createElement('button');
    deleteBtn.textContent = 'Xóa';
    deleteBtn.style.marginLeft = '10px';
    deleteBtn.addEventListener('click', function() {
        li.remove();
    });
    
    // Thêm vào li
    li.appendChild(checkbox);
    li.appendChild(span);
    li.appendChild(deleteBtn);
    
    // Thêm vào list
    todoList.appendChild(li);
    
    // Clear input
    todoInput.value = '';
    todoInput.focus();
}

// Event listeners
addBtn.addEventListener('click', addTodo);

todoInput.addEventListener('keypress', (e) => {
    if (e.key === 'Enter') {
        addTodo();
    }
});
```

## Ví dụ: Counter App

```javascript
let count = 0;

const counterDisplay = document.querySelector('#counter');
const increaseBtn = document.querySelector('#increase');
const decreaseBtn = document.querySelector('#decrease');
const resetBtn = document.querySelector('#reset');

function updateDisplay() {
    counterDisplay.textContent = count;
    
    // Change color based on value
    if (count > 0) {
        counterDisplay.style.color = 'green';
    } else if (count < 0) {
        counterDisplay.style.color = 'red';
    } else {
        counterDisplay.style.color = 'black';
    }
}

increaseBtn.addEventListener('click', () => {
    count++;
    updateDisplay();
});

decreaseBtn.addEventListener('click', () => {
    count--;
    updateDisplay();
});

resetBtn.addEventListener('click', () => {
    count = 0;
    updateDisplay();
});

// Initialize
updateDisplay();
```

## Best Practices

1. **Cache DOM queries**: Lưu kết quả querySelector
2. **Event delegation**: Cho danh sách động
3. **Use classList**: Thay vì thao tác className
4. **Prevent default**: Khi cần ngăn hành vi mặc định
5. **Remove event listeners**: Khi không cần nữa

```javascript
// ❌ Query nhiều lần
document.querySelector('#btn').addEventListener('click', () => {
    document.querySelector('#output').textContent = 'Clicked';
});

// ✅ Cache query
const btn = document.querySelector('#btn');
const output = document.querySelector('#output');

btn.addEventListener('click', () => {
    output.textContent = 'Clicked';
});
```

## Kết luận

DOM Manipulation cho phép:
- Chọn elements: querySelector, querySelectorAll
- Thay đổi content: textContent, innerHTML
- Thay đổi styles: style, classList
- Tạo/xóa elements: createElement, remove
- Xử lý events: addEventListener

Master DOM để build interactive web apps! 🎯
