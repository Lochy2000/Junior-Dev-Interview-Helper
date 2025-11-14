# React - Complete Guide for Junior Developers

> Master React components, hooks, state management, and common interview patterns

## 📋 Table of Contents

- [What is React?](#what-is-react)
- [Components](#components)
- [JSX](#jsx)
- [Props](#props)
- [State & Hooks](#state--hooks)
- [Lifecycle & Effects](#lifecycle--effects)
- [Event Handling](#event-handling)
- [Conditional Rendering](#conditional-rendering)
- [Lists & Keys](#lists--keys)
- [Forms](#forms)
- [Common Hooks](#common-hooks)
- [Interview Questions](#interview-questions)

---

## What is React?

**React** is a JavaScript library for building user interfaces, developed by Facebook. It uses a component-based architecture and virtual DOM for efficient updates.

### Key Concepts

- **Component-based**: UI built from reusable pieces
- **Declarative**: Describe what UI should look like
- **Virtual DOM**: Efficient updates by comparing changes
- **One-way data flow**: Data flows down through props

---

## Components

### Function Components (Modern)

```jsx
function Welcome(props) {
    return <h1>Hello, {props.name}</h1>;
}

// Arrow function
const Welcome = (props) => {
    return <h1>Hello, {props.name}</h1>;
};

// Implicit return
const Welcome = ({name}) => <h1>Hello, {name}</h1>;
```

### Class Components (Legacy)

```jsx
class Welcome extends React.Component {
    render() {
        return <h1>Hello, {this.props.name}</h1>;
    }
}
```

**Prefer function components with hooks** - they're simpler and more modern.

---

## JSX

JSX is a syntax extension that looks like HTML but is JavaScript.

### Basic JSX

```jsx
const element = <h1>Hello World</h1>;

// With expressions
const name = 'Alice';
const element = <h1>Hello, {name}</h1>;

// Multi-line (wrap in parentheses)
const element = (
    <div>
        <h1>Title</h1>
        <p>Paragraph</p>
    </div>
);
```

### JSX Rules

```jsx
// 1. Must return single parent element
// ❌ Wrong
return (
    <h1>Title</h1>
    <p>Text</p>
);

// ✅ Correct
return (
    <div>
        <h1>Title</h1>
        <p>Text</p>
    </div>
);

// ✅ Or use Fragment
return (
    <>
        <h1>Title</h1>
        <p>Text</p>
    </>
);

// 2. Use className instead of class
<div className="container">

// 3. Use camelCase for attributes
<div onClick={handleClick} tabIndex="0">

// 4. Self-closing tags need /
<img src="image.jpg" />
<input type="text" />
```

---

## Props

Props are arguments passed to components (like function parameters).

### Passing Props

```jsx
function App() {
    return <Welcome name="Alice" age={25} />;
}

function Welcome(props) {
    return <p>Hello, {props.name}, age {props.age}</p>;
}
```

### Destructuring Props

```jsx
// Instead of props.name, props.age
function Welcome({ name, age }) {
    return <p>Hello, {name}, age {age}</p>;
}

// With default values
function Welcome({ name = 'Guest', age = 0 }) {
    return <p>Hello, {name}, age {age}</p>;
}
```

### Children Prop

```jsx
function Card({ children }) {
    return <div className="card">{children}</div>;
}

// Usage
<Card>
    <h2>Title</h2>
    <p>Content</p>
</Card>
```

### Props are Read-Only

```jsx
// ❌ Never modify props
function Component(props) {
    props.value = 'new'; // ERROR!
}

// ✅ Use state for changing values
function Component({ initialValue }) {
    const [value, setValue] = useState(initialValue);
}
```

---

## State & Hooks

State is data that changes over time in a component.

### useState Hook

```jsx
import { useState } from 'react';

function Counter() {
    // [stateValue, setterFunction] = useState(initialValue)
    const [count, setCount] = useState(0);

    return (
        <div>
            <p>Count: {count}</p>
            <button onClick={() => setCount(count + 1)}>
                Increment
            </button>
        </div>
    );
}
```

### Multiple State Variables

```jsx
function Form() {
    const [name, setName] = useState('');
    const [email, setEmail] = useState('');
    const [age, setAge] = useState(0);

    // Or use object (requires spreading)
    const [form, setForm] = useState({
        name: '',
        email: '',
        age: 0
    });

    const updateName = (newName) => {
        setForm({ ...form, name: newName });
    };
}
```

### State Updates are Async

```jsx
function Counter() {
    const [count, setCount] = useState(0);

    const handleClick = () => {
        setCount(count + 1);
        console.log(count); // Still 0! (old value)
    };

    // To use previous state value
    const handleClick = () => {
        setCount(prevCount => prevCount + 1);
    };
}
```

---

## Lifecycle & Effects

### useEffect Hook

Runs side effects (data fetching, subscriptions, DOM manipulation).

```jsx
import { useEffect } from 'react';

function Component() {
    // Runs after every render
    useEffect(() => {
        console.log('Component rendered');
    });

    // Runs once on mount (empty dependency array)
    useEffect(() => {
        console.log('Component mounted');
    }, []);

    // Runs when dependency changes
    useEffect(() => {
        console.log('Count changed');
    }, [count]);

    // Cleanup function (unmount or before re-run)
    useEffect(() => {
        const timer = setInterval(() => {
            console.log('Tick');
        }, 1000);

        return () => clearInterval(timer); // Cleanup
    }, []);
}
```

### Common Use Cases

```jsx
// Fetch data on mount
useEffect(() => {
    async function fetchData() {
        const response = await fetch('/api/users');
        const data = await response.json();
        setUsers(data);
    }
    fetchData();
}, []);

// Update document title
useEffect(() => {
    document.title = `Count: ${count}`;
}, [count]);

// Event listener
useEffect(() => {
    function handleResize() {
        setWidth(window.innerWidth);
    }

    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
}, []);
```

---

## Event Handling

```jsx
function Button() {
    // Function declaration
    function handleClick() {
        console.log('Clicked!');
    }

    // With event parameter
    function handleClick(event) {
        event.preventDefault();
        console.log('Clicked!');
    }

    // With custom parameters
    function handleClick(id) {
        console.log('Clicked item:', id);
    }

    return (
        <>
            <button onClick={handleClick}>Click</button>

            {/* Arrow function (creates new function each render) */}
            <button onClick={() => handleClick(123)}>
                Click with ID
            </button>

            {/* Access event in arrow function */}
            <button onClick={(e) => {
                e.preventDefault();
                handleClick();
            }}>
                Click
            </button>
        </>
    );
}
```

### Form Events

```jsx
function Form() {
    const [value, setValue] = useState('');

    const handleChange = (e) => {
        setValue(e.target.value);
    };

    const handleSubmit = (e) => {
        e.preventDefault();
        console.log('Submitted:', value);
    };

    return (
        <form onSubmit={handleSubmit}>
            <input
                type="text"
                value={value}
                onChange={handleChange}
            />
            <button type="submit">Submit</button>
        </form>
    );
}
```

---

## Conditional Rendering

### If/Else with &&

```jsx
function Greeting({ isLoggedIn }) {
    return (
        <div>
            {isLoggedIn && <p>Welcome back!</p>}
            {!isLoggedIn && <p>Please log in</p>}
        </div>
    );
}
```

### Ternary Operator

```jsx
function Greeting({ isLoggedIn }) {
    return (
        <div>
            {isLoggedIn ? (
                <p>Welcome back!</p>
            ) : (
                <p>Please log in</p>
            )}
        </div>
    );
}
```

### Early Return

```jsx
function Component({ isLoading, error, data }) {
    if (isLoading) return <p>Loading...</p>;
    if (error) return <p>Error: {error.message}</p>;

    return <div>{data}</div>;
}
```

---

## Lists & Keys

### Rendering Lists

```jsx
function UserList({ users }) {
    return (
        <ul>
            {users.map((user) => (
                <li key={user.id}>
                    {user.name}
                </li>
            ))}
        </ul>
    );
}
```

### Keys are Important

```jsx
// ✅ Good - unique stable ID
<li key={user.id}>{user.name}</li>

// ⚠️ OK for static lists only
<li key={index}>{user.name}</li>

// ❌ Bad - will cause issues
<li>{user.name}</li>
```

**Why keys matter**: React uses keys to track which items changed, were added, or removed for efficient updates.

---

## Forms

### Controlled Components

```jsx
function Form() {
    const [formData, setFormData] = useState({
        username: '',
        email: '',
        message: ''
    });

    const handleChange = (e) => {
        const { name, value } = e.target;
        setFormData({
            ...formData,
            [name]: value
        });
    };

    const handleSubmit = (e) => {
        e.preventDefault();
        console.log(formData);
    };

    return (
        <form onSubmit={handleSubmit}>
            <input
                type="text"
                name="username"
                value={formData.username}
                onChange={handleChange}
            />
            <input
                type="email"
                name="email"
                value={formData.email}
                onChange={handleChange}
            />
            <textarea
                name="message"
                value={formData.message}
                onChange={handleChange}
            />
            <button type="submit">Submit</button>
        </form>
    );
}
```

---

## Common Hooks

### useContext

Share data without passing props down manually.

```jsx
import { createContext, useContext } from 'react';

const ThemeContext = createContext('light');

function App() {
    return (
        <ThemeContext.Provider value="dark">
            <Header />
        </ThemeContext.Provider>
    );
}

function Header() {
    const theme = useContext(ThemeContext);
    return <div className={theme}>Header</div>;
}
```

### useRef

Access DOM elements or persist values between renders.

```jsx
import { useRef } from 'react';

function Input() {
    const inputRef = useRef(null);

    const focusInput = () => {
        inputRef.current.focus();
    };

    return (
        <>
            <input ref={inputRef} type="text" />
            <button onClick={focusInput}>Focus</button>
        </>
    );
}
```

### useMemo

Memoize expensive calculations.

```jsx
import { useMemo } from 'react';

function Component({ items }) {
    const expensiveValue = useMemo(() => {
        return items.reduce((sum, item) => sum + item.price, 0);
    }, [items]);

    return <p>Total: {expensiveValue}</p>;
}
```

### useCallback

Memoize function references.

```jsx
import { useCallback } from 'react';

function Parent() {
    const [count, setCount] = useState(0);

    const handleClick = useCallback(() => {
        console.log('Clicked');
    }, []); // Function recreated only if dependencies change

    return <Child onClick={handleClick} />;
}
```

---

## Interview Questions

### 1. What is React and why use it?
**Answer**: React is a JavaScript library for building UIs using components. Benefits: reusable components, virtual DOM for performance, large ecosystem, declarative syntax.

### 2. What is the virtual DOM?
**Answer**: A lightweight copy of the real DOM. React compares virtual DOM snapshots (diffing) to determine minimal changes needed, then updates real DOM efficiently.

### 3. Class vs Function components?
**Answer**: Function components are simpler, modern, use hooks. Class components are older, use lifecycle methods. Always prefer function components for new code.

### 4. What are hooks?
**Answer**: Functions that let you use React features (state, effects, context) in function components. Examples: useState, useEffect, useContext.

### 5. Explain useState
**Answer**: Hook that adds state to function components. Returns array with [currentValue, setterFunction]. Setter triggers re-render.

### 6. What is useEffect?
**Answer**: Hook for side effects (data fetching, subscriptions, DOM manipulation). Runs after render. Optional cleanup function. Dependency array controls when it runs.

### 7. Props vs State?
**Answer**:
- Props: Passed from parent, read-only, like function parameters
- State: Managed within component, can change, triggers re-renders

### 8. Why do we need keys in lists?
**Answer**: Keys help React identify which items changed, were added, or removed. Should be unique and stable (prefer IDs over index).

### 9. What is prop drilling?
**Answer**: Passing props through multiple levels of components. Solutions: Context API, state management libraries (Redux, Zustand).

### 10. Controlled vs Uncontrolled components?
**Answer**:
- Controlled: Form data handled by React state
- Uncontrolled: Form data handled by DOM (using refs)

---

**Keywords**: React tutorial, React hooks, useState, useEffect, React components, JSX, React interview questions, virtual DOM, React props, React state management
