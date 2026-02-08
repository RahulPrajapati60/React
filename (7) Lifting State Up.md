
## 1️⃣ Lifting State Up 

### 🔹 What is Lifting State Up?

**Definition 
Lifting state up means **moving state to the closest common parent component** so that multiple child components can **share and sync the same data**.
It helps avoid **duplicate state** and keeps **single source of truth** in the parent.

---

### 🔹 Why Lifting State Up?

* When two or more components need **same state**
* To keep data **consistent**
* To manage state at **one place (parent)**

---

### 🔹 Without Lifting State (Problem)

```jsx
function ChildA() {
  const [count, setCount] = React.useState(0);
  return <button onClick={() => setCount(count + 1)}>A: {count}</button>;
}

function ChildB() {
  const [count, setCount] = React.useState(0);
  return <p>B: {count}</p>;
}
```

❌ Problem: `ChildA` and `ChildB` have **separate state**, not shared.

---

### ✅ With Lifting State Up (Correct Way)

```jsx
import React, { useState } from "react";

function ChildA({ count, setCount }) {
  return <button onClick={() => setCount(count + 1)}>A: {count}</button>;
}

function ChildB({ count }) {
  return <p>B: {count}</p>;
}

export default function Parent() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <ChildA count={count} setCount={setCount} />
      <ChildB count={count} />
    </div>
  );
}
```

### 🔹 Interview Lines:

* State ko parent me le jaake **children ko props ke through pass karna** = Lifting State Up
* Isse **single source of truth** maintain hota hai
* Sibling components ke beech data share karna easy hota hai

---

### 🔹 Real-life Example

Form ke multiple inputs ka state parent me rakhna, taaki submit pe **saara data ek jagah mile**.

---

## 2️⃣ Context API (Basic → In-depth)

### 🔹 What is Context API?

**Definition :**
Context API is used to **share global data** across components **without passing props at every level (prop drilling)**.
It is useful for **theme, user login, language, auth state**, etc.

---

### 🔹 What is Prop Drilling?

Parent → Child → GrandChild → GreatGrandChild
Same prop baar-baar pass karna = **Prop Drilling** ❌

---

### 🔹 When to use Context?

* Auth user info
* Theme (dark/light)
* Language
* Global settings

---

### ✅ Context API 

#### 1️⃣ Create Context

```jsx
import { createContext } from "react";

export const UserContext = createContext(null);
```

---

#### 2️⃣ Provide Context (Parent / App)

```jsx
import React, { useState } from "react";
import { UserContext } from "./UserContext";
import Child from "./Child";

export default function App() {
  const [user, setUser] = useState("Guru");

  return (
    <UserContext.Provider value={{ user, setUser }}>
      <Child />
    </UserContext.Provider>
  );
}
```

---

#### 3️⃣ Consume Context (Any Deep Child)

```jsx
import React, { useContext } from "react";
import { UserContext } from "./UserContext";

export default function Child() {
  const { user, setUser } = useContext(UserContext);

  return (
    <div>
      <h2>User: {user}</h2>
      <button onClick={() => setUser("React Dev")}>Change User</button>
    </div>
  );
}
```

---

### 🔹 How Context Works 

* `createContext()` → context banata hai
* `Provider` → data provide karta hai
* `useContext()` → data consume karta hai
* Beech ke components ko props pass nahi karne padte

---

## 3️⃣ Lifting State Up vs Context API (Difference)

| Feature      | Lifting State Up        | Context API             |
| ------------ | ----------------------- | ----------------------- |
| Scope        | Parent-Child / Siblings | Global                  |
| Props needed | Yes                     | No                      |
| Best for     | Small component tree    | Deep tree / global data |
| Example      | Form inputs             | Auth, Theme             |

**Interview Line:**
👉 “Small app me lifting state up best hota hai, but deep component tree me prop drilling se bachne ke liye Context API use karte hain.”

---

## 4️⃣ Common Interview Questions

### ❓ Can Context replace Redux?

**Answer:**
Context API is good for **small to medium global state**.
Redux is better for **large apps**, complex state logic, middleware, debugging tools.

---

### ❓ Performance Issue with Context?

**Answer:**
Context value change hone par **all consumers re-render hote hain**.
Isliye large data ke liye multiple contexts ya memoization use karte hain.

---

## 5️⃣ README.md Style Notes 

````md
# React Notes – Lifting State Up & Context API

## Lifting State Up
Lifting state up means moving state to the closest common parent so multiple child components can share the same data.
It helps maintain a single source of truth and avoids duplicate state.

### Example
```jsx
function Parent() {
  const [count, setCount] = useState(0);
  return (
    <>
      <ChildA count={count} setCount={setCount} />
      <ChildB count={count} />
    </>
  );
}
````

## Context API

Context API is used to share data globally without passing props at every level (avoids prop drilling).

### Steps

1. Create Context
2. Wrap with Provider
3. Consume using useContext

### Example

```jsx
const MyContext = createContext();

<MyContext.Provider value={data}>
  <App />
</MyContext.Provider>
```

```

---

## 6️⃣ Quick Revision

- **Lifting State Up:** Share state between siblings by moving state to parent  
- **Context API:** Share global data without prop drilling  
- **Difference:** Lifting = local sharing, Context = global sharing  
