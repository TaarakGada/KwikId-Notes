# React

## What is it?
- React is a **JavaScript library for building user interfaces**, made by Meta/Facebook
- It breaks the UI into reusable **components** (like LEGO blocks)
- React re-renders only the parts that changed (efficient UI updates)

## Core Concepts

### Component
- A function that returns HTML (JSX)
- Think of each component as a custom HTML element you define
```jsx
function Button({ label, onClick }) {
  return (
    <button onClick={onClick} className="btn">
      {label}
    </button>
  );
}

// Usage
<Button label="Click Me" onClick={() => alert('hi')} />
```

### JSX
- JavaScript + HTML combined syntax
- Looks like HTML but is actually JavaScript
```jsx
// JSX
const element = <h1 className="title">Hello, World!</h1>;

// What it becomes
const element = React.createElement('h1', { className: 'title' }, 'Hello, World!');
```

### Props
- Data passed from parent to child component
- Read-only — child cannot modify props
```jsx
function Greeting({ name, age }) {
  return <p>Hi {name}, you are {age} years old!</p>;
}

<Greeting name="Alice" age={25} />
```

### State
- Data that belongs to a component and can change
- When state changes, React re-renders the component
```jsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);  // [value, setter]

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>+1</button>
    </div>
  );
}
```

### useEffect — Side Effects
- Run code when component mounts, updates, or unmounts
```jsx
import { useState, useEffect } from 'react';

function UserProfile({ userId }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    // Runs when userId changes
    fetch(`/api/users/${userId}`)
      .then(res => res.json())
      .then(data => setUser(data));
  }, [userId]);  // Dependency array — runs when these change

  return user ? <p>{user.name}</p> : <p>Loading...</p>;
}
```

## Component Tree
```
App
├── Header
│   └── NavBar
├── Main
│   ├── Sidebar
│   └── Content
│       └── Card (used multiple times)
└── Footer
```

## React Hooks Cheat Sheet
| Hook | Purpose |
|------|---------|
| `useState` | Local component state |
| `useEffect` | Side effects (API calls, timers) |
| `useContext` | Access global context |
| `useRef` | Reference a DOM element |
| `useMemo` | Cache expensive calculations |
| `useCallback` | Cache functions to prevent re-renders |

## Good to Know
- React itself doesn't handle routing — use **React Router**
- For global state management, use **Redux**, **Zustand**, or **Context API**
- **Vite** or **Create React App** for new projects
- React Native lets you build mobile apps with React
