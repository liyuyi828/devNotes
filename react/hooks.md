# React Hooks

## Basic Hooks

### useState
**When to use:** Managing local component state.

```jsx
import { useState } from 'react';

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

### useEffect
**When to use:** Side effects like data fetching, subscriptions, or manually changing the DOM.

```jsx
import { useState, useEffect } from 'react';

function UserProfile({ userId }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    fetchUser(userId).then(setUser);
  }, [userId]); // Dependency array

  return user ? <div>{user.name}</div> : <div>Loading...</div>;
}
```

### useContext
**When to use:** Consuming context values without nested components.

```jsx
import { createContext, useContext, useState } from 'react';

// 1. Create the context with default value
const ThemeContext = createContext({
  theme: 'light',
  toggleTheme: () => {}
});

// 2. Create a provider component
function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');

  const toggleTheme = () => {
    setTheme(prevTheme => prevTheme === 'light' ? 'dark' : 'light');
  };

  const value = {
    theme,
    toggleTheme
  };

  return (
    <ThemeContext.Provider value={value}>
      {children}
    </ThemeContext.Provider>
  );
}

// 3. Custom hook to use the context
function useTheme() {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error('useTheme must be used within a ThemeProvider');
  }
  return context;
}

// 4. Components that consume the context
function Button() {
  const { theme, toggleTheme } = useTheme();

  return (
    <button
      style={{
        backgroundColor: theme === 'light' ? '#fff' : '#333',
        color: theme === 'light' ? '#333' : '#fff'
      }}
      onClick={toggleTheme}
    >
      Toggle Theme ({theme})
    </button>
  );
}

function Header() {
  const { theme } = useTheme();

  return (
    <header style={{
      backgroundColor: theme === 'light' ? '#f0f0f0' : '#222'
    }}>
      <h1>My App</h1>
    </header>
  );
}

// 5. App component wrapped with provider
function App() {
  return (
    <ThemeProvider>
      <div>
        <Header />
        <Button />
      </div>
    </ThemeProvider>
  );
}
```

**Key Points:**
- **Context Creation**: Use `createContext()` with optional default value
- **Provider Required**: Yes, you need `Context.Provider` to set values
- **Provider Placement**: Wrap components that need access to the context
- **Error Handling**: Check if context exists when consumed outside provider

## Additional Hooks

### useReducer
**When to use:** Complex state logic or when next state depends on previous state.

```jsx
import { useReducer } from 'react';

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
    <div>
      Count: {state.count}
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
    </div>
  );
}
```

### useCallback
**When to use:** Memoizing functions to prevent unnecessary re-renders of child components.

```jsx
import { useState, useCallback } from 'react';

function TodoList({ todos, onToggle }) {
  const [filter, setFilter] = useState('all');

  const handleToggle = useCallback((id) => {
    onToggle(id);
  }, [onToggle]);

  return (
    <ul>
      {todos.map(todo => (
        <TodoItem
          key={todo.id}
          todo={todo}
          onToggle={handleToggle}
        />
      ))}
    </ul>
  );
}
```

### useMemo
**When to use:** Memoizing expensive calculations.

```jsx
import { useMemo } from 'react';

function ExpensiveComponent({ items, filter }) {
  const filteredItems = useMemo(() => {
    return items.filter(item => item.category === filter);
  }, [items, filter]);

  return (
    <ul>
      {filteredItems.map(item => (
        <li key={item.id}>{item.name}</li>
      ))}
    </ul>
  );
}
```

### useRef
**When to use:** Accessing DOM elements directly or persisting values across renders.

```jsx
import { useRef, useEffect } from 'react';

function TextInput() {
  const inputRef = useRef(null);

  useEffect(() => {
    inputRef.current.focus();
  }, []);

  return <input ref={inputRef} type="text" />;
}
```

### useImperativeHandle
**When to use:** Customizing the instance value exposed to parent components when using ref.

```jsx
import { forwardRef, useImperativeHandle, useRef } from 'react';

const FancyInput = forwardRef((props, ref) => {
  const inputRef = useRef();

  useImperativeHandle(ref, () => ({
    focus: () => {
      inputRef.current.focus();
    },
    getValue: () => {
      return inputRef.current.value;
    }
  }));

  return <input ref={inputRef} {...props} />;
});
```

### useLayoutEffect
**When to use:** Synchronous DOM mutations before browser paint (rare use cases).

```jsx
import { useLayoutEffect, useRef, useState } from 'react';

function Tooltip() {
  const ref = useRef();
  const [tooltipHeight, setTooltipHeight] = useState(0);

  useLayoutEffect(() => {
    const height = ref.current.scrollHeight;
    setTooltipHeight(height);
  });

  return (
    <div ref={ref} style={{ top: -tooltipHeight }}>
      Tooltip content
    </div>
  );
}
```

### useDebugValue
**When to use:** Displaying custom hook values in React DevTools.

```jsx
import { useState, useDebugValue } from 'react';

function useOnlineStatus() {
  const [isOnline, setIsOnline] = useState(true);

  useDebugValue(isOnline ? 'Online' : 'Offline');

  // ... hook logic

  return isOnline;
}
```

## React 18+ Hooks

### useId
**When to use:** Generating unique IDs for accessibility attributes.

```jsx
import { useId } from 'react';

function PasswordField() {
  const passwordHintId = useId();

  return (
    <>
      <input
        type="password"
        aria-describedby={passwordHintId}
      />
      <p id={passwordHintId}>
        Password should contain at least 8 characters
      </p>
    </>
  );
}
```

### useDeferredValue
**When to use:** Deferring updates to less critical parts of UI.

```jsx
import { useDeferredValue, useState } from 'react';

function SearchResults({ query }) {
  const deferredQuery = useDeferredValue(query);

  return <ExpensiveSearchResults query={deferredQuery} />;
}
```

### useTransition
**When to use:** Marking state updates as non-urgent to keep UI responsive.

```jsx
import { useTransition, useState } from 'react';

function SearchBox() {
  const [isPending, startTransition] = useTransition();
  const [query, setQuery] = useState('');

  function handleChange(e) {
    startTransition(() => {
      setQuery(e.target.value);
    });
  }

  return (
    <div>
      <input onChange={handleChange} />
      {isPending && <div>Loading...</div>}
    </div>
  );
}
```

### useSyncExternalStore
**When to use:** Subscribing to external data sources.

```jsx
import { useSyncExternalStore } from 'react';

function useWindowWidth() {
  return useSyncExternalStore(
    (callback) => {
      window.addEventListener('resize', callback);
      return () => window.removeEventListener('resize', callback);
    },
    () => window.innerWidth
  );
}

function Component() {
  const width = useWindowWidth();
  return <div>Window width: {width}</div>;
}
```

### useInsertionEffect
**When to use:** Inserting styles before DOM mutations (CSS-in-JS libraries).

```jsx
import { useInsertionEffect } from 'react';

function useCSS(rule) {
  useInsertionEffect(() => {
    const style = document.createElement('style');
    style.textContent = rule;
    document.head.appendChild(style);
    return () => document.head.removeChild(style);
  });
}
```

## Hook Rules

1. **Only call hooks at the top level** - Don't call hooks inside loops, conditions, or nested functions
2. **Only call hooks from React functions** - Call them from React function components or custom hooks
3. **Custom hooks must start with "use"** - This allows React to verify hook rules

## Custom Hooks

Custom hooks are JavaScript functions that start with "use" and can call other hooks:

```jsx
function useCounter(initialValue = 0) {
  const [count, setCount] = useState(initialValue);

  const increment = useCallback(() => {
    setCount(c => c + 1);
  }, []);

  const decrement = useCallback(() => {
    setCount(c => c - 1);
  }, []);

  return { count, increment, decrement };
}

// Usage
function Counter() {
  const { count, increment, decrement } = useCounter(5);

  return (
    <div>
      <p>{count}</p>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
    </div>
  );
}
```