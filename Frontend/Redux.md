# Redux — State Management

## What is it?
- Redux is a **global state manager** for JavaScript apps
- Keeps all shared state in one place called the **Store**
- Any component can read from or write to the store
- Useful when many components need the same data (user auth, cart, settings)

## When Do You Need Redux?
- Passing props through many layers (prop drilling)
- Same data needed in multiple unrelated components
- Complex state logic with many updates

## Core Concepts

### Store
- The **single source of truth** — one JavaScript object holding all state

### Action
- A plain object describing **what happened**
```javascript
{ type: 'counter/increment' }
{ type: 'cart/addItem', payload: { id: 1, name: 'Shoe' } }
```

### Reducer
- A function that takes current state + action → returns new state
- Must be **pure** (no side effects, no mutation)

### Dispatch
- How you trigger a state change — you dispatch an action

## Modern Redux (Redux Toolkit) — Recommended
```javascript
// store.js
import { configureStore, createSlice } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: (state) => { state.value += 1; },  // Immer allows mutation syntax
    decrement: (state) => { state.value -= 1; },
    incrementBy: (state, action) => { state.value += action.payload; }
  }
});

export const { increment, decrement, incrementBy } = counterSlice.actions;

export const store = configureStore({
  reducer: { counter: counterSlice.reducer }
});
```

```jsx
// Component.jsx
import { useSelector, useDispatch } from 'react-redux';
import { increment, incrementBy } from './store';

function Counter() {
  const count = useSelector(state => state.counter.value);  // Read state
  const dispatch = useDispatch();

  return (
    <div>
      <p>{count}</p>
      <button onClick={() => dispatch(increment())}>+1</button>
      <button onClick={() => dispatch(incrementBy(5))}>+5</button>
    </div>
  );
}
```

## Data Flow
```
User Action
    ↓
dispatch(action)
    ↓
Reducer processes action → new state
    ↓
Store updates
    ↓
Components re-render with new data
```

## Good to Know
- Use **Redux Toolkit (RTK)** — plain Redux is too verbose
- RTK also has **RTK Query** for data fetching (like React Query)
- For simple apps, Context API + useState might be enough
- Redux DevTools browser extension lets you inspect state changes
