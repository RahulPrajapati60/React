# ⚛️ `useReducer` for Complex State 

---

# 1️⃣ What is `useReducer`?

`useReducer` is a React Hook used for **managing complex state logic**.

It is an alternative to `useState` when:

* State has multiple sub-values
* State transitions depend on previous state
* Logic is complex
* Multiple actions update state

Introduced in **React**.

---

# 2️⃣ Why Not Just useState?

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

# 3️⃣ Basic Syntax

```js
const [state, dispatch] = useReducer(reducer, initialState);
```

* `state` → current state
* `dispatch()` → sends action
* `reducer` → function that updates state

---

# 4️⃣ Reducer Function Structure

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

# 5️⃣ Simple Counter Example

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

# 6️⃣ useReducer for Complex State (Real Example)

### Example: Login Form with Loading + Error

---

## 🔹 Step 1: Initial State

```js
const initialState = {
  user: null,
  loading: false,
  error: null
};
```

---

## 🔹 Step 2: Reducer

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

## 🔹 Step 3: Component

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

# 7️⃣ When to Use useReducer?

✅ Complex state logic
✅ Multiple sub-values
✅ State depends on previous state
✅ Large form handling
✅ When using Context API

---

# 8️⃣ useReducer vs useState

| useState                | useReducer           |
| ----------------------- | -------------------- |
| Simple state            | Complex state        |
| Easy to use             | Structured           |
| Multiple setState calls | Single reducer logic |
| Small apps              | Large apps           |

---

# 9️⃣ Key Concepts for Interview

### 🔹 What is dispatch?

Function used to send actions to reducer.

### 🔹 What is action?

An object with:

```js
{
  type: "ACTION_NAME",
  payload: data
}
```

### 🔹 Why is reducer pure?

Because it:

* Does not mutate state
* Returns new state
* Has no side effects

---

# 🔟 Advanced Pattern (Reducer + Context)

Used for global state management.

Example:

```js
const AuthContext = createContext();

<AuthContext.Provider value={{ state, dispatch }}>
```

This is similar to how **Redux** works internally.

---

# 🎯 Interview Definition 

"useReducer is a React Hook used for managing complex state logic using a reducer function. It provides structured state updates through actions and is useful when state transitions are complex."

---
