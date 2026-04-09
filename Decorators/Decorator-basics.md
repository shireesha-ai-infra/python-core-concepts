# 🎭 Everything about Python Decorators

> **A complete, step-by-step journey into Python decorators**  
> No prior knowledge required. By the end, you'll write decorators like a pro.
> 
> 🎨 **[View the Interactive Animation](https://shireesha-ai-infra.github.io/visualizations/decorator_animation.html)**

---

## 📚 Table of Contents

1. [Prerequisites: What You Need to Know](#prerequisites)
2. [Chapter 1: Functions Are Objects](#chapter-1-functions-are-objects)
3. [Chapter 2: Functions in Variables](#chapter-2-functions-in-variables)
4. [Chapter 3: Functions as Arguments](#chapter-3-functions-as-arguments)
5. [Chapter 4: Functions Returning Functions](#chapter-4-functions-returning-functions)
6. [Chapter 5: Closures - The Memory Magic](#chapter-5-closures)
7. [Chapter 6: Your First Decorator](#chapter-6-your-first-decorator)
8. [Chapter 7: The @ Syntax](#chapter-7-the--syntax)
9. [Chapter 8: Decorating Functions with Arguments](#chapter-8-decorating-functions-with-arguments)
10. [Chapter 9: Understanding *args and **kwargs](#chapter-9-args-and-kwargs)
11. [Chapter 10: Universal Decorators](#chapter-10-universal-decorators)
12. [Chapter 11: Decorators with Arguments (Factory Pattern)](#chapter-11-decorators-with-arguments)
13. [Chapter 12: Stacking Decorators](#chapter-12-stacking-decorators)
14. [Chapter 13: functools.wraps](#chapter-13-functoolswraps)
15. [Chapter 14: Class-Based Decorators](#chapter-14-class-based-decorators)
16. [Chapter 15: Real-World Examples](#chapter-15-real-world-examples)
17. [Chapter 16: Common Pitfalls](#chapter-16-common-pitfalls)
18. [Chapter 17: Best Practices](#chapter-17-best-practices)
19. [Chapter 18: Advanced Patterns](#chapter-18-advanced-patterns)

---

## Prerequisites

Before diving into decorators, you should be comfortable with:

- ✅ Variables and assignment
- ✅ Functions (defining and calling)
- ✅ Basic Python syntax
- ✅ The `print()` function

**That's it!** Everything else, we'll build from scratch.

---

## Chapter 1: Functions Are Objects

### 🎯 Goal
Understand that functions in Python are **objects**, just like integers, strings, and lists.

### The Concept

In Python, when you write a function, you're creating an **object**. This object has a type, just like `5` is an integer and `"hello"` is a string.

```python
def greet():
    return "Hello!"

# Let's investigate what greet is
print(type(greet))  # <class 'function'>
print(type(5))      # <class 'int'>
print(type("hi"))   # <class 'str'>
```

**Output:**
```
<class 'function'>
<class 'int'>
<class 'str'>
```

### 💡 Key Insight

`greet` is a **function object**. It exists in memory, has a type, and has a unique identity.

```python
def greet():
    return "Hello!"

print(id(greet))  # Shows memory address (e.g., 140234567890)
```

### Mental Model

```
Memory Visualization:

┌─────────────────────┐
│  Function Object    │
│  Name: greet        │
│  Code: return "Hi!" │
│  Type: function     │
│  ID: 140234567890   │
└─────────────────────┘
         ↑
         │
    [greet]  ← This is just a name pointing to the object
```

### ✅ Checkpoint

**Question:** What is `greet` in `def greet(): ...`?

**Answer:** It's a variable name that references a function object in memory.

---

## Chapter 2: Functions in Variables

### 🎯 Goal
Understand that you can assign functions to variables, just like any other object.

### The Concept

Since functions are objects, you can assign them to variables:

```python
def greet():
    return "Hello!"

# Assign the function to a new variable
say_hello = greet  # Note: No parentheses!

# Now both names point to the SAME function
print(say_hello())  # "Hello!"
print(greet())      # "Hello!"
```

### ⚠️ Critical Distinction

```python
say_hello = greet     # Assigns the function object
say_hello = greet()   # Calls the function, assigns the RETURN VALUE
```

**Example:**
```python
def greet():
    return "Hello!"

# Assignment (no parentheses)
func = greet
print(func)      # <function greet at 0x...>
print(func())    # "Hello!"

# Call (with parentheses)
result = greet()
print(result)    # "Hello!"
print(result())  # ERROR! "Hello!" is not callable
```

### Mental Model

```
Memory:

┌──────────────────┐
│ Function Object  │
│ Code: return "!" │
└──────────────────┘
    ↑           ↑
    │           │
 [greet]   [say_hello]
    
Two names, one object!
```

### 🧪 Experiment

```python
def add(a, b):
    return a + b

# Create multiple names for the same function
plus = add
sum_numbers = add

print(plus(2, 3))         # 5
print(sum_numbers(2, 3))  # 5
print(add(2, 3))          # 5

# They're all the same function!
print(plus is add)        # True
```

### ✅ Checkpoint

**Question:** What's the difference between `f = greet` and `f = greet()`?

**Answer:** 
- `f = greet` → f points to the function object
- `f = greet()` → f gets the return value from calling greet

---

## Chapter 3: Functions as Arguments

### 🎯 Goal
Pass functions to other functions as arguments.

### The Concept

Since functions are objects, you can pass them as arguments:

```python
def greet():
    return "Hello!"

def shout(func):
    # func is a function object
    result = func()  # Call it
    return result.upper()

message = shout(greet)  # Pass greet to shout
print(message)  # "HELLO!"
```

### Step-by-Step Breakdown

```python
def greet():
    return "Hello!"

def call_twice(func):
    func()  # First call
    func()  # Second call

call_twice(greet)
```

**Output:**
```
Hello!
Hello!
```

**What happened:**
1. `call_twice(greet)` is called
2. Inside `call_twice`, parameter `func` receives the `greet` function object
3. `func()` is equivalent to `greet()` 
4. It's called twice

### Real Example: Higher-Order Function

```python
def add(a, b):
    return a + b

def subtract(a, b):
    return a - b

def calculate(operation, x, y):
    # operation is a function!
    return operation(x, y)

print(calculate(add, 10, 5))       # 15
print(calculate(subtract, 10, 5))  # 5
```

### Mental Model

```
calculate(add, 10, 5)
    ↓
Inside calculate:
    operation = add       ← function object
    x = 10
    y = 5
    return operation(x, y)  → return add(10, 5) → 15
```

### ✅ Checkpoint

**Question:** What does this code print?

```python
def double(x):
    return x * 2

def apply(func, value):
    return func(value)

print(apply(double, 5))
```

**Answer:** `10` (apply calls double(5) which returns 5 * 2)

---

## Chapter 4: Functions Returning Functions

### 🎯 Goal
Understand that functions can create and return other functions.

### The Concept

A function can define another function inside itself and return it:

```python
def create_greeter():
    def greet():
        return "Hello!"
    return greet  # Return the function object

# Get the function
my_function = create_greeter()

# Now call it
print(my_function())  # "Hello!"
```

### Step-by-Step

```python
def outer():
    print("Outer function running")
    
    def inner():
        print("Inner function running")
    
    print("Outer about to return inner")
    return inner

# Call outer, get inner
func = outer()
```

**Output:**
```
Outer function running
Outer about to return inner
```

**Now call the returned function:**
```python
func()  # "Inner function running"
```

### Mental Model

```
Step 1: outer() is called
┌─────────────────┐
│ outer executing │
│ - Creates inner │
│ - Returns inner │
└─────────────────┘
         ↓
      returns
         ↓
Step 2: We get inner
┌──────────────┐
│ inner object │
└──────────────┘
         ↑
         │
      [func]

Step 3: func() calls inner
┌──────────────────┐
│ inner executing  │
│ prints message   │
└──────────────────┘
```

### Real Example: Function Factory

```python
def make_multiplier(n):
    def multiplier(x):
        return x * n
    return multiplier

times_three = make_multiplier(3)
times_five = make_multiplier(5)

print(times_three(10))  # 30
print(times_five(10))   # 50
```

### ✅ Checkpoint

**Question:** What does this code print?

```python
def create_printer():
    def printer():
        print("Hello from printer")
    return printer

p = create_printer()
p()
```

**Answer:** 
```
Hello from printer
```

---

## Chapter 5: Closures

### 🎯 Goal
Understand how inner functions "remember" variables from outer functions.

### The Concept

When an inner function uses a variable from an outer function, it **captures** that variable. This is called a **closure**.

```python
def outer(x):
    # x exists in outer's scope
    
    def inner(y):
        # inner can access x from outer's scope
        return x + y
    
    return inner

add_five = outer(5)  # x = 5 is captured
print(add_five(10))  # 15 (uses captured x=5 plus y=10)
print(add_five(20))  # 25 (still remembers x=5!)
```

### 🔥 The Magic Moment

```python
def outer(x):
    def inner(y):
        return x + y
    return inner

add_five = outer(5)
# At this point, outer(5) has FINISHED executing
# But inner still remembers x = 5!
```

**After `outer(5)` finishes:**
- The execution frame is destroyed
- BUT: `inner` captured `x = 5` in its closure
- `x = 5` lives on inside `inner`

### Mental Model

```
Step 1: outer(5) creates inner
┌─────────────────┐
│ outer(5) frame  │
│ x = 5           │
│                 │
│ Creates: inner  │
└─────────────────┘

Step 2: outer returns, frame destroyed
┌─────────────────┐
│ inner function  │
│ 🔒 Closure:     │
│    x = 5        │ ← Captured!
└─────────────────┘
         ↑
         │
    [add_five]

Step 3: add_five(10) uses captured x
Result: 5 + 10 = 15
```

### Detailed Example

```python
def make_counter():
    count = 0
    
    def increment():
        nonlocal count  # Allows modifying count
        count += 1
        return count
    
    return increment

counter1 = make_counter()
counter2 = make_counter()

print(counter1())  # 1
print(counter1())  # 2
print(counter1())  # 3

print(counter2())  # 1 (separate closure!)
print(counter2())  # 2
```

**Key Insight:** Each call to `make_counter()` creates a NEW closure with its own `count`.

### Inspecting Closures

```python
def outer(x):
    def inner(y):
        return x + y
    return inner

add_five = outer(5)

# Python stores closure variables here:
print(add_five.__closure__)  # (<cell at 0x...: int object at 0x...>,)
print(add_five.__closure__[0].cell_contents)  # 5
```

### ✅ Checkpoint

**Question:** What does this print?

```python
def create_adder(x):
    def adder(y):
        return x + y
    return adder

add_10 = create_adder(10)
add_20 = create_adder(20)

print(add_10(5))
print(add_20(5))
```

**Answer:**
```
15  (10 + 5)
25  (20 + 5)
```

---

## Chapter 6: Your First Decorator

### 🎯 Goal
Build a decorator from scratch using everything we've learned.

### The Setup

We want to add "Before" and "After" messages around a function without modifying the function itself.

**Original function:**
```python
def greet():
    print("Hello!")

greet()
# Output: Hello!
```

**Desired output:**
```
Before the function
Hello!
After the function
```

### Manual Approach (Not Good)

```python
def greet():
    print("Before the function")
    print("Hello!")
    print("After the function")
```

**Problem:** We modified the original function. What if we have 100 functions to wrap?

### Decorator Approach (Good!)

```python
def my_decorator(func):
    """
    Takes a function, returns a wrapped version
    """
    def wrapper():
        print("Before the function")
        func()  # Call the original
        print("After the function")
    return wrapper

# Original function
def greet():
    print("Hello!")

# Wrap it
greet = my_decorator(greet)

# Call the wrapped version
greet()
```

**Output:**
```
Before the function
Hello!
After the function
```

### Step-by-Step Breakdown

**Step 1:** Define the decorator
```python
def my_decorator(func):
    # func is the function to wrap
```

**Step 2:** Create the wrapper
```python
    def wrapper():
        print("Before the function")
        func()  # Call original
        print("After the function")
```

**Step 3:** Return the wrapper
```python
    return wrapper
```

**Step 4:** Use it
```python
greet = my_decorator(greet)
```

### Memory Model

```
Before decoration:
┌──────────────┐
│ greet (orig) │
│ prints "Hi!" │
└──────────────┘
      ↑
      │
   [greet]

After greet = my_decorator(greet):
┌──────────────┐      ┌──────────────┐
│ greet (orig) │ ←──┐ │   wrapper    │
│ prints "Hi!" │    │ │ - print "B"  │
└──────────────┘    │ │ - call func  │
                    │ │ - print "A"  │
                    │ └──────────────┘
                    │        ↑
                    │        │
                    └──── [greet] (now points to wrapper)

When greet() is called:
→ wrapper executes
→ wrapper calls original via closure
```

### Full Example with Return Values

```python
def my_decorator(func):
    def wrapper():
        print("Before")
        result = func()  # Capture return value
        print("After")
        return result    # Pass it through
    return wrapper

def get_greeting():
    return "Hello!"

get_greeting = my_decorator(get_greeting)
message = get_greeting()
print(message)
```

**Output:**
```
Before
After
Hello!
```

### ✅ Checkpoint

Create a decorator that prints "STARTING" before a function and "FINISHED" after:

```python
def logger(func):
    # Your code here
    pass

def calculate():
    print("Calculating...")
    return 42

calculate = logger(calculate)
result = calculate()
```

**Solution:**
```python
def logger(func):
    def wrapper():
        print("STARTING")
        result = func()
        print("FINISHED")
        return result
    return wrapper
```

---

## Chapter 7: The @ Syntax

### 🎯 Goal
Learn the `@decorator` syntax - Python's syntactic sugar.

### The Problem

Writing `func = decorator(func)` is repetitive and error-prone:

```python
def greet():
    print("Hi")

greet = my_decorator(greet)  # Easy to forget or mess up
```

### The Solution: @ Syntax

Python provides the `@` symbol as a shortcut:

```python
@my_decorator
def greet():
    print("Hi")

# This is EXACTLY the same as:
# def greet():
#     print("Hi")
# greet = my_decorator(greet)
```

### Side-by-Side Comparison

**Without @:**
```python
def my_decorator(func):
    def wrapper():
        print("Decorated!")
        return func()
    return wrapper

def greet():
    return "Hello"

greet = my_decorator(greet)
```

**With @:**
```python
def my_decorator(func):
    def wrapper():
        print("Decorated!")
        return func()
    return wrapper

@my_decorator
def greet():
    return "Hello"
```

**They are IDENTICAL!**

### What @ Really Does

```python
@decorator
def func():
    pass
```

**Python translates this to:**
```python
def func():
    pass
func = decorator(func)
```

### Multiple Examples

```python
def shout(func):
    def wrapper():
        return func().upper()
    return wrapper

def whisper(func):
    def wrapper():
        return func().lower()
    return wrapper

@shout
def greet():
    return "Hello"

@whisper
def farewell():
    return "GOODBYE"

print(greet())     # "HELLO"
print(farewell())  # "goodbye"
```

### ✅ Checkpoint

**Question:** What's the difference between these two?

```python
# Version A
@decorator
def func():
    pass

# Version B
def func():
    pass
func = decorator(func)
```

**Answer:** No difference! They're exactly the same. `@` is just prettier syntax.

---

## Chapter 8: Decorating Functions with Arguments

### 🎯 Goal
Handle functions that take parameters.

### The Problem

Our current decorator only works with functions that take no arguments:

```python
def my_decorator(func):
    def wrapper():  # No parameters!
        print("Before")
        result = func()
        print("After")
        return result
    return wrapper

@my_decorator
def greet(name):  # This takes an argument!
    return f"Hello, {name}!"

greet("Alice")  # ERROR! wrapper() takes 0 arguments
```

**Error:**
```
TypeError: wrapper() takes 0 positional arguments but 1 was given
```

### The Solution: Pass Arguments Through

```python
def my_decorator(func):
    def wrapper(name):  # Accept the same arguments
        print("Before")
        result = func(name)  # Pass them through
        print("After")
        return result
    return wrapper

@my_decorator
def greet(name):
    return f"Hello, {name}!"

print(greet("Alice"))
```

**Output:**
```
Before
After
Hello, Alice!
```

### Mental Model

```
greet("Alice")
    ↓
wrapper("Alice")  ← wrapper receives the argument
    ↓
print("Before")
    ↓
func("Alice")  ← passes it to original greet
    ↓
return "Hello, Alice!"
    ↓
print("After")
    ↓
return "Hello, Alice!"
```

### Multiple Arguments

```python
def my_decorator(func):
    def wrapper(a, b):  # Match the signature
        print("Calculating...")
        result = func(a, b)
        print("Done!")
        return result
    return wrapper

@my_decorator
def add(x, y):
    return x + y

print(add(5, 3))
```

**Output:**
```
Calculating...
Done!
8
```

### The Problem with This Approach

What if we want to decorate functions with different signatures?

```python
def greet(name):           # 1 argument
    pass

def add(a, b):             # 2 arguments
    pass

def introduce(name, age, city):  # 3 arguments
    pass
```

We'd need different decorators for each! 😞

**Solution in next chapter:** `*args` and `**kwargs`

### ✅ Checkpoint

Write a decorator that prints the arguments before calling the function:

```python
@print_args
def multiply(x, y):
    return x * y

multiply(3, 4)
# Should print: "Arguments: 3, 4"
# Then return: 12
```

**Solution:**
```python
def print_args(func):
    def wrapper(x, y):
        print(f"Arguments: {x}, {y}")
        return func(x, y)
    return wrapper
```

---

## Chapter 9: *args and **kwargs

### 🎯 Goal
Understand flexible argument handling to create universal decorators.

### The Problem Revisited

We need a decorator that works with ANY function:

```python
def one_arg(x):
    pass

def two_args(x, y):
    pass

def three_args(x, y, z):
    pass

# We want ONE decorator for all of them!
```

### Understanding *args

`*args` collects all positional arguments into a **tuple**:

```python
def print_all(*args):
    print("Type:", type(args))
    print("Values:", args)
    for arg in args:
        print(f"  - {arg}")

print_all(1, 2, 3)
```

**Output:**
```
Type: <class 'tuple'>
Values: (1, 2, 3)
  - 1
  - 2
  - 3
```

**Key Points:**
- `*args` creates a tuple of all positional arguments
- You can call it anything (`*params`, `*values`), but `*args` is convention
- Inside the function, use it as a regular tuple

### Understanding **kwargs

`**kwargs` collects all keyword arguments into a **dictionary**:

```python
def print_all(**kwargs):
    print("Type:", type(kwargs))
    print("Values:", kwargs)
    for key, value in kwargs.items():
        print(f"  {key} = {value}")

print_all(name="Alice", age=30, city="NYC")
```

**Output:**
```
Type: <class 'dict'>
Values: {'name': 'Alice', 'age': 30, 'city': 'NYC'}
  name = Alice
  age = 30
  city = NYC
```

### Using Both Together

```python
def print_everything(*args, **kwargs):
    print("Positional arguments:", args)
    print("Keyword arguments:", kwargs)

print_everything(1, 2, 3, name="Alice", age=30)
```

**Output:**
```
Positional arguments: (1, 2, 3)
Keyword arguments: {'name': 'Alice', 'age': 30}
```

### Unpacking with * and **

You can also UNPACK with `*` and `**`:

```python
def add(a, b, c):
    return a + b + c

numbers = [1, 2, 3]
result = add(*numbers)  # Unpacks to add(1, 2, 3)
print(result)  # 6

def greet(name, greeting):
    print(f"{greeting}, {name}!")

params = {"name": "Alice", "greeting": "Hello"}
greet(**params)  # Unpacks to greet(name="Alice", greeting="Hello")
```

**Output:**
```
6
Hello, Alice!
```

### Complete Example

```python
def demo(*args, **kwargs):
    print("Got positional:", args)
    print("Got keyword:", kwargs)
    
    # Call another function with same arguments
    other_func(*args, **kwargs)

def other_func(a, b, name=""):
    print(f"a={a}, b={b}, name={name}")

demo(1, 2, name="Test")
```

**Output:**
```
Got positional: (1, 2)
Got keyword: {'name': 'Test'}
a=1, b=2, name=Test
```

### ✅ Checkpoint

**Question:** What does this print?

```python
def mystery(*args, **kwargs):
    print(len(args))
    print(len(kwargs))

mystery(1, 2, 3, x=4, y=5)
```

**Answer:**
```
3  (3 positional arguments)
2  (2 keyword arguments)
```

---

## Chapter 10: Universal Decorators

### 🎯 Goal
Create a decorator that works with ANY function, regardless of its signature.

### The Universal Pattern

```python
def my_decorator(func):
    def wrapper(*args, **kwargs):
        # Before code
        result = func(*args, **kwargs)
        # After code
        return result
    return wrapper
```

**This works with ANY function!**

### Example: Logging Decorator

```python
def logger(func):
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__}")
        result = func(*args, **kwargs)
        print(f"{func.__name__} returned {result}")
        return result
    return wrapper

@logger
def add(a, b):
    return a + b

@logger
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

print(add(5, 3))
print(greet("Alice"))
print(greet("Bob", greeting="Hi"))
```

**Output:**
```
Calling add
add returned 8
8
Calling greet
greet returned Hello, Alice!
Hello, Alice!
Calling greet
greet returned Hi, Bob!
Hi, Bob!
```

### Example: Timer Decorator

```python
import time

def timer(func):
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"{func.__name__} took {end - start:.4f} seconds")
        return result
    return wrapper

@timer
def slow_function():
    time.sleep(1)
    return "Done!"

@timer
def calculate(n):
    total = sum(range(n))
    return total

print(slow_function())
print(calculate(1000000))
```

**Output:**
```
slow_function took 1.0012 seconds
Done!
calculate took 0.0234 seconds
499999500000
```

### Example: Validation Decorator

```python
def validate_positive(func):
    def wrapper(*args, **kwargs):
        for arg in args:
            if isinstance(arg, (int, float)) and arg < 0:
                raise ValueError(f"Negative value not allowed: {arg}")
        result = func(*args, **kwargs)
        return result
    return wrapper

@validate_positive
def calculate_square_root(n):
    return n ** 0.5

print(calculate_square_root(16))  # 4.0
print(calculate_square_root(-4))  # ValueError!
```

### Mental Model

```
Function with ANY signature:
func(arg1, arg2, key1=val1, key2=val2)
    ↓
Decorator collects all:
wrapper(*args, **kwargs)
    args = (arg1, arg2)
    kwargs = {'key1': val1, 'key2': val2}
    ↓
Passes everything through:
func(*args, **kwargs)
    ↓
Unpacks to original call:
func(arg1, arg2, key1=val1, key2=val2)
```

### ✅ Checkpoint

Create a decorator that doubles the return value of any function:

```python
@double_result
def add(a, b):
    return a + b

@double_result
def get_five():
    return 5

print(add(3, 4))   # Should print 14 (7 * 2)
print(get_five())  # Should print 10 (5 * 2)
```

**Solution:**
```python
def double_result(func):
    def wrapper(*args, **kwargs):
        result = func(*args, **kwargs)
        return result * 2
    return wrapper
```

---

## Chapter 11: Decorators with Arguments

### 🎯 Goal
Create decorators that accept their own arguments.

### The Goal

We want to write:
```python
@repeat(3)
def greet():
    print("Hello!")

greet()
# Should print "Hello!" three times
```

### The Problem

How do we pass `3` to the decorator?

```python
# This doesn't work:
def repeat(func, times):  # Where does 'times' come from?
    def wrapper():
        for _ in range(times):
            func()
    return wrapper
```

### The Solution: Decorator Factory (3 Levels!)

```python
def repeat(times):           # 1. Outer: takes decorator argument
    def decorator(func):     # 2. Middle: takes the function
        def wrapper(*args, **kwargs):  # 3. Inner: replacement function
            for _ in range(times):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator
```

### Step-by-Step Breakdown

**Level 1: The Factory**
```python
def repeat(times):
    # times is captured here
    # This function returns a decorator
```

**Level 2: The Decorator**
```python
    def decorator(func):
        # func is the decorated function
        # times is still accessible (closure!)
        # This returns the wrapper
```

**Level 3: The Wrapper**
```python
        def wrapper(*args, **kwargs):
            # This is what actually runs
            # Has access to both func and times
            for _ in range(times):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator
```

### How @ Works with Arguments

```python
@repeat(3)
def greet():
    print("Hello!")
```

**Python does this:**
```python
# Step 1: Call repeat(3) → returns decorator
decorator = repeat(3)

# Step 2: Apply decorator to greet
greet = decorator(greet)
```

**Equivalent to:**
```python
greet = repeat(3)(greet)
```

### Mental Model

```
@repeat(3)
    ↓
Step 1: repeat(3) is called
    ↓
Returns a decorator function (with times=3 captured)
    ↓
Step 2: That decorator is applied to greet
    ↓
@[decorator that remembers times=3]
def greet():
    print("Hello!")
```

### Complete Example

```python
def repeat(times):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for _ in range(times):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator

@repeat(3)
def greet(name):
    print(f"Hello, {name}!")

greet("Alice")
```

**Output:**
```
Hello, Alice!
Hello, Alice!
Hello, Alice!
```

### Another Example: Prefix Decorator

```python
def add_prefix(prefix):
    def decorator(func):
        def wrapper(*args, **kwargs):
            result = func(*args, **kwargs)
            return f"{prefix}: {result}"
        return wrapper
    return decorator

@add_prefix("IMPORTANT")
def get_message():
    return "Meeting at 3pm"

@add_prefix("FYI")
def get_info():
    return "Server maintenance tonight"

print(get_message())  # "IMPORTANT: Meeting at 3pm"
print(get_info())     # "FYI: Server maintenance tonight"
```

### Pattern Template

```python
def decorator_with_args(arg1, arg2):
    def decorator(func):
        def wrapper(*args, **kwargs):
            # Use arg1, arg2, func here
            result = func(*args, **kwargs)
            # Process result
            return result
        return wrapper
    return decorator

@decorator_with_args(value1, value2)
def my_function():
    pass
```

### ✅ Checkpoint

Create a decorator that multiplies the result by a given factor:

```python
@multiply_by(5)
def get_number():
    return 10

print(get_number())  # Should print 50
```

**Solution:**
```python
def multiply_by(factor):
    def decorator(func):
        def wrapper(*args, **kwargs):
            result = func(*args, **kwargs)
            return result * factor
        return wrapper
    return decorator
```

---

## Chapter 12: Stacking Decorators

### 🎯 Goal
Apply multiple decorators to a single function.

### The Concept

You can stack decorators using multiple `@` symbols:

```python
@decorator1
@decorator2
@decorator3
def my_function():
    pass
```

### Order of Application

**Critical Rule:** Decorators apply from **BOTTOM to TOP**

```python
@bold
@italic
def greet():
    return "Hello"
```

**Python does this:**
```python
def greet():
    return "Hello"

greet = italic(greet)  # First (closest to function)
greet = bold(greet)    # Second
```

**Equivalent to:**
```python
greet = bold(italic(greet))
```

### Visual Example

```python
def bold(func):
    def wrapper():
        return f"<b>{func()}</b>"
    return wrapper

def italic(func):
    def wrapper():
        return f"<i>{func()}</i>"
    return wrapper

@bold
@italic
def greet():
    return "Hello"

print(greet())
```

**Output:**
```
<b><i>Hello</i></b>
```

### Step-by-Step

```
Original:
greet() → "Hello"

After @italic:
greet() → italic_wrapper() → "<i>Hello</i>"

After @bold:
greet() → bold_wrapper() → "<b>" + italic_wrapper() + "</b>"
                         → "<b><i>Hello</i></b>"
```

### Mental Model

```
Think of wrapping layers (like an onion):

@outer        ← Applied last (outermost layer)
@middle       ← Applied second
@inner        ← Applied first (closest to function)
def func():
    pass

Execution order when called:
outer → middle → inner → func → inner → middle → outer
```

### Another Example

```python
def uppercase(func):
    def wrapper():
        return func().upper()
    return wrapper

def exclamation(func):
    def wrapper():
        return func() + "!"
    return wrapper

@uppercase
@exclamation
def greet():
    return "hello"

print(greet())
```

**Output:**
```
HELLO!
```

**Why?**
1. `@exclamation` applied first: `"hello"` → `"hello!"`
2. `@uppercase` applied second: `"hello!"` → `"HELLO!"`

### Reverse Order

```python
@exclamation
@uppercase
def greet():
    return "hello"

print(greet())
```

**Output:**
```
HELLO!
```

**Why?**
1. `@uppercase` applied first: `"hello"` → `"HELLO"`
2. `@exclamation` applied second: `"HELLO"` → `"HELLO!"`

### Real-World Example

```python
def login_required(func):
    def wrapper(*args, **kwargs):
        if not user_logged_in:
            return "Please login first"
        return func(*args, **kwargs)
    return wrapper

def admin_required(func):
    def wrapper(*args, **kwargs):
        if not user_is_admin:
            return "Admin access required"
        return func(*args, **kwargs)
    return wrapper

@admin_required
@login_required
def delete_user(user_id):
    return f"User {user_id} deleted"
```

**Order matters!**
1. Check login first (inner)
2. Then check admin (outer)

### ✅ Checkpoint

**Question:** What does this print?

```python
def add_a(func):
    def wrapper():
        return "A" + func()
    return wrapper

def add_b(func):
    def wrapper():
        return "B" + func()
    return wrapper

@add_a
@add_b
def get_letter():
    return "C"

print(get_letter())
```

**Answer:** `"ABC"`

**Why?**
- `@add_b` first: `"C"` → `"BC"`
- `@add_a` second: `"BC"` → `"ABC"`

---

## Chapter 13: functools.wraps

### 🎯 Goal
Preserve function metadata when decorating.

### The Problem

Decorators replace functions, losing metadata:

```python
def my_decorator(func):
    def wrapper():
        return func()
    return wrapper

@my_decorator
def greet():
    """Say hello"""
    return "Hello!"

print(greet.__name__)  # "wrapper" (not "greet"!)
print(greet.__doc__)   # None (lost the docstring!)
```

**This breaks:**
- Documentation tools
- Debugging
- Introspection
- Help systems

### The Solution: @wraps

```python
from functools import wraps

def my_decorator(func):
    @wraps(func)  # ← This decorator preserves metadata!
    def wrapper():
        return func()
    return wrapper

@my_decorator
def greet():
    """Say hello"""
    return "Hello!"

print(greet.__name__)  # "greet" ✓
print(greet.__doc__)   # "Say hello" ✓
```

### What @wraps Does

`@wraps(func)` copies these attributes from `func` to `wrapper`:
- `__name__` - Function name
- `__doc__` - Docstring
- `__module__` - Module name
- `__annotations__` - Type hints
- `__qualname__` - Qualified name
- `__wrapped__` - Reference to original function

### Before and After

**Without @wraps:**
```python
def logger(func):
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__}")
        return func(*args, **kwargs)
    return wrapper

@logger
def add(a, b):
    """Add two numbers"""
    return a + b

print(add.__name__)  # "wrapper" ❌
print(add.__doc__)   # None ❌
```

**With @wraps:**
```python
from functools import wraps

def logger(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__}")
        return func(*args, **kwargs)
    return wrapper

@logger
def add(a, b):
    """Add two numbers"""
    return a + b

print(add.__name__)  # "add" ✓
print(add.__doc__)   # "Add two numbers" ✓
```

### Accessing the Original Function

`@wraps` also adds `__wrapped__`:

```python
from functools import wraps

def decorator(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapper

@decorator
def greet():
    return "Hello!"

# Access the original unwrapped function
print(greet.__wrapped__())  # Calls original greet
```

### Complete Example

```python
from functools import wraps

def timer(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        import time
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"{func.__name__} took {end-start:.4f}s")
        return result
    return wrapper

@timer
def calculate(n):
    """Calculate sum of first n numbers"""
    return sum(range(n))

# Metadata preserved
print(calculate.__name__)  # "calculate"
print(calculate.__doc__)   # "Calculate sum of first n numbers"

# Function still works
print(calculate(1000))
```

**Output:**
```
calculate
Calculate sum of first n numbers
calculate took 0.0001s
499500
```

### Best Practice Template

```python
from functools import wraps

def my_decorator(func):
    @wraps(func)  # ← ALWAYS include this!
    def wrapper(*args, **kwargs):
        # Your decorator logic
        result = func(*args, **kwargs)
        return result
    return wrapper
```

### ✅ Checkpoint

**Question:** Why should you always use `@wraps`?

**Answer:** 
1. Preserves function name and docstring
2. Makes debugging easier
3. Allows documentation tools to work
4. Enables introspection
5. Provides access to original function via `__wrapped__`

---

## Chapter 14: Class-Based Decorators

### 🎯 Goal
Create decorators using classes instead of functions.

### The Concept

Decorators don't have to be functions - they can be classes with `__call__`:

```python
class MyDecorator:
    def __init__(self, func):
        self.func = func
    
    def __call__(self, *args, **kwargs):
        print("Before")
        result = self.func(*args, **kwargs)
        print("After")
        return result

@MyDecorator
def greet():
    print("Hello!")

greet()
```

**Output:**
```
Before
Hello!
After
```

### How It Works

```python
@MyDecorator
def greet():
    print("Hello!")
```

**Python does:**
```python
def greet():
    print("Hello!")

greet = MyDecorator(greet)  # Creates instance, stores func
# greet is now a MyDecorator instance

greet()  # Calls MyDecorator.__call__()
```

### When to Use Classes

**Use classes when:**
- Decorator needs to maintain state
- You want to organize complex logic
- You need multiple methods

**Use functions when:**
- Simple wrapping behavior
- No state needed
- Quick and simple

### Example: Counter Decorator

```python
class CountCalls:
    def __init__(self, func):
        self.func = func
        self.count = 0
    
    def __call__(self, *args, **kwargs):
        self.count += 1
        print(f"Call {self.count} to {self.func.__name__}")
        return self.func(*args, **kwargs)

@CountCalls
def greet():
    print("Hello!")

greet()  # Call 1 to greet
greet()  # Call 2 to greet
greet()  # Call 3 to greet
```

### Example: Cache Decorator

```python
class Cache:
    def __init__(self, func):
        self.func = func
        self.cache = {}
    
    def __call__(self, *args):
        if args in self.cache:
            print(f"Returning cached result for {args}")
            return self.cache[args]
        
        print(f"Computing result for {args}")
        result = self.func(*args)
        self.cache[args] = result
        return result

@Cache
def expensive_operation(n):
    import time
    time.sleep(1)  # Simulate expensive computation
    return n * n

print(expensive_operation(5))  # Computing... (1 second)
print(expensive_operation(5))  # Cached! (instant)
print(expensive_operation(3))  # Computing... (1 second)
```

### Class Decorator with Arguments

```python
class Repeat:
    def __init__(self, times):
        self.times = times
    
    def __call__(self, func):
        def wrapper(*args, **kwargs):
            for _ in range(self.times):
                result = func(*args, **kwargs)
            return result
        return wrapper

@Repeat(3)
def greet():
    print("Hello!")

greet()
# Hello!
# Hello!
# Hello!
```

### Comparison: Function vs Class

**Function decorator:**
```python
def timer(func):
    def wrapper(*args, **kwargs):
        import time
        start = time.time()
        result = func(*args, **kwargs)
        print(f"Took {time.time() - start}s")
        return result
    return wrapper
```

**Class decorator:**
```python
class Timer:
    def __init__(self, func):
        self.func = func
    
    def __call__(self, *args, **kwargs):
        import time
        start = time.time()
        result = self.func(*args, **kwargs)
        print(f"Took {time.time() - start}s")
        return result
```

**Both work the same way when used!**

### ✅ Checkpoint

Create a class decorator that tracks the last result:

```python
@LastResult
def add(a, b):
    return a + b

print(add(2, 3))        # 5
print(add.last_result)  # 5
print(add(10, 20))      # 30
print(add.last_result)  # 30
```

**Solution:**
```python
class LastResult:
    def __init__(self, func):
        self.func = func
        self.last_result = None
    
    def __call__(self, *args, **kwargs):
        self.last_result = self.func(*args, **kwargs)
        return self.last_result
```

---

## Chapter 15: Real-World Examples

### 🎯 Goal
Learn practical decorators used in production code.

### 1. Authentication Decorator

```python
from functools import wraps

# Mock user state
current_user = None

def login_required(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        if current_user is None:
            return "Error: Please login first"
        return func(*args, **kwargs)
    return wrapper

@login_required
def view_profile():
    return f"Profile of {current_user}"

@login_required
def delete_account():
    return f"Account {current_user} deleted"

# Try without login
print(view_profile())  # "Error: Please login first"

# Login and try again
current_user = "Alice"
print(view_profile())  # "Profile of Alice"
```

### 2. Retry Decorator

```python
from functools import wraps
import time

def retry(max_attempts=3, delay=1):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            attempts = 0
            while attempts < max_attempts:
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    attempts += 1
                    if attempts == max_attempts:
                        raise
                    print(f"Attempt {attempts} failed: {e}")
                    print(f"Retrying in {delay}s...")
                    time.sleep(delay)
        return wrapper
    return decorator

@retry(max_attempts=3, delay=0.5)
def unreliable_function():
    import random
    if random.random() < 0.7:  # 70% chance of failure
        raise Exception("Random failure")
    return "Success!"

result = unreliable_function()
print(result)
```

### 3. Rate Limiter

```python
from functools import wraps
import time

def rate_limit(calls_per_second):
    min_interval = 1.0 / calls_per_second
    
    def decorator(func):
        last_called = [0.0]  # Use list to allow modification
        
        @wraps(func)
        def wrapper(*args, **kwargs):
            elapsed = time.time() - last_called[0]
            if elapsed < min_interval:
                time.sleep(min_interval - elapsed)
            
            last_called[0] = time.time()
            return func(*args, **kwargs)
        return wrapper
    return decorator

@rate_limit(calls_per_second=2)
def api_call():
    print(f"API called at {time.time()}")
    return "Data"

# Will be limited to 2 calls per second
for i in range(5):
    api_call()
```

### 4. Memoization / Caching

```python
from functools import wraps

def memoize(func):
    cache = {}
    
    @wraps(func)
    def wrapper(*args):
        if args in cache:
            print(f"Cache hit for {args}")
            return cache[args]
        
        print(f"Computing for {args}")
        result = func(*args)
        cache[args] = result
        return result
    
    return wrapper

@memoize
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

print(fibonacci(10))  # Much faster with memoization!
```

### 5. Type Checking

```python
from functools import wraps

def type_check(*expected_types):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            for arg, expected_type in zip(args, expected_types):
                if not isinstance(arg, expected_type):
                    raise TypeError(
                        f"Expected {expected_type}, got {type(arg)}"
                    )
            return func(*args, **kwargs)
        return wrapper
    return decorator

@type_check(int, int)
def add(a, b):
    return a + b

print(add(5, 3))      # Works: 8
print(add("5", "3"))  # TypeError!
```

### 6. Deprecation Warning

```python
from functools import wraps
import warnings

def deprecated(reason):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            warnings.warn(
                f"{func.__name__} is deprecated: {reason}",
                category=DeprecationWarning,
                stacklevel=2
            )
            return func(*args, **kwargs)
        return wrapper
    return decorator

@deprecated("Use new_function() instead")
def old_function():
    return "Old way"

old_function()  # Warning: old_function is deprecated...
```

### 7. Performance Profiler

```python
from functools import wraps
import time
import statistics

def profile(func):
    execution_times = []
    
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        elapsed = time.time() - start
        
        execution_times.append(elapsed)
        
        print(f"\n{func.__name__} Performance:")
        print(f"  Last call: {elapsed:.4f}s")
        print(f"  Average: {statistics.mean(execution_times):.4f}s")
        print(f"  Min: {min(execution_times):.4f}s")
        print(f"  Max: {max(execution_times):.4f}s")
        print(f"  Total calls: {len(execution_times)}")
        
        return result
    
    return wrapper

@profile
def process_data(n):
    return sum(range(n))

for i in range(5):
    process_data(1000000)
```

### 8. Flask-Style Route Decorator

```python
class Application:
    def __init__(self):
        self.routes = {}
    
    def route(self, path):
        def decorator(func):
            self.routes[path] = func
            return func
        return decorator
    
    def handle_request(self, path):
        handler = self.routes.get(path)
        if handler:
            return handler()
        return "404 Not Found"

app = Application()

@app.route("/")
def home():
    return "Home Page"

@app.route("/about")
def about():
    return "About Page"

print(app.handle_request("/"))      # "Home Page"
print(app.handle_request("/about")) # "About Page"
```

---

## Chapter 16: Common Pitfalls

### 🎯 Goal
Learn what NOT to do with decorators.

### Pitfall 1: Forgetting @wraps

**Problem:**
```python
def bad_decorator(func):
    def wrapper():
        return func()
    return wrapper

@bad_decorator
def greet():
    """Say hello"""
    return "Hello"

print(greet.__name__)  # "wrapper" ❌
print(greet.__doc__)   # None ❌
```

**Solution:**
```python
from functools import wraps

def good_decorator(func):
    @wraps(func)
    def wrapper():
        return func()
    return wrapper
```

### Pitfall 2: Not Passing Through Arguments

**Problem:**
```python
def bad_decorator(func):
    def wrapper():  # No *args, **kwargs!
        return func()
    return wrapper

@bad_decorator
def greet(name):
    return f"Hello, {name}"

greet("Alice")  # TypeError: wrapper() takes 0 arguments
```

**Solution:**
```python
def good_decorator(func):
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapper
```

### Pitfall 3: Not Returning the Result

**Problem:**
```python
def bad_decorator(func):
    def wrapper(*args, **kwargs):
        func(*args, **kwargs)
        # Oops! Forgot to return
    return wrapper

@bad_decorator
def add(a, b):
    return a + b

result = add(5, 3)
print(result)  # None ❌
```

**Solution:**
```python
def good_decorator(func):
    def wrapper(*args, **kwargs):
        result = func(*args, **kwargs)
        return result  # ✓
    return wrapper
```

### Pitfall 4: Decorating with Parentheses

**Problem:**
```python
def my_decorator(func):
    def wrapper():
        return func()
    return wrapper

@my_decorator()  # ❌ Calling it!
def greet():
    return "Hello"

# TypeError: my_decorator() missing 1 required positional argument
```

**Solution:**
```python
# Without arguments - no parentheses
@my_decorator
def greet():
    return "Hello"

# With arguments - use factory pattern
def repeat(times):
    def decorator(func):
        # ...
        return wrapper
    return decorator

@repeat(3)  # ✓ This is correct
def greet():
    pass
```

### Pitfall 5: Modifying Mutable Default Arguments

**Problem:**
```python
def cache(func, _cache={}):  # ❌ Shared across all uses!
    def wrapper(*args):
        if args in _cache:
            return _cache[args]
        result = func(*args)
        _cache[args] = result
        return result
    return wrapper
```

**Solution:**
```python
def cache(func):
    _cache = {}  # ✓ New cache for each decorated function
    def wrapper(*args):
        if args in _cache:
            return _cache[args]
        result = func(*args)
        _cache[args] = result
        return result
    return wrapper
```

### Pitfall 6: Losing Function on Re-decoration

**Problem:**
```python
def decorator(func):
    def wrapper():
        return func()
    return wrapper

@decorator
@decorator  # Double decoration!
def greet():
    return "Hello"

# Now you can't access the original greet
```

**Be aware of what you're decorating!**

### Pitfall 7: Order with Decorator Arguments

**Problem:**
```python
# Wrong order - confusing!
@login_required
@admin_required
def delete_user():
    pass

# This checks admin BEFORE login!
```

**Solution:**
```python
# Correct order
@admin_required
@login_required
def delete_user():
    pass

# Checks login first, then admin
```

---

## Chapter 17: Best Practices

### 🎯 Goal
Learn the conventions and patterns professionals use.

### 1. Always Use @wraps

```python
from functools import wraps

def my_decorator(func):
    @wraps(func)  # ← ALWAYS!
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapper
```

### 2. Use *args and **kwargs

```python
# Unless you have a SPECIFIC reason not to:
def decorator(func):
    @wraps(func)
    def wrapper(*args, **kwargs):  # Accept anything
        return func(*args, **kwargs)  # Pass everything through
    return wrapper
```

### 3. Return the Result

```python
def decorator(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        # Do stuff before
        result = func(*args, **kwargs)  # Capture
        # Do stuff after
        return result  # Return!
    return wrapper
```

### 4. Document Your Decorators

```python
def retry(max_attempts=3, delay=1):
    """
    Retry decorator that attempts to call a function multiple times.
    
    Args:
        max_attempts: Maximum number of attempts (default 3)
        delay: Delay in seconds between attempts (default 1)
    
    Example:
        @retry(max_attempts=5, delay=2)
        def unreliable_api_call():
            ...
    """
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            # Implementation...
            pass
        return wrapper
    return decorator
```

### 5. Consider Optional Arguments

```python
from functools import wraps

def smart_decorator(func=None, *, option=None):
    """Works with or without parentheses"""
    def decorator(f):
        @wraps(f)
        def wrapper(*args, **kwargs):
            # Use option here
            return f(*args, **kwargs)
        return wrapper
    
    if func is None:
        # Called with arguments: @smart_decorator(option="value")
        return decorator
    else:
        # Called without: @smart_decorator
        return decorator(func)

# Both work!
@smart_decorator
def func1():
    pass

@smart_decorator(option="value")
def func2():
    pass
```

### 6. Organize Related Decorators

```python
# decorators.py
from functools import wraps

class AuthDecorators:
    @staticmethod
    def login_required(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            # Check login
            return func(*args, **kwargs)
        return wrapper
    
    @staticmethod
    def admin_required(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            # Check admin
            return func(*args, **kwargs)
        return wrapper

# Usage
from decorators import AuthDecorators

@AuthDecorators.login_required
@AuthDecorators.admin_required
def admin_panel():
    pass
```

### 7. Make Decorators Testable

```python
def timer(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        import time
        start = time.time()
        result = func(*args, **kwargs)
        wrapper.last_duration = time.time() - start  # Expose!
        return result
    
    wrapper.last_duration = None  # Initialize
    return wrapper

@timer
def slow_function():
    import time
    time.sleep(0.1)

slow_function()
print(f"Took {timer.last_duration}s")  # Testable!
```

### 8. Naming Conventions

```python
# Verb-based names for action decorators
@measure_time
@log_calls
@retry_on_failure
def my_function():
    pass

# Adjective/state names for requirement decorators
@authenticated
@cached
@deprecated
def my_function():
    pass

# Noun-based for factories
@route("/home")
@repeat(3)
@add_prefix("Error")
def my_function():
    pass
```

---

## Chapter 18: Advanced Patterns

### 🎯 Goal
Master sophisticated decorator techniques.

### Pattern 1: Decorator Factory with Optional Args

```python
from functools import wraps

def repeat(func=None, *, times=2):
    """Can be used with or without arguments"""
    def decorator(f):
        @wraps(f)
        def wrapper(*args, **kwargs):
            for _ in range(times):
                result = f(*args, **kwargs)
            return result
        return wrapper
    
    if func is None:
        # Called with arguments
        return decorator
    else:
        # Called without arguments
        return decorator(func)

# Both syntaxes work:
@repeat
def greet1():
    print("Hello!")

@repeat(times=3)
def greet2():
    print("Hello!")
```

### Pattern 2: Stateful Decorators

```python
class RateLimiter:
    def __init__(self, max_calls, time_window):
        self.max_calls = max_calls
        self.time_window = time_window
        self.calls = []
    
    def __call__(self, func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            import time
            now = time.time()
            
            # Remove old calls
            self.calls = [c for c in self.calls 
                         if now - c < self.time_window]
            
            if len(self.calls) >= self.max_calls:
                raise Exception("Rate limit exceeded")
            
            self.calls.append(now)
            return func(*args, **kwargs)
        return wrapper

@RateLimiter(max_calls=5, time_window=60)
def api_call():
    return "Data"
```

### Pattern 3: Context-Manager Decorator

```python
from contextlib import contextmanager
from functools import wraps

@contextmanager
def timer_context():
    import time
    start = time.time()
    yield
    print(f"Took {time.time() - start:.4f}s")

def timed(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        with timer_context():
            return func(*args, **kwargs)
    return wrapper

@timed
def slow_function():
    import time
    time.sleep(1)
    return "Done"
```

### Pattern 4: Decorator with Async Support

```python
from functools import wraps
import asyncio

def retry_async(max_attempts=3):
    def decorator(func):
        @wraps(func)
        async def wrapper(*args, **kwargs):
            for attempt in range(max_attempts):
                try:
                    return await func(*args, **kwargs)
                except Exception as e:
                    if attempt == max_attempts - 1:
                        raise
                    await asyncio.sleep(1)
        return wrapper
    return decorator

@retry_async(max_attempts=3)
async def fetch_data():
    # Async API call
    pass
```

### Pattern 5: Parameterized Class Decorators

```python
class Route:
    routes = {}
    
    def __init__(self, path, methods=None):
        self.path = path
        self.methods = methods or ['GET']
    
    def __call__(self, func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            return func(*args, **kwargs)
        
        Route.routes[self.path] = {
            'handler': wrapper,
            'methods': self.methods
        }
        return wrapper

@Route('/home', methods=['GET', 'POST'])
def home_handler():
    return "Home"

@Route('/api/users', methods=['GET'])
def users_handler():
    return "Users"

print(Route.routes)
```

### Pattern 6: Validation Decorator

```python
from functools import wraps

def validate(**validators):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            # Validate kwargs
            for param, validator in validators.items():
                if param in kwargs:
                    value = kwargs[param]
                    if not validator(value):
                        raise ValueError(
                            f"{param}={value} failed validation"
                        )
            return func(*args, **kwargs)
        return wrapper
    return decorator

@validate(
    age=lambda x: 0 < x < 150,
    name=lambda x: len(x) > 0
)
def create_user(name, age):
    return f"User {name}, age {age}"

create_user(name="Alice", age=30)  # OK
create_user(name="", age=30)       # ValueError!
```

### Pattern 7: Method Decorators

```python
from functools import wraps

def log_method_calls(func):
    @wraps(func)
    def wrapper(self, *args, **kwargs):
        print(f"Calling {func.__name__} on {self.__class__.__name__}")
        return func(self, *args, **kwargs)
    return wrapper

class Calculator:
    @log_method_calls
    def add(self, a, b):
        return a + b
    
    @log_method_calls
    def multiply(self, a, b):
        return a * b

calc = Calculator()
calc.add(5, 3)       # Logs: "Calling add on Calculator"
calc.multiply(5, 3)  # Logs: "Calling multiply on Calculator"
```

---

## 🎓 Conclusion

You've now mastered Python decorators from fundamentals to advanced patterns!

### What You've Learned:

✅ Functions as first-class objects  
✅ Closures and variable capture  
✅ Basic decorator pattern  
✅ The `@` syntax  
✅ `*args` and `**kwargs`  
✅ Universal decorators  
✅ Decorators with arguments  
✅ Stacking decorators  
✅ `functools.wraps`  
✅ Class-based decorators  
✅ Real-world examples  
✅ Common pitfalls  
✅ Best practices  
✅ Advanced patterns  

### Next Steps:

1. **Practice:** Write your own decorators for your projects
2. **Read:** Study decorators in popular libraries (Flask, Django, pytest)
3. **Build:** Create a decorator library for common tasks
4. **Experiment:** Try combining decorators in creative ways

### Remember:

> Decorators are just functions that take functions and return functions.  
> Everything else is just syntax sugar and patterns!

---

**Happy decorating! 🎭**