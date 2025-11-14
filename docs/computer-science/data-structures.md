# Data Structures - Visual Guide

> Master fundamental data structures with visual explanations and code examples

## 📋 Table of Contents

- [Why Data Structures Matter](#why-data-structures-matter)
- [Arrays](#arrays)
- [Linked Lists](#linked-lists)
- [Stacks](#stacks)
- [Queues](#queues)
- [Hash Tables](#hash-tables)
- [Trees](#trees)
- [Graphs](#graphs)
- [Heaps](#heaps)
- [Comparison Chart](#comparison-chart)
- [Interview Questions](#interview-questions)

---

## Why Data Structures Matter

**Different structures = different trade-offs**

- Want fast search? Use hash table
- Need ordered data? Use tree
- LIFO operations? Use stack
- FIFO operations? Use queue

---

## Arrays

**Fixed-size, contiguous memory**

### Visual Representation

```
Index:  0    1    2    3    4
       ┌────┬────┬────┬────┬────┐
Array: │ 10 │ 20 │ 30 │ 40 │ 50 │
       └────┴────┴────┴────┴────┘
Memory: 1000 1004 1008 1012 1016
```

### Operations

```javascript
const arr = [10, 20, 30, 40, 50];

// Access - O(1)
arr[2]  // 30 (instant!)

// Search - O(n)
arr.indexOf(30)  // Must check each element

// Insert at end - O(1)
arr.push(60)

// Insert at beginning - O(n)
arr.unshift(5)  // Must shift all elements!

// Delete - O(n)
arr.splice(2, 1)  // Must shift elements
```

### Time Complexity

| Operation | Time |
|-----------|------|
| Access | O(1) |
| Search | O(n) |
| Insert (end) | O(1) |
| Insert (beginning) | O(n) |
| Delete | O(n) |

### When to Use

✅ Need fast access by index
✅ Know size in advance
✅ Mostly reading, not inserting/deleting
❌ Frequent insertions/deletions
❌ Unknown size

---

## Linked Lists

**Nodes connected by pointers**

### Visual Representation

```
Singly Linked List:

    ┌────┬────┐    ┌────┬────┐    ┌────┬────┐    ┌────┬────┐
head│ 10 │  ●─┼───►│ 20 │  ●─┼───►│ 30 │  ●─┼───►│ 40 │null│
    └────┴────┘    └────┴────┘    └────┴────┘    └────┴────┘
     value next     value next     value next     value next

Doubly Linked List:

         ┌────┬────┬────┐    ┌────┬────┬────┐    ┌────┬────┬────┐
    head │null│ 10 │  ●─┼───►│ ●  │ 20 │  ●─┼───►│ ●  │ 30 │null│
         └────┴────┴────┘    └────┴────┴────┘    └────┴────┴────┘
         prev value next ◄────┘    value next ◄────┘    value next
```

### Implementation

```javascript
class Node {
    constructor(value) {
        this.value = value;
        this.next = null;
    }
}

class LinkedList {
    constructor() {
        this.head = null;
        this.tail = null;
        this.length = 0;
    }

    // Add to end - O(1) with tail pointer
    append(value) {
        const newNode = new Node(value);

        if (!this.head) {
            this.head = newNode;
            this.tail = newNode;
        } else {
            this.tail.next = newNode;
            this.tail = newNode;
        }

        this.length++;
        return this;
    }

    // Add to beginning - O(1)
    prepend(value) {
        const newNode = new Node(value);
        newNode.next = this.head;
        this.head = newNode;

        if (!this.tail) {
            this.tail = newNode;
        }

        this.length++;
        return this;
    }

    // Find node - O(n)
    find(value) {
        let current = this.head;

        while (current) {
            if (current.value === value) {
                return current;
            }
            current = current.next;
        }

        return null;
    }

    // Delete node - O(n)
    delete(value) {
        if (!this.head) return;

        // If head needs to be deleted
        if (this.head.value === value) {
            this.head = this.head.next;
            this.length--;
            return;
        }

        let current = this.head;
        while (current.next) {
            if (current.next.value === value) {
                current.next = current.next.next;
                if (!current.next) {
                    this.tail = current;
                }
                this.length--;
                return;
            }
            current = current.next;
        }
    }

    // Print list
    print() {
        const values = [];
        let current = this.head;

        while (current) {
            values.push(current.value);
            current = current.next;
        }

        console.log(values.join(' → '));
    }
}

// Usage
const list = new LinkedList();
list.append(10);
list.append(20);
list.append(30);
list.prepend(5);
list.print();  // 5 → 10 → 20 → 30
```

### Time Complexity

| Operation | Time |
|-----------|------|
| Access | O(n) |
| Search | O(n) |
| Insert (head) | O(1) |
| Insert (tail) | O(1) with tail pointer |
| Delete | O(n) |

### When to Use

✅ Frequent insertions/deletions
✅ Don't know size in advance
✅ Don't need random access
❌ Need fast access by index
❌ Need to search frequently

---

## Stacks

**LIFO (Last In, First Out)**

### Visual Representation

```
        push(30) →     ← pop()

        ┌──────┐
        │  30  │  ← top
        ├──────┤
        │  20  │
        ├──────┤
        │  10  │
        └──────┘
```

### Implementation

```javascript
class Stack {
    constructor() {
        this.items = [];
    }

    // Add to top - O(1)
    push(element) {
        this.items.push(element);
    }

    // Remove from top - O(1)
    pop() {
        if (this.isEmpty()) {
            return null;
        }
        return this.items.pop();
    }

    // View top element - O(1)
    peek() {
        if (this.isEmpty()) {
            return null;
        }
        return this.items[this.items.length - 1];
    }

    // Check if empty
    isEmpty() {
        return this.items.length === 0;
    }

    // Get size
    size() {
        return this.items.length;
    }

    // Clear stack
    clear() {
        this.items = [];
    }

    // Print stack
    print() {
        console.log(this.items.join(' '));
    }
}

// Usage
const stack = new Stack();
stack.push(10);
stack.push(20);
stack.push(30);
stack.pop();     // 30
stack.peek();    // 20
```

### Real-World Uses

- **Undo/Redo** - Text editors
- **Browser history** - Back button
- **Function call stack** - Program execution
- **Balanced parentheses** - Code parsing

### Example: Balanced Parentheses

```javascript
function isBalanced(str) {
    const stack = new Stack();
    const pairs = {
        ')': '(',
        ']': '[',
        '}': '{'
    };

    for (let char of str) {
        if (char === '(' || char === '[' || char === '{') {
            stack.push(char);
        } else if (char in pairs) {
            if (stack.pop() !== pairs[char]) {
                return false;
            }
        }
    }

    return stack.isEmpty();
}

isBalanced('({[]})');  // true
isBalanced('({[})');   // false
```

---

## Queues

**FIFO (First In, First Out)**

### Visual Representation

```
enqueue(40) →                    → dequeue()
           ┌────┬────┬────┬────┐
           │ 10 │ 20 │ 30 │ 40 │
           └────┴────┴────┴────┘
           front          rear
```

### Implementation

```javascript
class Queue {
    constructor() {
        this.items = [];
    }

    // Add to rear - O(1)
    enqueue(element) {
        this.items.push(element);
    }

    // Remove from front - O(n) for array!
    // Better: use object with pointers
    dequeue() {
        if (this.isEmpty()) {
            return null;
        }
        return this.items.shift();
    }

    // View front element
    front() {
        if (this.isEmpty()) {
            return null;
        }
        return this.items[0];
    }

    isEmpty() {
        return this.items.length === 0;
    }

    size() {
        return this.items.length;
    }
}

// Better Queue using Object (O(1) dequeue)
class QueueOptimized {
    constructor() {
        this.items = {};
        this.frontIndex = 0;
        this.rearIndex = 0;
    }

    enqueue(element) {
        this.items[this.rearIndex] = element;
        this.rearIndex++;
    }

    dequeue() {
        if (this.isEmpty()) {
            return null;
        }

        const item = this.items[this.frontIndex];
        delete this.items[this.frontIndex];
        this.frontIndex++;
        return item;
    }

    front() {
        return this.items[this.frontIndex];
    }

    isEmpty() {
        return this.rearIndex === this.frontIndex;
    }

    size() {
        return this.rearIndex - this.frontIndex;
    }
}
```

### Real-World Uses

- **Print queue** - Documents waiting to print
- **Task scheduling** - CPU processes
- **Breadth-First Search** - Graph traversal
- **Request handling** - Web servers

---

## Hash Tables

**Key-value pairs with O(1) average lookup**

### Visual Representation

```
Hash Function: key → index

   Keys          Hash Table (Array)
  ┌────┐
  │"a" ├──────►  0: []
  └────┘
  ┌────┐
  │"b" ├──────►  1: [("b", 25)]
  └────┘
  ┌────┐
  │"c" ├──────►  2: [("c", 30)]
  └────┘
                 3: []
  ┌────┐
  │"d" ├──────►  4: [("d", 40)]
  └────┘
                 ...
```

### Implementation

```javascript
class HashTable {
    constructor(size = 53) {
        this.keyMap = new Array(size);
    }

    // Hash function
    _hash(key) {
        let total = 0;
        const PRIME = 31;

        for (let i = 0; i < Math.min(key.length, 100); i++) {
            const char = key[i];
            const value = char.charCodeAt(0) - 96;
            total = (total * PRIME + value) % this.keyMap.length;
        }

        return total;
    }

    // Set key-value pair - O(1) average
    set(key, value) {
        const index = this._hash(key);

        if (!this.keyMap[index]) {
            this.keyMap[index] = [];
        }

        // Check if key exists, update if so
        for (let pair of this.keyMap[index]) {
            if (pair[0] === key) {
                pair[1] = value;
                return;
            }
        }

        this.keyMap[index].push([key, value]);
    }

    // Get value by key - O(1) average
    get(key) {
        const index = this._hash(key);
        const bucket = this.keyMap[index];

        if (bucket) {
            for (let pair of bucket) {
                if (pair[0] === key) {
                    return pair[1];
                }
            }
        }

        return undefined;
    }

    // Get all keys
    keys() {
        const keysArray = [];

        for (let bucket of this.keyMap) {
            if (bucket) {
                for (let pair of bucket) {
                    keysArray.push(pair[0]);
                }
            }
        }

        return keysArray;
    }

    // Get all values
    values() {
        const valuesArray = [];

        for (let bucket of this.keyMap) {
            if (bucket) {
                for (let pair of bucket) {
                    if (!valuesArray.includes(pair[1])) {
                        valuesArray.push(pair[1]);
                    }
                }
            }
        }

        return valuesArray;
    }
}

// Usage
const ht = new HashTable();
ht.set('name', 'Alice');
ht.set('age', 25);
ht.set('city', 'NYC');

ht.get('name');  // 'Alice'
ht.keys();       // ['name', 'age', 'city']
```

### JavaScript Built-in

```javascript
// Map (ordered, any key type)
const map = new Map();
map.set('name', 'Alice');
map.set(1, 'one');
map.get('name');  // 'Alice'

// Object (string/symbol keys only)
const obj = {
    name: 'Alice',
    age: 25
};
obj.name;  // 'Alice'
obj['age'];  // 25
```

### Time Complexity

| Operation | Average | Worst |
|-----------|---------|-------|
| Insert | O(1) | O(n) |
| Delete | O(1) | O(n) |
| Search | O(1) | O(n) |

### When to Use

✅ Need fast lookups
✅ Key-value associations
✅ Checking existence
✅ Counting frequencies
❌ Need ordered data
❌ Need to iterate in order

---

## Trees

**Hierarchical data structure**

### Binary Tree

```
          10
         /  \
        5    15
       / \   / \
      3   7 12  20
```

### Binary Search Tree (BST)

**Left < Parent < Right**

```
          50
         /  \
       30    70
      / \   / \
    20  40 60  80

Inorder: 20, 30, 40, 50, 60, 70, 80 (sorted!)
```

### Implementation

```javascript
class TreeNode {
    constructor(value) {
        this.value = value;
        this.left = null;
        this.right = null;
    }
}

class BinarySearchTree {
    constructor() {
        this.root = null;
    }

    // Insert - O(log n) average, O(n) worst
    insert(value) {
        const newNode = new TreeNode(value);

        if (!this.root) {
            this.root = newNode;
            return this;
        }

        let current = this.root;

        while (true) {
            if (value < current.value) {
                if (!current.left) {
                    current.left = newNode;
                    return this;
                }
                current = current.left;
            } else {
                if (!current.right) {
                    current.right = newNode;
                    return this;
                }
                current = current.right;
            }
        }
    }

    // Search - O(log n) average
    find(value) {
        if (!this.root) return false;

        let current = this.root;

        while (current) {
            if (value === current.value) return true;
            if (value < current.value) {
                current = current.left;
            } else {
                current = current.right;
            }
        }

        return false;
    }

    // Inorder traversal (sorted order)
    inorder(node = this.root, result = []) {
        if (node) {
            this.inorder(node.left, result);
            result.push(node.value);
            this.inorder(node.right, result);
        }
        return result;
    }

    // Preorder traversal
    preorder(node = this.root, result = []) {
        if (node) {
            result.push(node.value);
            this.preorder(node.left, result);
            this.preorder(node.right, result);
        }
        return result;
    }

    // Postorder traversal
    postorder(node = this.root, result = []) {
        if (node) {
            this.postorder(node.left, result);
            this.postorder(node.right, result);
            result.push(node.value);
        }
        return result;
    }
}

// Usage
const bst = new BinarySearchTree();
bst.insert(50);
bst.insert(30);
bst.insert(70);
bst.insert(20);
bst.insert(40);

bst.find(30);     // true
bst.inorder();    // [20, 30, 40, 50, 70]
```

### Tree Traversals

```
        1
       / \
      2   3
     / \
    4   5

Inorder (Left, Root, Right):   4, 2, 5, 1, 3
Preorder (Root, Left, Right):  1, 2, 4, 5, 3
Postorder (Left, Right, Root): 4, 5, 2, 3, 1
```

---

## Comparison Chart

| Data Structure | Access | Search | Insert | Delete | Notes |
|----------------|--------|--------|--------|--------|-------|
| **Array** | O(1) | O(n) | O(n) | O(n) | Fast access, slow insert/delete |
| **Linked List** | O(n) | O(n) | O(1)* | O(n) | Fast insert at head |
| **Stack** | O(n) | O(n) | O(1) | O(1) | LIFO |
| **Queue** | O(n) | O(n) | O(1) | O(1) | FIFO |
| **Hash Table** | N/A | O(1)† | O(1)† | O(1)† | Fast lookup |
| **BST** | O(log n)† | O(log n)† | O(log n)† | O(log n)† | Ordered data |

*With tail pointer
†Average case, O(n) worst case

---

## Interview Questions

### 1. Array vs Linked List?
**Answer**:
- **Array**: O(1) access, O(n) insert/delete, contiguous memory
- **Linked List**: O(n) access, O(1) insert at head, scattered memory

### 2. When to use Stack vs Queue?
**Answer**:
- **Stack (LIFO)**: Undo/redo, function calls, DFS
- **Queue (FIFO)**: Task scheduling, BFS, print queue

### 3. How does a hash table work?
**Answer**: Hash function converts key to index. Value stored at that index. Handles collisions with chaining (array) or open addressing.

### 4. What is a Binary Search Tree?
**Answer**: Binary tree where left < parent < right. Enables O(log n) search if balanced.

### 5. How to detect cycle in linked list?
**Answer**: Floyd's cycle detection (tortoise and hare). Fast pointer moves 2x, slow moves 1x. If they meet, there's a cycle.

---

**Keywords**: data structures, arrays, linked lists, stacks, queues, hash tables, binary search trees, data structures interview, algorithms, time complexity

