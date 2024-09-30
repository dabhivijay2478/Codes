# React Hooks Guide

React Hooks are functions that allow you to use state and other React features in functional components. This guide covers the built-in hooks and how to create custom hooks.

## Built-in Hooks

### 1. useState

The `useState` hook allows you to add state to functional components.

```jsx
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

### 2. useEffect

The `useEffect` hook lets you perform side effects in functional components.

```jsx
import React, { useState, useEffect } from 'react';

function DataFetcher() {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetch('https://api.example.com/data')
      .then(response => response.json())
      .then(result => setData(result));
  }, []); // Empty dependency array means this effect runs once on mount

  return <div>{data ? JSON.stringify(data) : 'Loading...'}</div>;
}
```

### 3. useContext

The `useContext` hook allows you to subscribe to React context without introducing nesting.

```jsx
import React, { useContext } from 'react';

const ThemeContext = React.createContext('light');

function ThemedButton() {
  const theme = useContext(ThemeContext);
  return <button style={{ background: theme }}>I'm styled by theme context!</button>;
}
```

### 4. useReducer

The `useReducer` hook is used for managing more complex state logic.

```jsx
import React, { useReducer } from 'react';

const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      throw new Error();
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

### 5. useCallback

The `useCallback` hook returns a memoized version of the callback that only changes if one of the dependencies has changed.

```jsx
import React, { useState, useCallback } from 'react';

function ParentComponent() {
  const [count, setCount] = useState(0);

  const incrementCount = useCallback(() => {
    setCount(prevCount => prevCount + 1);
  }, []);

  return <ChildComponent onIncrement={incrementCount} />;
}
```

### 6. useMemo

The `useMemo` hook is used to memoize expensive computations.

```jsx
import React, { useMemo } from 'react';

function ExpensiveComputation({ a, b }) {
  const result = useMemo(() => {
    // Assume this is an expensive computation
    return a * b;
  }, [a, b]);

  return <div>Result: {result}</div>;
}
```

### 7. useRef

The `useRef` hook creates a mutable ref object whose `.current` property is initialized to the passed argument.

```jsx
import React, { useRef, useEffect } from 'react';

function TextInputWithFocusButton() {
  const inputEl = useRef(null);

  const onButtonClick = () => {
    inputEl.current.focus();
  };

  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
}
```

## Creating Custom Hooks

Custom hooks allow you to extract component logic into reusable functions. Here's an example of creating a custom hook:

```jsx
import { useState, useEffect } from 'react';

function useWindowWidth() {
  const [width, setWidth] = useState(window.innerWidth);

  useEffect(() => {
    const handleResize = () => setWidth(window.innerWidth);
    window.addEventListener('resize', handleResize);
    return () => {
      window.removeEventListener('resize', handleResize);
    };
  }, []);

  return width;
}

// Usage
function MyComponent() {
  const width = useWindowWidth();
  return <div>Window width is {width}</div>;
}
```

When creating custom hooks:

1. Start the function name with "use" (e.g., `useWindowWidth`).
2. You can use other hooks inside your custom hook.
3. Custom hooks can return any value you want.
4. You can pass arguments to custom hooks if needed.

Custom hooks are a powerful way to share stateful logic between components without changing their structure.
