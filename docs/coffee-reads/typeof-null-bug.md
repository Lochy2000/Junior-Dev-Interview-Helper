# Why `typeof null === 'object'` Is a 30-Year-Old Bug ☕

> **3 min read** · JavaScript

Open your browser console right now and type:

```javascript
typeof null
```

It returns `"object"`.

`null` is not an object. Everyone knows this. The creator of JavaScript knows this. It's been a known bug since 1995. And it will never, ever be fixed.

Here's why.

---

## A Bit of History

JavaScript was created by Brendan Eich in **10 days** in 1995. The language shipped with a small bug in how it identified types internally.

In the original C implementation, JavaScript values were stored with a type tag — a few bits at the start of the value that said what kind of thing it was.

The tag for objects was `000`.

The representation for `null` was a **null pointer** — which in most systems is literally the value `0x00000000`, i.e., all zeros.

So when `typeof` read the type tag of `null`... it saw `000`. It said: *object*.

---

## Why Not Just Fix It?

In 2006, a proposal was submitted to fix `typeof null` to return `"null"`.

It was **rejected**.

The reason: too much code on the internet already relied on the broken behaviour. Fixing it would break millions of existing websites. The bug had become the standard.

This is a fundamental challenge in web development — the internet never truly forgets. Languages used by billions of people can't make breaking changes, even to fix genuine mistakes.

---

## What You Actually Use Instead

Since `typeof null` is unreliable, here's how you check for null properly:

```javascript
// ❌ Don't rely on typeof for null
typeof null === 'object'  // true, but misleading

// ✅ Direct equality check
value === null  // true only if value is exactly null

// ✅ Check for null OR undefined
value == null   // true for both null and undefined
```

And if you need to check that something is a *real* object (not null):

```javascript
function isObject(value) {
  return typeof value === 'object' && value !== null;
}

isObject({})    // true
isObject([])    // true
isObject(null)  // false ✓
```

---

## The Wider Lesson

`typeof` has always been a bit odd in JavaScript:

```javascript
typeof undefined   // "undefined"
typeof true        // "boolean"
typeof 42          // "number"
typeof "hello"     // "string"
typeof {}          // "object"
typeof []          // "object"  ← arrays aren't flagged differently
typeof null        // "object"  ← the bug
typeof function(){} // "function"  ← functions get special treatment
```

For real type-checking, `typeof` is a starting point — not a complete solution. Use `Array.isArray()` for arrays, `=== null` for null, and `instanceof` for class instances.

---

## The Takeaway

> `typeof null === 'object'` is a bug from 1995 that will never be fixed because fixing it would break the internet. It's a good reminder that languages used at scale carry their history with them — for better and worse.

When an interviewer asks about JavaScript quirks, this is one of the best answers you can give. It shows you understand not just *what* is weird, but *why* it happened and *what to do about it*.

---

## Want to Go Deeper?

- 📄 [JavaScript Guide](../frontend/javascript.md#variables--data-types) — data types, type checking
- 🌐 [The history of `typeof null` — 2ality.com](https://2ality.com/2013/10/typeof-null.html) — Axel Rauschmayer's deep dive

---

*← [Back to Coffee Reads](./README.md)*
