# 🐍 Python Core concepts

<div align="center">

![Python Version](https://img.shields.io/badge/python-3.8%2B-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-active-success)
![Contributions](https://img.shields.io/badge/contributions-welcome-brightgreen)

**Deep dive into how Python really works under the hood**

*A comprehensive guide to Python's internal mechanisms, core concepts, and implementation details.*

[Explore Concepts](#-core-concepts) • [Interactive Demos](#-interactive-visualizations) • [Get Started](#-getting-started) • [Contribute](#-contributing)

</div>

---

## 🎯 What You'll Learn

This repository is your guide to understanding Python at a **fundamental level**. We go beyond syntax and explore:

- 🧠 How Python manages memory and objects in RAM
- 🔗 The truth about variables, references, and identity
- 🎭 Functions as first-class objects, closures, and decorators
- 📦 How data structures are implemented internally
- 🏗️ Object-oriented programming mechanics (classes, metaclasses, MRO)
- ♻️ Garbage collection and memory management
- ⚡ Generators, iterators, and lazy evaluation
- 🔮 Context managers and the `with` statement
- 🚀 Async/await and the event loop
- 🎪 Magic methods and Python's data model

## 📚 Core Concepts

### 1️⃣ [Objects & References](./01-objects-and-references/)
- Everything is an object
- Variables as references, not containers
- Identity vs equality (`is` vs `==`)
- Memory model visualization
- Mutable vs immutable objects

### 2️⃣ [Functions & Closures](./02-functions-and-closures/)
- Functions as first-class objects
- Closures and variable scope (LEGB rule)
- **Decorators demystified** ⭐
- Function signatures and `*args`, `**kwargs`
- Lambda functions and functional programming

### 3️⃣ [Data Structures Internals](./03-data-structures/)
- How lists really work (dynamic arrays)
- Dictionary implementation (hash tables)
- Set operations and complexity
- Collections module deep dive
- Time complexity analysis

### 4️⃣ [Object-Oriented Programming](./04-oop-internals/)
- How classes are created
- Method Resolution Order (MRO)
- Descriptors and properties
- Metaclasses: classes that create classes
- `__init__` vs `__new__`

### 5️⃣ [Memory Management](./05-memory-management/)
- Reference counting mechanism
- Garbage collection (generational GC)
- Memory profiling tools
- Common memory leaks
- Best practices

### 6️⃣ [Iterators & Generators](./06-iterators-generators/)
- The iterator protocol
- Generators and `yield`
- Generator expressions
- `itertools` internals
- Memory efficiency

### 7️⃣ [Context Managers](./07-context-managers/)
- The `with` statement
- `__enter__` and `__exit__`
- `contextlib` utilities
- Custom context managers
- Resource management

### 8️⃣ [Async Internals](./08-async-internals/)
- Event loop mechanics
- Coroutines and `async`/`await`
- `asyncio` internals
- Concurrency vs parallelism
- When to use async

---

## 🎨 Interactive Visualizations

One of the unique features of this repository is **interactive visual explanations** that show you exactly what happens in memory — step by step, with live animations.

### 🎭 Python Decorators — Memory Animation

**[→ Open Interactive Demo](./Decorators/decorator_animation.html)**

> Open in any browser — no installation needed.

This interactive visualization walks you through **5 progressive lessons** that build up to a complete understanding of how decorators really work under the hood:

| # | Lesson | What You'll See |
|---|--------|-----------------|
| 1️⃣ | **Function as Object** | How Python creates a function object in memory and how the name is just a *reference* (pointer) to it |
| 2️⃣ | **Function in a Variable** | Assigning a function to another variable — both names point to the *same* object in memory |
| 3️⃣ | **Closures — Memory Magic** | How `inner` captures `x` from `outer`'s scope and keeps it alive even after `outer`'s frame is destroyed |
| 4️⃣ | **Simple Decorator** | The full decorator mechanics: creating a `wrapper`, capturing the original function in a closure, and reassigning the name |
| 5️⃣ | **Decorator with Arguments** | How `*args` and `**kwargs` let wrappers pass through any arguments transparently |

**Animation features:**
- 🖥️ **Side-by-side view** — Python code (with highlighted active lines) + live memory canvas
- 🧠 **Memory canvas** — shows function objects (blue), variables (purple), and execution frames (orange/red) as the code runs
- 🔗 **Animated arrows** — traces exactly which variable points to which object at each step
- 🔒 **Closure indicator** — visually shows captured variables locked inside inner functions
- ▶️ **Play / Next Step / Reset** controls + adjustable speed slider
- 📚 **Step explanation panel** — plain-English description of what's happening at each memory level

### Coming Soon:
- 🔄 Memory allocation and garbage collection
- 🎪 Class instantiation flow
- 🔗 MRO visualization
- ⚡ Generator execution
- 🌊 Event loop animation

---

## 🚀 Getting Started

### Prerequisites
```bash
Python 3.8 or higher
```

### Clone the Repository
```bash
git clone https://github.com/yourusername/python-internals.git
cd python-internals
```

### Explore the Content
1. **Read the guides** - Start with [Objects & References](./01-objects-and-references/)
2. **Run the examples** - Each section has runnable code
3. **Try interactive demos** - Open HTML visualizations in your browser
4. **Experiment** - Modify examples and see what happens

### Run Examples
```bash
# Navigate to any section
cd 02-functions-and-closures/examples

# Run the code
python closures_example.py
python decorators_step_by_step.py
```

---

## 🎓 Who Is This For?

### ✅ Perfect for:
- **Intermediate Python developers** wanting to level up
- **Job interview preparation** (technical deep dives)
- **Developers from other languages** (C++, Java, JavaScript) learning Python properly
- **Computer Science students** studying language implementation
- **Curious minds** who want to know "how does this *really* work?"

### 📖 Prerequisites:
- Basic Python syntax (variables, functions, classes)
- Comfort reading code
- Curiosity and patience!

---

## 🗺️ Learning Path

### Beginner → Intermediate
```
1. Objects & References
2. Functions & Closures
3. Data Structures
4. Iterators & Generators
```

### Intermediate → Advanced
```
5. OOP Internals
6. Memory Management
7. Context Managers
8. Async Internals
```

### 💡 Tips:
- Don't rush - internals take time to understand
- Run every code example yourself
- Use the visualizations to build mental models
- Experiment and break things!
- Read the CPython source when referenced

---

## 🛠️ Repository Structure

```
python-internals/
│
├── 📁 01-objects-and-references/
│   ├── README.md                    # Concept explanation
│   ├── examples/                    # Runnable code
│   └── diagrams/                    # Visual aids
│
├── 📁 02-functions-and-closures/
│   ├── closures-explained.md
│   ├── decorators-deep-dive.md
│   └── examples/
│
├── 📁 03-data-structures/
├── 📁 04-oop-internals/
├── 📁 05-memory-management/
├── 📁 06-iterators-generators/
├── 📁 07-context-managers/
├── 📁 08-async-internals/
│
├── 📁 visualizations/               # Interactive HTML demos
│   └── decorator_animation.html
│
├── 📁 resources/
│   ├── cheatsheets/
│   ├── references/                  # CPython source links
│   └── recommended-reading/
│
└── README.md
```

---

## 🎯 Key Features

### 📊 Visual Learning
- Memory diagrams showing object relationships
- Step-by-step animations
- Flowcharts of execution paths
- Color-coded code examples

### 💻 Hands-On Examples
- Every concept has working code
- Comments explain each step
- "Try it yourself" challenges
- Common pitfalls highlighted

### 🔗 CPython Source References
- Links to actual CPython implementation
- Explanations of C-level code
- Understanding the "why" behind decisions

### 🎪 Real-World Applications
- When to use each concept
- Performance implications
- Best practices
- Anti-patterns to avoid

---

## 📖 Example: Decorator Deep Dive

Here's a taste of what you'll learn:

```python
# What you write:
@timer
def calculate():
    return sum(range(1000000))

# What Python actually does:
def calculate():
    return sum(range(1000000))

calculate = timer(calculate)  # Function wrapping!
```

**In memory, this creates:**
1. Original `calculate` function object
2. `timer` creates a `wrapper` function
3. `wrapper` captures `calculate` in a closure
4. Name `calculate` now points to `wrapper`
5. `wrapper` remembers original `calculate`

**[→ See it animated](./Decorators/decorator_animation.html)**

---

## 🤝 Contributing

Contributions are **highly encouraged**! Here's how:

### Ways to Contribute:
- 📝 Fix typos or improve explanations
- 💡 Add new examples or visualizations
- 🐛 Report issues or confusing sections
- ✨ Suggest new topics to cover
- 🎨 Improve diagrams or animations

### Contribution Process:
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-explanation`)
3. Commit your changes (`git commit -m 'Add explanation of metaclasses'`)
4. Push to the branch (`git push origin feature/amazing-explanation`)
5. Open a Pull Request

### Guidelines:
- Keep explanations clear and beginner-friendly
- Include runnable code examples
- Add visual aids when possible
- Test all code before submitting
- Follow existing formatting style

---

## 📚 Resources & References

### Official Documentation
- [Python Documentation](https://docs.python.org/3/)
- [CPython Source Code](https://github.com/python/cpython)
- [PEPs (Python Enhancement Proposals)](https://www.python.org/dev/peps/)

### Recommended Reading
- **Fluent Python** by Luciano Ramalho
- **Python Cookbook** by David Beazley
- **Effective Python** by Brett Slatkin
- **CPython Internals** by Anthony Shaw

### Online Resources
- [Real Python](https://realpython.com/)
- [Python Tutor](http://pythontutor.com/) - Visualize code execution
- [Python Module of the Week](https://pymotw.com/)

---

## 🏆 Goals of This Repository

### What This Is:
✅ Deep explanations of Python internals  
✅ Visual learning with animations  
✅ Reference material for experienced developers  
✅ Interview preparation resource  
✅ Understanding the "why" behind Python's design  

### What This Is Not:
❌ Basic Python tutorial  
❌ Algorithm/data structure course  
❌ Web development guide  
❌ Comprehensive CPython implementation guide  

---

## 📈 Progress & Roadmap

### ✅ Completed
- [x] Interactive decorator visualization
- [x] Repository structure
- [x] Initial documentation

### 🚧 In Progress
- [ ] Objects & References guide
- [ ] Closures deep dive
- [ ] Memory model diagrams

### 📋 Planned
- [ ] All 8 core concept sections
- [ ] 10+ interactive visualizations
- [ ] Video explanations
- [ ] Quiz/practice problems
- [ ] Performance benchmarking tools

---

## 💬 Community

### Get Help
- 🐛 [Report Issues](https://github.com/shireesha-ai-infra/python-core-concepts/issues)
- 💡 [Discussions](https://github.com/shireesha-ai-infra/python-core-concepts/discussions)
- 📧 Email: shireeshagovindu24@gmail.com

### Connect
- 🐦 Twitter: [@shireesha__G](https://x.com/shireesha__G)
- 💼 LinkedIn: [Shireesha Govindu](https://www.linkedin.com/in/shireesha-govindu-510018161/)
- 🌐 Website: [shireesha-ai-infra.github.io](https://shireesha-ai-infra.github.io)

---

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

```
MIT License - feel free to use this for learning, teaching, or reference!
```

---

## 🙏 Acknowledgments

- Python core developers for creating such an elegant language
- The Python community for amazing documentation and discussions
- Everyone who asks "but how does it *really* work?"

---

## ⭐ Star History

If you find this repository helpful, please consider giving it a star! ⭐

It helps others discover this resource and motivates continued development.

<div align="center">

**[⬆ Back to Top](#-python-internals)**

---

Made with ❤️ and ☕ by shireesha

*"Understanding the internals makes you a better Python developer"*

</div>
