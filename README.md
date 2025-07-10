# Why React?
### We can use states which means that once we update the state variable, it changes across the page
### We can split our app into multiple components and reuse those components
### React uses a virtual DOM to efficiently update the UI which is better than updating content using DOM Manipulation
### Debugging and maintainance is easy

# useEffect
### In React, the `useEffect` hook handles side effects in functional components, like data fetching or DOM updates. It runs after render and takes a callback function and an optional dependency array
```
const [count, setCount] = useState(0)
const [first, setCount] = useState(0)
const [color, setCount] = useState(0)


// Case 1: Run on every render 
  useEffect(() => {
    alert("Hey I will run on every render")
  })


  // Case 2: Run only on first render 
  useEffect(() => {
    alert("Hey welcome to my page. This is the first render")
  }, [])


  // Case 3: Run only when certain values change
  useEffect(() => {
    alert("Hey I am running because color was changed")
  }, [color])


  // Example of Cleanup function
  useEffect(() => {
    alert("Hey welcome to my page. This is the first render of app.jsx")

    return () => {
      alert("component was unmounted")
    }
  }, [])
```
# The useRef Hook
### The useRef hook in React creates a mutable reference object that persists across renders without triggering re-renders when its value changes. It's primarily used to access DOM elements or store mutable values that don't affect the component's rendering.
#### you learn more content of useRef by using this link `(https://react.dev/reference/react/useRef)`

## Condition Reandering and Rendering List

---

### **Conditional Rendering in React**

### Conditional rendering allows you to display different UI elements based on certain conditions, similar to using `if` statements in JavaScript.

#### **Key Approaches**
1. **Using `if` Statements**:
   You can use regular JavaScript `if` conditions outside the JSX to determine what to render.

   ```jsx
   function Greeting({ isLoggedIn }) {
     if (isLoggedIn) {
       return <h1>Welcome back!</h1>;
     } else {
       return <h1>Please sign in.</h1>;
     }
   }
   ```

2. **Using Ternary Operator**:
   For inline conditional rendering within JSX, the ternary operator is concise.

   ```jsx
   function Greeting({ isLoggedIn }) {
     return (
       <div>
         {isLoggedIn ? <h1>Welcome back!</h1> : <h1>Please sign in.</h1>}
       </div>
     );
   }
   ```

3. **Using Logical && Operator**:
   
   Render something only if a condition is true, otherwise render nothing.

   ```jsx
   function Notification({ hasUnread }) {
     return (
       <div>
         {hasUnread && <span>You have unread messages!</span>}
       </div>
     );
   }
   ```

5. **Using null or Early Return**:
   
   Return `null` to render nothing, or use early returns to skip rendering.

   ```jsx
   function Component({ show }) {
     if (!show) return null;
     return <div>This is visible!</div>;
   }
   ```

---

### **Rendering Lists in React**

Rendering lists involves displaying a collection of items, typically using the `map()` function to iterate over an array and generate JSX elements.

#### **Basic Example**
```jsx
function ItemList({ items }) {
  return (
    <ul>
      {items.map((item) => (
        <li key={item.id}>{item.name}</li>
      ))}
    </ul>
  );
}
```

#### **Key Points**
1. **Key Prop**:
   - React requires a unique `key` prop for each element in a list to optimize rendering and track elements efficiently.
   - Use a unique identifier (e.g., `item.id`) rather than the array index unless the list is static and never reorders.

   ```jsx
   const items = [
     { id: 1, name: "Apple" },
     { id: 2, name: "Banana" },
   ];

   function ItemList() {
     return (
       <ul>
         {items.map((item) => (
           <li key={item.id}>{item.name}</li>
         ))}
       </ul>
     );
   }
   ```

2. **Rendering Complex Lists**:
   You can render more complex components within a list.

   ```jsx
   function UserList({ users }) {
     return (
       <div>
         {users.map((user) => (
           <div key={user.id} className="user-card">
             <h2>{user.name}</h2>
             <p>{user.email}</p>
           </div>
         ))}
       </div>
     );
   }
   ```

3. **Conditional Rendering in Lists**:
   Combine conditional rendering with lists to filter or modify what’s displayed.

   ```jsx
   function FilteredList({ items }) {
     return (
       <ul>
         {items
           .filter((item) => item.isActive)
           .map((item) => (
             <li key={item.id}>{item.name}</li>
           ))}
       </ul>
     );
   }
   ```

4. **Empty List Handling**:
   Display a fallback UI when the list is empty.

   ```jsx
   function ItemList({ items }) {
     if (items.length === 0) {
       return <p>No items available.</p>;
     }
     return (
       <ul>
         {items.map((item) => (
           <li key={item.id}>{item.name}</li>
         ))}
       </ul>
     );
   }
   ```

---

### **Combining Conditional Rendering and List Rendering**
You can combine both techniques for dynamic UIs. For example:

```jsx
function Dashboard({ user, items }) {
  return (
    <div>
      {user.isLoggedIn ? (
        <>
          <h1>Welcome, {user.name}!</h1>
          <ul>
            {items.length > 0 ? (
              items.map((item) => (
                <li key={item.id}>{item.name}</li>
              ))
            ) : (
              <p>No items to display.</p>
            )}
          </ul>
        </>
      ) : (
        <p>Please log in to view your dashboard.</p>
      )}
    </div>
  );
}
```

---
