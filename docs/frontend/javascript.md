# JavaScript Essentials - Modern JS Guide (ES6+)

> Master JavaScript fundamentals, async programming, ES6+ features, DOM manipulation, and interview concepts

## 📋 Table of Contents

- [JavaScript Basics](#javascript-basics)
- [Variables & Data Types](#variables--data-types)
- [Functions](#functions)
- [Arrays & Array Methods](#arrays--array-methods)
- [Objects](#objects)
- [Async JavaScript](#async-javascript)
- [DOM Manipulation](#dom-manipulation)
- [ES6+ Features](#es6-features)
- [Common Patterns](#common-patterns)
- [Interview Questions](#interview-questions)

---

## JavaScript Basics

JavaScript is a high-level, interpreted programming language that runs in browsers and on servers (Node.js).

### Where to Write JavaScript

```html
<!-- Inline (avoid) -->
<button onclick="alert('Clicked')">Click</button>

<!-- Internal -->
<script>
    console.log('Hello World');
</script>

<!-- External (recommended) -->
<script src="script.js"></script>
<!-- For modern JS modules -->
<script type="module" src="app.js"></script>
```

---

## Variables & Data Types

### Variable Declaration

```javascript
// const - Cannot reassign (preferred)
const name = 'John';
const PI = 3.14159;

// let - Can reassign, block-scoped
let age = 25;
age = 26; // OK

// var - Old way, function-scoped (avoid)
var oldStyle = 'avoid using var';
```

### Primitive Data Types

```javascript
// String
const name = "Alice";
const greeting = 'Hello';
const template = `Hi, ${name}!`; // Template literal

// Number
const integer = 42;
const float = 3.14;
const negative = -10;

// Boolean
const isActive = true;
const isComplete = false;

// Null (intentional absence)
const empty = null;

// Undefined (not yet assigned)
let notAssigned;
console.log(notAssigned); // undefined

// Symbol (unique identifier)
const id = Symbol('id');

// BigInt (large integers)
const huge = 9007199254740991n;
```

### Type Checking

```javascript
typeof "hello"     // "string"
typeof 42          // "number"
typeof true        // "boolean"
typeof undefined   // "undefined"
typeof null        // "object" (historical bug)
typeof {}          // "object"
typeof []          // "object" (use Array.isArray())
typeof function(){} // "function"
```

---

## Functions

### Function Declaration

```javascript
function greet(name) {
    return `Hello, ${name}!`;
}

console.log(greet('Alice')); // "Hello, Alice!"
```

### Function Expression

```javascript
const greet = function(name) {
    return `Hello, ${name}!`;
};
```

### Arrow Functions (ES6)

```javascript
// Traditional
const add = function(a, b) {
    return a + b;
};

// Arrow function
const add = (a, b) => {
    return a + b;
};

// Implicit return (one-liner)
const add = (a, b) => a + b;

// Single parameter (no parentheses needed)
const double = x => x * 2;

// No parameters
const greet = () => 'Hello!';
```

### Default Parameters

```javascript
function greet(name = 'Guest') {
    return `Hello, ${name}!`;
}

greet();        // "Hello, Guest!"
greet('Alice'); // "Hello, Alice!"
```

### Rest Parameters

```javascript
function sum(...numbers) {
    return numbers.reduce((total, num) => total + num, 0);
}

sum(1, 2, 3, 4); // 10
```

---

## Arrays & Array Methods

### Creating Arrays

```javascript
const fruits = ['apple', 'banana', 'orange'];
const numbers = [1, 2, 3, 4, 5];
const mixed = [1, 'two', true, null, {name: 'object'}];
```

### Essential Array Methods

```javascript
const numbers = [1, 2, 3, 4, 5];

// map - Transform each element
const doubled = numbers.map(n => n * 2);
// [2, 4, 6, 8, 10]

// filter - Keep elements that pass test
const evens = numbers.filter(n => n % 2 === 0);
// [2, 4]

// reduce - Reduce to single value
const sum = numbers.reduce((total, n) => total + n, 0);
// 15

// find - First element that matches
const found = numbers.find(n => n > 3);
// 4

// findIndex - Index of first match
const index = numbers.findIndex(n => n > 3);
// 3

// some - At least one matches
const hasEven = numbers.some(n => n % 2 === 0);
// true

// every - All must match
const allPositive = numbers.every(n => n > 0);
// true

// forEach - Execute for each (no return value)
numbers.forEach(n => console.log(n));

// includes - Check if value exists
numbers.includes(3); // true

// sort - Sort array (mutates original!)
const sorted = [...numbers].sort((a, b) => a - b);

// reverse - Reverse array (mutates!)
const reversed = [...numbers].reverse();
```

### Spread Operator

```javascript
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];

// Combine arrays
const combined = [...arr1, ...arr2];
// [1, 2, 3, 4, 5, 6]

// Copy array
const copy = [...arr1];

// Add to array
const withExtra = [...arr1, 4, 5];
```

### Destructuring

```javascript
const [first, second, ...rest] = [1, 2, 3, 4, 5];
// first = 1, second = 2, rest = [3, 4, 5]

// Skip elements
const [a, , c] = [1, 2, 3];
// a = 1, c = 3
```

---

## Objects

### Creating Objects

```javascript
const person = {
    name: 'Alice',
    age: 25,
    city: 'New York',
    greet() {
        return `Hi, I'm ${this.name}`;
    }
};
```

### Accessing Properties

```javascript
// Dot notation
person.name; // 'Alice'

// Bracket notation (for dynamic keys)
person['age']; // 25
const key = 'city';
person[key]; // 'New York'
```

### Object Methods

```javascript
// Get keys
Object.keys(person);
// ['name', 'age', 'city', 'greet']

// Get values
Object.values(person);
// ['Alice', 25, 'New York', function]

// Get entries
Object.entries(person);
// [['name', 'Alice'], ['age', 25], ...]

// Merge objects
const merged = { ...person, country: 'USA' };

// Clone object
const clone = { ...person };
```

### Object Destructuring

```javascript
const { name, age } = person;
// name = 'Alice', age = 25

// Rename variable
const { name: fullName } = person;
// fullName = 'Alice'

// Default values
const { job = 'Unemployed' } = person;
// job = 'Unemployed'
```

### Shorthand Property Names

```javascript
const name = 'Bob';
const age = 30;

// Old way
const person = { name: name, age: age };

// ES6 shorthand
const person = { name, age };
```

---

## Async JavaScript

### Callbacks

```javascript
function fetchData(callback) {
    setTimeout(() => {
        callback('Data received');
    }, 1000);
}

fetchData(data => {
    console.log(data); // After 1 second
});
```

### Promises

```javascript
// Creating a promise
const promise = new Promise((resolve, reject) => {
    const success = true;

    setTimeout(() => {
        if (success) {
            resolve('Success!');
        } else {
            reject('Error!');
        }
    }, 1000);
});

// Using a promise
promise
    .then(result => console.log(result))
    .catch(error => console.error(error))
    .finally(() => console.log('Done'));
```

### Async/Await

```javascript
// Async function returns a promise
async function fetchUser() {
    try {
        const response = await fetch('/api/user');
        const data = await response.json();
        return data;
    } catch (error) {
        console.error('Error:', error);
    }
}

// Using it
fetchUser().then(user => console.log(user));

// Or with async
async function main() {
    const user = await fetchUser();
    console.log(user);
}
```

### Promise Methods

```javascript
// Promise.all - Wait for all (fails if any fails)
const [users, posts] = await Promise.all([
    fetch('/api/users'),
    fetch('/api/posts')
]);

// Promise.race - First to complete
const result = await Promise.race([
    fetch('/api/fast'),
    fetch('/api/slow')
]);

// Promise.allSettled - Wait for all (doesn't fail)
const results = await Promise.allSettled([
    fetch('/api/success'),
    fetch('/api/fail')
]);
```

---

## DOM Manipulation

### Selecting Elements

```javascript
// Single element
const element = document.getElementById('myId');
const element = document.querySelector('.myClass');
const element = document.querySelector('#myId');

// Multiple elements
const elements = document.getElementsByClassName('myClass');
const elements = document.getElementsByTagName('p');
const elements = document.querySelectorAll('.myClass');
```

### Modifying Elements

```javascript
// Change content
element.textContent = 'New text';
element.innerHTML = '<strong>Bold text</strong>';

// Change attributes
element.setAttribute('data-id', '123');
element.id = 'newId';
element.className = 'new-class';

// Change styles
element.style.color = 'red';
element.style.backgroundColor = 'blue';

// Add/remove classes
element.classList.add('active');
element.classList.remove('hidden');
element.classList.toggle('visible');
element.classList.contains('active'); // true/false
```

### Creating & Inserting Elements

```javascript
// Create element
const div = document.createElement('div');
div.textContent = 'Hello';
div.className = 'message';

// Insert
parent.appendChild(div);
parent.insertBefore(div, referenceElement);
parent.append(div); // Can add multiple nodes/text
parent.prepend(div); // Add at start

// Remove
element.remove();
parent.removeChild(element);
```

### Event Listeners

```javascript
// Add event listener
button.addEventListener('click', (event) => {
    console.log('Clicked!', event);
});

// Remove event listener
function handleClick(event) {
    console.log('Clicked!');
}
button.addEventListener('click', handleClick);
button.removeEventListener('click', handleClick);

// Common events
element.addEventListener('click', handler);
element.addEventListener('submit', handler);
element.addEventListener('input', handler);
element.addEventListener('change', handler);
element.addEventListener('mouseover', handler);
element.addEventListener('keydown', handler);

// Event delegation (efficient for many elements)
parent.addEventListener('click', (event) => {
    if (event.target.matches('.button')) {
        console.log('Button clicked!');
    }
});
```

---

## ES6+ Features

### Let & Const

```javascript
// Block scoping
if (true) {
    let x = 10;
    const y = 20;
}
// x and y not accessible here
```

### Template Literals

```javascript
const name = 'Alice';
const age = 25;

// Old way
const message = 'My name is ' + name + ' and I am ' + age;

// Template literal
const message = `My name is ${name} and I am ${age}`;

// Multiline
const html = `
    <div>
        <h1>${name}</h1>
        <p>Age: ${age}</p>
    </div>
`;
```

### Modules

```javascript
// export.js
export const PI = 3.14159;
export function add(a, b) {
    return a + b;
}
export default class Calculator {}

// import.js
import Calculator from './export.js';
import { PI, add } from './export.js';
import * as math from './export.js';
```

### Optional Chaining

```javascript
const user = {
    name: 'Alice',
    address: {
        city: 'NYC'
    }
};

// Old way (can error if undefined)
const zip = user.address.zip;

// Optional chaining (safe)
const zip = user.address?.zip; // undefined (no error)
const country = user.contact?.phone?.country; // undefined
```

### Nullish Coalescing

```javascript
// || returns first truthy value
const value = '' || 'default'; // 'default'

// ?? returns first non-nullish value
const value = '' ?? 'default'; // '' (not null/undefined)
const value = null ?? 'default'; // 'default'
```

---

## Common Patterns

### Debouncing

```javascript
function debounce(func, delay) {
    let timeoutId;
    return function(...args) {
        clearTimeout(timeoutId);
        timeoutId = setTimeout(() => func.apply(this, args), delay);
    };
}

// Usage
const handleSearch = debounce((query) => {
    console.log('Searching for:', query);
}, 300);

searchInput.addEventListener('input', (e) => handleSearch(e.target.value));
```

### Throttling

```javascript
function throttle(func, limit) {
    let inThrottle;
    return function(...args) {
        if (!inThrottle) {
            func.apply(this, args);
            inThrottle = true;
            setTimeout(() => inThrottle = false, limit);
        }
    };
}

// Usage
const handleScroll = throttle(() => {
    console.log('Scrolling...');
}, 1000);

window.addEventListener('scroll', handleScroll);
```

---

## Interview Questions

### 1. What is the difference between `==` and `===`?
**Answer**:
- `==` does type coercion: `'5' == 5` is `true`
- `===` checks type and value: `'5' === 5` is `false`

### 2. What is hoisting?
**Answer**: Variable and function declarations are moved to top of scope during compilation. `var` is hoisted and initialized with `undefined`, `let`/`const` are hoisted but not initialized (temporal dead zone).

### 3. Explain closures
**Answer**: A closure is when a function remembers variables from its outer scope even after the outer function has returned.
```javascript
function outer() {
    const x = 10;
    return function inner() {
        console.log(x); // Remembers x
    };
}
const fn = outer();
fn(); // 10
```

### 4. What is the event loop?
**Answer**: JavaScript is single-threaded. The event loop handles async operations by queuing callbacks to execute after the call stack is clear.

### 5. What is `this`?
**Answer**: `this` refers to the context object. In methods it's the object, in arrow functions it's lexical (inherited from parent scope).

### 6. Difference between `null` and `undefined`?
**Answer**:
- `undefined`: Variable declared but not assigned
- `null`: Intentional absence of value

### 7. What are promises?
**Answer**: Objects representing eventual completion/failure of async operation. Better than callbacks (avoids callback hell).

### 8. Explain `map` vs `forEach`
**Answer**:
- `map`: Returns new array with transformed values
- `forEach`: Iterates but returns `undefined`

### 9. What is destructuring?
**Answer**: Syntax to extract values from arrays/objects:
```javascript
const [a, b] = [1, 2];
const {name, age} = person;
```

### 10. What is the spread operator?
**Answer**: `...` expands iterables:
```javascript
const arr = [1, 2, 3];
const copy = [...arr];
const merged = [...arr1, ...arr2];
```

---

**Keywords**: JavaScript tutorial, ES6, async await, promises, DOM manipulation, JavaScript interview questions, array methods, closures, event loop, JavaScript fundamentals
