# Python Complete Topics List (Beginner to Advanced)

This guide covers Python fundamentals, advanced concepts, backend development with Flask and FastAPI, and best practices for learning and building production-ready applications.

---

# 1. Python Introduction

## What is Python?
Definition: Python is a high-level, interpreted programming language known for readability, simplicity, and versatility.

Code Snippet:
```py
print('Hello, Python!')
```

Advantages:
- Easy to read and write
- Large standard library
- Strong ecosystem for web, data, and automation

Disadvantages:
- Slower than compiled languages for CPU-heavy tasks
- Global Interpreter Lock (GIL) limits true multi-threading

Real-world Use Case: Automation scripts, web backends, and data science workflows.

## Features of Python
- Dynamic typing
- Garbage collection
- Cross-platform support
- Rich standard library
- Multiple programming paradigms

Use Case: Rapid prototyping and scripting.

## Installing Python
Definition: Install Python from python.org or package managers.

Example:
```bash
python --version
```

## Python Interpreter
Definition: Interactive shell for running Python commands.

Usage:
```bash
python
>>> 2 + 2
```

## Running Python Scripts
Example:
```bash
python script.py
```

## Python Syntax
Definition: Python uses indentation and simple syntax to express logic.

Example:
```py
if True:
    print('Yes')
```

## Comments
Definition: Use `#` for single-line comments.

Example:
```py
# This is a comment
```

## Indentation
Definition: Python uses indentation to define blocks.

Example:
```py
for i in range(3):
    print(i)
```

---

# 2. Variables & Data Types

## Variables
Definition: Containers for storing values.

Example:
```py
name = 'Alice'
age = 30
```

## Naming Conventions
- Use `snake_case`
- Avoid reserved keywords

## Data Types
### int
Definition: Integer numbers.

### float
Definition: Floating-point numbers.

### str
Definition: Text strings.

### bool
Definition: True/False values.

### complex
Definition: Complex numbers with real and imaginary parts.

Example:
```py
x = 10
y = 3.14
name = 'Python'
is_active = True
z = 2 + 3j
```

## Type Casting
Example:
```py
x = int('5')
pi = float('3.14')
```

## Dynamic Typing
Definition: Variables can hold any type and types can change at runtime.

Example:
```py
value = 10
value = 'ten'
```

---

# 3. Operators

## Arithmetic Operators
Example:
```py
result = 5 + 3
```

## Comparison Operators
Example:
```py
is_equal = 5 == 5
```

## Logical Operators
Example:
```py
status = True and False
```

## Assignment Operators
Example:
```py
count += 1
```

## Bitwise Operators
Example:
```py
bitwise = 5 & 3
```

## Membership Operators
Example:
```py
exists = 'a' in 'abc'
```

## Identity Operators
Example:
```py
same = a is b
```

---

# 4. Input & Output

## `input()`
Example:
```py
name = input('Enter your name: ')
```

## `print()`
Example:
```py
print('Hello,', name)
```

## Formatting Output
Example:
```py
print('Value:', value)
```

## f-Strings
Example:
```py
print(f'Hello, {name}!')
```

---

# 5. Control Flow

## `if`
Example:
```py
if x > 0:
    print('Positive')
```

## `elif`
Example:
```py
if x > 0:
    print('Positive')
elif x == 0:
    print('Zero')
```

## `else`
Example:
```py
else:
    print('Negative')
```

## Nested Conditions
Example:
```py
if x > 0:
    if y > 0:
        print('Both positive')
```

## `for` Loop
Example:
```py
for item in [1, 2, 3]:
    print(item)
```

## `while` Loop
Example:
```py
while count < 5:
    count += 1
```

## `break`
Example:
```py
for item in items:
    if item == target:
        break
```

## `continue`
Example:
```py
for item in items:
    if item is None:
        continue
```

## `pass`
Example:
```py
if condition:
    pass
```

---

# 6. Functions

## Defining Functions
Example:
```py
def greet(name):
    return f'Hello, {name}'
```

## Parameters & Arguments
Example:
```py
def add(a, b):
    return a + b
```

## Return Values
Example:
```py
result = add(2, 3)
```

## Default Arguments
Example:
```py
def greet(name='Guest'):
    print(name)
```

## Keyword Arguments
Example:
```py
def greet(name, age=None):
    print(name, age)
```

## `*args`
Example:
```py
def add(*nums):
    return sum(nums)
```

## `**kwargs`
Example:
```py
def build_profile(**info):
    return info
```

## Lambda Functions
Example:
```py
square = lambda x: x * x
```

## Recursion
Example:
```py
def factorial(n):
    return 1 if n <= 1 else n * factorial(n-1)
```

## Scope
Definition: Local vs global variables.

Example:
```py
global_var = 10

def func():
    local_var = 5
```

## Closures
Example:
```py
def outer(msg):
    def inner():
        print(msg)
    return inner
```

## Decorators
Example:
```py
def log(fn):
    def wrapper(*args, **kwargs):
        print('Calling', fn.__name__)
        return fn(*args, **kwargs)
    return wrapper
```

---

# 7. Data Structures

## Lists
### List Methods
Example:
```py
items = [1, 2, 3]
items.append(4)
```

### List Comprehensions
Example:
```py
squares = [x * x for x in range(5)]
```

## Tuples
### Tuple Operations
Example:
```py
point = (1, 2)
```

## Sets
### Set Methods
Example:
```py
unique = set([1, 2, 2])
```

## Dictionaries
### Dictionary Methods
Example:
```py
user = {'name': 'Alice', 'age': 30}
```

### Nested Dictionaries
Example:
```py
data = {'user': {'name': 'Alice'}}
```

---

# 8. Strings

## String Methods
Example:
```py
text = 'hello'.upper()
```

## String Formatting
Example:
```py
message = 'Hello {}'.format(name)
```

## Slicing
Example:
```py
slice = text[0:3]
```

## Regular Expressions
Example:
```py
import re
match = re.search(r'\d+', 'abc123')
```

---

# 9. Exception Handling

## `try`
Example:
```py
try:
    x = 1 / 0
```

## `except`
Example:
```py
except ZeroDivisionError:
    print('Cannot divide by zero')
```

## `finally`
Example:
```py
finally:
    print('Cleanup')
```

## `raise`
Example:
```py
raise ValueError('Invalid')
```

## Custom Exceptions
Example:
```py
class AppError(Exception):
    pass
```

---

# 10. File Handling

## Reading Files
Example:
```py
with open('data.txt', 'r') as file:
    content = file.read()
```

## Writing Files
Example:
```py
with open('output.txt', 'w') as file:
    file.write('Hello')
```

## Appending Files
Example:
```py
with open('log.txt', 'a') as file:
    file.write('New line\n')
```

## CSV Files
Example:
```py
import csv
with open('data.csv') as file:
    reader = csv.reader(file)
```

## JSON Files
Example:
```py
import json
with open('data.json') as file:
    data = json.load(file)
```

## Context Managers (`with`)
Definition: Ensure resources are cleaned up automatically.

Use Case: Safely reading and writing files.

---

# 11. Object-Oriented Programming (OOP)

## Classes & Objects
Example:
```py
class User:
    pass
```

## Constructors
Example:
```py
class User:
    def __init__(self, name):
        self.name = name
```

## Instance Variables
Example:
```py
user = User('Alice')
```

## Class Variables
Example:
```py
class Counter:
    count = 0
```

## Methods
Example:
```py
def greet(self):
    print(self.name)
```

## Inheritance
Example:
```py
class Admin(User):
    pass
```

## Polymorphism
Example:
```py
class Shape:
    def area(self):
        pass
```

## Encapsulation
Definition: Keep data private with conventions.

## Abstraction
Definition: Hide implementation details behind interfaces.

## Magic Methods
Example:
```py
def __str__(self):
    return self.name
```

---

# 12. Modules & Packages

## Importing Modules
Example:
```py
import math
```

## Creating Modules
Definition: Use `.py` files as modules.

## Python Packages
Definition: Use folders with `__init__.py`.

## pip
Definition: Install packages from PyPI.

Example:
```bash
pip install requests
```

## Virtual Environments
Definition: Isolate project dependencies.

Example:
```bash
python -m venv venv
```

Use Case: Prevent dependency conflicts across projects.

---

# 13. Iterators & Generators

## Iterators
Definition: Objects with `__iter__()` and `__next__()`.

## `iter()`
Example:
```py
it = iter([1, 2, 3])
```

## `next()`
Example:
```py
print(next(it))
```

## Generators
Definition: Functions that yield values lazily.

Example:
```py
def count_up_to(n):
    for i in range(n):
        yield i
```

## `yield`
Use Case: Memory-efficient iteration over large datasets.

---

# 14. Advanced Python

## Decorators
Definition: Wrap functions to extend behavior.

## Context Managers
Definition: Create resource-safe blocks with `__enter__` and `__exit__`.

## Multithreading
Definition: Run threads concurrently for I/O-bound work.

## Multiprocessing
Definition: Use separate processes for CPU-bound tasks.

## Asyncio
Definition: Asynchronous programming with coroutines.

## GIL
Definition: Global Interpreter Lock that limits native thread parallelism.

## Metaclasses
Definition: Customize class creation behavior.

Execution flow concept:
Coroutine → Event Loop → Await

Use Case: Web scrapers, async APIs, and concurrent workflows.

---

# 15. Functional Programming

## Map
Example:
```py
squares = list(map(lambda x: x*x, range(5)))
```

## Filter
Example:
```py
evens = list(filter(lambda x: x % 2 == 0, nums))
```

## Reduce
Example:
```py
from functools import reduce
product = reduce(lambda a, b: a*b, nums)
```

## Lambda
Definition: Anonymous inline functions.

## Functional Concepts
Definition: Use immutability and pure functions.

Use Case: Data transformation pipelines.

---

# 16. Python Standard Library

## `os`
Definition: Interact with the operating system.

## `sys`
Definition: Access runtime environment and arguments.

## `math`
Definition: Math functions and constants.

## `datetime`
Definition: Work with dates and times.

## `random`
Definition: Generate random values.

## `collections`
Definition: Specialized container datatypes.

## `itertools`
Definition: Iterator building tools.

## `functools`
Definition: Higher-order functions and decorators.

Use Case: Build tools, scripts, and apps without external dependencies.

---

# 17. Database Connectivity

## SQLite
Definition: Lightweight file-based relational database.

## PostgreSQL
Definition: Powerful open-source relational database.

## MySQL
Definition: Widely used SQL database.

## ORM Basics
Definition: Map database tables to Python objects.

## SQLAlchemy
Definition: Popular ORM for Python.

Use Case: Persist application data for web apps and analytics.

---

# 18. APIs & Networking

## HTTP Requests
Definition: Communicate with web services.

## `requests` Library
Example:
```py
import requests
response = requests.get('https://api.example.com')
```

## REST APIs
Definition: Build and consume RESTful services.

## JSON Handling
Example:
```py
import json
obj = json.loads(response.text)
```

## Web Scraping
Definition: Extract data from websites.

Use Case: API clients, automation tools, and data collection scripts.

---

# 19. Testing

## unittest
Definition: Built-in testing framework.

## pytest
Definition: Powerful third-party testing framework.

## Mocking
Definition: Replace dependencies during tests.

## Test Coverage
Definition: Measure how much code is tested.

Use Case: Ensure correctness and prevent regressions.

---

# 20. Python Best Practices

## PEP8
Definition: Python style guide.

## Clean Code
Definition: Write readable, maintainable code.

## Type Hinting
Example:
```py
def greet(name: str) -> str:
    return f'Hello, {name}'
```

## Logging
Definition: Use the `logging` module instead of print.

## Code Optimization
Definition: Improve performance with efficient algorithms.

Use Case: Professional software development and maintainable projects.

---

# 21. Data Science Basics

## NumPy
Definition: Numerical computing library.

## Pandas
Definition: Data manipulation and analysis.

## Matplotlib
Definition: Plotting library.

## Seaborn
Definition: Statistical data visualization.

Use Case: Data analysis, reporting, and research.

---

# 22. Machine Learning Basics

## scikit-learn
Definition: Machine learning library for Python.

## TensorFlow Basics
Definition: Deep learning framework.

## PyTorch Basics
Definition: Deep learning library with dynamic computation graphs.

Use Case: Building models for prediction, classification, and NLP.

---

# 23. Deployment & DevOps

## Docker
Definition: Containerize Python applications.

## CI/CD
Definition: Automate testing and deployment.

## Linux Basics
Definition: Use Linux commands and environments.

## Git & GitHub
Definition: Version control and collaboration.

Use Case: Production deployment and team workflows.

---

# 24. Python Interview Topics

## OOP Concepts
Definition: Classes, inheritance, and polymorphism.

## Decorators
Definition: Function wrappers and higher-order behavior.

## Generators
Definition: Lazy iteration with `yield`.

## GIL
Definition: Understand threading limitations in CPython.

## Asyncio
Definition: Async programming model.

## Memory Management
Definition: Garbage collection and reference counting.

Use Case: Prepare for Python developer interviews.

---

# Flask Complete Topics List

---

# 1. Flask Introduction

## What is Flask?
Definition: Flask is a lightweight Python web framework for quick API and web app development.

## Flask vs Django
Definition: Flask is minimal and flexible, Django is full-stack and opinionated.

## Installing Flask
Example:
```bash
pip install flask
```

## Flask Project Structure
Example:
```text
app.py
templates/
static/
```

Use Case: Small web apps and REST APIs.

---

# 2. Flask Basics

## Creating Flask App
Example:
```py
from flask import Flask
app = Flask(__name__)
```

## Routes
Example:
```py
@app.route('/')
def home():
    return 'Hello, Flask'
```

## HTTP Methods
Definition: GET, POST, PUT, DELETE.

## URL Parameters
Example:
```py
@app.route('/user/<name>')
def user(name):
    return name
```

## Request & Response
Example:
```py
from flask import request, jsonify
```

Basic request flow:
Client Request → Flask Route → Response

Use Case: REST APIs and simple web apps.

---

# 3. Templates

## Jinja2 Templates
Definition: Template engine for rendering HTML.

## Template Inheritance
Definition: Base templates with blocks.

## Loops & Conditions
Example:
```html
{% for item in items %}
  <li>{{ item }}</li>
{% endfor %}
```

## Static Files
Definition: Serve CSS, JS, and images from `static/`.

Use Case: Dynamic HTML pages with templates.

---

# 4. Forms

## WTForms
Definition: Form-handling library for Flask.

## Form Validation
Definition: Validate input fields.

## CSRF Protection
Definition: Prevent cross-site request forgery.

Use Case: Login, signup, and contact forms.

---

# 5. Database Integration

## SQLAlchemy
Definition: ORM for relational databases.

## Flask-SQLAlchemy
Definition: Flask extension for SQLAlchemy.

## Models
Example:
```py
class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
```

## CRUD Operations
Definition: Create, read, update, delete data.

## Migrations
Definition: Database schema versioning with Flask-Migrate.

Use Case: Persistent web apps and admin panels.

---

# 6. Authentication

## Flask-Login
Definition: User session management extension.

## JWT Authentication
Definition: Token-based auth for APIs.

## Sessions
Definition: Server-side login state.

## Password Hashing
Definition: Securely store passwords.

Use Case: Secure user logins and dashboards.

---

# 7. REST API Development

## Flask RESTful
Definition: Extension for building REST APIs.

## JSON Responses
Definition: Return JSON payloads.

## CRUD APIs
Definition: Build standard REST endpoints.

## API Validation
Definition: Validate request data.

Use Case: Backend APIs for frontend apps.

---

# 8. Middleware & Extensions

## Flask Extensions
Examples:
- Flask-Migrate
- Flask-Mail
- Flask-Caching

## Middleware Concepts
Definition: Process requests and responses globally.

## Blueprints
Definition: Modularize large Flask apps.

Use Case: Organize routes and reuse components.

---

# 9. Error Handling

## Custom Errors
Definition: Define application-specific exceptions.

## Exception Handling
Definition: Catch and respond to errors.

## Logging
Definition: Record errors and request context.

Use Case: Improve reliability and debugging.

---

# 10. Security

## XSS Protection
Definition: Escape output to prevent script injection.

## CSRF Protection
Definition: Protect form submissions.

## Secure Cookies
Definition: Use secure and httpOnly flags.

Use Case: Secure web applications.

---

# 11. Testing Flask Apps

## pytest
Definition: Popular Python testing framework.

## Flask Testing Client
Definition: Simulate requests against Flask apps.

## API Testing
Definition: Test endpoints and responses.

Use Case: Ensure route behavior and regression safety.

---

# 12. Deployment

## Gunicorn
Definition: Python WSGI server for production.

## Nginx
Definition: Reverse proxy and static asset server.

## Docker
Definition: Containerize Flask apps.

## AWS/Render/Railway
Definition: Common hosting platforms.

Use Case: Deploy Flask apps to production.

---

# 13. Advanced Flask

## Flask Signals
Definition: Events for app lifecycle hooks.

## Caching
Definition: Speed up responses with caching.

## WebSockets
Definition: Real-time communication with Flask-SocketIO.

## Async Flask
Definition: Async route handlers in newer Flask versions.

Use Case: Real-time and high-performance Flask applications.

---

# FastAPI Complete Topics List

---

# 1. FastAPI Introduction

## What is FastAPI?
Definition: FastAPI is a modern Python web framework for building fast APIs with automatic docs and async support.

## Why FastAPI?
Advantages:
- Fast performance with ASGI
- Automatic OpenAPI documentation
- Built-in validation with Pydantic

## FastAPI vs Flask vs Django
Definition: FastAPI is async-first and API-focused, Flask is lightweight, Django is full-stack.

## ASGI
Definition: Asynchronous Server Gateway Interface for async Python frameworks.

## Installing FastAPI
Example:
```bash
pip install fastapi uvicorn
```

Use Case: Backend APIs requiring speed, validation, and docs.

---

# 2. FastAPI Basics

## Creating FastAPI App
Example:
```py
from fastapi import FastAPI
app = FastAPI()
```

## Routes
Example:
```py
@app.get('/')
def read_root():
    return {'message': 'Hello'}
```

## Path Parameters
Example:
```py
@app.get('/items/{item_id}')
def read_item(item_id: int):
    return {'item_id': item_id}
```

## Query Parameters
Example:
```py
@app.get('/search')
def search(q: str = None):
    return {'q': q}
```

## Request Body
Example:
```py
from pydantic import BaseModel
class Item(BaseModel):
    name: str

@app.post('/items/')
def create_item(item: Item):
    return item
```

FastAPI flow:
Request → Validation → Route Handler → JSON Response

Use Case: API-first applications with strong typing.

---

# 3. Pydantic

## Data Validation
Definition: Validate request and response data.

## Models
Example:
```py
class User(BaseModel):
    name: str
    age: int
```

## Nested Models
Example:
```py
class Address(BaseModel):
    city: str
```

## Type Validation
Definition: Ensure correct data types.

## Serialization
Definition: Convert Python objects to JSON-compatible data.

Use Case: Secure and reliable API payloads.

---

# 4. Request Handling

## GET
Example:
```py
@app.get('/items/')
def read_items():
    return []
```

## POST
Example:
```py
@app.post('/items/')
def create_item(item: Item):
    return item
```

## PUT
Example:
```py
@app.put('/items/{item_id}')
def update_item(item_id: int, item: Item):
    return item
```

## PATCH
Definition: Partial updates.

## DELETE
Example:
```py
@app.delete('/items/{item_id}')
def delete_item(item_id: int):
    return {'deleted': item_id}
```

## Headers
Example:
```py
from fastapi import Header
```

## Cookies
Example:
```py
from fastapi import Cookie
```

## Forms
Example:
```py
from fastapi import Form
```

## File Uploads
Example:
```py
from fastapi import UploadFile
```

Use Case: Build complete REST APIs and file endpoints.

---

# 5. Response Handling

## JSON Responses
Definition: Return Python dicts and Pydantic models.

## Custom Responses
Example:
```py
from fastapi.responses import HTMLResponse
```

## Status Codes
Example:
```py
return JSONResponse(status_code=201, content={'ok': True})
```

## Response Models
Definition: Define response schemas for docs and validation.

Use Case: Consistent API contracts and documentation.

---

# 6. Dependency Injection

## Dependencies
Definition: Reusable functions for shared logic.

## Reusable Components
Example:
```py
def get_db():
    return db
```

## Security Dependencies
Definition: Use dependencies for auth and permissions.

Use Case: Shared database sessions and auth checks.

---

# 7. Async Programming

## Async Endpoints
Example:
```py
@app.get('/async')
async def read_async():
    return {'status': 'ok'}
```

## Await
Definition: Pause execution until a coroutine completes.

## Async Database Calls
Definition: Use async DB clients for non-blocking I/O.

## Concurrency
Definition: Handle multiple requests efficiently.

Use Case: High-performance APIs and streaming endpoints.

---

# 8. Database Integration

## SQLAlchemy
Definition: ORM for SQL databases.

## SQLModel
Definition: Pydantic-backed ORM for FastAPI.

## PostgreSQL
Definition: Relational database for production apps.

## MongoDB
Definition: NoSQL database for flexible schemas.

## Alembic
Definition: Database migrations for SQLAlchemy.

Use Case: Persistent storage and API data layers.

---

# 9. Authentication & Authorization

## OAuth2
Definition: Standard protocol for authorization.

## JWT
Definition: Token-based authentication.

## Password Hashing
Definition: Securely store credentials.

## Role-Based Access
Definition: Restrict actions by user role.

Use Case: Secure REST APIs and user management.

---

# 10. Middleware

## Custom Middleware
Definition: Intercept requests and responses.

## CORS Middleware
Definition: Allow cross-origin access safely.

## Logging Middleware
Definition: Track request details and performance.

Use Case: Cross-cutting concerns in API apps.

---

# 11. Background Tasks

## Background Tasks
Definition: Run work after response returns.

## Task Queues
Definition: Process jobs asynchronously.

## Celery Integration
Definition: Use Celery for distributed task processing.

Use Case: Email sending, reports, and async workflows.

---

# 12. WebSockets

## Real-Time Communication
Definition: Persistent connection for real-time data.

## Chat Applications
Definition: Build live chat with WebSockets.

Use Case: Real-time dashboards and collaboration tools.

---

# 13. Testing

## pytest
Definition: Test runner for Python.

## TestClient
Definition: Simulate FastAPI requests.

## Async Testing
Definition: Test async endpoints.

Use Case: Ensure API reliability and behavior.

---

# 14. Security

## CORS
Definition: Control cross-origin requests.

## OAuth2
Definition: Secure authorization workflows.

## Rate Limiting
Definition: Prevent abuse by limiting requests.

## Secure Headers
Definition: Set security policies on responses.

Use Case: Harden APIs and services.

---

# 15. Performance Optimization

## Async Optimization
Definition: Use async I/O for fast concurrency.

## Caching
Definition: Store results to reduce load.

## Connection Pooling
Definition: Reuse database connections.

Use Case: Scalable API performance.

---

# 16. Deployment

## Uvicorn
Definition: ASGI server for FastAPI.

## Gunicorn
Definition: Production WSGI/ASGI server.

## Docker
Definition: Containerize FastAPI apps.

## Kubernetes
Definition: Container orchestration for scaling.

## Nginx
Definition: Reverse proxy and load balancer.

Use Case: Deploy FastAPI services in production.

---

# 17. Advanced FastAPI

## Lifespan Events
Definition: App startup and shutdown hooks.

## Dependency Overrides
Definition: Replace dependencies in tests.

## Streaming Responses
Definition: Stream data to clients.

## GraphQL Integration
Definition: Add GraphQL support to FastAPI.

Use Case: Advanced API features and testing.

---

# 18. FastAPI Interview Topics

## ASGI vs WSGI
Definition: Async vs sync interface for Python web servers.

## Async/Await
Definition: Non-blocking function execution.

## Dependency Injection
Definition: Reusable and testable logic.

## Pydantic
Definition: Data validation and serialization.

## Background Tasks
Definition: Deferred work after responses.

Use Case: Interview preparation for API and backend roles.

---

# Recommended Learning Order

## Python
1. Basics
2. Functions
3. OOP
4. File Handling
5. Modules
6. Advanced Python
7. APIs & Databases

## Flask
1. Flask Basics
2. Templates
3. Database
4. Authentication
5. REST APIs
6. Deployment

## FastAPI
1. FastAPI Basics
2. Pydantic
3. Async Programming
4. Database Integration
5. Authentication
6. Deployment

---

# Best Backend Stack with Python

## Backend Framework
- FastAPI / Flask

## Database
- PostgreSQL / MongoDB

## ORM
- SQLAlchemy

## Deployment
- Docker
- Nginx
- AWS

## Authentication
- JWT / OAuth2

If you want, I can also provide:
- Python roadmap with timeline
- Flask project ideas
- FastAPI production architecture
- Python interview questions
- FastAPI vs Flask comparison
- Backend developer roadmap
- Full stack Python roadmap
- Advanced Python internals topics
