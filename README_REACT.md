# React Complete Topics List (Beginner to Advanced)

## Who this guide is for
This guide is for beginners learning React and modern user interface development.
It explains components, JSX, state, and common React patterns clearly.

## What you'll learn
- What React is and why it matters
- How components and props work
- How to build interactive user interfaces


# 1. React Introduction

## What is React?
Definition: React is a JavaScript library for building user interfaces using composable components and declarative rendering.

Code Snippet:
```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';

function App() {
  return <h1>Welcome to React</h1>;
}

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
```

Advantages:
- Fast UI updates with Virtual DOM
- Strong component reuse
- Rich ecosystem

Disadvantages:
- Learning curve for advanced patterns
- Requires build tooling for production

Real-time Use Case: Building interactive dashboards, admin panels, or e-commerce storefront UIs.

## Why React?
Definition: React focuses on building component-driven UIs and managing component state efficiently.

Advantages:
- Declarative UI code
- Reusable components
- Broad community support

Disadvantages:
- Not an all-in-one framework
- Requires additional libraries for routing, state, and data fetching

Use Case: Replacing legacy jQuery-based UIs with a maintainable component architecture.

## SPA vs MPA
Definition: SPA (Single Page Application) loads one HTML page and updates dynamically, while MPA (Multi Page Application) loads new pages from the server.

Advantages of SPA:
- Fast transitions
- Better UX for complex apps

Disadvantages of SPA:
- SEO challenges without SSR
- Larger initial bundle

Real-world Use Case: SPAs are used for email clients, SaaS apps, and CRM tools.

## Virtual DOM
Definition: A lightweight in-memory representation of the UI that React uses to efficiently update the real DOM.

Advantages:
- Minimizes costly DOM operations
- Improves rendering performance

Use Case: Updating filters, sorting, and animations in a data-heavy web app.

## Real DOM vs Virtual DOM
Definition: Real DOM updates modify the browser tree directly; Virtual DOM lets React compute the minimal changes first.

Advantages: Virtual DOM reduces unnecessary browser reflows and paints.

Disadvantages: Virtual DOM adds a small memory overhead.

Use Case: High-frequency UI updates such as typing, dragging, and real-time feeds.

## React Architecture
Definition: React apps are built with components, state, props, and hooks that interact through a tree structure.

Advantages:
- Modular architecture
- Easier testing

Use Case: Large apps use container/presentational patterns and feature folders.

## Declarative Programming
Definition: You describe what UI should look like, and React handles the rendering.

Example:
```jsx
const message = isLoggedIn ? 'Welcome' : 'Please sign in';
return <div>{message}</div>;
```

Advantages:
- Easier to reason about state changes
- Less imperative DOM manipulation

Use Case: Form state updates and conditional displays in dashboards.

## Component-Based Architecture
Definition: UI is broken down into reusable components with encapsulated logic and styling.

Advantages:
- Reuse across pages
- Simplifies maintenance

Use Case: Shared buttons, cards, tables, and modals in enterprise apps.

## Setting up React
Definition: Use tools like Vite, Create React App, or Next.js to scaffold React apps.

Example:
```bash
npm create vite@latest my-app -- --template react-ts
cd my-app
npm install
npm run dev
```

Use Case: Starting new projects with modern build tooling.

## React Project Structure
Definition: Organize source files into components, hooks, pages, services, and styles.

Example structure:
```text
src/
  components/
  hooks/
  pages/
  services/
  styles/
  App.tsx
  main.tsx
```

Advantages:
- Better scalability
- Easier collaboration

Use Case: Growing projects with multiple developers and feature teams.

---

# 2. JSX

## Introduction to JSX
Definition: JSX is a syntax extension that looks like HTML but compiles to JavaScript function calls.

Example:
```jsx
const element = <h1>Hello, React!</h1>;
```

Advantages:
- Familiar HTML-like syntax
- Works well with JavaScript

Disadvantages:
- Requires transpilation

Use Case: Defining visual components with inline expressions.

## JSX Syntax
Definition: JSX allows tags, components, attributes, and children in a declarative way.

Example:
```jsx
const card = (
  <div className="card">
    <h2>Title</h2>
    <p>Description</p>
  </div>
);
```

Use Case: Building reusable UI snippets like cards and lists.

## Expressions in JSX
Definition: You can embed JavaScript expressions inside `{}`.

Example:
```jsx
const name = 'Alex';
return <p>Hello, {name.toUpperCase()}</p>;
```

Use Case: Rendering dynamic values, formatted strings, and computed content.

## Embedding JavaScript in JSX
Definition: Include variables, ternaries, arrays, and function calls in JSX.

Example:
```jsx
return <div>{items.map(item => <span key={item.id}>{item.name}</span>)}</div>;
```

Use Case: Rendering search results, menus, and user data.

## JSX Attributes
Definition: Attributes like `className`, `htmlFor`, and `onClick` work inside JSX.

Example:
```jsx
<button className="btn" onClick={handleClick}>Click</button>
```

Use Case: Adding interactivity and styling to components.

## JSX Styling
Definition: Apply inline styles using objects or class names.

Example:
```jsx
<div style={{ color: 'blue', padding: '16px' }}>Styled</div>
```

Use Case: Quick one-off styling or dynamic styles based on props.

## Fragments
Definition: Use `<>...</>` to return multiple elements without extra DOM nodes.

Example:
```jsx
return (
  <>
    <Header />
    <Main />
  </>
);
```

Use Case: Grouping sibling elements inside components like lists or layouts.

## Conditional Rendering in JSX
Definition: Use ternaries, `&&`, and functions to render conditionally.

Example:
```jsx
{isLoading ? <Spinner /> : <Content />}
```

Use Case: Showing loaders, error messages, or permissions-based UI.

## Lists Rendering
Definition: Render arrays using `.map()`.

Example:
```jsx
<ul>{items.map(item => <li key={item.id}>{item.name}</li>)}</ul>
```

Use Case: Displaying products, comments, or notifications.

## Keys in React
Definition: Keys help React identify which list items changed.

Example:
```jsx
{todos.map(todo => <TodoItem key={todo.id} todo={todo} />)}
```

Advantages:
- Optimized list diffing
- Stable item identity

Disadvantages:
- Using array index can cause bugs on reorder

Use Case: Dynamic lists, tables, and feeds.

---

# 3. Components

## Functional Components
Definition: Functions that return JSX and can use hooks.

Example:
```jsx
function Greeting({ name }) {
  return <h1>Hello, {name}</h1>;
}
```

Advantages:
- Simpler syntax
- Easier to test

Use Case: Standard UI blocks like buttons and cards.

## Class Components
Definition: ES6 classes that extend `React.Component` and support lifecycle methods.

Example:
```jsx
class Timer extends React.Component {
  state = { seconds: 0 };

  componentDidMount() {
    this.interval = setInterval(() => this.setState({ seconds: this.state.seconds + 1 }), 1000);
  }

  componentWillUnmount() {
    clearInterval(this.interval);
  }

  render() {
    return <div>{this.state.seconds}s</div>;
  }
}
```

Advantages:
- Familiar lifecycle hooks

Disadvantages:
- More verbose than function components

Use Case: Legacy projects or patterns requiring error boundaries.

## Component Composition
Definition: Combine components by nesting and passing children.

Example:
```jsx
function Card({ title, children }) {
  return (
    <div className="card">
      <h3>{title}</h3>
      {children}
    </div>
  );
}
```

Use Case: Build layout primitives and design systems.

## Reusable Components
Definition: Create components with props to reuse across UI.

Example:
```jsx
function Button({ label, onClick }) {
  return <button onClick={onClick}>{label}</button>;
}
```

Advantages:
- Less duplicate code
- Easier maintenance

Use Case: Shared input fields, nav items, cards.

## Nested Components
Definition: Components inside components for hierarchical UI.

Example:
```jsx
function List({ items }) {
  return (
    <ul>{items.map(item => <Item key={item.id} item={item} />)}</ul>
  );
}
```

Use Case: Nested menus and dashboards.

## Component Communication
Definition: Data flows from parent to child via props and from child to parent via callbacks.

Example:
```jsx
function Parent() {
  const [value, setValue] = useState('');
  return <Child onChange={setValue} />;
}
```

Use Case: Forms, filters, and selection components.

## Smart vs Dumb Components
Definition: Smart components manage state and logic; dumb components focus on presentation.

Advantages:
- Clear separation of concerns

Use Case: Container components with API calls and presentational UI components.

## Pure Components
Definition: Components that avoid re-rendering when props/state do not change.

Example:
```jsx
const PureUser = React.memo(function User({ user }) {
  return <div>{user.name}</div>;
});
```

Advantages:
- Performance optimization

Use Case: Rendering large lists and repeated UI elements.

## Higher Order Components (HOC)
Definition: Functions that take a component and return a new enhanced component.

Example:
```jsx
function withLoading(Component) {
  return function WithLoading({ isLoading, ...props }) {
    if (isLoading) return <p>Loading...</p>;
    return <Component {...props} />;
  };
}
```

Advantages:
- Reuse shared logic

Disadvantages:
- Can make component trees harder to follow

Use Case: Adding authentication, logging, or data fetching to components.

---

# 4. Props

## What are Props?
Definition: Props are inputs passed into components to configure rendering.

Example:
```jsx
function Badge({ label }) {
  return <span>{label}</span>;
}
```

Advantages:
- Makes components configurable
- Supports reuse

Use Case: Passing metadata like titles, icons, and callbacks.

## Passing Props
Example:
```jsx
<Greeting name="Sara" />
```

## Default Props
Definition: Provide default values when props are missing.

Example:
```jsx
function Avatar({ size = 48 }) {
  return <img width={size} src="avatar.jpg" alt="avatar" />;
}
```

Use Case: Default theme values and optional settings.

## Props Destructuring
Definition: Extract prop values from the object in the parameter list.

Example:
```jsx
function Card({ title, subtitle }) {
  return <div>{title} - {subtitle}</div>;
}
```

Use Case: Cleaner component code and fewer `props.` references.

## Children Props
Definition: The `children` prop contains nested JSX passed to a component.

Example:
```jsx
function Layout({ children }) {
  return <div className="layout">{children}</div>;
}
```

Use Case: Wrapper components, modals, cards.

## Props Drilling
Definition: Passing props through intermediate components to reach deep children.

Disadvantages:
- Can become verbose and brittle

Use Case: Small apps or shallow component trees; avoid with context for deep trees.

## Props Validation
Definition: Validate props using TypeScript or `prop-types`.

Example (TypeScript):
```tsx
interface ButtonProps { label: string; onClick: () => void; }
function Button({ label, onClick }: ButtonProps) {
  return <button onClick={onClick}>{label}</button>;
}
```

Use Case: Prevent runtime bugs and document component contracts.

---

# 5. State

## Introduction to State
Definition: State is component-local data that changes over time and drives the UI.

Example:
```jsx
const [count, setCount] = useState(0);
```

Advantages:
- Dynamic interactivity
- Localized updates

Disadvantages:
- Can lead to complex state logic if unmanaged

Use Case: Form inputs, toggles, and counters.

## useState
Definition: Hook to add local state in a functional component.

Example:
```jsx
const [theme, setTheme] = useState('light');
```

## Updating State
Definition: Use setter functions to change state.

Example:
```jsx
setCount(count + 1);
```

## Functional Updates
Definition: Use a function when state depends on previous state.

Example:
```jsx
setCount(prev => prev + 1);
```

Advantages:
- Avoids stale values in asynchronous updates

Use Case: Incrementing counters or toggling boolean state.

## State Management
Definition: Managing state across components using hooks, context, or state libraries.

Use Case: Cart state in e-commerce or user session data.

## Lifting State Up
Definition: Move shared state to the nearest common parent.

Example:
```jsx
function Parent() {
  const [value, setValue] = useState('');
  return <Child input={value} onChange={setValue} />;
}
```

Use Case: Synchronized inputs and filter controls.

## Derived State
Definition: Computed values based on existing state.

Example:
```jsx
const completedItems = items.filter(item => item.done);
```

Advantages:
- Avoid duplicate state

Use Case: Showing filtered totals, summaries, or badges.

## Immutable State Updates
Definition: Create new objects/arrays instead of mutating directly.

Example:
```jsx
setTodos(prev => prev.map(todo => todo.id === id ? { ...todo, done: true } : todo));
```

Advantages:
- Predictable updates
- Better performance with memoization

Use Case: Updating lists without mutating original data.

---

# 6. Event Handling

## Handling Events
Definition: React uses event props like `onClick`, `onChange`, and `onSubmit`.

Example:
```jsx
<button onClick={() => alert('Clicked!')}>Click me</button>
```

Use Case: Button clicks, form interaction, toggles.

## Synthetic Events
Definition: React wraps native events with cross-browser compatibility.

Example:
```jsx
function handleInput(event) {
  console.log(event.target.value);
}
```

Use Case: Unified event handling across browsers.

## Event Binding
Definition: Ensure the correct `this` context in class event handlers.

Example:
```jsx
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
  }
  handleClick() {
    this.setState({ on: !this.state.on });
  }
  render() {
    return <button onClick={this.handleClick}>Toggle</button>;
  }
}
```

Use Case: Older class-based apps and compatibility.

## Passing Arguments to Events
Definition: Pass values using inline callbacks.

Example:
```jsx
<button onClick={() => handleDelete(item.id)}>Delete</button>
```

Use Case: Item-specific actions in lists and tables.

## Form Events
Definition: Handle `onSubmit`, `onChange`, and `onBlur` for forms.

Example:
```jsx
function handleSubmit(e) {
  e.preventDefault();
  submitForm(values);
}
```

Use Case: Sign-in, registration, and checkout forms.

## Keyboard Events
Definition: Use `onKeyDown`, `onKeyPress`, and `onKeyUp`.

Example:
```jsx
function handleKey(event) {
  if (event.key === 'Enter') {
    submit();
  }
}
```

Use Case: Search bars, shortcuts, and accessibility.

## Mouse Events
Definition: Use `onMouseEnter`, `onMouseLeave`, `onClick`, and `onDoubleClick`.

Example:
```jsx
<div onMouseEnter={() => setHover(true)}>Hover me</div>
```

Use Case: Tooltips, hover menus, and drag interactions.

---

# 7. Conditional Rendering

## if/else
Definition: Use standard JavaScript blocks to choose UI.

Example:
```jsx
if (!isLoggedIn) {
  return <Login />;
}
return <Dashboard />;
```

## Ternary Operator
Definition: Inline conditional rendering.

Example:
```jsx
return isLoading ? <Spinner /> : <Content />;
```

## Logical &&
Definition: Render an element when a condition is truthy.

Example:
```jsx
{hasError && <ErrorMessage />}
```

## Switch Rendering
Definition: Use `switch` to select components based on values.

Example:
```jsx
switch (view) {
  case 'home': return <Home />;
  case 'profile': return <Profile />;
  default: return <NotFound />;
}
```

## Dynamic UI Rendering
Definition: Render components based on props, state, or roles.

Use Case: Feature flags, user roles, and responsive layouts.

---

# 8. Lists and Keys

## Rendering Lists
Definition: Use `.map()` to render array items.

Example:
```jsx
<ul>{users.map(user => <li key={user.id}>{user.name}</li>)}</ul>
```

## Dynamic Lists
Definition: Create lists from state and update them dynamically.

Example:
```jsx
const [items, setItems] = useState([]);
```

## Keys Importance
Definition: Keys help React track list elements across renders.

## Unique Keys
Recommendation: Use stable unique identifiers such as database IDs.

## List Optimization
Definition: Use memoization and virtualization for large lists.

Example:
```jsx
const memoizedItems = useMemo(() => items.map(renderItem), [items]);
```

Use Case: Chat messages, product catalogs, activity feeds.

---

# 9. Forms in React

## Controlled Components
Definition: Input values stored in state.

Example:
```jsx
const [text, setText] = useState('');
return <input value={text} onChange={e => setText(e.target.value)} />;
```

Advantages:
- Easier validation
- Synchronized state

Disadvantages:
- More boilerplate for many fields

Use Case: Profile forms and search inputs.

## Uncontrolled Components
Definition: Use refs to access DOM values.

Example:
```jsx
const inputRef = useRef(null);
function handleSubmit() {
  console.log(inputRef.current.value);
}
return <input ref={inputRef} />;
```

Advantages:
- Simpler for basic forms

Disadvantages:
- Harder to validate and manage state

## Form Validation
Definition: Check inputs before submission.

Example:
```jsx
if (!email.includes('@')) setError('Invalid email');
```

Use Case: Registration and checkout.

## Form Submission
Definition: Use `onSubmit` and prevent default browser refresh.

## Handling Multiple Inputs
Definition: Use a single state object for many fields.

Example:
```jsx
const [form, setForm] = useState({ email: '', password: '' });
function handleChange(e) {
  setForm(prev => ({ ...prev, [e.target.name]: e.target.value }));
}
```

## Refs in Forms
Definition: Use `useRef` for focusing and reading values directly.

Use Case: Autofocus fields and managing third-party UI libraries.

---

# 10. React Hooks

## Basic Hooks

### useState
Definition: Hook for local state.

Example:
```jsx
const [count, setCount] = useState(0);
```

### useEffect
Definition: Hook for side effects like data fetching.

Example:
```jsx
useEffect(() => { console.log('mounted'); }, []);
```

### useContext
Definition: Hook for consuming React context.

Example:
```jsx
const theme = useContext(ThemeContext);
```

## Additional Hooks

### useReducer
Definition: Manage complex state logic with reducer functions.

Example:
```jsx
const [state, dispatch] = useReducer(reducer, initialState);
```

### useMemo
Definition: Memoize computed values.

Example:
```jsx
const expensive = useMemo(() => compute(data), [data]);
```

### useCallback
Definition: Memoize callback functions.

Example:
```jsx
const handleClick = useCallback(() => doSomething(value), [value]);
```

### useRef
Definition: Store mutable values and access DOM nodes.

Example:
```jsx
const ref = useRef(null);
```

### useLayoutEffect
Definition: Similar to `useEffect` but runs before browser paint.

Use Case: Measuring layout and synchronizing animations.

### useImperativeHandle
Definition: Customize the instance value exposed to parent refs.

Example:
```jsx
useImperativeHandle(ref, () => ({ focus: () => inputRef.current.focus() }));
```

### useTransition
Definition: Manage transitions and mark updates as non-urgent.

Example:
```jsx
const [isPending, startTransition] = useTransition();
```

### useDeferredValue
Definition: Defer updating a value to avoid blocking renders.

Example:
```jsx
const deferredSearch = useDeferredValue(searchText);
```

### useId
Definition: Generate stable unique IDs for accessibility.

Example:
```jsx
const id = useId();
```

Advantages:
- Simplifies state and lifecycle logic
- Enables custom reusable hooks

Disadvantages:
- Hook rules must be followed precisely

Use Case: Most modern React development uses hooks instead of classes.

---

# 11. useEffect Deep Dive

## Side Effects
Definition: Operations outside render, e.g., fetch, subscriptions, timers.

Example:
```jsx
useEffect(() => {
  const timer = setInterval(() => setTime(Date.now()), 1000);
  return () => clearInterval(timer);
}, []);
```

## Dependency Array
Definition: Controls when effect runs.

Example:
```jsx
useEffect(() => { fetchData(userId); }, [userId]);
```

## Cleanup Functions
Definition: Return a cleanup callback from `useEffect`.

## API Calls
Definition: Use effects to fetch data after render.

Example:
```jsx
useEffect(() => {
  fetch('/api/data')
    .then(res => res.json())
    .then(setData);
}, []);
```

## Lifecycle Replacement
Definition: Hooks replace `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount`.

## Infinite Loops
Definition: Caused by missing or unstable dependencies.

Prevention:
- Add correct dependencies
- Use stable callbacks and memoization

## Best Practices
- Keep effects focused
- Clean up subscriptions
- Avoid heavy work inside render

Effect execution idea:
Render → Commit → useEffect

Use Case: Fetching user profiles, subscribing to sockets, and synchronizing localStorage.

---

# 12. React Lifecycle Methods (Class Components)

## Mounting Phase
Methods:
- `constructor`
- `componentDidMount`

Use Case: Initializing state and fetching initial data.

## Updating Phase
Methods:
- `shouldComponentUpdate`
- `componentDidUpdate`

Use Case: Responding to prop changes.

## Unmounting Phase
Method:
- `componentWillUnmount`

Use Case: Cleanup timers and subscriptions.

## Error Handling Lifecycle
Method:
- `componentDidCatch`

Use Case: Catch render errors in child trees.

---

# 13. Styling in React

## Inline CSS
Definition: Apply styles directly using objects.

Example:
```jsx
<div style={{ backgroundColor: 'blue', padding: '16px' }}>Hello</div>
```

Advantages:
- Easy for dynamic styling

Disadvantages:
- Harder to reuse

## CSS Modules
Definition: Scoped CSS class names.

Example:
```jsx
import styles from './Button.module.css';
<button className={styles.btn}>Click</button>
```

## Styled Components
Definition: CSS-in-JS library using tagged templates.

Example:
```jsx
import styled from 'styled-components';
const Card = styled.div`padding: 20px; background: white;`;
```

## Sass/SCSS
Definition: Preprocessor for nested and reusable CSS.

## Tailwind CSS
Definition: Utility-first CSS framework.

Example:
```jsx
<button className="px-4 py-2 bg-blue-600 text-white rounded">Save</button>
```

## Emotion
Definition: CSS-in-JS library similar to styled components.

## Dynamic Styling
Definition: Change styles based on props or state.

Example:
```jsx
const style = { color: isActive ? 'green' : 'gray' };
```

Use Case: Themes, responsive layouts, and interactive states.

---

# 14. React Router

## React Router Introduction
Definition: Library for client-side navigation in React.

## Route Configuration
Example:
```jsx
<BrowserRouter>
  <Routes>
    <Route path="/" element={<Home />} />
    <Route path="/about" element={<About />} />
  </Routes>
</BrowserRouter>
```

## BrowserRouter
Definition: Uses HTML5 history API.

## Routes & Route
Definition: Define matching UI components.

## Nested Routes
Example:
```jsx
<Route path="dashboard" element={<Dashboard />}>
  <Route path="stats" element={<Stats />} />
</Route>
```

## Route Parameters
Example:
```jsx
<Route path="/user/:id" element={<UserProfile />} />
```

## Navigation
Definition: Use links and navigation hooks.

Example:
```jsx
<Link to="/about">About</Link>
```

## useNavigate
Definition: Imperative navigation.

Example:
```jsx
const navigate = useNavigate();
navigate('/login');
```

## Protected Routes
Definition: Guard routes based on authentication.

Example:
```jsx
function PrivateRoute({ children }) {
  return isAuthenticated ? children : <Navigate to="/login" />;
}
```

## Lazy Loading Routes
Definition: Load route components on demand.

Example:
```jsx
const Profile = React.lazy(() => import('./Profile'));
```

Use Case: Multi-page SPAs with nested pages and access control.

---

# 15. API Handling

## Fetch API
Definition: Native browser API for HTTP requests.

Example:
```jsx
fetch('/api/products')
  .then(res => res.json())
  .then(setProducts);
```

## Axios
Definition: Promise-based HTTP client.

Example:
```jsx
axios.get('/api/products').then(response => setProducts(response.data));
```

## GET/POST Requests
Example:
```jsx
await axios.post('/api/login', { email, password });
```

## Error Handling
Example:
```jsx
try {
  const response = await fetch(url);
  if (!response.ok) throw new Error('Fetch failed');
} catch (error) {
  setError(error.message);
}
```

## Loading States
Definition: Show UI while requests are pending.

Example:
```jsx
if (isLoading) return <Spinner />;
```

## Data Fetching Patterns
Definition: useEffect, custom hooks, server-driven rendering.

## Custom Hooks for APIs
Example:
```jsx
function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch(url)
      .then(res => res.json())
      .then(data => {
        setData(data);
        setLoading(false);
      });
  }, [url]);

  return { data, loading };
}
```

Use Case: API-driven dashboards, search filters, user profiles.

---

# 16. State Management

## Context API
### Context Creation
Example:
```jsx
const ThemeContext = createContext('light');
```

### Provider
Example:
```jsx
<ThemeContext.Provider value="dark">...</ThemeContext.Provider>
```

### Consumer
Example:
```jsx
<ThemeContext.Consumer>{value => <div>{value}</div>}</ThemeContext.Consumer>
```

### useContext
Example:
```jsx
const theme = useContext(ThemeContext);
```

Use Case: Theme, authentication, and locale values.

## Redux
### Redux Basics
Definition: Central store for global state.

### Store
Definition: Holds application state.

### Reducers
Definition: Pure functions to update state.

### Actions
Definition: Objects describing state changes.

### Redux Toolkit
Definition: Official Redux utilities for simpler setup.

Example:
```jsx
const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: (state) => { state.value += 1; },
  },
});
```

### Middleware
Definition: Intercepts actions for logging, async, or side effects.

### Thunk
Definition: Handle async actions with functions.

Example:
```jsx
const fetchUser = createAsyncThunk('user/fetch', async (id) => {
  const response = await fetch(`/api/users/${id}`);
  return response.json();
});
```

## Other Libraries

### Zustand
Definition: Lightweight state library with minimal boilerplate.

### Recoil
Definition: Atom-based state management from Facebook.

### MobX
Definition: Observable state library with reactive updates.

Use Case: Large apps needing predictable global state or optimized state sharing.

---

# 17. Performance Optimization

## React.memo
Definition: Memoize component rendering based on props.

Example:
```jsx
const MemoButton = React.memo(Button);
```

## useMemo
Definition: Cache computed values.

Example:
```jsx
const filtered = useMemo(() => items.filter(item => item.active), [items]);
```

## useCallback
Definition: Cache callback functions.

Example:
```jsx
const onSubmit = useCallback(() => submit(data), [data]);
```

## Lazy Loading
Definition: Load components asynchronously.

## Code Splitting
Definition: Break bundles into smaller chunks.

## Suspense
Definition: Display a fallback while lazy components load.

## Virtualization
Definition: Render only visible list items.

Use libraries like `react-window` or `react-virtualized`.

## Preventing Re-renders
Definition: Use memoization and avoid inline object/array props.

Use Case: Large tables, dashboards, and grids.

---

# 18. Refs

## useRef
Definition: Create a mutable ref object.

Example:
```jsx
const inputRef = useRef(null);
```

## DOM Access
Example:
```jsx
inputRef.current.focus();
```

## Forward Refs
Definition: Pass refs through components.

Example:
```jsx
const Input = forwardRef((props, ref) => <input ref={ref} {...props} />);
```

## Imperative Methods
Definition: Expose imperative functions to parents.

Example:
```jsx
useImperativeHandle(ref, () => ({ focus: () => inputRef.current.focus() }));
```

Use Case: Custom input components and third-party integration.

---

# 19. Advanced React Concepts

## Higher Order Components
Definition: Reuse component logic by wrapping components.

## Render Props
Definition: Pass render logic as a function prop.

Example:
```jsx
<DataFetcher render={data => <List data={data} />} />
```

## Compound Components
Definition: Components that share implicit state.

Example: Tabs, accordions, dropdowns.

## Portals
Definition: Render outside parent DOM hierarchy.

Example:
```jsx
ReactDOM.createPortal(<Modal />, document.body);
```

## Error Boundaries
Definition: Catch JavaScript errors during rendering.

## Controlled vs Uncontrolled Components
Definition: Controlled components are driven by React state; uncontrolled rely on DOM.

## Context Patterns
Definition: Use context for global or shared data.

Use Case: Reusable UI patterns and library components.

---

# 20. Custom Hooks

## Creating Hooks
Definition: Functions that use existing hooks to share logic.

Example:
```jsx
function useToggle(initial = false) {
  const [on, setOn] = useState(initial);
  const toggle = () => setOn(prev => !prev);
  return [on, toggle];
}
```

## Reusable Logic
Definition: Extract repeated behavior from components.

## Hook Rules
Definition: Only call hooks at the top level and from React functions.

## Hook Composition
Definition: Combine multiple hooks inside one custom hook.

Use Case: Authentication, data fetching, and form handling logic.

---

# 21. React Architecture

## Folder Structure
Definition: Organize files by feature, route, or component type.

## Feature-Based Architecture
Definition: Group code by feature to improve scalability.

## Scalable Project Structure
Definition: Use clear boundaries and reusable modules.

## Clean Code Practices
Definition: Use small components, meaningful names, and separation of concerns.

Use Case: Large applications with multiple teams and releases.

---

# 22. Authentication

## JWT Authentication
Definition: Use JSON Web Tokens for stateless auth.

## Protected Routes
Definition: Guard access to route components.

## Login Systems
Definition: Handle credentials, tokens, and sessions.

## OAuth
Definition: Third-party login via providers like Google or GitHub.

## Firebase Authentication
Definition: Managed auth service for React apps.

Use Case: SaaS admin portals, social apps, and private dashboards.

---

# 23. React with TypeScript

## React + TS Setup
Definition: Add static typing to React projects.

## Typing Props
Example:
```tsx
interface CardProps { title: string; }
function Card({ title }: CardProps) {
  return <div>{title}</div>;
}
```

## Typing Hooks
Example:
```tsx
const [count, setCount] = useState<number>(0);
```

## Interfaces
Definition: Define object shapes.

## Generics
Definition: Write reusable typed hooks and components.

## Event Typing
Example:
```tsx
function handleChange(e: React.ChangeEvent<HTMLInputElement>) {
  setValue(e.target.value);
}
```

Advantages:
- Catches errors at compile time
- Improves IDE experience

Disadvantages:
- More upfront typing work

Use Case: Large codebases, scalable teams, and libraries.

---

# 24. Testing React Applications

## Unit Testing
Definition: Test individual components or functions.

## Integration Testing
Definition: Test component interactions and flows.

## Jest
Definition: Test runner and assertion library.

## React Testing Library
Definition: Test React components using DOM queries.

Example:
```jsx
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import Counter from './Counter';

test('increments count on click', () => {
  render(<Counter />);
  userEvent.click(screen.getByRole('button', { name: /increment/i }));
  expect(screen.getByText(/count: 1/i)).toBeInTheDocument();
});
```

## Mocking
Definition: Replace dependencies with test doubles.

## Snapshot Testing
Definition: Capture rendered component output.

Use Case: Regression testing and verifying UI changes.

---

# 25. Error Handling

## Error Boundaries
Definition: Catch render-time errors in child components.

## API Error Handling
Definition: Display error UI for failed requests.

## Async Errors
Definition: Handle rejected promises in effects and callbacks.

## Global Error Management
Definition: Centralize error reporting and user feedback.

Use Case: Improving reliability in production apps.

---

# 26. React Ecosystem

## Next.js
Definition: React framework for SSR, SSG, and hybrid apps.

## Gatsby
Definition: Static site generator for content-heavy sites.

## Remix
Definition: Full-stack framework focusing on web fundamentals.

## Vite
Definition: Fast build tool used for React development.

## CRA
Definition: Create React App bootstrapper.

Use Case: Choose based on SEO, performance, and backend integration.

---

# 27. Server-Side Rendering (SSR)

## CSR vs SSR
Definition: CSR renders in browser, SSR renders on server.

Advantages SSR:
- Better SEO
- Faster first paint

Disadvantages SSR:
- More complex setup

## Static Site Generation (SSG)
Definition: Pre-render pages at build time.

## Incremental Static Regeneration (ISR)
Definition: Update static pages after deployment.

## Hydration
Definition: Attach client-side behavior to server-rendered HTML.

Use Case: Marketing sites, blogs, and commerce landing pages.

---

# 28. React Query / Data Fetching

## React Query Basics
Definition: Library for client state and caching.

## Caching
Definition: Store fetched data and reuse it.

## Mutations
Definition: Send POST/PUT/DELETE updates and manage optimistic UI.

## Pagination
Definition: Fetch pages of data.

## Infinite Queries
Definition: Load more data as users scroll.

## SWR
Definition: Another library for caching and stale-while-revalidate.

Use Case: Data-heavy apps with polling, caching, and real-time updates.

---

# 29. Real-Time Applications

## WebSockets
Definition: Persistent two-way connection between client and server.

## Socket.io
Definition: Library for WebSockets and real-time communication.

## Chat Applications
Use Case: Live chat, team collaboration, and support systems.

## Live Notifications
Use Case: Alerts, activity streams, and stock tickers.

Example:
```jsx
useEffect(() => {
  const socket = io('/');
  socket.on('message', handleMessage);
  return () => socket.disconnect();
}, []);
```

---

# 30. React Security

## XSS Prevention
Definition: Avoid injecting untrusted HTML.

Example:
```jsx
<div>{userInput}</div>
```

## Secure Authentication
Definition: Use HTTPS, secure cookies, and token expiry.

## Environment Variables
Definition: Store API keys and endpoints outside code.

## Token Storage
Definition: Prefer httpOnly cookies or secure storage.

Use Case: Financial apps, user dashboards, and admin systems.

---

# 31. Deployment

## Build Process
Command:
```bash
npm run build
```

## Environment Setup
Definition: Set different API endpoints for dev, staging, production.

## Netlify
Definition: Static hosting for React apps.

## Vercel
Definition: Platform for React, Next.js, and serverless APIs.

## Docker
Definition: Containerize React apps for consistent deployments.

## CI/CD
Definition: Automate lint, test, build, and deploy pipelines.

Use Case: Production readiness, staging previews, and automated releases.

---

# 32. React Design Patterns

## Atomic Design
Definition: Build UIs from atoms, molecules, organisms, templates, and pages.

## Container/Presenter Pattern
Definition: Separate data logic and presentation.

## Hooks Pattern
Definition: Keep logic reusable with custom hooks.

## Compound Components
Definition: Flexible parent-child components with shared behavior.

## Provider Pattern
Definition: Share state through context providers.

Use Case: Design systems, component libraries, and reusable UI frameworks.

---

# 33. React Interview Topics

## Virtual DOM
Concept: How React diffing optimizes DOM updates.

## Reconciliation
Concept: React compares previous and next tree to update only changed nodes.

## Diffing Algorithm
Concept: React uses heuristics and keys to minimize changes.

## Hooks Questions
Examples:
- When should you use `useEffect`?
- What are hook rules?

## Redux Questions
Examples:
- What is a reducer?
- Why use Redux Toolkit?

## Performance Questions
Examples:
- How do you prevent unnecessary renders?
- When should you use code splitting?

## Lifecycle Questions
Examples:
- What are mounting and unmounting phases?
- How does `componentDidUpdate` compare to `useEffect`?

---

# 34. Advanced Ecosystem Topics

## Micro Frontends
Definition: Split a UI into independently deployable apps.

## Monorepos
Definition: Manage multiple packages in one repository.

## Module Federation
Definition: Share code between webpack-based apps.

## React Native Basics
Definition: Build native mobile apps with React.

## Electron + React
Definition: Build desktop apps using React and Node.js.

Use Case: Large enterprises, multi-platform products, and desktop clients.

---

# 35. Project Ideas

## Beginner
- Counter App
- To-Do App
- Weather App
- Notes App

## Intermediate
- Expense Tracker
- Movie App
- Quiz App
- E-commerce UI

## Advanced
- Full Stack Social Media
- Real-Time Chat App
- Admin Dashboard
- Video Streaming Platform

---

# Recommended Learning Order

1. React Basics
2. JSX
3. Components
4. Props
5. State
6. Event Handling
7. Hooks
8. Forms
9. API Calls
10. Routing
11. Context API
12. Redux Toolkit
13. Performance Optimization
14. Advanced React
15. TypeScript
16. Testing
17. Next.js
18. Deployment

---

# Best Additional Skills Along with React

- JavaScript ES6+
- TypeScript
- Git/GitHub
- REST APIs
- Tailwind CSS
- Node.js
- Express.js
- MongoDB/PostgreSQL
- Testing
- Docker Basics
