# ⚛️ `useReducer` for Complex State 

---

# 1️ What is `useReducer`?

`useReducer` is a React Hook used for **managing complex state logic**.

It is an alternative to `useState` when:

* State has multiple sub-values
* State transitions depend on previous state
* Logic is complex
* Multiple actions update state

Introduced in **React**.

---

# 2️ Why Not Just useState?

If state looks like this:

```js
const [user, setUser] = useState({
  name: "",
  email: "",
  isLoading: false,
  error: null
});
```

Updating nested properties becomes messy.

👉 `useReducer` solves this with structured state management.

---

# 3️ Basic Syntax

```js
const [state, dispatch] = useReducer(reducer, initialState);
```

* `state` → current state
* `dispatch()` → sends action
* `reducer` → function that updates state

---

# 4️ Reducer Function Structure

```js
const reducer = (state, action) => {
  switch (action.type) {
    case "INCREMENT":
      return { count: state.count + 1 };

    case "DECREMENT":
      return { count: state.count - 1 };

    default:
      return state;
  }
};
```

---

# 5️ Simple Counter Example

```js
import React, { useReducer } from "react";

const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case "INCREMENT":
      return { count: state.count + 1 };

    case "DECREMENT":
      return { count: state.count - 1 };

    case "RESET":
      return initialState;

    default:
      return state;
  }
}

export default function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <h2>{state.count}</h2>
      <button onClick={() => dispatch({ type: "INCREMENT" })}>+</button>
      <button onClick={() => dispatch({ type: "DECREMENT" })}>-</button>
      <button onClick={() => dispatch({ type: "RESET" })}>Reset</button>
    </div>
  );
}
```

---

# 6️ useReducer for Complex State (Real Example)

### Example: Login Form with Loading + Error

---

##  Step 1: Initial State

```js
const initialState = {
  user: null,
  loading: false,
  error: null
};
```

---

##  Step 2: Reducer

```js
function authReducer(state, action) {
  switch (action.type) {
    case "LOGIN_REQUEST":
      return { ...state, loading: true, error: null };

    case "LOGIN_SUCCESS":
      return { ...state, loading: false, user: action.payload };

    case "LOGIN_FAILURE":
      return { ...state, loading: false, error: action.payload };

    case "LOGOUT":
      return initialState;

    default:
      return state;
  }
}
```

---

##  Step 3: Component

```js
import React, { useReducer } from "react";

function Login() {
  const [state, dispatch] = useReducer(authReducer, initialState);

  const handleLogin = async () => {
    dispatch({ type: "LOGIN_REQUEST" });

    try {
      // fake API call
      const user = { name: "Guru", email: "guru@gmail.com" };

      setTimeout(() => {
        dispatch({ type: "LOGIN_SUCCESS", payload: user });
      }, 1000);

    } catch (err) {
      dispatch({ type: "LOGIN_FAILURE", payload: "Login failed" });
    }
  };

  return (
    <div>
      {state.loading && <p>Loading...</p>}
      {state.error && <p>{state.error}</p>}
      {state.user && <p>Welcome {state.user.name}</p>}

      <button onClick={handleLogin}>Login</button>
      <button onClick={() => dispatch({ type: "LOGOUT" })}>Logout</button>
    </div>
  );
}
```

---

# 7️ When to Use useReducer?

✅ Complex state logic
✅ Multiple sub-values
✅ State depends on previous state
✅ Large form handling
✅ When using Context API

---

# 8️ useReducer vs useState

| useState                | useReducer           |
| ----------------------- | -------------------- |
| Simple state            | Complex state        |
| Easy to use             | Structured           |
| Multiple setState calls | Single reducer logic |
| Small apps              | Large apps           |

---

# 9️ Key Concepts for Interview

###  What is dispatch?

Function used to send actions to reducer.

###  What is action?

An object with:

```js
{
  type: "ACTION_NAME",
  payload: data
}
```

###  Why is reducer pure?

Because it:

* Does not mutate state
* Returns new state
* Has no side effects

---

# 10 Advanced Pattern (Reducer + Context)

Used for global state management.

Example:

```js
const AuthContext = createContext();

<AuthContext.Provider value={{ state, dispatch }}>
```

This is similar to how **Redux** works internally.

---

#  Interview Definition 

"useReducer is a React Hook used for managing complex state logic using a reducer function. It provides structured state updates through actions and is useful when state transitions are complex."

---


# ⚛️ useReducer – Interview Questions (Beginner ➝ Advanced)


---

# 🟢 Beginner Level

### 1️ What is `useReducer`?

`useReducer` is a React Hook used to manage complex state logic using a reducer function.

---

### 2️ When should we use `useReducer` instead of `useState`?

When:

* State is complex
* Multiple related values exist
* State depends on previous state
* Many state transitions

---

### 3️ What does `useReducer` return?

```js
const [state, dispatch] = useReducer(reducer, initialState);
```

It returns:

* `state`
* `dispatch` function

---

### 4️ What is a reducer function?

A pure function that takes `(state, action)` and returns new state.

---

### 5️ What is dispatch?

A function used to send an action object to the reducer.

---

### 6️ What is an action?

An object like:

```js
{
  type: "ACTION_NAME",
  payload: data
}
```

---

### 7️ Why must reducer be pure?

Because it:

* Should not mutate state
* Should not cause side effects
* Must always return new state

---

# 🟡 Intermediate Level

### 8️ What is the difference between `useState` and `useReducer`?

| useState         | useReducer       |
| ---------------- | ---------------- |
| Simple state     | Complex state    |
| Multiple setters | Single reducer   |
| Less structured  | Structured logic |

---

### 9️ Can we use multiple reducers in one component?

Yes, you can use multiple `useReducer` hooks.

---

### 10 What happens if reducer returns undefined?

React throws an error. You must always return state.

---

### 1️1️ Why do we use switch-case in reducer?

To handle multiple action types clearly and maintain structure.

---

### 1️2️ Can reducer contain API calls?

No ❌
Reducer must be pure.
API calls should be outside (inside event handler or useEffect).

---

### 1️3 How does useReducer help with form handling?

It centralizes form logic and handles multiple fields in one structured state.

---

### 1️4️ What is lazy initialization in useReducer?

```js
const [state, dispatch] = useReducer(reducer, initialArg, initFunction);
```

Used when initial state calculation is expensive.

---

# 🔴 Advanced Level

### 1️5️ How does useReducer improve performance?

It reduces unnecessary re-renders by centralizing state updates.

---

### 1️6️ How is useReducer related to Redux?

`useReducer` works similarly to **Redux**:

* Both use reducers
* Both use actions
* Both follow predictable state transitions

---

### 1️7️ Can we combine useReducer with Context?

Yes ✅
This is a common pattern for global state management in **React**.

---

### 1️8️ What are side effects in reducer?

Things like:

* API calls
* setTimeout
* DOM manipulation

Reducers should not contain them.

---

### 1️9️ How does immutability work in useReducer?

We must return a new state object:

```js
return { ...state, count: state.count + 1 }
```

Never modify directly:

```js
state.count++ ❌
```

---

### 2️0️ What are common mistakes in useReducer?

* Mutating state
* Forgetting default case
* Doing async inside reducer
* Not structuring actions properly

---

### 2️1️ If you have a login form with loading and error state, would you use useState or useReducer?

useReducer, because multiple related states are involved.

---

### 2️2️ If state is just a boolean toggle, should we use useReducer?

No, useState is better for simple state.

---

### 2️3️ How would you manage global auth state without Redux?

Use `useReducer` + Context API.

---

### 2️4️ How do you reset state in useReducer?

Return `initialState` in a RESET action.

---
---

