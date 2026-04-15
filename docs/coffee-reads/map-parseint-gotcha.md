# The Day `['10', '10', '10'].map(parseInt)` Broke Someone's Brain ☕

> **4 min read** · JavaScript

Here's a question that trips up experienced JavaScript developers.

What does this return?

```javascript
['10', '10', '10'].map(parseInt)
```

Your gut says `[10, 10, 10]`.

The actual answer is `[10, NaN, 2]`.

---

## Why Does This Happen?

To understand it, you need to know two things:

**1. `map` passes three arguments to its callback, not one.**

```javascript
array.map((element, index, array) => ...)
```

Most of the time you only use the first. But `map` always passes all three.

**2. `parseInt` accepts two arguments: the string AND a radix (base).**

```javascript
parseInt('10', 10)  // 10 — parse "10" in base 10
parseInt('10', 2)   // 2  — parse "10" in base 2 (binary)
parseInt('10', 1)   // NaN — base 1 doesn't exist
```

The radix is the *number base* to use when parsing. Base 10 is normal decimal. Base 2 is binary.

---

## Putting It Together

When you write `['10', '10', '10'].map(parseInt)`, you're effectively writing:

```javascript
parseInt('10', 0)  // index 0 → special case, defaults to base 10 → 10
parseInt('10', 1)  // index 1 → base 1 doesn't exist → NaN
parseInt('10', 2)  // index 2 → base 2 (binary) → "10" in binary = 2
```

`map` passes the **index as the second argument**, and `parseInt` interprets it as a **radix**.

Result: `[10, NaN, 2]`

---

## The Fix

If you want to parse each string as a plain integer, be explicit:

```javascript
// ✅ Option 1: Arrow function wrapper
['10', '10', '10'].map(str => parseInt(str, 10))
// [10, 10, 10]

// ✅ Option 2: Number()
['10', '10', '10'].map(Number)
// [10, 10, 10]
```

`Number()` only takes one argument, so it doesn't have this problem.

---

## The Broader Lesson

This gotcha isn't just about `parseInt`. It reveals something important about passing functions directly into higher-order functions:

```javascript
// These all look clean but can surprise you:
['1', '2', '3'].map(parseInt)   // [1, NaN, NaN]
['a', 'b', 'c'].forEach(console.log)  // logs element, index, AND array

// What you probably meant:
['1', '2', '3'].map(n => parseInt(n, 10))
['a', 'b', 'c'].forEach(item => console.log(item))
```

When a function's signature doesn't exactly match what `map`, `forEach`, or `filter` needs, wrap it in an arrow function. It costs you a few characters and saves you an hour of debugging.

---

## The Takeaway

> `map` passes `(element, index, array)` to its callback. `parseInt` takes `(string, radix)`. Passing `parseInt` directly to `map` means the index becomes the radix — giving unexpected results. Always wrap functions in arrow functions when the signatures don't match cleanly.

This is one of the most well-known JavaScript gotchas. Knowing it (and more importantly, *explaining* it) makes a strong impression in interviews.

---

## Want to Go Deeper?

- 📄 [JavaScript Guide](../frontend/javascript.md#arrays--array-methods) — array methods in detail
- 📄 [Array Methods Cheat Sheet](../cheat-sheets/array-methods.md) — map, filter, reduce and more

---

*← [Back to Coffee Reads](./README.md)*
