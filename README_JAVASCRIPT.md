# JavaScript Roadmap: Beginner to Advanced

## Who this guide is for
This guide is for learners starting with JavaScript and moving toward more advanced web development.
It explains the concepts step by step, with simple examples and clear definitions.

## What you'll learn
- JavaScript basics like variables, functions, and arrays
- How control flow and data structures work
- Key browser APIs and modern programming features
- How to prepare for interviews with common questions


# 1. JavaScript Fundamentals

* Introduction to JavaScript
* History of JavaScript & ECMAScript
* How JavaScript works in browsers
* Setting up environment
* Variables (`var`, `let`, `const`)
* Data Types
  * String
  * Number
  * Boolean
  * Null
  * Undefined
  * Symbol
  * BigInt
* Operators
* Type Conversion & Coercion
* Input/Output
* Comments
* Template Literals

## Definitions
JavaScript is a high-level, interpreted programming language for browsers and server-side platforms such as Node.js.

## Code Snippet
```javascript
console.log('Hello, JavaScript!');
```

## Advantages
- Universal browser support
- Fast development cycle
- Large ecosystem and libraries

## Disadvantages
- Weak typing can lead to runtime bugs
- Inconsistent browser behavior requires compatibility handling

## Interview Questions
Q: What is JavaScript?
A: JavaScript is a scripting language used for interactive web pages and server-side applications.

Q: What is ECMAScript?
A: ECMAScript is the language standard that defines JavaScript.

---

# 2. Control Flow

* Conditional Statements
  * `if`
  * `else`
  * `switch`
* Loops
  * `for`
  * `while`
  * `do while`
  * `for...of`
  * `for...in`
* Break & Continue
* Nested Loops
* Ternary Operator

## Definitions
Control flow directs the order in which statements are executed.

## Code Snippet
```javascript
if (age >= 18) {
  console.log('Adult');
} else {
  console.log('Minor');
}

for (let i = 0; i < 5; i++) {
  console.log(i);
}
```

## Advantages
- Enables dynamic logic based on conditions
- Supports iteration over collections

## Disadvantages
- Deep nesting reduces readability
- Infinite loops can crash programs

## Interview Questions
Q: When should you use `switch` vs `if`?
A: Use `switch` for multiple discrete cases and `if` for general boolean conditions.

Q: What is the difference between `for...of` and `for...in`?
A: `for...of` iterates values of an iterable; `for...in` iterates enumerable property keys.

---

# 3. Functions

* Function Declaration
* Function Expression
* Arrow Functions
* Parameters & Arguments
* Default Parameters
* Rest Parameters
* Spread Operator
* Callback Functions
* Higher Order Functions
* Pure vs Impure Functions
* Recursion
* IIFE
* Closures

## Definitions
Functions are reusable blocks of code. A closure is a function with access to its lexical scope.

## Code Snippet
```javascript
function add(a, b = 0) {
  return a + b;
}

const multiply = (a, b) => a * b;

const sumAll = (...values) => values.reduce((total, value) => total + value, 0);
```

## Advantages
- Encourages reusable code
- Supports modular design

## Disadvantages
- Misusing global state can lead to bugs
- Complex callback chains can be hard to follow

## Interview Questions
Q: What is an IIFE?
A: An Immediately Invoked Function Expression runs as soon as it is defined.

Q: Why are closures useful?
A: They preserve state across function invocations without global variables.

---

# 4. Arrays

* Creating Arrays
* Array Methods
  * `push`
  * `pop`
  * `shift`
  * `unshift`
  * `splice`
  * `slice`
* Iteration Methods
  * `map`
  * `filter`
  * `reduce`
  * `find`
  * `some`
  * `every`
  * `forEach`
* Sorting
* Destructuring
* Array Spread

## Definitions
Arrays are ordered collections of values.

## Code Snippet
```javascript
const numbers = [1, 2, 3, 4];
numbers.push(5);
const doubled = numbers.map(n => n * 2);
const filtered = numbers.filter(n => n % 2 === 0);
```

## Advantages
- Built-in methods simplify list processing
- Supports many functional operations

## Disadvantages
- Some operations allocate new arrays
- Poor choice for large sparse data structures

## Interview Questions
Q: What does `reduce()` do?
A: It aggregates array values into a single result.

Q: How does `slice()` differ from `splice()`?
A: `slice()` returns a new array; `splice()` modifies the original array.

---

# 5. Objects

* Object Creation
* Properties & Methods
* `this` keyword
* Object Methods
  * `keys`
  * `values`
  * `entries`
  * `assign`
  * `freeze`
  * `seal`
* Destructuring Objects
* Nested Objects
* Optional Chaining
* Nullish Coalescing

## Definitions
Objects store named properties and functions.

## Code Snippet
```javascript
const user = {
  id: 1,
  name: 'Alice',
  greet() {
    return `Hello, ${this.name}`;
  }
};

const { id, name } = user;
const city = user.address?.city ?? 'Unknown';
```

## Advantages
- Flexible data representation
- Easy to model real-world entities

## Disadvantages
- Mutable by default
- Deep nested structures can be error-prone

## Interview Questions
Q: What does `Object.freeze()` do?
A: Makes an object immutable, preventing new property additions or changes.

Q: What is optional chaining used for?
A: Safe access to deeply nested properties when a parent may be null or undefined.

---

# 6. DOM (Document Object Model)

* DOM Introduction
* Selecting Elements
* Modifying Elements
* Events
* Event Bubbling & Capturing
* Event Delegation
* Forms Handling
* Creating Elements
* DOM Traversal
* Browser APIs

## Definitions
The DOM is a tree-structure representation of HTML documents accessible from JavaScript.

## Code Snippet
```javascript
const button = document.querySelector('#submit');
button.addEventListener('click', event => {
  const name = document.querySelector('#name').value;
  document.querySelector('#message').textContent = `Hello, ${name}`;
});
```

## Advantages
- Enables interactive web experiences
- Updates the page without reloads

## Disadvantages
- Manual DOM updates can be verbose
- Direct manipulation may cause performance issues if overused

## Interview Questions
Q: What is event delegation?
A: Handling events at a parent level so one listener can respond to many child elements.

Q: How do you create a new DOM element?
A: Use `document.createElement()` and attach it via `appendChild()`.

---

# 7. Advanced JavaScript Concepts

* Execution Context
* Call Stack
* Hoisting
* Scope
* Lexical Environment
* Closures
* Temporal Dead Zone
* Memory Management
* Garbage Collection
* `this` binding
* `call`, `apply`, `bind`
* Prototype
* Prototypal Inheritance
* Classes
* Encapsulation
* Polymorphism
* Inheritance
* Iterators & Generators

## Definitions
Advanced JS concepts explain how the language executes code, manages scope, and shares behavior.

## Code Snippet
```javascript
function Person(name) {
  this.name = name;
}
Person.prototype.greet = function() {
  return `Hi, ${this.name}`;
};

function* counter() {
  let i = 0;
  while (true) {
    yield i++;
  }
}
```

## Advantages
- Enables deep mastery of language behavior
- Helps debug tricky runtime issues

## Disadvantages
- Can be difficult for beginners
- Misunderstanding scope or `this` leads to bugs

## Interview Questions
Q: What is hoisting?
A: Variable and function declarations are moved to the top of their scope before execution.

Q: What is the Temporal Dead Zone?
A: The period between entering a scope and initializing a `let` or `const` variable.

---

# 8. Asynchronous JavaScript

* Synchronous vs Asynchronous
* Web APIs
* Callbacks
* Callback Hell
* Promises
* Promise Chaining
* Async/Await
* Event Loop
* Microtasks vs Macrotasks
* Fetch API
* AJAX
* JSON
* Error Handling

## Definitions
Asynchronous JavaScript lets code wait for I/O operations without blocking the main thread.

## Code Snippet
```javascript
fetch('/api/data')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error(error));

async function loadData() {
  try {
    const response = await fetch('/api/data');
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error(error);
  }
}
```

## Advantages
- Keeps UI responsive
- Better support for I/O-heavy workflows

## Disadvantages
- More complex control flow
- Harder to reason about than synchronous code

## Interview Questions
Q: What is Promise chaining?
A: Sequential handling of promises using `.then()`.

Q: What is the event loop?
A: The mechanism that manages execution order of synchronous code, microtasks, and macrotasks.

---

# 9. ES6+ Features

* `let` & `const`
* Arrow Functions
* Template Literals
* Destructuring
* Modules
* Classes
* Promises
* Async/Await
* Optional Chaining
* Nullish Coalescing
* Dynamic Imports

## Definitions
Modern JavaScript features introduced since ES6 improve readability, modularity, and async support.

## Code Snippet
```javascript
const [first, ...rest] = [1, 2, 3];
const greeting = `Hello, ${first}`;
```

## Advantages
- Cleaner code and better patterns
- Enables modern tooling and frameworks

## Disadvantages
- Requires transpilation for older environments
- Some new syntax may be unfamiliar

## Interview Questions
Q: What is optional chaining?
A: A syntax to safely access nested object properties.

Q: What is the nullish coalescing operator?
A: `??` returns the right-hand value when the left-hand side is `null` or `undefined`.

---

# 10. Modules

* CommonJS
* ES Modules
* Export/Import
* Named Export
* Default Export
* Module Bundlers

## Definitions
Modules let you split code into reusable files and manage dependencies cleanly.

## Code Snippet
```javascript
// math.js
export function add(a, b) {
  return a + b;
}
// app.js
import { add } from './math.js';
```

## Advantages
- Better code organization
- Encapsulation and reuse

## Disadvantages
- Bundling complexity for browsers
- Module resolution issues can be confusing

## Interview Questions
Q: What is the difference between CommonJS and ESM?
A: CommonJS uses `require` and `module.exports`; ESM uses `import` and `export`.

Q: What is a default export?
A: A module can export one primary value without using curly braces on import.

---

# 11. Error Handling

* `try...catch`
* `throw`
* Custom Errors
* Finally Block
* Debugging
* Console Methods

## Definitions
Error handling catches and responds to runtime exceptions.

## Code Snippet
```javascript
try {
  throw new Error('Invalid input');
} catch (err) {
  console.error(err.message);
} finally {
  console.log('Cleanup');
}
```

## Advantages
- Prevents application crashes
- Improves fault tolerance

## Disadvantages
- Overuse can mask bugs
- Catch blocks should not be too broad

## Interview Questions
Q: When is `finally` executed?
A: Always, whether an error occurs or not.

Q: How do you create a custom error?
A: Extend the `Error` class and set a custom message.

---

# 12. Browser Storage

* Cookies
* Local Storage
* Session Storage
* IndexedDB

## Definitions
Browser storage provides persistence for client-side data.

## Code Snippet
```javascript
localStorage.setItem('key', 'value');
const value = localStorage.getItem('key');
```

## Advantages
- Simple client-side persistence
- Supports offline and stateful experiences

## Disadvantages
- Storage size limits
- Sensitive data should not be stored insecurely

## Interview Questions
Q: What is the difference between localStorage and sessionStorage?
A: `localStorage` persists across tabs and browser restarts; `sessionStorage` lasts only during the tab session.

Q: When would you use IndexedDB?
A: For complex or large client-side data storage.

---

# 13. JavaScript in Browser

* BOM (Browser Object Model)
* Window Object
* Location
* History
* Navigator
* Timers
  * `setTimeout`
  * `setInterval`

## Definitions
Browser APIs extend JavaScript beyond the DOM to browser-level functionality.

## Code Snippet
```javascript
setTimeout(() => console.log('Delayed'), 1000);
window.location.href = '/home';
```

## Advantages
- Enables browser-specific behavior
- Supports navigation, timing, and environment introspection

## Disadvantages
- Browser API behavior varies across platforms
- Some APIs require user permission

## Interview Questions
Q: What is the Window object?
A: The global object in browser environments that represents the browser window.

Q: How do you cancel a timer?
A: With `clearTimeout()` or `clearInterval()`.

---

# 14. OOP in JavaScript

* Object-Oriented Programming
* Constructor Functions
* Prototypes
* ES6 Classes
* Inheritance
* Static Methods
* Getters & Setters

## Definitions
JavaScript supports OOP with constructor functions, prototypes, and class syntax.

## Code Snippet
```javascript
class Person {
  constructor(name) {
    this.name = name;
  }
  static info() {
    return 'Person class';
  }
  get displayName() {
    return `Name: ${this.name}`;
  }
}
```

## Advantages
- Organizes code around objects and behavior
- Supports inheritance and code reuse

## Disadvantages
- Prototypal inheritance differs from classical OOP
- Overuse can lead to rigid code structures

## Interview Questions
Q: What is a constructor function?
A: A function used with `new` to create an object instance.

Q: How do getters work?
A: They define a property that executes a function when accessed.

---

# 15. Functional Programming

* First-Class Functions
* Pure Functions
* Immutability
* Composition
* Currying
* Memoization

## Definitions
Functional programming emphasizes pure functions, immutable data, and composition.

## Code Snippet
```javascript
const add = x => y => x + y;
const compose = (f, g) => x => f(g(x));
```

## Advantages
- Easier reasoning and testing
- Reduces side effects

## Disadvantages
- Can be unfamiliar to imperative developers
- May require more intermediate data structures

## Interview Questions
Q: What is a pure function?
A: A function that returns the same output for the same input and has no side effects.

Q: What is currying?
A: Transforming a function so it can be called with partial arguments.

---

# 16. Data Structures & Algorithms in JS

* Arrays
* Linked Lists
* Stack
* Queue
* Hash Table
* Tree
* Graph
* Sorting Algorithms
* Searching Algorithms
* Recursion Problems

## Definitions
Data structures organize data; algorithms operate on it.

## Code Snippet
```javascript
function binarySearch(arr, value) {
  let low = 0;
  let high = arr.length - 1;
  while (low <= high) {
    const mid = Math.floor((low + high) / 2);
    if (arr[mid] === value) return mid;
    if (arr[mid] < value) low = mid + 1;
    else high = mid - 1;
  }
  return -1;
}
```

## Advantages
- Helps solve complex problems efficiently
- Essential for technical interviews

## Disadvantages
- Can be challenging to implement optimally
- Requires practice to master

## Interview Questions
Q: What is the difference between a stack and a queue?
A: A stack is LIFO; a queue is FIFO.

Q: When use binary search?
A: When the array is sorted.

---

# 17. Regular Expressions

* Regex Syntax
* Character Sets
* Quantifiers
* Groups
* Validation Patterns

## Definitions
Regular expressions are patterns for text matching.

## Code Snippet
```javascript
const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
console.log(emailRegex.test('user@example.com'));
```

## Advantages
- Powerful text validation and search
- Useful for parsing structured text

## Disadvantages
- Hard to read for complex patterns
- Easy to produce incorrect matches

## Interview Questions
Q: What does `\d+` match?
A: One or more digits.

Q: How do you capture groups in regex?
A: Use parentheses `(...)`.

---

# 18. APIs & Networking

* REST APIs
* HTTP Methods
* Status Codes
* Fetch API
* Axios
* Authentication
* JWT
* CORS

## Definitions
APIs enable communication between clients and servers over HTTP.

## Code Snippet
```javascript
fetch('/api/items', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ name: 'item' })
});
```

## Advantages
- Standardized communication model
- Enables frontend-backend separation

## Disadvantages
- Need to handle network errors and latency
- Requires secure auth and CORS handling

## Interview Questions
Q: What is CORS?
A: Cross-Origin Resource Sharing controls access between different origins.

Q: What is JWT used for?
A: Stateless authentication tokens.

---

# 19. Node.js

* Introduction to Node.js
* Node Architecture
* npm
* Package.json
* Modules
* File System
* Streams
* Events
* HTTP Module
* Express.js
* Environment Variables

## Definitions
Node.js is a JavaScript runtime built on Chrome's V8 engine for backend development.

## Code Snippet
```javascript
const http = require('http');
const server = http.createServer((req, res) => {
  res.end('Hello from Node');
});
server.listen(3000);
```

## Advantages
- Enables full-stack JavaScript development
- Large package ecosystem

## Disadvantages
- Single-threaded model requires async code patterns
- Callback-heavy legacy code can be hard to maintain

## Interview Questions
Q: What is npm?
A: Node Package Manager for installing packages and managing dependencies.

Q: How do you read environment variables in Node?
A: Use `process.env.VARIABLE_NAME`.

---

# 20. Database with JavaScript

* MongoDB
* Mongoose
* SQL Basics
* PostgreSQL/MySQL
* ORM/ODM

## Definitions
JavaScript can interact with NoSQL and SQL databases using drivers or ORMs.

## Code Snippet
```javascript
const mongoose = require('mongoose');
const UserSchema = new mongoose.Schema({ name: String });
```

## Advantages
- Fast prototyping with JavaScript across stack
- Rich database drivers and ORMs

## Disadvantages
- Need to choose between SQL and NoSQL models carefully
- Database logic can become complex

## Interview Questions
Q: What is an ORM?
A: Object-Relational Mapping maps database rows to objects.

Q: When use MongoDB vs PostgreSQL?
A: Use MongoDB for flexible document data; PostgreSQL for relational integrity and complex queries.

---

# 21. Testing

* Unit Testing
* Integration Testing
* Jest
* Mocha
* Chai
* Cypress

## Definitions
Testing verifies application behavior and prevents regressions.

## Code Snippet
```javascript
const add = (a, b) => a + b;
test('adds values', () => {
  expect(add(1, 2)).toBe(3);
});
```

## Advantages
- Increases confidence in code changes
- Prevents bugs and regression

## Disadvantages
- Tests require maintenance
- Poorly written tests can slow development

## Interview Questions
Q: What is the difference between unit and integration testing?
A: Unit tests cover small isolated components; integration tests cover multiple components working together.

Q: What is a snapshot test?
A: A test that compares rendered output to a stored snapshot.

---

# 22. Build Tools & Tooling

* npm/yarn/pnpm
* Webpack
* Vite
* Babel
* ESLint
* Prettier

## Definitions
Tooling bundles code, transpiles syntax, enforces style, and manages dependencies.

## Code Snippet
```json
{
  "scripts": {
    "build": "vite build",
    "lint": "eslint src"
  }
}
```

## Advantages
- Enables modern syntax and optimizations
- Improves code quality and consistency

## Disadvantages
- Toolchains can become complex
- Configuration requires learning

## Interview Questions
Q: What is Babel used for?
A: Transpiling modern JavaScript into compatible syntax for older environments.

Q: What is tree shaking?
A: Removing unused code from bundles.

---

# 23. TypeScript

* Type System
* Interfaces
* Types
* Generics
* Enums
* Utility Types
* Type Inference

## Definitions
TypeScript is a superset of JavaScript that adds static typing.

## Code Snippet
```typescript
interface User {
  id: number;
  name: string;
}
function greet(user: User): string {
  return `Hello, ${user.name}`;
}
```

## Advantages
- Catches many errors at compile time
- Improves IDE tooling and documentation

## Disadvantages
- Adds build complexity
- Requires type definitions for some libraries

## Interview Questions
Q: What is an interface?
A: A shape definition for objects in TypeScript.

Q: What is type inference?
A: The compiler automatically determines a variable's type.

---

# 24. Frontend Frameworks

## React
* JSX
* Components
* Props
* State
* Hooks
* Context API
* Routing
* Redux/Zustand

## Vue
* Vue Basics
* Composition API

## Angular
* Components
* Services
* RxJS

## Interview Questions
Q: What is JSX?
A: A syntax extension that looks like HTML inside JavaScript.

Q: What is a React hook?
A: A function that lets components use state or lifecycle features.

---

# 25. Backend Development

* Express.js
* Authentication
* Middleware
* MVC Architecture
* REST API Development
* WebSockets
* Socket.io

## Definitions
Backend JavaScript powers APIs, auth, data access, and real-time communication.

## Code Snippet
```javascript
const express = require('express');
const app = express();
app.get('/api', (req, res) => res.json({ message: 'ok' }));
app.listen(3000);
```

## Advantages
- Unified JavaScript stack
- Fast API development

## Disadvantages
- Single-threaded CPU-bound tasks need special handling
- Requires careful async design

## Interview Questions
Q: What is Express.js?
A: A minimal web framework for Node.js.

Q: What is middleware?
A: Functions that process requests before reaching route handlers.

---

# 26. Security

* XSS
* CSRF
* CORS
* HTTPS
* Authentication
* Authorization
* Password Hashing

## Definitions
Security topics protect applications from attacks and unauthorized access.

## Code Snippet
```javascript
res.setHeader('Content-Security-Policy', "default-src 'self'");
```

## Advantages
- Protects users and data
- Prevents common exploits

## Disadvantages
- Requires careful implementation
- Security is a moving target

## Interview Questions
Q: What is XSS?
A: Cross-site scripting, where malicious scripts run in the browser.

Q: What is CSRF?
A: Forged requests made on behalf of an authenticated user.

---

# 27. Performance Optimization

* Debouncing
* Throttling
* Lazy Loading
* Code Splitting
* Caching
* Memory Optimization

## Definitions
Optimization improves responsiveness, load time, and resource usage.

## Code Snippet
```javascript
function debounce(fn, delay) {
  let timeout;
  return (...args) => {
    clearTimeout(timeout);
    timeout = setTimeout(() => fn(...args), delay);
  };
}
```

## Advantages
- Better user experience
- Reduced CPU and network usage

## Disadvantages
- Premature optimization can be wasted effort
- More complex code paths

## Interview Questions
Q: What is debouncing useful for?
A: Reducing the rate of expensive calls such as resize or input events.

Q: What is lazy loading?
A: Loading resources only when they are needed.

---

# 28. Design Patterns

* Module Pattern
* Singleton
* Factory
* Observer
* MVC
* Pub/Sub

## Definitions
Design patterns solve common architecture problems with reusable structures.

## Code Snippet
```javascript
const PubSub = (() => {
  const events = {};
  return {
    subscribe(event, fn) {
      events[event] = events[event] || [];
      events[event].push(fn);
    },
    publish(event, data) {
      (events[event] || []).forEach(fn => fn(data));
    }
  };
})();
```

## Advantages
- Standardizes solutions
- Improves maintainability

## Disadvantages
- Can add unnecessary complexity if overused
- Some patterns are not idiomatic in JS

## Interview Questions
Q: What is the factory pattern?
A: A function that creates objects without exposing construction logic.

Q: What is the observer pattern?
A: Subscribers react to published events.

---

# 29. DevOps & Deployment

* Git & GitHub
* CI/CD
* Docker Basics
* Hosting
* Netlify
* Vercel
* AWS Basics

## Definitions
DevOps topics connect code to production through version control, automation, and cloud deployment.

## Code Snippet
```yaml
name: Deploy
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm install
      - run: npm run build
```

## Advantages
- Faster, repeatable deployment pipelines
- Better team collaboration

## Disadvantages
- Pipeline complexity can grow quickly
- Requires configuration and maintenance

### Interview Questions
Q: What is CI/CD?
A: Continuous Integration and Continuous Deployment.

Q: Why use Docker?
A: To package applications and dependencies in portable containers.

---

# 30. Advanced Topics

* Event Loop Deep Dive
* Web Workers
* Service Workers
* Progressive Web Apps
* SSR
* CSR
* Hydration
* WebSockets
* GraphQL
* Micro Frontends

## Definitions
Advanced topics cover modern performance, offline, rendering, and architectural techniques.

## Code Snippet
```javascript
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/sw.js');
}
```

## Advantages
- Enables scalable and modern web apps
- Improves performance and resilience

## Disadvantages
- More complex architecture
- Can require significant tooling and infrastructure

## Interview Questions
Q: What is SSR?
A: Server-Side Rendering.

Q: What is a Progressive Web App?
A: A web app that behaves like a native app with offline and installable capabilities.

---

# 31. Interview Preparation

* JavaScript Coding Questions
* Output-Based Questions
* Closures & Hoisting
* Event Loop Questions
* Polyfills
* System Design Basics
* DSA Practice

## Definitions
Interview prep focuses on common JavaScript questions, problem solving, and system thinking.

## Advantages
- Helps pass technical interviews
- Reveals gaps in understanding

## Disadvantages
- Can feel overwhelming without structure
- Requires consistent practice

## Interview Questions
Q: How do you prepare for JavaScript interviews?
A: Practice coding problems, understand language fundamentals, and review runtime behavior.

Q: What is a common JavaScript interview topic?
A: Closures, hoisting, event loop, and async behavior.

---

# Suggested Learning Order
1. Fundamentals
2. Control Flow
3. Functions
4. Arrays & Objects
5. DOM
6. ES6+
7. Async JavaScript
8. Advanced Concepts
9. APIs
10. Node.js
11. Database
12. Framework (React recommended)
13. TypeScript
14. Testing
15. Performance & Security
16. Deployment

---

# Recommended Projects

## Beginner
* Calculator
* To-Do App
* Weather App
* Counter App

## Intermediate
* Notes App
* Movie Search App
* Expense Tracker
* Quiz App

## Advanced
* E-commerce App
* Chat Application
* Video Streaming App
* Full Stack Social Media App

---

If you want, I can also provide:
* **JavaScript roadmap with timelines**
* **JavaScript interview topics**
* **JavaScript projects list**
* **JavaScript coding questions**
* **Frontend + Backend complete roadmap**
* **30/60/90 day JavaScript study plan**
* **JavaScript topics with resources and links**


