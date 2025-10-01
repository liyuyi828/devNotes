# React Key Concepts

## Rendering Strategies

### Server-Side Rendering (SSR)
- **Definition**: HTML is generated on the server and sent to the client
- **Benefits**: Better SEO, faster initial page load, improved performance on slow devices
- **Drawbacks**: Increased server load, more complex deployment, hydration issues
- **Use Cases**: Content-heavy sites, e-commerce, marketing pages
- **Tools**: Next.js, Remix, Gatsby (SSG)

### Client-Side Rendering (CSR)
- **Definition**: JavaScript renders the entire application in the browser
- **Benefits**: Rich interactivity, reduced server load, better user experience after initial load
- **Drawbacks**: Slower initial load, SEO challenges, requires JavaScript
- **Use Cases**: SPAs, dashboards, admin panels
- **Tools**: Create React App, Vite

### Static Site Generation (SSG)
- **Definition**: HTML pages are pre-built at build time
- **Benefits**: Fastest loading, great SEO, can be served from CDN
- **Drawbacks**: Not suitable for dynamic content, requires rebuild for updates
- **Use Cases**: Blogs, documentation, marketing sites

### Incremental Static Regeneration (ISR)
- **Definition**: Combines SSG with on-demand regeneration
- **Benefits**: Static performance with dynamic content capability
- **Use Cases**: E-commerce product pages, news sites

## Component Patterns

### Controlled Components
- Form inputs where React controls the value via state
- Single source of truth
- Predictable data flow

### Uncontrolled Components
- Form inputs that manage their own state internally
- Use refs to access values
- Less React overhead

### Higher-Order Components (HOC)
- Function that takes a component and returns a new component
- Used for cross-cutting concerns (authentication, logging)
- Being replaced by hooks in modern React

### Render Props
- Technique for sharing code between components using a prop whose value is a function
- Provides flexibility in what gets rendered

### Compound Components
- Components that work together to form a complete UI
- Example: `<Select>`, `<Option>` components

## State Management

### Local State (useState)
- Component-level state management
- Best for simple, isolated state

### Lifted State
- Moving state up to common ancestor
- Enables sharing state between siblings

### Context API
- Built-in state management for avoiding prop drilling
- Good for theme, user authentication, language settings
- Can cause performance issues if overused

### External State Management
- **Redux**: Predictable state container, time-travel debugging
- **Zustand**: Lightweight alternative to Redux
- **Jotai**: Atomic approach to state management
- **Recoil**: Facebook's experimental state management

## Performance Optimization

### React.memo
- Higher-order component that memoizes the result
- Prevents unnecessary re-renders when props haven't changed
- Shallow comparison by default

### useMemo
- Memoizes expensive calculations
- Only recalculates when dependencies change
- Avoid premature optimization

### useCallback
- Memoizes function references
- Prevents child re-renders when passing callbacks
- Useful when function is a dependency

### Code Splitting
- **React.lazy()**: Dynamic imports for components
- **Suspense**: Handles loading states for lazy components
- **Bundle splitting**: Separate vendor and app code

### Virtual DOM Optimization
- **Key prop**: Helps React identify which items have changed
- **Fragment**: Avoids extra DOM nodes
- **Portal**: Render children into different DOM subtree

## Advanced Hooks

### useReducer
- Alternative to useState for complex state logic
- Predictable state transitions
- Good for state machines

### useContext
- Consume context values
- Alternative to Context.Consumer

### useEffect Patterns
- **Cleanup**: Return function for cleanup logic
- **Dependencies**: Control when effect runs
- **Conditional effects**: Use conditions inside useEffect

### Custom Hooks
- Extract component logic into reusable functions
- Share stateful logic between components
- Must start with "use"

## Development Tools & Practices

### React Strict Mode
- **Purpose**: Identifies potential problems in development
- **Features**: Double-invokes functions, warns about deprecated APIs
- **Benefits**: Catches side effects, prepares for Concurrent Mode
- **Usage**: Wrap app or components in `<React.StrictMode>`

### React Developer Tools
- Browser extension for debugging React
- Component tree inspection
- Props and state examination
- Profiler for performance analysis

### Error Boundaries
- Components that catch JavaScript errors in component tree
- Prevent entire app from crashing
- Only catch errors during rendering, lifecycle methods, and constructors

## Concurrent Features (React 18+)

### Automatic Batching
- Multiple state updates are batched into single re-render
- Improves performance by reducing renders

### Transitions
- **startTransition**: Mark updates as non-urgent
- **useDeferredValue**: Defer expensive updates
- Keeps UI responsive during heavy computations

### Suspense for Data Fetching
- Declarative way to handle async operations
- Better than loading states in components
- Works with React Query, SWR, Relay

## Testing Concepts

### Testing Library Principles
- Test behavior, not implementation
- Query by accessibility roles and labels
- Avoid testing internal state directly

### Common Testing Patterns
- **Rendering**: Use `render()` from testing library
- **User interactions**: `fireEvent` or `userEvent`
- **Async testing**: `waitFor`, `findBy` queries
- **Mocking**: Mock API calls, external dependencies

## Build & Bundling

### Webpack Configuration
- Entry points, loaders, plugins
- Code splitting strategies
- Hot module replacement

### Vite
- Fast build tool with instant HMR
- ES modules in development
- Optimized production builds

### Module Federation
- Share code between separate applications
- Micro-frontend architecture
- Runtime code sharing

## TypeScript Integration

### Component Typing
- `React.FC` vs function declarations
- Props interfaces and type safety
- Generic components

### Hook Typing
- Typed useState and useReducer
- Custom hook type definitions
- Context typing patterns