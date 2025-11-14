# TypeScript - Complete Guide

> Type-safe JavaScript for scalable applications

## 📋 Table of Contents

- [What is TypeScript?](#what-is-typescript)
- [Basic Types](#basic-types)
- [Interfaces](#interfaces)
- [Type Annotations](#type-annotations)
- [Generics](#generics)
- [Common Patterns](#common-patterns)
- [Interview Questions](#interview-questions)

---

## What is TypeScript?

**TypeScript** is a superset of JavaScript that adds static typing. It compiles to plain JavaScript.

### Why Use TypeScript?

✅ Catch errors at compile-time, not runtime
✅ Better IDE support (autocomplete, refactoring)
✅ Self-documenting code
✅ Easier to maintain large codebases
✅ Works with existing JavaScript

---

## Basic Types

```typescript
// Primitives
let name: string = "Alice";
let age: number = 25;
let isActive: boolean = true;

// Arrays
let numbers: number[] = [1, 2, 3];
let names: Array<string> = ["Alice", "Bob"];

// Tuple
let person: [string, number] = ["Alice", 25];

// Any (avoid when possible)
let anything: any = "could be anything";

// Unknown (safer than any)
let userInput: unknown;

// Void (no return value)
function logMessage(message: string): void {
    console.log(message);
}

// Never (never returns)
function throwError(message: string): never {
    throw new Error(message);
}

// Null and Undefined
let nullable: null = null;
let undefined: undefined = undefined;
```

---

## Interfaces

```typescript
// Basic interface
interface User {
    id: number;
    name: string;
    email: string;
    age?: number; // Optional property
}

const user: User = {
    id: 1,
    name: "Alice",
    email: "alice@example.com"
};

// Readonly properties
interface Config {
    readonly apiKey: string;
    readonly baseUrl: string;
}

// Function types
interface MathOperation {
    (a: number, b: number): number;
}

const add: MathOperation = (a, b) => a + b;

// Extending interfaces
interface Employee extends User {
    employeeId: string;
    department: string;
}
```

---

## Type Annotations

```typescript
// Function parameters and return type
function greet(name: string): string {
    return `Hello, ${name}!`;
}

// Optional parameters
function buildName(first: string, last?: string): string {
    return last ? `${first} ${last}` : first;
}

// Default parameters
function greet(name: string = "Guest"): string {
    return `Hello, ${name}!`;
}

// Rest parameters
function sum(...numbers: number[]): number {
    return numbers.reduce((a, b) => a + b, 0);
}

// Object type
function printUser(user: { name: string; age: number }): void {
    console.log(`${user.name} is ${user.age}`);
}
```

---

## Generics

```typescript
// Generic function
function identity<T>(value: T): T {
    return value;
}

identity<string>("hello"); // string
identity<number>(42);      // number

// Generic interface
interface Box<T> {
    value: T;
}

const stringBox: Box<string> = { value: "hello" };
const numberBox: Box<number> = { value: 42 };

// Generic constraints
interface HasLength {
    length: number;
}

function logLength<T extends HasLength>(item: T): void {
    console.log(item.length);
}

logLength("hello");    // ✅ string has length
logLength([1, 2, 3]);  // ✅ array has length
// logLength(42);      // ❌ number doesn't have length
```

---

## Common Patterns

### Union Types

```typescript
// Value can be one of multiple types
let value: string | number;
value = "hello"; // ✅
value = 42;      // ✅
// value = true; // ❌

// Function with union types
function format(value: string | number): string {
    if (typeof value === "string") {
        return value.toUpperCase();
    }
    return value.toFixed(2);
}
```

### Type Aliases

```typescript
// Create custom type
type ID = string | number;
type User = {
    id: ID;
    name: string;
    email: string;
};

// Union of specific values
type Status = "pending" | "approved" | "rejected";
let status: Status = "pending"; // ✅
// let status: Status = "invalid"; // ❌
```

### Intersection Types

```typescript
type Person = {
    name: string;
    age: number;
};

type Employee = {
    employeeId: string;
    department: string;
};

// Combine types
type EmployeePerson = Person & Employee;

const employee: EmployeePerson = {
    name: "Alice",
    age: 25,
    employeeId: "E123",
    department: "Engineering"
};
```

---

## Interview Questions

### 1. What is TypeScript?
**Answer**: Superset of JavaScript that adds static typing. Compiles to JavaScript. Helps catch errors early and improves code quality.

### 2. Interface vs Type?
**Answer**:
- Both define object shapes
- Interfaces can be extended and merged
- Types can represent unions, primitives, tuples
- Use interfaces for objects, types for everything else

### 3. What is `any` and when to use it?
**Answer**: Disables type checking. Use sparingly, only when type is truly unknown or during migration from JavaScript.

### 4. Explain generics
**Answer**: Allow creating reusable components that work with multiple types while maintaining type safety.

### 5. What is type inference?
**Answer**: TypeScript automatically determines types based on values. Don't need to explicitly annotate everything.

---

**Keywords**: TypeScript tutorial, TypeScript basics, TypeScript generics, TypeScript interfaces, type safety, TypeScript interview questions, TypeScript vs JavaScript

---

*Note: This is a starter guide. More comprehensive content coming soon!*
