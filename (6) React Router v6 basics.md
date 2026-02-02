
##  What is React Router?

React Router is a library used for **client-side routing** in React applications.

It helps you show different components for different URLs like:

* `/`
* `/login`
* `/dashboard`

Without reloading the page (Single Page Application behavior).

---

##  Installation

```bash
npm install react-router-dom
```

---

##  Basic Setup (Step by Step)

###  1. Wrap your app with `BrowserRouter`

**main.jsx / index.js**

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import { BrowserRouter } from 'react-router-dom';
import App from './App';

ReactDOM.createRoot(document.getElementById('root')).render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
);
```

**Why?**

* `BrowserRouter` enables routing in your React app.
* Without it, `Route`, `Link`, `useNavigate` will not work.

---

###  2. Define routes using `Routes` and `Route`

**App.jsx**

```jsx
import { Routes, Route } from 'react-router-dom';
import Home from './pages/Home';
import Login from './pages/Login';
import About from './pages/About';

function App() {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/login" element={<Login />} />
      <Route path="/about" element={<About />} />
    </Routes>
  );
}

export default App;
```

**Explanation:**

* `<Routes>`: container for all routes
* `<Route>`: defines a single route
* `path`: URL
* `element`: component to render

> In v6, `component={}` is replaced with `element={<Component />}`

---

##  Navigation

###  3. Navigation using `Link`

```jsx
import { Link } from 'react-router-dom';

function Navbar() {
  return (
    <nav>
      <Link to="/">Home</Link>
      <Link to="/login">Login</Link>
      <Link to="/about">About</Link>
    </nav>
  );
}
```

**Why `Link` instead of `<a>`?**

* `<a>` reloads the page ❌
* `<Link>` changes route without reloading the page 

---

###  4. Programmatic navigation using `useNavigate`

```jsx
import { useNavigate } from 'react-router-dom';

function Login() {
  const navigate = useNavigate();

  const handleLogin = () => {
    // after successful login
    navigate('/dashboard');
  };

  return <button onClick={handleLogin}>Login</button>;
}

export default Login;
```

**Why use `useNavigate`?**

* To redirect after login, form submit, logout, etc.

---

##  Dynamic Routes (URL Parameters)

```jsx
<Route path="/user/:id" element={<User />} />
```

```jsx
import { useParams } from 'react-router-dom';

function User() {
  const { id } = useParams();
  return <h2>User ID: {id}</h2>;
}
```

**Example URL:**

```
/user/101
```

---

##  Nested Routes (Layout Routes)

```jsx
<Route path="/dashboard" element={<Dashboard />}>
  <Route path="profile" element={<Profile />} />
  <Route path="settings" element={<Settings />} />
</Route>
```

```jsx
import { Outlet } from 'react-router-dom';

function Dashboard() {
  return (
    <div>
      <h1>Dashboard</h1>
      <Outlet /> {/* Child routes render here */}
    </div>
  );
}
```

**URLs:**

* `/dashboard/profile`
* `/dashboard/settings`

---

##  404 Page (Not Found Route)

```jsx
<Route path="*" element={<h1>404 - Page Not Found</h1>} />
```

---

##  Protected Routes (Basic Auth Guard)

```jsx
import { Navigate } from 'react-router-dom';

const PrivateRoute = ({ children }) => {
  const isLoggedIn = localStorage.getItem("token");
  return isLoggedIn ? children : <Navigate to="/login" />;
};
```

```jsx
<Route
  path="/dashboard"
  element={
    <PrivateRoute>
      <Dashboard />
    </PrivateRoute>
  }
/>
```

---

##  Important Changes in React Router v6

| v5                 | v6                   |
| ------------------ | -------------------- |
| `Switch`           | `Routes`             |
| `component={Home}` | `element={<Home />}` |
| `useHistory()`     | `useNavigate()`      |
| `Redirect`         | `Navigate`           |

---

##  Interview Questions (Quick)

**Q1. What is React Router?**
👉 A library for client-side routing in React apps.

**Q2. Difference between `Link` and `<a>`?**
👉 `Link` does not reload the page, `<a>` reloads the page.

**Q3. What does `useNavigate` do?**
👉 Navigates programmatically to another route.

**Q4. What is `useParams` used for?**
👉 To read dynamic values from the URL.

**Q5. What is `Outlet`?**
👉 Renders child routes inside a parent layout.

---

##  One-Line Summary (For Interview)

> React Router v6 enables client-side routing in React applications using `BrowserRouter`, `Routes`, `Route`, and hooks like `useNavigate` and `useParams` to navigate without reloading the page.

---
