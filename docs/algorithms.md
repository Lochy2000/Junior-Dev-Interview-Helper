# Algorithms & Data Structures - Interview Prep

> Essential algorithms and data structures for coding interviews

## 📋 Table of Contents

- [Big O Notation](#big-o-notation)
- [Arrays](#arrays)
- [Linked Lists](#linked-lists)
- [Stacks & Queues](#stacks--queues)
- [Trees](#trees)
- [Hash Tables](#hash-tables)
- [Sorting Algorithms](#sorting-algorithms)
- [Searching Algorithms](#searching-algorithms)
- [Common Patterns](#common-patterns)

---

## Big O Notation

Time complexity - how runtime grows with input size.

### Common Time Complexities

```
O(1)       - Constant     - Array access, hash table lookup
O(log n)   - Logarithmic  - Binary search
O(n)       - Linear       - Loop through array
O(n log n) - Linearithmic - Merge sort, quick sort
O(n²)      - Quadratic    - Nested loops
O(2^n)     - Exponential  - Recursive Fibonacci
O(n!)      - Factorial    - Permutations
```

### Examples

```javascript
// O(1) - Constant
function getFirst(arr) {
    return arr[0];
}

// O(n) - Linear
function findMax(arr) {
    let max = arr[0];
    for (let i = 1; i < arr.length; i++) {
        if (arr[i] > max) max = arr[i];
    }
    return max;
}

// O(n²) - Quadratic
function hasDuplicates(arr) {
    for (let i = 0; i < arr.length; i++) {
        for (let j = i + 1; j < arr.length; j++) {
            if (arr[i] === arr[j]) return true;
        }
    }
    return false;
}
```

---

## Arrays

```javascript
// Two Pointers
function isPalindrome(s) {
    let left = 0;
    let right = s.length - 1;

    while (left < right) {
        if (s[left] !== s[right]) return false;
        left++;
        right--;
    }
    return true;
}

// Sliding Window
function maxSubarraySum(arr, k) {
    let maxSum = 0;
    let windowSum = 0;

    // First window
    for (let i = 0; i < k; i++) {
        windowSum += arr[i];
    }
    maxSum = windowSum;

    // Slide window
    for (let i = k; i < arr.length; i++) {
        windowSum = windowSum - arr[i - k] + arr[i];
        maxSum = Math.max(maxSum, windowSum);
    }

    return maxSum;
}
```

---

## Linked Lists

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
        this.size = 0;
    }

    // Add to end
    append(value) {
        const node = new Node(value);

        if (!this.head) {
            this.head = node;
        } else {
            let current = this.head;
            while (current.next) {
                current = current.next;
            }
            current.next = node;
        }
        this.size++;
    }

    // Reverse
    reverse() {
        let prev = null;
        let current = this.head;

        while (current) {
            let next = current.next;
            current.next = prev;
            prev = current;
            current = next;
        }

        this.head = prev;
    }
}
```

---

## Stacks & Queues

```javascript
// Stack (LIFO)
class Stack {
    constructor() {
        this.items = [];
    }

    push(element) {
        this.items.push(element);
    }

    pop() {
        return this.items.pop();
    }

    peek() {
        return this.items[this.items.length - 1];
    }

    isEmpty() {
        return this.items.length === 0;
    }
}

// Queue (FIFO)
class Queue {
    constructor() {
        this.items = [];
    }

    enqueue(element) {
        this.items.push(element);
    }

    dequeue() {
        return this.items.shift();
    }

    front() {
        return this.items[0];
    }

    isEmpty() {
        return this.items.length === 0;
    }
}
```

---

## Trees

```javascript
class TreeNode {
    constructor(value) {
        this.value = value;
        this.left = null;
        this.right = null;
    }
}

// Binary Search Tree
class BST {
    constructor() {
        this.root = null;
    }

    insert(value) {
        const node = new TreeNode(value);

        if (!this.root) {
            this.root = node;
            return;
        }

        let current = this.root;
        while (true) {
            if (value < current.value) {
                if (!current.left) {
                    current.left = node;
                    return;
                }
                current = current.left;
            } else {
                if (!current.right) {
                    current.right = node;
                    return;
                }
                current = current.right;
            }
        }
    }

    // In-order traversal (sorted order)
    inOrder(node = this.root, result = []) {
        if (node) {
            this.inOrder(node.left, result);
            result.push(node.value);
            this.inOrder(node.right, result);
        }
        return result;
    }
}
```

---

## Sorting Algorithms

```javascript
// Bubble Sort - O(n²)
function bubbleSort(arr) {
    for (let i = 0; i < arr.length; i++) {
        for (let j = 0; j < arr.length - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
            }
        }
    }
    return arr;
}

// Quick Sort - O(n log n) average
function quickSort(arr) {
    if (arr.length <= 1) return arr;

    const pivot = arr[Math.floor(arr.length / 2)];
    const left = arr.filter(x => x < pivot);
    const middle = arr.filter(x => x === pivot);
    const right = arr.filter(x => x > pivot);

    return [...quickSort(left), ...middle, ...quickSort(right)];
}

// Merge Sort - O(n log n)
function mergeSort(arr) {
    if (arr.length <= 1) return arr;

    const mid = Math.floor(arr.length / 2);
    const left = mergeSort(arr.slice(0, mid));
    const right = mergeSort(arr.slice(mid));

    return merge(left, right);
}

function merge(left, right) {
    const result = [];
    let i = 0, j = 0;

    while (i < left.length && j < right.length) {
        if (left[i] < right[j]) {
            result.push(left[i++]);
        } else {
            result.push(right[j++]);
        }
    }

    return [...result, ...left.slice(i), ...right.slice(j)];
}
```

---

## Common Patterns

### Two Sum

```javascript
function twoSum(nums, target) {
    const map = new Map();

    for (let i = 0; i < nums.length; i++) {
        const complement = target - nums[i];
        if (map.has(complement)) {
            return [map.get(complement), i];
        }
        map.set(nums[i], i);
    }

    return [];
}
```

### Reverse String

```javascript
function reverseString(s) {
    let left = 0;
    let right = s.length - 1;
    const arr = s.split('');

    while (left < right) {
        [arr[left], arr[right]] = [arr[right], arr[left]];
        left++;
        right--;
    }

    return arr.join('');
}
```

---

**Keywords**: algorithms, data structures, coding interview, Big O notation, binary search, sorting algorithms, linked lists, trees, interview prep

---

*Note: This is a starter guide. More comprehensive content with visual diagrams coming soon!*
