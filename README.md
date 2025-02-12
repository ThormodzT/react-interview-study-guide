# **React Study Guide**

## **JSX (JavaScript XML)**

JSX is a syntax extension for JavaScript that allows writing HTML-like code inside JavaScript. It makes code more readable and allows seamless integration of JavaScript expressions.

**Example:**

```jsx
const element = <h1>Hello, World!</h1>;
```

JSX gets compiled into JavaScript functions like `React.createElement()` before rendering.

---

## **Element vs. Component**

| Feature       | Element                                                    | Component                                     |
| ------------- | ---------------------------------------------------------- | --------------------------------------------- |
| Definition    | A plain object describing what should appear on the screen | A reusable piece of UI logic that returns JSX |
| Creation      | `React.createElement()` or JSX                             | Function or Class                             |
| Functionality | Static, does not handle state                              | Can handle state and lifecycle methods        |

---

### 3. React Rendering Process

React re-renders components **when state or props change**. The rendering process consists of:

1. **Rendering Phase** – React calls the component function.
2. **Reconciliation** – React compares the new and previous virtual DOM.
3. **Commit Phase** – React updates the real DOM if necessary.

#### Example: What Triggers Re-renders?

| Action                      | Causes Re-render? |
| --------------------------- | ----------------- |
| Changing `useState`         | ✅ Yes            |
| Changing `useRef`           | ❌ No             |
| Changing props              | ✅ Yes            |
| Parent component re-renders | ✅ Yes            |
| Updating context value      | ✅ Yes            |

### 4. Optimizing React Rendering

#### 1. **Use `useMemo` and `useCallback`**

Prevents unnecessary re-renders by memoizing values and functions.

```javascript
import { useState, useMemo } from "react";

function ExpensiveCalculation({ num }) {
  const result = useMemo(() => num * 2, [num]);
  return <p>Result: {result}</p>;
}
```

#### 2. **Use `React.memo` for Component Memoization**

Prevents re-rendering if props haven't changed.

```javascript
import React from "react";
const MemoizedComponent = React.memo(({ value }) => <p>{value}</p>);
```

#### 3. **Avoid Unnecessary State Updates**

Changing state when it's not needed can cause extra renders.

```javascript
// Avoid re-rendering unnecessarily
setState((prev) => prev + 1);
```

---

## **How to Create a Component?**

### Functional Component (Recommended)

A JavaScript function that returns JSX:

```jsx
const MyComponent = (props) => <h1>Hello, {props.name}!</h1>;
```

### Class Component (Legacy)

```jsx
class MyComponent extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}!</h1>;
  }
}
```

---

## **Class vs. Functional Components**

| Feature           | Class Component                         | Functional Component   |
| ----------------- | --------------------------------------- | ---------------------- |
| Syntax            | ES6 class                               | JavaScript function    |
| State             | Uses `this.state`                       | Uses `useState()` hook |
| Lifecycle Methods | Uses methods like `componentDidMount()` | Uses `useEffect()`     |
| Performance       | Less efficient due to `this` binding    | More optimized         |
| Recommended?      | ❌ No (except for Error Boundaries)     | ✅ Yes                 |

---

## **Pure Components**

Components that render the same output for the same state and props. They prevent unnecessary re-renders.

- **Class Component:** `React.PureComponent`
- **Functional Component:** `React.memo()`

```jsx
const MemoizedComponent = React.memo(MyComponent);
```

---

## **Virtual DOM (VDOM) vs. Shadow DOM**

| Feature          | Virtual DOM          | Shadow DOM                         |
| ---------------- | -------------------- | ---------------------------------- |
| Purpose          | Efficient UI updates | Encapsulation of styles and markup |
| React Usage      | Yes                  | No                                 |
| Browser Feature? | No                   | Yes                                |

---

## **State in React**

State is an object that holds data specific to a component. When the state changes, the component re-renders.

```jsx
const [count, setCount] = useState(0);
```

---

## **State vs. Props**

| Feature       | State                      | Props              |
| ------------- | -------------------------- | ------------------ |
| Mutability    | Mutable                    | Immutable          |
| Scope         | Local to the component     | Passed from parent |
| Update Method | `setState` or `useState()` | Cannot be updated  |

---

## **Common React Hooks**

| Hook            | Purpose                                   |
| --------------- | ----------------------------------------- |
| `useState()`    | Adds local state                          |
| `useEffect()`   | Runs side effects                         |
| `useContext()`  | Manages global state                      |
| `useMemo()`     | Memoizes expensive computations           |
| `useCallback()` | Memoizes functions                        |
| `useRef()`      | Persist values without causing re renders |

---

## Understanding `useRef` in React and React Rendering

### 1. What is `useRef` in React?

`useRef` is a React **hook** that allows you to persist values **without causing re-renders**. It is commonly used for:

- **Accessing DOM elements**
- **Persisting values between renders**
- **Storing mutable values without triggering a re-render**

### 2. How `useRef` Works

`useRef` returns a **mutable object** with a `.current` property that persists across renders.

#### Example: Using `useRef` to Access a DOM Element

```javascript
import { useRef, useEffect } from "react";

function FocusInput() {
  const inputRef = useRef(null);

  useEffect(() => {
    inputRef.current.focus(); // Automatically focus input field on mount
  }, []);

  return <input ref={inputRef} placeholder="Type here..." />;
}
```

- The `ref` is attached to the `<input>` element.
- When the component mounts, `useEffect` sets focus on the input.

#### Example: Persisting Values Without Re-Renders

```javascript
import { useRef, useState } from "react";

function Counter() {
  const countRef = useRef(0);
  const [state, setState] = useState(0);

  function increment() {
    countRef.current += 1;
    console.log("Ref value:", countRef.current);
  }

  return (
    <div>
      <p>State: {state}</p>
      <button onClick={() => setState(state + 1)}>Increment State</button>
      <button onClick={increment}>Increment Ref</button>
    </div>
  );
}
```

- Clicking **Increment State** causes a re-render.
- Clicking **Increment Ref** changes `countRef.current` but does **not** trigger a re-render.

---

## **Key Prop in Lists**

Keys help React identify elements that change. Keys must be unique. ✅ Correct:

```jsx
items.map((item) => <li key={item.id}>{item.name}</li>);
```

❌ Incorrect:

```jsx
items.map((item, index) => <li key={index}>{item.name}</li>);
```

---

## **Component Lifecycle Methods**

| Phase      | Class Component          | Functional Component                   |
| ---------- | ------------------------ | -------------------------------------- |
| Mounting   | `componentDidMount()`    | `useEffect(() => {...}, [])`           |
| Updating   | `componentDidUpdate()`   | `useEffect(() => {...}, [dependency])` |
| Unmounting | `componentWillUnmount()` | `useEffect(() => () => {...}, [])`     |

---

## **Next.js vs. React.js**

| Feature       | Next.js                              | React.js                    |
| ------------- | ------------------------------------ | --------------------------- |
| Rendering     | Supports SSR, SSG & ISR              | Only CSR by default         |
| Routing       | File-based routing                   | Uses React Router           |
| Performance   | Optimized via SSR & ISR              | CSR-dependent               |
| Data Fetching | Built-in support (server components) | Requires external libraries |
| SEO           | Better, since it pre-renders pages   | Requires extra setup        |
| API Routes    | Built-in API handling                | Needs an external backend   |

---

## **React Server Components**

React Server Components improve performance by splitting components into:

- **Server Components:** Rendered on the server, do not use interactivity features (`useState`, `useEffect`). Ideal for fetching data and backend operations.
- **Client Components:** Rendered in the browser, support interactivity, state, and effects.

Using more server components enhances performance, while client components should be reserved for interactive features.

---

## **Redux Basics**

- **Store**: Holds the application state.
- **Reducer**: Defines how state changes.
- **Actions**: Objects describing state changes.
- **Dispatch**: Sends actions to update state.
- **Selectors**: Retrieve specific parts of the state.

Example:

```jsx
const reducer = (state, action) => {
  switch (action.type) {
    case "INCREMENT":
      return { count: state.count + 1 };
    default:
      return state;
  }
};
```

---

## **React Rendering and Updates**

React follows these steps for rendering:

1. A state/prop change triggers a re-render.
2. React generates a new Virtual DOM.
3. React compares it with the previous Virtual DOM (diffing process).
4. React updates only the affected parts of the actual DOM.

---

## **Event Handling in React**

React provides event handlers to manage user interactions:

- `onClick`: Handles click events.
- `onChange`: Detects input field changes.
- `onSubmit`: Handles form submissions.

Example:

```jsx
<button onClick={() => alert("Clicked!")}>Click Me</button>
```

---
# How to Create a Custom Hook in React

A **custom hook** in React is a reusable function that uses built-in hooks (`useState`, `useEffect`, etc.) to encapsulate shared logic.

## Steps to Create a Custom Hook

1. **Define a function** that starts with `use`.
2. **Use other hooks** inside the function.
3. **Return values or functions** so the component using it can access them.

## Example: `useCounter` (A Custom Hook for a Counter)

We create a hook called `useCounter` that manages a counter with functions to increment and decrement.

```jsx
import { useState } from 'react';

function useCounter(initialValue = 0) {
  const [count, setCount] = useState(initialValue);

  const increment = () => setCount(count + 1);
  const decrement = () => setCount(count - 1);
  const reset = () => setCount(initialValue);

  return { count, increment, decrement, reset };
}

export default useCounter;
decrement.

import React from 'react';
import useCounter from './useCounter';

function CounterComponent() {
  const { count, increment, decrement, reset } = useCounter(10);

  return (
    <div>
      <h1>Counter: {count}</h1>
      <button onClick={increment}>Increment</button>
      <button onClick={decrement}>Decrement</button>
      <button onClick={reset}>Reset</button>
    </div>
  );
}

export default CounterComponent;
```

