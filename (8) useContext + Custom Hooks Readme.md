##  `useContext` + `Custom Hooks`

---

#  1. `useContext` Hook

###  What is `useContext`?

`useContext` is a React Hook that allows you to access data from a **Context** without passing props manually at every level (no prop drilling).

It is used with:

* `createContext()`
* `<Context.Provider>`
* `useContext()`

---

##  Why We Need It?
`When building React apps, we often need to share data across many components.`

Example:

1 Logged-in user
2 Theme (dark/light)
3 Language
4 Cart data
5 Authentication state

Problem:
Passing props deeply like this 

```
App → Layout → Sidebar → UserProfile
```

Instead of passing `user` in every component, we use Context.

---

#  Basic Example

### 1️ Create Context

```jsx
import { createContext } from "react";

export const UserContext = createContext();
```

---

### 2️ Provide Context

```jsx
import { UserContext } from "./UserContext";

function App() {
  const user = "Rahul";

  return (
    <UserContext.Provider value={user}>
      <Dashboard />
    </UserContext.Provider>
  );
}
```

---

### 3️ Consume Context

```jsx
import { useContext } from "react";
import { UserContext } from "./UserContext";

function Dashboard() {
  const user = useContext(UserContext);

  return <h1>Welcome {user}</h1>;
}
```

---

#  Intermediate Example (With State)

```jsx
import { createContext, useState } from "react";

export const ThemeContext = createContext();

export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState("light");

  const toggleTheme = () =>
    setTheme(prev => (prev === "light" ? "dark" : "light"));

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}
```

Usage:

```jsx
import { useContext } from "react";
import { ThemeContext } from "./ThemeProvider";

function Navbar() {
  const { theme, toggleTheme } = useContext(ThemeContext);

  return (
    <button onClick={toggleTheme}>
      Current Theme: {theme}
    </button>
  );
}
```

---

#  Problems with Raw `useContext`

❌ Repeating `useContext(ThemeContext)` everywhere
❌ No error handling if used outside provider
❌ Hard to maintain in large apps

---

#  🟢🟣Solution: Custom Hook (Industry Pattern)

---

#  2. Custom Hooks

###  What is Custom Hook?

A custom hook is a **normal JavaScript function** that:

* Starts with `use`
* Uses other hooks internally
* Reuses logic across components

---

#  Basic Custom Hook Example

```jsx
import { useState } from "react";

export function useCounter() {
  const [count, setCount] = useState(0);

  const increment = () => setCount(c => c + 1);
  const decrement = () => setCount(c => c - 1);

  return { count, increment, decrement };
}
```

Usage:

```jsx
function Counter() {
  const { count, increment, decrement } = useCounter();

  return (
    <>
      <h2>{count}</h2>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
    </>
  );
}
```

---

#  Industry Level: useContext + Custom Hook Together

This is the **real-world pattern**.

---

##  Step 1: Create Context

```jsx
import { createContext } from "react";

export const AuthContext = createContext(null);
```

---

##  Step 2: Create Provider

```jsx
import { useState } from "react";
import { AuthContext } from "./AuthContext";

export function AuthProvider({ children }) {
  const [user, setUser] = useState(null);

  const login = (userData) => {
    setUser(userData);
  };

  const logout = () => {
    setUser(null);
  };

  const value = {
    user,
    login,
    logout,
    isAuthenticated: !!user,
  };

  return (
    <AuthContext.Provider value={value}>
      {children}
    </AuthContext.Provider>
  );
}
```

---

##  Step 3: Create Custom Hook (IMPORTANT )

```jsx
import { useContext } from "react";
import { AuthContext } from "./AuthContext";

export function useAuth() {
  const context = useContext(AuthContext);

  if (!context) {
    throw new Error("useAuth must be used inside AuthProvider");
  }

  return context;
}
```

---

##  Step 4: Use in Components

```jsx
import { useAuth } from "./useAuth";

function Profile() {
  const { user, logout } = useAuth();

  return (
    <div>
      <h2>{user?.name}</h2>
      <button onClick={logout}>Logout</button>
    </div>
  );
}
```

---

#  Why This Is Industry Standard?

✅ Clean components
✅ Error safety
✅ Reusable logic
✅ Scalable architecture
✅ Centralized state

---

#  Advanced Pattern (With Reducer)

For big apps, we use:

* `useReducer`
* Context
* Custom hook

---

## Example

```jsx
import { createContext, useReducer } from "react";

const CartContext = createContext();

const initialState = {
  items: [],
};

function cartReducer(state, action) {
  switch (action.type) {
    case "ADD":
      return { ...state, items: [...state.items, action.payload] };
    case "REMOVE":
      return {
        ...state,
        items: state.items.filter(item => item.id !== action.payload),
      };
    default:
      return state;
  }
}

export function CartProvider({ children }) {
  const [state, dispatch] = useReducer(cartReducer, initialState);

  return (
    <CartContext.Provider value={{ ...state, dispatch }}>
      {children}
    </CartContext.Provider>
  );
}
```

Custom Hook:

```jsx
import { useContext } from "react";
import { CartContext } from "./CartProvider";

export function useCart() {
  const context = useContext(CartContext);

  if (!context) {
    throw new Error("useCart must be used inside CartProvider");
  }

  return context;
}
```

---

# ⚡ Advanced Concepts (Interview Level)

### 🔹 When NOT to use Context?

* Frequently changing large data
* High performance apps
* Instead use:

  * Redux
  * Zustand
  * React Query

---

### 🔹 Performance Optimization

```jsx
const value = useMemo(() => ({
  user,
  login,
  logout
}), [user]);
```

Prevent unnecessary re-renders.

---

Redux → external state manager
Context → built-in lightweight state sharing

### 4️⃣ When to use useReducer with Context?

Complex state logic (cart, auth, dashboard)

---

#  Real Industry Folder Structure

```
/context
   AuthContext.js
   AuthProvider.js
   useAuth.js

/hooks
   useCounter.js
   useDebounce.js
   useFetch.js
```

---

# 🎯 Final Summary

| Concept              | Purpose             |
| -------------------- | ------------------- |
| createContext        | Create global store |
| Provider             | Supply data         |
| useContext           | Consume data        |
| Custom Hook          | Reusable logic      |
| useReducer + Context | Complex state       |

---


# 🏆 `useContext` + Custom Hooks — Interview Questions (Basic ➝ Advanced)

---

## 🟢 Basic Level

### 1️⃣ What is `useContext`?

`useContext` is a React Hook that allows components to consume values from a Context without prop drilling.
It provides direct access to global/shared data.

---

### 2️⃣ What problem does Context solve?

It solves **prop drilling**, where props must be passed through multiple unnecessary components.

---

### 3️⃣ What is Prop Drilling?

Passing props from parent to deeply nested child components even if intermediate components don’t need them.

---

### 4️⃣ What are the three steps to use Context?

1. `createContext()`
2. Wrap components with `Provider`
3. Use `useContext()` to consume values

---

### 5️⃣ What is a Custom Hook?

A reusable function that starts with `use` and can use other React hooks inside it.

---

## 🟡 Intermediate Level

### 6️⃣ Why combine Custom Hook with Context?

To:

* Avoid repeating `useContext`
* Add error handling
* Keep components clean
* Follow industry best practices

---

### 7️⃣ What happens if `useContext` is used outside a Provider?

It returns `undefined` (or default value).
Industry pattern: throw an error inside custom hook.

---

### 8️⃣ How do you prevent unnecessary re-renders in Context?

Use `useMemo()` for value object.

```jsx
const value = useMemo(() => ({ user, login }), [user]);
```

---

### 9️⃣ Can Context replace Redux?

For small/medium apps → Yes
For large complex apps → Redux/Zustand is better

---

### 🔟 When should you NOT use Context?

* Frequently changing data
* Large-scale state management
* Performance-sensitive apps

---

## 🔵 Advanced Level

### 1️⃣1️⃣ Why does Context cause re-renders?

When Provider value changes, **all consumers re-render**, even if they use only one part of the value.

---

### 1️⃣2️⃣ How to optimize Context performance?

* Split contexts (AuthContext, ThemeContext separately)
* Use `useMemo`
* Use `useReducer` for complex logic

---

### 1️⃣3️⃣ Difference between `useContext` and `useReducer`?

`useContext` → Share state
`useReducer` → Manage complex state logic
Together → Powerful global state system

---

### 1️⃣4️⃣ Explain Context with useReducer pattern.

We wrap reducer inside Provider and expose state + dispatch via custom hook.

This is similar to a lightweight Redux.

---

### 1️⃣5️⃣ What is the industry pattern for Context?

✔ Create Context
✔ Create Provider
✔ Create Custom Hook (`useAuth`)
✔ Throw error if used outside provider
✔ Memoize value

---

## 🟣 Scenario-Based Questions

### 1️⃣6️⃣ How would you implement authentication using Context?

Store:

* `user`
* `login()`
* `logout()`
* `isAuthenticated`

Wrap entire app with `AuthProvider`.

---

### 1️⃣7️⃣ How would you create protected routes?

Check `isAuthenticated` from context and conditionally render route or redirect.

---

### 1️⃣8️⃣ What are alternatives to Context?

* Redux
* Zustand
* Recoil
* React Query (for server state)

---

### 1️⃣9️⃣ Difference between Context API and Redux?

| Context             | Redux               |
| ------------------- | ------------------- |
| Built-in            | External library    |
| Simple setup        | Boilerplate         |
| Good for small apps | Good for large apps |

---

### 2️⃣0️⃣ What is the rule for naming custom hooks?

Must start with `use` so React can detect Hook usage.

---

#  One-Line Interview Summary

> `useContext` helps share global state without prop drilling, and combining it with custom hooks makes the code scalable and industry-ready.

---

