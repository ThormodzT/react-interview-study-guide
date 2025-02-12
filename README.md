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
| Recommended?      | ❌ No (except for Error Boundaries)      | ✅ Yes                  |

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

| Hook            | Purpose                         |
| --------------- | ------------------------------- |
| `useState()`    | Adds local state                |
| `useEffect()`   | Runs side effects               |
| `useContext()`  | Manages global state            |
| `useMemo()`     | Memoizes expensive computations |
| `useCallback()` | Memoizes functions              |

---

## **Key Prop in Lists**

Keys help React identify elements that change. Keys must be unique. ✅ Correct:

```jsx
items.map(item => <li key={item.id}>{item.name}</li>)
```

❌ Incorrect:

```jsx
items.map((item, index) => <li key={index}>{item.name}</li>)
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
    case 'INCREMENT':
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

