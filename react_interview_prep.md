# The Master React Interview Prep Guide
## Full-Stack React & Spring Boot Integration for Java/Python Developers

This guide systematically covers React architectures, hooks, state, performance, and Spring Boot integration. Below is a comprehensive glossary of React terms, followed by 30 high-frequency Q&As and 14 detailed functional modules.

---

## 📂 Table of Contents
1. [React Concept Glossary (Core Architecture & Ecosystem)](#react-concept-glossary-core-architecture--ecosystem)
2. [30 High-Frequency React Interview Questions & Answers](#30-high-frequency-react-interview-questions--answers)
3. [Module 1: React Fundamentals](#module-1-react-fundamentals)
4. [Module 2: Hooks Deep Dive](#module-2-hooks-deep-dive)
5. [Module 3: Component Lifecycle & `useEffect`](#module-3-component-lifecycle--useeffect)
6. [Module 4: State Management Patterns](#module-4-state-management-patterns)
7. [Module 5: Rendering Mechanics & Optimization](#module-5-rendering-mechanics--optimization)
8. [Module 6: Form Handling & Validation](#module-6-form-handling--validation)
9. [Module 7: Client-Side Routing (React Router)](#module-7-client-side-routing-react-router)
10. [Module 8: API Integration & Data Fetching](#module-8-api-integration--data-fetching)
11. [Module 9: Web Performance Optimization](#module-9-web-performance-optimization)
12. [Module 10: React Internals & Fiber](#module-10-react-internals--fiber)
13. [Module 11: Advanced React Architectures](#module-11-advanced-react-architectures)
14. [Module 12: Testing (Jest & React Testing Library)](#module-12-testing-jest--react-testing-library)
15. [Module 13: React Ecosystem Map](#module-13-react-ecosystem-map)
16. [Module 14: React + Spring Boot Integration (Full Stack)](#module-14-react--spring-boot-integration-full-stack)

---

## React Concept Glossary (Core Architecture & Ecosystem)

Use this glossary to study basic, intermediate, and advanced React terms often tested in senior or FAANG interviews:

*   **Virtual DOM (VDOM):** A lightweight, in-memory representation of the real DOM in JavaScript. React reads and writes updates to the VDOM first, which is cheap, before syncing the changes to the browser DOM.
*   **Reconciliation:** React's process of sync-updating the virtual DOM and mapping differences to the actual DOM.
*   **Diffing Algorithm:** The $O(N)$ heuristic search process comparing virtual node changes. React uses two rules: elements of different types will rebuild the tree, and keys must be stable, predictable, and unique across siblings.
*   **React Fiber:** React's core reconciliation engine introduced in React 16. It enables asynchronous scheduling, allowing React to pause, resume, and prioritize rendering work (e.g., keeping typing inputs interactive while processing background lists).
*   **JSX (JavaScript XML):** A syntax extension for JavaScript that allows writing HTML-like structures directly inside JS files. Under the hood, JSX compiles to native `React.createElement()` functions returning standard JS objects.
*   **Props:** Read-only configuration inputs passed from a parent component to a child component. Props are **immutable**; a child component cannot modify the props it receives.
*   **State:** A built-in React object representing local, mutable component data that can change over time. When a component’s state is updated, React triggers a re-render of that component and its children.
*   **Keys:** Unique string identifiers passed to items in a list. They help React identify which items have changed, been added, or been removed, ensuring efficient rendering and preventing state mismatches in dynamic lists.
*   **Hooks:** Built-in functions starting with `use` that let functional components tap into React state and lifecycle systems.
*   **Lifecycle:** The mounting, updating, and unmounting phases of components. Functional components manage these phases using the `useEffect` hook.
*   **Batching:** React combines multiple state updates inside event handlers into a single render pass to optimize performance.
*   **React.memo:** A higher-order component that optimizes performance by caching the component. It prevents a component from re-rendering if its incoming props have not changed.
*   **Synthetic Events:** React's cross-browser event wrapper system. It standardizes native browser events into a single API format and delegates listeners to the root container node (where the app is mounted) instead of the document, reducing listener overhead.
*   **Portals:** Portals provide a way to render React children components into a DOM node that exists outside the main DOM hierarchy of the parent component (e.g., rendering a Modal directly under `<body>`).
*   **Error Boundaries:** Class components that catch JavaScript errors anywhere in their child component tree, preventing the entire application from crashing and rendering a fallback UI.
*   **Suspense:** A built-in React component that lets you specify a loading fallback UI (like a spinner) while child components are waiting for asynchronous data loading or code imports to complete.
*   **Hydration:** The client-side process where React binds event listeners to server-rendered HTML, turning static HTML into an interactive single-page application.
*   **Server-Side Rendering (SSR) vs. Client-Side Rendering (CSR):**
    *   **SSR:** The server pre-renders components into fully populated HTML strings and sends them to the browser (better SEO, fast initial paint).
    *   **CSR:** The browser downloads a minimal HTML shell and a large JS file, then builds the DOM (fast navigation once loaded).
*   **Prop Drilling:** The anti-pattern of passing props down through multiple intermediate levels of components that do not need the data, just to reach a deeply nested child component.
*   **Controlled vs. Uncontrolled Components:**
    *   **Controlled:** Input values are bound to React state, and changes are handled via `onChange` events. React is the single source of truth.
    *   **Uncontrolled:** Inputs are managed by the DOM directly, and values are accessed on demand using a `useRef` reference.
*   **Context API:** A built-in mechanism to share state globally down the component tree without manually passing props through every intermediate level (prop drilling).
*   **Concurrent Rendering:** Allows React to prepare multiple versions of UI concurrently without blocking the main browser thread.
*   **Redux / Redux Toolkit (RTK):** State management standard for large, complex enterprise data flows utilizing actions, reducers, and selectors.
*   **Zustand:** A minimalist, hook-based external state management library that provides central state stores with minimal boilerplate.
*   **React Query (TanStack Query):** A library that simplifies server state management, caching database responses, handling loading/fetching states, and synchronizing server state automatically.
*   **Vite:** Extremely fast frontend builder tool utilizing native ES modules.
*   **Next.js:** A full-stack framework built on React supporting SSR, SSG, and API routing.
*   **Higher-Order Components (HOC) vs. Render Props:**
    *   **HOC:** Functions wrapping a component to inject extra properties/behaviors.
    *   **Render Props:** A technique for sharing code by passing a function as a prop to evaluate layouts.
*   **Stale Closures:** A bug in React hooks where a callback captures variable values from an older render scope. Because that closure is not recreated, it references outdated state values.

---

## 30 High-Frequency React Interview Questions & Answers

#### Q1: What is React?
React is a declarative, component-based JavaScript library developed by Meta for building user interfaces, primarily for single-page applications. It allows developers to create reusable UI components that manage their own state.

#### Q2: Why React instead of Angular or Vue?
*   **React:** A lightweight library focused purely on the View layer, using JSX (pure JS syntax) and giving developers flexibility to choose state and routing libraries.
*   **Angular:** A heavy, opinionated framework with a steep learning curve, built-in features (routing, HTTP client, forms), and mandatory TypeScript structure.
*   **Vue:** A progressive framework matching Angular’s templates and React’s reactivity, but with less adoption in large enterprise/FAANG environments compared to React.

#### Q3: What is JSX?
JSX (JavaScript XML) is a syntax extension for JavaScript that allows writing HTML-like structures directly inside JS files. Under the hood, JSX compiles to native `React.createElement()` functions returning standard JS objects.

#### Q4: Functional vs Class Components
*   **Functional Components:** Standard JS functions accepting props and returning JSX. They use **Hooks** for state and lifecycle operations. They are the modern React standard.
*   **Class Components:** ES6 classes extending `React.Component`, using `this.state`, and lifecycle methods (e.g. `componentDidMount`). Mostly deprecated but still supported.

#### Q5: What are Props?
Props (properties) are read-only configuration inputs passed from a parent component to a child component. Props are **immutable**; a child component cannot modify the props it receives.

#### Q6: What is State?
State is a built-in React object representing local, mutable component data that can change over time. When a component’s state is updated, React triggers a re-render of that component and its children.

#### Q7: What is the Virtual DOM?
The Virtual DOM is a lightweight, in-memory copy of the real DOM representation in JavaScript. React writes UI updates to the Virtual DOM first, which is cheap, before syncing the changes to the browser DOM.

#### Q8: Explain Reconciliation.
Reconciliation is React's process of comparing the updated Virtual DOM tree with the previous Virtual DOM tree (using a diffing algorithm), identifying what changed, and batching only those differences to update the real DOM.

#### Q9: What triggers a re-render?
A component re-renders when:
1.  Its internal state (`useState`) changes.
2.  The props passed to it change.
3.  The parent component re-renders.
4.  A context value it consumes changes.

#### Q10: Explain useState.
`useState` is a React Hook that adds state variables to functional components. It returns an array containing the current state value and a setter function to update that value.
```javascript
const [count, setCount] = useState(0);
```

#### Q11: Explain useEffect.
`useEffect` is a React Hook that handles side effects (API calls, subscriptions, manual DOM manipulation) in functional components. It runs after rendering and can execute cleanups upon unmounting.

#### Q12: Explain useMemo.
`useMemo` is a performance optimization hook that memoizes (caches) the value returned by an expensive calculation. It only recalculates the value when one of its dependencies changes.
```javascript
const cachedVal = useMemo(() => heavyCompute(x), [x]);
```

#### Q13: Explain useCallback.
`useCallback` is a performance optimization hook that memoizes the **function definition reference** itself. It prevents functions from being recreated on every render, avoiding unnecessary renders of child components.

#### Q14: Explain useRef.
`useRef` is a hook returning a mutable object whose `.current` property persists across renders. Modifying it does not trigger a re-render. It is commonly used to access HTML DOM elements directly.

#### Q15: Explain useContext.
`useContext` is a hook that allows functional components to subscribe to and consume global React Context values directly without passing props down through parent components.

#### Q16: What are Custom Hooks?
Custom Hooks are reusable JavaScript functions whose names start with `use` and which can call other React hooks. They encapsulate stateful logic so it can be shared across multiple components.

#### Q17: Controlled vs Uncontrolled Components
*   **Controlled:** Input values are bound to React state, and changes are handled via `onChange` events. React is the single source of truth.
*   **Uncontrolled:** Inputs are managed by the DOM directly, and values are accessed on demand using a `useRef` reference.

#### Q18: Why are Keys important?
Keys are unique string identifiers passed to items in a list. They help React identify which items have changed, been added, or been removed, ensuring efficient rendering and preventing state mismatches in dynamic lists.

#### Q19: What is React.memo?
`React.memo` is a higher-order component that optimizes performance by caching the component. It prevents a component from re-rendering if its incoming props have not changed.

#### Q20: Context API vs Redux
*   **Context API:** Built-in React tool for sharing static global data (theme, auth status). It triggers re-renders on all children consuming it when the value changes (can cause performance lag if updated frequently).
*   **Redux:** An external state management library that uses actions and selectors, optimized for high-frequency state updates and large-scale complex data.

#### Q21: What is Redux Toolkit (RTK)?
Redux Toolkit is the modern, official standard for writing Redux logic. It wraps the core Redux library to eliminate boilerplate code, incorporating utilities like `createSlice` for automated actions/reducers.

#### Q22: What is React Router?
React Router is the standard declarative routing library for React. It manages single-page application navigation, mapping browser URLs to specific component hierarchies without full page refreshes.

#### Q23: How do you optimize a React app?
1.  Use `React.memo` to skip rendering stable components.
2.  Cache functions and values using `useCallback` and `useMemo`.
3.  Implement lazy loading and code splitting.
4.  Use list virtualization (`react-window`) for long datasets.

#### Q24: Explain lazy loading.
Lazy loading is a design pattern that defers the loading of React components or pages until they are actually needed (e.g., when a user navigates to that page), improving initial website loading speeds.

#### Q25: Explain code splitting.
Code splitting is a technique that splits a large JavaScript bundle into smaller chunks (bundles). This is done using dynamic imports (`import()`) and integrated using `React.lazy` and `React.Suspense`.

#### Q26: What are Error Boundaries?
Error Boundaries are class components that catch JavaScript errors anywhere in their child component tree, preventing the entire application from crashing and rendering a fallback UI.

#### Q27: What are Portals?
Portals provide a way to render React children components into a DOM node that exists outside the main DOM hierarchy of the parent component (e.g., rendering a Modal directly under `<body>`).

#### Q28: Explain Suspense.
`Suspense` is a built-in React component that lets you specify a loading fallback UI (like a spinner) while child components are waiting for asynchronous data loading or code imports to complete.

#### Q29: How do you fetch API data?
You fetch data in React using the native browser `fetch()` API or libraries like **Axios** inside a `useEffect` hook, or by utilizing data-fetching libraries like **React Query** for automated caching.

#### Q30: How do you secure a React application?
1.  **Sanitize user input** to prevent Cross-Site Scripting (XSS).
2.  **Store JWTs in HttpOnly secure cookies** instead of LocalStorage to prevent script access.
3.  **Implement Route Guards** in React Router to restrict UI access.
4.  **Enforce CORS** on the backend to authorize requests only from the frontend's origin URL.

---

## Module 1: React Fundamentals

### What is React & Why Use It?
*   **What:** A declarative, component-based, open-source JS library for building user interfaces.
*   **Why:** Unlike traditional imperative DOM manipulation (`document.getElementById().innerText = x`), React uses a **declarative** style where you define what the UI should look like for a given state, and React handles the UI updates automatically. It organizes code into reusable, self-contained modular components.

### Real DOM vs. Virtual DOM
*   **Real DOM:** An XML/HTML representation of the page. Updating it requires the browser to recalculate layouts, CSS styles, and repaint the screen (slow).
*   **Virtual DOM (VDOM):** A lightweight JavaScript object copy of the Real DOM. React writes updates to the VDOM first, compares the new VDOM with the old one (Diffing), and batches only the differences to the Real DOM (Reconciliation), rendering pages much faster.

### JSX (JavaScript XML)
A syntax extension for JS that looks like HTML. Under the hood, JSX compiles to plain JS function calls:
```jsx
// JSX input
const element = <h1 className="title">Hello</h1>;

// Compiles to
const elementJS = React.createElement('h1', { className: 'title' }, 'Hello');
```

### Components: Functional Components
A component is a JavaScript function that accepts an object of inputs (`props`) and returns JSX.
```jsx
function ProfileCard({ name, role }) {
  return (
    <div className="card">
      <h3>{name}</h3>
      <p>{role}</p>
    </div>
  );
}
```

### Props vs. State
*   **Props:** Short for "properties". Read-only data passed down from parent to child. Props are **immutable**.
*   **State:** Local data storage managed inside the component itself. When state changes, the component re-renders.

### Event Handling
Passed as camelCase functions directly on the JSX tags:
```jsx
function Button() {
  const handleClick = (e) => {
    console.log("Clicked!", e.target);
  };
  return <button onClick={handleClick}>Click Me</button>;
}
```

### Conditional Rendering & Lists
*   **Conditional Rendering:** Done using JS ternary operators or logical `&&` shortcuts.
*   **Lists:** Rendered by looping over arrays with `.map()`. Every item in a mapped list **must** have a unique, stable `key` prop.
```jsx
function UserDashboard({ isLoggedIn, items }) {
  return (
    <div className="dashboard">
      {isLoggedIn ? <h2>Welcome back!</h2> : <h2>Please log in.</h2>}
      
      <h3>Your Items:</h3>
      <ul>
        {items.map(item => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
    </div>
  );
}
```

### Fragments
Fragments (`<>...</>`) allow grouping multiple children without adding unnecessary nodes to the HTML DOM.

---

## Module 2: Hooks Deep Dive

Hooks are built-in functions starting with `use` that let functional components tap into React state and lifecycle systems.

### 1. `useState`
Manages local component state.
```javascript
const [value, setValue] = useState(initialValue);
```
*   *Common Mistake:* Mutating state directly (e.g., `state.push(item)`) instead of returning a new copy.
```javascript
// WRONG
list.push(item);
setList(list);

// RIGHT
setList(prevList => [...prevList, item]);
```

### 2. `useEffect`
Performs side effects.
*   **No Dependency Array:** Runs on every single render.
*   **Empty Array `[]`:** Runs once on component mount.
*   **With Dependencies `[dep1, dep2]`:** Runs on mount and whenever dependencies change.
*   **Cleanup Function:** Returning a function inside `useEffect` clears resources (timers, subscriptions) before the component unmounts.
```javascript
useEffect(() => {
  const timer = setInterval(() => console.log("tick"), 1000);
  return () => clearInterval(timer); // Cleanup prevents memory leaks
}, []);
```

### 3. `useRef`
Creates a mutable object whose `.current` property persists between renders. Modifying it **does not** cause the component to re-render.
*   *Use Cases:* Referencing DOM nodes directly, or keeping track of mutable values (like previous state or interval IDs).
```javascript
const inputRef = useRef(null);
const focusInput = () => inputRef.current.focus();
```

### 4. `useMemo` & `useCallback`
*   `useMemo`: Memoizes (caches) the output of an expensive computation.
*   `useCallback`: Memoizes the function declaration itself to prevent recreation.
```javascript
const expensiveCalculation = useMemo(() => compute(data), [data]);
const memoizedCallback = useCallback(() => doSomething(id), [id]);
```

### 5. `useContext`
Subscribes to global context objects, preventing prop drilling.
```javascript
const theme = useContext(ThemeContext);
```

### 6. `useReducer`
An alternative to `useState` for complex state logic involving multiple sub-values. It acts like a Redux store at a component level.
```jsx
const initialState = { count: 0 };
function reducer(state, action) {
  switch (action.type) {
    case 'increment': return { count: state.count + 1 };
    case 'decrement': return { count: state.count - 1 };
    default: throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
    </>
  );
}
```

### 7. Custom Hooks
Extracting stateful logic into a custom reusable function prefixing with `use`.
```javascript
function useLocalStorage(key, initialVal) {
  const [val, setVal] = useState(() => {
    return JSON.parse(localStorage.getItem(key)) || initialVal;
  });
  useEffect(() => {
    localStorage.setItem(key, JSON.stringify(val));
  }, [key, val]);
  return [val, setVal];
}
```

---

## Module 3: Component Lifecycle & `useEffect`

The standard class-based lifecycle methods map directly to the modern `useEffect` hook:

```
Class Component Lifecycle               Functional Hook Equivalent (useEffect)
┌────────────────────────┐              ┌──────────────────────────────────────┐
│  componentDidMount()   │  ─────────>  │  useEffect(() => {}, [])             │
└────────────────────────┘              └──────────────────────────────────────┘
┌────────────────────────┐              ┌──────────────────────────────────────┐
│  componentDidUpdate()  │  ─────────>  │  useEffect(() => {}, [dependencies]) │
└────────────────────────┘              └──────────────────────────────────────┘
┌────────────────────────┐              ┌──────────────────────────────────────┐
│  componentWillUnmount()│  ─────────>  │  useEffect(() => {                   │
└────────────────────────┘              │    return () => { /* cleanup */ };   │
                                        │  }, []);                             │
                                        └──────────────────────────────────────┘
```

---

## Module 4: State Management Patterns

| State Type | Mechanism | Best Use Case |
| :--- | :--- | :--- |
| **Local State** | `useState`, `useReducer` | Form inputs, toggles, loading animations. |
| **Lifted State** | Lift state to shared parent | Component controls that need to coordinate with a sibling. |
| **Context API** | Context Providers | Static global values (User Auth, App Themes, Locale). |
| **External Store** | Redux Toolkit, Zustand | High-frequency updates, cached database results, undo/redo logic. |

### Redux basics: Actions, Reducers, Store
*   **Store:** Single source of truth containing global application state.
*   **Action:** Plain JavaScript objects describing an event (e.g., `{ type: 'TODO_ADDED', payload: text }`).
*   **Reducer:** Pure functions that take the current State and Action, and return the next state.
*   **Redux Toolkit (RTK):** Modern wrapper around Redux simplifying setup by eliminating boilerplate (using `createSlice` to manage actions/reducers together).

---

## Module 5: Rendering Mechanics & Optimization

### What causes a re-render?
1.  **State changes** inside the component.
2.  **Parent component** re-renders (all child components nested underneath re-render automatically).
3.  **Props** passed into the component change.
4.  **Context values** that the component consumes change.

### Preventing Unnecessary Re-renders:
1.  **`React.memo`:** Higher-order component wrapping a functional component. It performs a shallow comparison of props and skips rendering if props haven't changed.
2.  **`useMemo` & `useCallback`:** Keep object references and function references stable so children don't see "new" props on parent re-renders.

### Reconciliation & Diffing Heuristics
React compares virtual trees with an $O(N)$ heuristic algorithm:
*   Two elements of different types will produce different trees (destroying the old tree and mounting the new one).
*   Keys must be stable, predictable, and unique across siblings to ensure elements are updated/moved instead of rebuilt.

---

## Module 6: Form Handling & Validation

### Controlled vs. Uncontrolled Components
*   **Controlled:** Input values are bound to React state. Updates happen via `onChange` handlers (standard).
*   **Uncontrolled:** Values are handled by the DOM directly. React accesses them on demand using `useRef`.

### Validation Code Example: React Hook Form vs. Standard State
React Hook Form is a popular library that reduces re-renders by utilizing uncontrolled inputs behind the scenes.

```jsx
import { useForm } from 'react-hook-form';

function ModernForm() {
  const { register, handleSubmit, formState: { errors } } = useForm();
  
  const onSubmit = data => console.log(data);

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input {...register("email", { required: true, pattern: /^\S+@\S+$/i })} />
      {errors.email && <span>Invalid email format</span>}
      <button type="submit">Submit</button>
    </form>
  );
}
```

---

## Module 7: Client-Side Routing (React Router)

React Router enables page navigation in single-page apps without causing browser page reloads.

### Setup and Route Structures:
```jsx
import { BrowserRouter, Routes, Route, Link, useParams, Navigate } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/user/123">User Profile</Link>
      </nav>

      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/user/:userId" element={<UserProfile />} />
        {/* Protected Route Example */}
        <Route path="/dashboard" element={
          <ProtectedRoute isAuthenticated={false}>
            <Dashboard />
          </ProtectedRoute>
        } />
      </Routes>
    </BrowserRouter>
  );
}

function UserProfile() {
  const { userId } = useParams(); // Fetch dynamic URL parameters
  return <h3>User ID: {userId}</h3>;
}

function ProtectedRoute({ isAuthenticated, children }) {
  return isAuthenticated ? children : <Navigate to="/" replace />;
}
```

---

## Module 8: API Integration & Data Fetching

### Fetching data & Handling Loading/Errors (using Axios)
Axios is preferred over native `fetch` because it supports interceptors, automatic JSON transformation, and simpler error handling.

```jsx
import React, { useState, useEffect } from 'react';
import axios from 'axios';

function DataFetcher() {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const cancelTokenSource = axios.CancelToken.source();

    axios.get('https://api.example.com/items', {
      cancelToken: cancelTokenSource.token
    })
      .then(res => {
        setData(res.data);
        setLoading(false);
      })
      .catch(err => {
        if (!axios.isCancel(err)) {
          setError(err.message);
          setLoading(false);
        }
      });

    return () => cancelTokenSource.cancel("Request cancelled by cleanup"); // Cancel request if component unmounts
  }, []);

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  return <div>Data: {JSON.stringify(data)}</div>;
}
```

---

## Module 9: Web Performance Optimization

1.  **Code Splitting / Lazy Loading:** Splitting JS bundles so users load only the code needed for the current page.
```jsx
const HeavyComponent = React.lazy(() => import('./HeavyComponent'));
function MyPage() {
  return (
    <React.Suspense fallback={<div>Loading component...</div>}>
      <HeavyComponent />
    </React.Suspense>
  );
}
```
2.  **Virtualization:** Rendering only visible list rows on screen (using libraries like `react-window` or `react-virtualized`), crucial for lists with thousands of items.
3.  **Debouncing / Throttling:** Restricting API call or execution frequencies.
4.  **Bundle Optimization:** Configuring production builds to clean out console statements and minify JS.

---

## Module 10: React Internals & Fiber

*   **React Fiber:** React's core reconciliation engine. Prior to Fiber (React 16), rendering was synchronous and blocking. Fiber introduces an asynchronous scheduling architecture that can pause, resume, and prioritize rendering work (e.g., keeping typing inputs interactive while processing background lists).
*   **Batching:** React combines multiple state updates inside event handlers into a single render pass to optimize performance.
*   **Concurrent Mode:** Allows React to prepare multiple versions of UI concurrently without blocking the main browser thread.

---

## Module 11: Advanced React Architectures

### 1. Error Boundaries
Class components that catch JS errors anywhere in their child component tree, logging errors and displaying fallback UIs instead of crashing the entire page.
```jsx
class ErrorBoundary extends React.Component {
  state = { hasError: false };
  static getDerivedStateFromError(error) { return { hasError: true }; }
  componentDidCatch(error, info) { console.error(error, info); }
  render() {
    if (this.state.hasError) return <h1>Something went wrong.</h1>;
    return this.props.children;
  }
}
```

### 2. Portals
Render components into DOM nodes outside of their parent component hierarchy (essential for modals, tooltips, and dialogs).
```javascript
ReactDOM.createPortal(<Modal />, document.getElementById('modal-root'));
```

### 3. Higher-Order Components (HOC) vs. Render Props
*   **HOC:** Functions wrapping a component to inject extra properties/behaviors: `const Enhanced = withAuth(MyComponent);`.
*   **Render Props:** A technique for sharing code by passing a function as a prop to evaluate layouts: `<DataProvider render={data => <List data={data} />} />`.
*   *Note:* Both patterns are mostly replaced by **Custom Hooks** in modern React.

---

## Module 12: Testing (Jest & React Testing Library)

React Testing Library (RTL) tests component behaviors from the user's perspective rather than implementation details.

```javascript
import { render, screen, fireEvent } from '@testing-library/react';
import Counter from './Counter';

test('increments counter on button click', () => {
  render(<Counter />);
  
  const countVal = screen.getByText(/Counter: 0/i);
  const btn = screen.getByRole('button', { name: /increment/i });
  
  fireEvent.click(btn);
  
  expect(screen.getByText(/Counter: 1/i)).toBeInTheDocument();
});
```

---

## Module 13: React Ecosystem Map

*   **Redux Toolkit (RTK):** State management standard for large, complex enterprise data flows.
*   **React Router:** Standard library for web single-page navigation.
*   **Axios:** Flexible HTTP client supporting configurations and interceptors.
*   **React Query (TanStack Query):** Caches server data, manages loading/fetching states, and synchronizes server state automatically (replaces redundant Redux code for API storage).
*   **Vite:** Extremely fast frontend builder tool utilizing native ES modules.
*   **Next.js:** A framework built on React supporting Server-Side Rendering (SSR), Static Site Generation (SSG), and API routing.

---

## Module 14: React + Spring Boot Integration (Full Stack)

This section maps React interactions to a **Spring Boot REST API** backend.

```
       FRONTEND (React)                           BACKEND (Spring Boot)
 ┌──────────────────────────┐               ┌────────────────────────────────┐
 │ Axios Client             │  ───────────> │ REST Controller                │
 │ (Attaches JWT in Header) │               │ (Validates JWT, Checks Roles)  │
 └──────────────────────────┘               └────────────────────────────────┘
              ▲                                             │
              │                                             ▼
 ┌──────────────────────────┐               ┌────────────────────────────────┐
 │ Interceptor              │ <───────────  │ CORS Configurations            │
 │ (Refreshes Stale Token)  │               │ (Authorizes Client Origin URL) │
 └──────────────────────────┘               └────────────────────────────────┘
```

### 1. Spring Boot CORS Configuration
By default, browsers block React (running on `http://localhost:5173`) from calling Spring Boot APIs (on `http://localhost:8080`). You must enable CORS on your Spring Boot controllers:

```java
@CrossOrigin(origins = "http://localhost:5173") // Allow React Origin
@RestController
@RequestMapping("/api/v1/items")
public class ItemController {
    
    @GetMapping
    public ResponseEntity<List<Item>> getItems() {
        return ResponseEntity.ok(itemService.getAll());
    }
}
```

### 2. Frontend Axios Client with JWT & Interceptors
To handle authorized API endpoints, configure an Axios instance to attach JWT tokens to every outgoing request automatically. If the API returns a `401 Unauthorized` response, the interceptor attempts to refresh the JWT token using a Refresh Token.

```javascript
import axios from 'axios';

const apiClient = axios.create({
  baseURL: 'http://localhost:8080/api/v1',
});

// Request Interceptor: Attach JWT to request headers
apiClient.interceptors.request.use((config) => {
  const token = localStorage.getItem('accessToken');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
}, (error) => Promise.reject(error));

// Response Interceptor: Handle Token Expiration
apiClient.interceptors.response.use(
  (response) => response,
  async (error) => {
    const originalRequest = error.config;
    
    if (error.response.status === 401 && !originalRequest._retry) {
      originalRequest._retry = true;
      try {
        // Attempt to fetch a new access token using the refresh token
        const refreshRes = await axios.post('http://localhost:8080/api/v1/auth/refresh', {
          refreshToken: localStorage.getItem('refreshToken')
        });
        
        const newAccessToken = refreshRes.data.accessToken;
        localStorage.setItem('accessToken', newAccessToken);
        
        // Retry the original request with the new access token
        originalRequest.headers.Authorization = `Bearer ${newAccessToken}`;
        return apiClient(originalRequest);
      } catch (refreshError) {
        console.error("Refresh token expired. Logging out.");
        localStorage.clear();
        window.location.href = '/login';
      }
    }
    return Promise.reject(error);
  }
);

export default apiClient;
```

### 3. File Uploads (React Component to Spring Controller)
*   **Spring Boot Endpoint:**
```java
@PostMapping("/upload")
public ResponseEntity<String> uploadFile(@RequestParam("file") MultipartFile file) {
    fileService.save(file);
    return ResponseEntity.ok("File uploaded successfully");
}
```
*   **React Frontend Action:**
```jsx
function FileUploader() {
  const [file, setFile] = useState(null);

  const handleUpload = async () => {
    if (!file) return;
    const formData = new FormData();
    formData.append('file', file); // Maps to Spring Param

    try {
      await apiClient.post('/upload', formData, {
        headers: { 'Content-Type': 'multipart/form-data' }
      });
      alert('Upload Successful');
    } catch (err) {
      console.error(err);
    }
  };

  return (
    <div>
      <input type="file" onChange={(e) => setFile(e.target.files[0])} />
      <button onClick={handleUpload}>Upload to Server</button>
    </div>
  );
}
```

### 4. Paginated API Calls & Search Filtering
*   **Spring Controller:**
```java
@GetMapping("/search")
public Page<Item> searchItems(
    @RequestParam String query,
    @RequestParam(defaultValue = "0") int page,
    @RequestParam(defaultValue = "10") int size
) {
    Pageable pageable = PageRequest.of(page, size);
    return itemService.search(query, pageable);
}
```
*   **React Frontend Component:**
```jsx
import React, { useState, useEffect } from 'react';
import apiClient from './apiClient';

function SearchList() {
  const [query, setQuery] = useState('');
  const [items, setItems] = useState([]);
  const [page, setPage] = useState(0);
  const [totalPages, setTotalPages] = useState(0);

  useEffect(() => {
    const delayDebounceFn = setTimeout(() => {
      fetchResults();
    }, 400); // Debounce API requests

    return () => clearTimeout(delayDebounceFn);
  }, [query, page]);

  const fetchResults = async () => {
    try {
      const res = await apiClient.get(`/search?query=${query}&page=${page}&size=5`);
      setItems(res.data.content);      // Spring Boot Page.content
      setTotalPages(res.data.totalPages); // Spring Boot Page.totalPages
    } catch (err) {
      console.error(err);
    }
  };

  return (
    <div>
      <input 
        type="text" 
        placeholder="Search items..." 
        value={query} 
        onChange={(e) => { setQuery(e.target.value); setPage(0); }} 
      />
      
      <ul>
        {items.map(item => <li key={item.id}>{item.name}</li>)}
      </ul>

      <div>
        <button onClick={() => setPage(p => Math.max(0, p - 1))} disabled={page === 0}>
          Previous
        </button>
        <span> Page {page + 1} of {totalPages} </span>
        <button onClick={() => setPage(p => Math.min(totalPages - 1, p + 1))} disabled={page >= totalPages - 1}>
          Next
        </button>
      </div>
    </div>
  );
}
```
