# JavaScript Array Methods Cheat Sheet

> Quick reference for essential array methods with examples

## 📋 Table of Contents

- [Adding/Removing Elements](#addingremoving-elements)
- [Searching](#searching)
- [Transformation](#transformation)
- [Iteration](#iteration)
- [Sorting & Reversing](#sorting--reversing)
- [Other Useful Methods](#other-useful-methods)

---

## Adding/Removing Elements

### `push()` - Add to End

```javascript
const arr = [1, 2, 3];
arr.push(4);        // [1, 2, 3, 4]
arr.push(5, 6);     // [1, 2, 3, 4, 5, 6]
// Returns: new length
```

### `pop()` - Remove from End

```javascript
const arr = [1, 2, 3];
const last = arr.pop();  // last = 3, arr = [1, 2]
// Returns: removed element
```

### `unshift()` - Add to Beginning

```javascript
const arr = [2, 3];
arr.unshift(1);     // [1, 2, 3]
arr.unshift(-1, 0); // [-1, 0, 1, 2, 3]
// Returns: new length
```

### `shift()` - Remove from Beginning

```javascript
const arr = [1, 2, 3];
const first = arr.shift();  // first = 1, arr = [2, 3]
// Returns: removed element
```

### `splice()` - Add/Remove Anywhere

```javascript
const arr = [1, 2, 3, 4, 5];

// Remove 2 elements starting at index 2
arr.splice(2, 2);  // arr = [1, 2, 5]

// Insert elements at index 1
arr.splice(1, 0, 'a', 'b');  // arr = [1, 'a', 'b', 2, 5]

// Replace elements
arr.splice(1, 2, 'x', 'y');  // arr = [1, 'x', 'y', 2, 5]
// Returns: array of removed elements
```

---

## Searching

### `find()` - First Match

```javascript
const users = [
    { id: 1, name: 'Alice' },
    { id: 2, name: 'Bob' },
    { id: 3, name: 'Charlie' }
];

const user = users.find(u => u.id === 2);
// { id: 2, name: 'Bob' }

const notFound = users.find(u => u.id === 99);
// undefined
```

### `findIndex()` - Index of First Match

```javascript
const numbers = [5, 12, 8, 130, 44];

const index = numbers.findIndex(n => n > 10);
// 1 (index of 12)

const notFound = numbers.findIndex(n => n > 200);
// -1
```

### `indexOf()` - Index of Value

```javascript
const fruits = ['apple', 'banana', 'orange', 'banana'];

fruits.indexOf('banana');     // 1
fruits.indexOf('banana', 2);  // 3 (start searching from index 2)
fruits.indexOf('grape');      // -1 (not found)
```

### `includes()` - Check if Value Exists

```javascript
const arr = [1, 2, 3, 4, 5];

arr.includes(3);    // true
arr.includes(10);   // false
arr.includes(3, 3); // false (start search from index 3)
```

### `some()` - At Least One Matches

```javascript
const numbers = [1, 2, 3, 4, 5];

numbers.some(n => n > 3);   // true
numbers.some(n => n > 10);  // false
```

### `every()` - All Must Match

```javascript
const numbers = [1, 2, 3, 4, 5];

numbers.every(n => n > 0);  // true
numbers.every(n => n > 3);  // false
```

---

## Transformation

### `map()` - Transform Each Element

```javascript
const numbers = [1, 2, 3, 4, 5];

// Double each number
const doubled = numbers.map(n => n * 2);
// [2, 4, 6, 8, 10]

// Extract property from objects
const users = [
    { name: 'Alice', age: 25 },
    { name: 'Bob', age: 30 }
];
const names = users.map(u => u.name);
// ['Alice', 'Bob']
```

### `filter()` - Keep Elements That Pass Test

```javascript
const numbers = [1, 2, 3, 4, 5, 6];

// Get even numbers
const evens = numbers.filter(n => n % 2 === 0);
// [2, 4, 6]

// Filter objects
const users = [
    { name: 'Alice', age: 25 },
    { name: 'Bob', age: 17 },
    { name: 'Charlie', age: 30 }
];
const adults = users.filter(u => u.age >= 18);
// [{ name: 'Alice', age: 25 }, { name: 'Charlie', age: 30 }]
```

### `reduce()` - Reduce to Single Value

```javascript
const numbers = [1, 2, 3, 4, 5];

// Sum all numbers
const sum = numbers.reduce((total, n) => total + n, 0);
// 15

// Count occurrences
const fruits = ['apple', 'banana', 'apple', 'orange', 'banana', 'apple'];
const count = fruits.reduce((acc, fruit) => {
    acc[fruit] = (acc[fruit] || 0) + 1;
    return acc;
}, {});
// { apple: 3, banana: 2, orange: 1 }

// Max value
const max = numbers.reduce((max, n) => n > max ? n : max, numbers[0]);
// 5
```

### `flat()` - Flatten Nested Arrays

```javascript
const nested = [1, [2, 3], [4, [5, 6]]];

nested.flat();      // [1, 2, 3, 4, [5, 6]]
nested.flat(2);     // [1, 2, 3, 4, 5, 6]
nested.flat(Infinity);  // Flatten all levels
```

### `flatMap()` - Map then Flatten

```javascript
const sentences = ['Hello world', 'How are you'];

const words = sentences.flatMap(s => s.split(' '));
// ['Hello', 'world', 'How', 'are', 'you']

// Equivalent to:
sentences.map(s => s.split(' ')).flat();
```

---

## Iteration

### `forEach()` - Execute Function for Each

```javascript
const numbers = [1, 2, 3, 4, 5];

numbers.forEach(n => console.log(n));
// Logs: 1, 2, 3, 4, 5

// With index
numbers.forEach((n, index) => {
    console.log(`Index ${index}: ${n}`);
});

// Note: forEach doesn't return anything (returns undefined)
```

---

## Sorting & Reversing

### `sort()` - Sort Array (Mutates!)

```javascript
const fruits = ['banana', 'apple', 'cherry'];
fruits.sort();  // ['apple', 'banana', 'cherry']

// Numbers (need compare function!)
const numbers = [10, 5, 40, 25, 1000, 1];

// ❌ Wrong (treats as strings)
numbers.sort();  // [1, 10, 1000, 25, 40, 5]

// ✅ Correct (ascending)
numbers.sort((a, b) => a - b);  // [1, 5, 10, 25, 40, 1000]

// ✅ Descending
numbers.sort((a, b) => b - a);  // [1000, 40, 25, 10, 5, 1]

// Sort objects
const users = [
    { name: 'Charlie', age: 30 },
    { name: 'Alice', age: 25 },
    { name: 'Bob', age: 35 }
];

users.sort((a, b) => a.age - b.age);
// Sorted by age ascending
```

### `reverse()` - Reverse Array (Mutates!)

```javascript
const arr = [1, 2, 3, 4, 5];
arr.reverse();  // [5, 4, 3, 2, 1]

// To avoid mutation, copy first
const original = [1, 2, 3, 4, 5];
const reversed = [...original].reverse();
// original = [1, 2, 3, 4, 5]
// reversed = [5, 4, 3, 2, 1]
```

---

## Other Useful Methods

### `slice()` - Extract Portion (No Mutation)

```javascript
const arr = [1, 2, 3, 4, 5];

arr.slice(2);       // [3, 4, 5] (from index 2 to end)
arr.slice(1, 3);    // [2, 3] (from index 1 to 2)
arr.slice(-2);      // [4, 5] (last 2 elements)

// Copy array
const copy = arr.slice();
```

### `concat()` - Merge Arrays

```javascript
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];

const merged = arr1.concat(arr2);  // [1, 2, 3, 4, 5, 6]

// Or use spread operator
const merged = [...arr1, ...arr2];
```

### `join()` - Array to String

```javascript
const words = ['Hello', 'World'];

words.join();       // "Hello,World"
words.join(' ');    // "Hello World"
words.join('-');    // "Hello-World"
```

### `fill()` - Fill with Static Value

```javascript
const arr = [1, 2, 3, 4, 5];

arr.fill(0);        // [0, 0, 0, 0, 0]
arr.fill(7, 2, 4);  // [1, 2, 7, 7, 5] (from index 2 to 3)

// Create array filled with value
const zeros = new Array(5).fill(0);  // [0, 0, 0, 0, 0]
```

### `Array.from()` - Create Array from Iterable

```javascript
// From string
Array.from('hello');  // ['h', 'e', 'l', 'l', 'o']

// From Set
const set = new Set([1, 2, 3]);
Array.from(set);  // [1, 2, 3]

// With mapping function
Array.from([1, 2, 3], x => x * 2);  // [2, 4, 6]

// Create range
Array.from({ length: 5 }, (_, i) => i);  // [0, 1, 2, 3, 4]
```

### `Array.isArray()` - Check if Array

```javascript
Array.isArray([1, 2, 3]);      // true
Array.isArray('hello');        // false
Array.isArray({ a: 1 });       // false
```

---

## Chaining Methods

```javascript
const users = [
    { name: 'Alice', age: 25, active: true },
    { name: 'Bob', age: 17, active: false },
    { name: 'Charlie', age: 30, active: true },
    { name: 'David', age: 22, active: true }
];

// Get names of active adult users
const names = users
    .filter(u => u.active)
    .filter(u => u.age >= 18)
    .map(u => u.name)
    .sort();

// ['Alice', 'Charlie', 'David']
```

---

## Performance Tips

### Mutating vs Non-Mutating

**Mutating (change original):**
- `push()`, `pop()`, `shift()`, `unshift()`
- `splice()`, `reverse()`, `sort()`, `fill()`

**Non-mutating (return new array):**
- `map()`, `filter()`, `reduce()`
- `slice()`, `concat()`, spread `[...arr]`

### Efficiency

```javascript
// ✅ Good: Single pass
const result = arr.filter(x => x > 5).map(x => x * 2);

// ❌ Inefficient: Multiple passes
arr.forEach(...)
arr.map(...)
arr.filter(...)
```

---

**Keywords**: JavaScript array methods, map filter reduce, array manipulation, JS array cheat sheet, array methods examples, JavaScript arrays, forEach map filter
