
## 1️ What is `useEffect`? 

useEffect `useEffect` is a **React Hook** that lets you run **side effects** in a component.

### Side effects

* Fetching data from an API
* Calling backend
* Updating DOM manually
* Using timers (`setTimeout`, `setInterval`)
* Listening to events

**Key idea:**

> `useEffect` runs **after the component renders**


## 2️ Basic Syntax of `useEffect`

```js
useEffect(() => {
  // code runs here
}, []);
```

### Explanation

* **First argument** → function (what to run)
* **Second argument** → dependency array (when to run)

---

## 3️ Dependency Array Explained (VERY IMPORTANT)

### 1. Run on every render (rarely used)

```js
useEffect(() => {
  console.log("Runs every render");
});
```

---

### 2. Run only ONCE (most common for API calls) 

```js
useEffect(() => {
  console.log("Runs only once when component loads");
}, []);
```

---

### 3. Run when a value changes 

```js
useEffect(() => {
  console.log("Runs when count changes");
}, [count]);
```

---

## 4️ What is Fetch API?

Fetch API is used to **get data from backend / server**

Example API

```
https://jsonplaceholder.typicode.com/users
```

---

## 5️ Basic Fetch API Example (JS only)

```js
fetch("https://jsonplaceholder.typicode.com/users")
  .then((response) => response.json())
  .then((data) => console.log(data))
  .catch((error) => console.log(error));
```

---

## 6️ useEffect + Fetch API (REAL React Example)

###  Step-by-step example

```js
import React, { useEffect, useState } from "react";

function Users() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/users")
      .then((response) => response.json())
      .then((data) => {
        setUsers(data);
      })
      .catch((error) => {
        console.log(error);
      });
  }, []); // runs once

  return (
    <div>
      <h2>User List</h2>

      {users.map((user) => (
        <p key={user.id}>{user.name}</p>
      ))}
    </div>
  );
}

export default Users;
```

---

## 7️ Line-by-Line Explanation

```js
const [users, setUsers] = useState([]);
```

 State to store API data

---

```js
useEffect(() => {
```

 Run side effect after component loads

---

```js
fetch("https://jsonplaceholder.typicode.com/users")
```

 Call backend API

---

```js
.then((response) => response.json())
```

 Convert response to JSON

---

```js
.then((data) => {
  setUsers(data);
});
```

 Save data in state → triggers re-render

---

```js
}, []);
```

 Empty array → run only once

---

## 8️ useEffect with async/await

```js
useEffect(() => {
  const fetchUsers = async () => {
    try {
      const response = await fetch(
        "https://jsonplaceholder.typicode.com/users"
      );
      const data = await response.json();
      setUsers(data);
    } catch (error) {
      console.log(error);
    }
  };

  fetchUsers();
}, []);
```

 **Why async function inside useEffect?**
Because `useEffect` callback **cannot be async directly**

---

## 9️ Common Mistakes 

 Forgetting dependency array → infinite API calls
 Calling async directly in useEffect
 Not handling errors
 Not using `key` while mapping

---
