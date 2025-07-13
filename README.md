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
## Event Handling

### Handling events in React is a key aspect of building interactive user interfaces. React uses a synthetic event system to handle DOM events in a consistent, performant way.
---

### **Key Concepts**

1. **Synthetic Events**:
   - React wraps native DOM events into a `SyntheticEvent` object, providing a consistent API across browsers.
   - Event handlers are attached to JSX elements using camelCase event names (e.g., `onClick`, `onChange`).

2. **Event Handler Syntax**:
   - Event handlers are functions defined in your component, passed to elements as props.
   - Use arrow functions or bind methods to ensure the correct `this` context.

---

### **Basic Event Handling**

#### **1. Handling Click Events**
```jsx
function Button() {
  const handleClick = () => {
    alert("Button clicked!");
  };

  return <button onClick={handleClick}>Click Me</button>;
}
```

- The `onClick` prop is assigned the `handleClick` function, which runs when the button is clicked.
- Do **not** call the function directly (e.g., `onClick={handleClick()}`), as it will execute immediately on render.

#### **2. Passing Arguments to Event Handlers**
You can pass parameters to event handlers using an arrow function.

```jsx
function Item({ id, name }) {
  const handleDelete = (itemId) => {
    console.log(`Deleting item with ID: ${itemId}`);
  };

  return (
    <button onClick={() => handleDelete(id)}>
      Delete {name}
    </button>
  );
}
```

#### **3. Handling Form Events**
Common form events include `onChange`, `onSubmit`, and `onInput`.

```jsx
function Form() {
  const handleSubmit = (event) => {
    event.preventDefault(); // Prevent default form submission
    console.log("Form submitted!");
  };

  const handleChange = (event) => {
    console.log("Input value:", event.target.value);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" onChange={handleChange} />
      <button type="submit">Submit</button>
    </form>
  );
}
```

- `event.preventDefault()` stops the default behavior (e.g., page reload on form submission).
- Access input values via `event.target.value`.

---

### **Binding `this` in Class Components**
In class components, event handlers need proper binding to access `this`.

```jsx
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = { isOn: false };
    // Bind in constructor
    this.handleToggle = this.handleToggle.bind(this);
  }

  handleToggle() {
    this.setState({ isOn: !this.state.isOn });
  }

  render() {
    return (
      <button onClick={this.handleToggle}>
        {this.state.isOn ? "ON" : "OFF"}
      </button>
    );
  }
}
```

Alternatively, use arrow functions to avoid manual binding:

```jsx
class Toggle extends React.Component {
  state = { isOn: false };

  handleToggle = () => {
    this.setState({ isOn: !this.state.isOn });
  };

  render() {
    return (
      <button onClick={this.handleToggle}>
        {this.state.isOn ? "ON" : "OFF"}
      </button>
    );
  }
}
```

In functional components, binding is unnecessary since they don’t use `this`.

---

### **Common Event Types**
Here are some frequently used events in React:
- `onClick`: For clicks on buttons, links, etc.
- `onChange`: For input, select, or textarea changes.
- `onSubmit`: For form submissions.
- `onMouseOver`, `onMouseOut`: For hover effects.
- `onKeyDown`, `onKeyUp`: For keyboard interactions.
- `onFocus`, `onBlur`: For input focus and blur.

Example with multiple events:

```jsx
function InputField() {
  const handleKeyDown = (event) => {
    if (event.key === "Enter") {
      console.log("Enter key pressed!");
    }
  };

  const handleFocus = () => {
    console.log("Input focused!");
  };

  return (
    <input
      type="text"
      onKeyDown={handleKeyDown}
      onFocus={handleFocus}
      placeholder="Type here"
    />
  );
}
```

---

### **Event Object Properties**
The `SyntheticEvent` object provides properties like:
- `event.target`: The element that triggered the event.
- `event.type`: The type of event (e.g., "click").
- `event.preventDefault()`: Stops default behavior.
- `event.stopPropagation()`: Prevents event bubbling to parent elements.

Example:

```jsx
function StopPropagation() {
  const handleClick = (event) => {
    event.stopPropagation();
    console.log("Child clicked, propagation stopped.");
  };

  return (
    <div onClick={() => console.log("Parent clicked")}>
      <button onClick={handleClick}>Click Me</button>
    </div>
  );
}
```

---

### What is React Router DOM?
`react-router-dom` is a library that enables navigation and routing in React applications, allowing you to create single-page applications (SPAs) with multiple views or pages without full page reloads. It synchronizes the UI with the URL, making it easy to manage navigation, pass parameters, and handle nested routes.

### Installation
To use `react-router-dom`, install it via npm or yarn:
```bash
npm install react-router-dom
```
or
```bash
yarn add react-router-dom
```

1. **BrowserRouter**:
   - Wraps your application to enable routing using the HTML5 history API.
   - Example:
     ```jsx
     import { BrowserRouter } from 'react-router-dom';

     function App() {
       return (
         <BrowserRouter>
           {/* Routes go here */}
         </BrowserRouter>
       );
     }
     ```

2. **Routes and Route**:
   - `<Routes>` is a container for defining routes, and `<Route>` defines a specific path and the component to render.
   - Example:
     ```jsx
     import { Routes, Route } from 'react-router-dom';
     import Home from './Home';
     import About from './About';

     function App() {
       return (
         <BrowserRouter>
           <Routes>
             <Route path="/" element={<Home />} />
             <Route path="/about" element={<About />} />
           </Routes>
         </BrowserRouter>
       );
     }
     ```

3. **Link**:
   - Used to create navigational links without reloading the page.
   - Example:
     ```jsx
     import { Link } from 'react-router-dom';

     function Navbar() {
       return (
         <nav>
           <Link to="/">Home</Link>
           <Link to="/about">About</Link>
         </nav>
       );
     }
     ```

4. **NavLink**:
   - Similar to `<Link>`, but allows styling the active link.
   - Example:
     ```jsx
     import { NavLink } from 'react-router-dom';

     function Navbar() {
       return (
         <nav>
           <NavLink to="/" className={({ isActive }) => (isActive ? 'active' : '')}>
             Home
           </NavLink>
           <NavLink to="/about" className={({ isActive }) => (isActive ? 'active' : '')}>
             About
           </NavLink>
         </nav>
       );
     }
     ```

5. **useNavigate**:
   - A hook to programmatically navigate to a different route.
   - Example:
     ```jsx
     import { useNavigate } from 'react-router-dom';

     function Button() {
       const navigate = useNavigate();
       return <button onClick={() => navigate('/about')}>Go to About</button>;
     }
     ```

6. **URL Parameters**:
   - Handle dynamic routes using `:param` syntax.
   - Example:
     ```jsx
     import { useParams } from 'react-router-dom';

     function User() {
       const { id } = useParams();
       return <h1>User ID: {id}</h1>;
     }

     // In Routes
     <Route path="/user/:id" element={<User />} />
     ```

7. **Nested Routes**:
   - Routes can be nested to create layouts or sub-pages.
   - Example:
     ```jsx
     import { Outlet } from 'react-router-dom';

     function Dashboard() {
       return (
         <div>
           <h1>Dashboard</h1>
           <Outlet /> {/* Renders child routes */}
         </div>
       );
     }

     // In Routes
     <Route path="/dashboard" element={<Dashboard />}>
       <Route path="profile" element={<Profile />} />
       <Route path="settings" element={<Settings />} />
     </Route>
     ```

8. **useLocation**:
   - A hook to access the current location object (e.g., pathname, search).
   - Example:
     ```jsx
     import { useLocation } from 'react-router-dom';

     function CurrentPath() {
       const location = useLocation();
       return <p>Current path: {location.pathname}</p>;
     }
     ```


### Example: Full Application
Here’s a simple example combining the above concepts:
```jsx
import { BrowserRouter, Routes, Route, Link, NavLink, useParams } from 'react-router-dom';

function Home() {
  return <h1>Home Page</h1>;
}

function About() {
  return <h1>About Page</h1>;
}

function User() {
  const { id } = useParams();
  return <h1>User ID: {id}</h1>;
}

function App() {
  return (
    <BrowserRouter>
      <nav>
        <NavLink to="/" className={({ isActive }) => (isActive ? 'active' : '')}>
          Home
        </NavLink>
        <NavLink to="/about" className={({ isActive }) => (isActive ? 'active' : '')}>
          About
        </NavLink>
        <NavLink to="/user/123" className={({ isActive }) => (isActive ? 'active' : '')}>
          User
        </NavLink>
      </nav>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/user/:id" element={<User />} />
      </Routes>
    </BrowserRouter>
  );
}

export default App;
```
