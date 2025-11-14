# Python Fundamentals - Beginner to Intermediate Guide

> Essential Python concepts, data structures, OOP, and interview questions

## 📋 Table of Contents

- [Python Basics](#python-basics)
- [Data Types](#data-types)
- [Control Flow](#control-flow)
- [Functions](#functions)
- [Data Structures](#data-structures)
- [Object-Oriented Programming](#object-oriented-programming)
- [File Handling](#file-handling)
- [Common Patterns](#common-patterns)
- [Interview Questions](#interview-questions)

---

## Python Basics

Python is a high-level, interpreted language known for readability and versatility.

### Running Python

```bash
# Interactive shell
python3

# Run file
python3 script.py

# Check version
python3 --version
```

### Print & Input

```python
# Print to console
print("Hello, World!")
print("Value:", 42)
print(f"Name: {name}, Age: {age}")  # F-string (3.6+)

# Get user input
name = input("Enter your name: ")
age = int(input("Enter your age: "))
```

---

## Data Types

### Numbers

```python
# Integer
x = 10
y = -5

# Float
pi = 3.14159
temp = -40.0

# Operations
a + b  # Addition
a - b  # Subtraction
a * b  # Multiplication
a / b  # Division (returns float)
a // b # Floor division (returns int)
a % b  # Modulus (remainder)
a ** b # Exponentiation

# Type conversion
int("42")    # 42
float("3.14") # 3.14
str(123)     # "123"
```

### Strings

```python
# Creation
name = "Alice"
message = 'Hello'
multiline = """This is
a multiline
string"""

# F-strings (formatted strings)
name = "Bob"
age = 30
print(f"My name is {name} and I am {age} years old")

# String methods
text = "Hello World"
text.lower()          # "hello world"
text.upper()          # "HELLO WORLD"
text.strip()          # Remove whitespace
text.split()          # ["Hello", "World"]
text.replace("World", "Python")  # "Hello Python"
"Python" in text      # False
text.startswith("He") # True
text.endswith("ld")   # True

# Slicing
text = "Hello"
text[0]      # "H" (first character)
text[-1]     # "o" (last character)
text[0:3]    # "Hel" (index 0 to 2)
text[2:]     # "llo" (index 2 to end)
text[:3]     # "Hel" (start to index 2)
```

### Booleans

```python
is_active = True
is_complete = False

# Comparisons
x == y  # Equal
x != y  # Not equal
x > y   # Greater than
x < y   # Less than
x >= y  # Greater than or equal
x <= y  # Less than or equal

# Logical operators
and  # Both must be True
or   # At least one True
not  # Negation
```

---

## Control Flow

### If/Elif/Else

```python
age = 18

if age < 13:
    print("Child")
elif age < 20:
    print("Teenager")
else:
    print("Adult")

# One-liner (ternary)
status = "Adult" if age >= 18 else "Minor"
```

### For Loops

```python
# Loop through range
for i in range(5):  # 0, 1, 2, 3, 4
    print(i)

for i in range(1, 6):  # 1, 2, 3, 4, 5
    print(i)

for i in range(0, 10, 2):  # 0, 2, 4, 6, 8
    print(i)

# Loop through list
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)

# With index
for index, fruit in enumerate(fruits):
    print(f"{index}: {fruit}")
```

### While Loops

```python
count = 0
while count < 5:
    print(count)
    count += 1

# Break and continue
while True:
    user_input = input("Enter 'quit' to exit: ")
    if user_input == "quit":
        break  # Exit loop
    if user_input == "skip":
        continue  # Skip to next iteration
    print(f"You entered: {user_input}")
```

---

## Functions

### Basic Functions

```python
def greet(name):
    return f"Hello, {name}!"

result = greet("Alice")  # "Hello, Alice!"

# Multiple parameters
def add(a, b):
    return a + b

# Default parameters
def greet(name="Guest"):
    return f"Hello, {name}!"

greet()         # "Hello, Guest!"
greet("Alice")  # "Hello, Alice!"
```

### Args and Kwargs

```python
# *args - Variable number of positional arguments
def sum_all(*args):
    return sum(args)

sum_all(1, 2, 3, 4)  # 10

# **kwargs - Variable number of keyword arguments
def print_info(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_info(name="Alice", age=25, city="NYC")
```

### Lambda Functions

```python
# Anonymous one-liner functions
square = lambda x: x ** 2
add = lambda a, b: a + b

# Often used with map, filter, sorted
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x ** 2, numbers))
# [1, 4, 9, 16, 25]

evens = list(filter(lambda x: x % 2 == 0, numbers))
# [2, 4]
```

---

## Data Structures

### Lists

```python
# Creation
fruits = ["apple", "banana", "cherry"]
numbers = [1, 2, 3, 4, 5]
mixed = [1, "two", 3.0, True]

# Access
fruits[0]   # "apple"
fruits[-1]  # "cherry" (last item)

# Methods
fruits.append("orange")      # Add to end
fruits.insert(1, "kiwi")     # Insert at index
fruits.remove("banana")      # Remove by value
popped = fruits.pop()        # Remove and return last
popped = fruits.pop(0)       # Remove and return at index
fruits.extend(["grape", "mango"])  # Add multiple
fruits.sort()                # Sort in place
fruits.reverse()             # Reverse in place
count = fruits.count("apple")  # Count occurrences
index = fruits.index("cherry") # Find index

# Slicing
numbers[1:4]   # [2, 3, 4]
numbers[:3]    # [1, 2, 3]
numbers[2:]    # [3, 4, 5]
numbers[::2]   # [1, 3, 5] (every 2nd item)

# List comprehension
squares = [x ** 2 for x in range(10)]
evens = [x for x in range(10) if x % 2 == 0]
```

### Tuples

```python
# Immutable lists
coordinates = (10, 20)
person = ("Alice", 25, "NYC")

# Access
person[0]  # "Alice"

# Unpacking
name, age, city = person

# Tuples are immutable
# coordinates[0] = 15  # ERROR!
```

### Dictionaries

```python
# Key-value pairs
person = {
    "name": "Alice",
    "age": 25,
    "city": "NYC"
}

# Access
person["name"]         # "Alice"
person.get("age")      # 25
person.get("job", "Unknown")  # "Unknown" (default)

# Modify
person["age"] = 26           # Update
person["job"] = "Engineer"   # Add

# Methods
person.keys()          # dict_keys(['name', 'age', 'city'])
person.values()        # dict_values(['Alice', 25, 'NYC'])
person.items()         # Key-value pairs

# Loop through dict
for key, value in person.items():
    print(f"{key}: {value}")

# Dict comprehension
squares = {x: x**2 for x in range(5)}
# {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```

### Sets

```python
# Unique, unordered collection
numbers = {1, 2, 3, 4, 5}
unique = {1, 2, 2, 3, 3, 3}  # {1, 2, 3}

# Methods
numbers.add(6)
numbers.remove(3)
numbers.discard(10)  # No error if not found

# Set operations
a = {1, 2, 3}
b = {3, 4, 5}

a.union(b)         # {1, 2, 3, 4, 5}
a.intersection(b)  # {3}
a.difference(b)    # {1, 2}
```

---

## Object-Oriented Programming

### Classes & Objects

```python
class Person:
    # Constructor
    def __init__(self, name, age):
        self.name = name
        self.age = age

    # Method
    def greet(self):
        return f"Hi, I'm {self.name}"

    # Method with parameters
    def have_birthday(self):
        self.age += 1

# Create instance
person = Person("Alice", 25)
print(person.name)       # "Alice"
print(person.greet())    # "Hi, I'm Alice"
person.have_birthday()
print(person.age)        # 26
```

### Inheritance

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        return "Some sound"

class Dog(Animal):
    def speak(self):
        return "Woof!"

class Cat(Animal):
    def speak(self):
        return "Meow!"

dog = Dog("Buddy")
print(dog.speak())  # "Woof!"
print(dog.name)     # "Buddy"
```

### Class vs Instance Variables

```python
class Car:
    # Class variable (shared by all instances)
    wheels = 4

    def __init__(self, brand, model):
        # Instance variables (unique to each instance)
        self.brand = brand
        self.model = model

car1 = Car("Toyota", "Camry")
car2 = Car("Honda", "Civic")

print(car1.wheels)  # 4
print(Car.wheels)   # 4
```

---

## File Handling

### Reading Files

```python
# Method 1: Manual close
file = open("data.txt", "r")
content = file.read()
file.close()

# Method 2: Context manager (recommended)
with open("data.txt", "r") as file:
    content = file.read()
# File automatically closed

# Read line by line
with open("data.txt", "r") as file:
    for line in file:
        print(line.strip())

# Read all lines into list
with open("data.txt", "r") as file:
    lines = file.readlines()
```

### Writing Files

```python
# Write (overwrites)
with open("output.txt", "w") as file:
    file.write("Hello World\n")
    file.write("Second line\n")

# Append
with open("output.txt", "a") as file:
    file.write("Added line\n")

# Write list of lines
lines = ["Line 1\n", "Line 2\n", "Line 3\n"]
with open("output.txt", "w") as file:
    file.writelines(lines)
```

---

## Common Patterns

### List Comprehensions

```python
# Basic
squares = [x**2 for x in range(10)]

# With condition
evens = [x for x in range(10) if x % 2 == 0]

# Nested
matrix = [[i*j for j in range(3)] for i in range(3)]
```

### Exception Handling

```python
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero!")
except Exception as e:
    print(f"Error: {e}")
else:
    print("Success!")  # Runs if no exception
finally:
    print("Always runs")  # Cleanup code
```

### Working with JSON

```python
import json

# Python dict to JSON string
data = {"name": "Alice", "age": 25}
json_string = json.dumps(data)

# JSON string to Python dict
json_string = '{"name": "Bob", "age": 30}'
data = json.loads(json_string)

# Write to file
with open("data.json", "w") as file:
    json.dump(data, file, indent=2)

# Read from file
with open("data.json", "r") as file:
    data = json.load(file)
```

---

## Interview Questions

### 1. What is Python?
**Answer**: High-level, interpreted, dynamically-typed language known for readability and versatility. Used for web dev, data science, automation, AI.

### 2. List vs Tuple?
**Answer**:
- List: Mutable, `[]`, slower
- Tuple: Immutable, `()`, faster, hashable (can be dict key)

### 3. What is a dictionary?
**Answer**: Key-value data structure. Keys must be immutable (strings, numbers, tuples). O(1) lookup time.

### 4. Explain list comprehension
**Answer**: Concise way to create lists:
```python
[expression for item in iterable if condition]
```

### 5. What is `self`?
**Answer**: Reference to instance in class methods. Convention (can be named anything), but always first parameter.

### 6. `*args` vs `**kwargs`?
**Answer**:
- `*args`: Variable positional arguments (tuple)
- `**kwargs`: Variable keyword arguments (dict)

### 7. How does Python handle memory?
**Answer**: Automatic memory management with garbage collection. Reference counting + cycle detection.

### 8. Mutable vs Immutable?
**Answer**:
- Mutable: Can change (list, dict, set)
- Immutable: Cannot change (int, str, tuple)

### 9. What are decorators?
**Answer**: Functions that modify other functions. Syntax: `@decorator`

### 10. Explain GIL (Global Interpreter Lock)
**Answer**: Mutex that allows only one thread to execute Python bytecode at a time. Affects CPU-bound multi-threading (use multiprocessing instead).

---

**Keywords**: Python tutorial, Python basics, Python OOP, Python data structures, Python interview questions, Python for beginners, list comprehension, Python functions, file handling
