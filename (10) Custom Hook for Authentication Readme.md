## 🔐 Custom Hook for Authentication 

---

#  1. What is a Custom Hook?

A **Custom Hook** is a reusable JavaScript function that:

* Starts with `use`
* Uses built-in React hooks (`useState`, `useEffect`, `useContext`)
* Shares logic across components

Example:

```js
useAuth()
useFetch()
useForm()
```

---

#  2. Why We Need Custom Hook for Auth?

Without custom hook:

* Every component writes login/logout logic
* Duplicate code
* Hard to manage token
* Poor scalability

With custom hook:

* Centralized auth logic
* Clean components
* Easy token handling
* Reusable across app

---

#  3. Basic Auth Flow (JWT Based)

1. User logs in
2. Backend sends JWT token
3. Token stored in:

   * localStorage / cookies
4. Token sent in headers for protected routes
5. Logout removes token

---

#  4. Simple Custom Auth Hook (Frontend Only)

### 📁 `useAuth.js`

```js
import { useState, useEffect } from "react";

export const useAuth = () => {
  const [user, setUser] = useState(null);

  useEffect(() => {
    const storedUser = localStorage.getItem("user");
    if (storedUser) {
      setUser(JSON.parse(storedUser));
    }
  }, []);

  const login = (userData) => {
    localStorage.setItem("user", JSON.stringify(userData));
    setUser(userData);
  };

  const logout = () => {
    localStorage.removeItem("user");
    setUser(null);
  };

  return { user, login, logout };
};
```

---

#  5. Using It in Component

```js
import { useAuth } from "./useAuth";

function Navbar() {
  const { user, logout } = useAuth();

  return (
    <div>
      {user ? (
        <>
          <span>Welcome {user.name}</span>
          <button onClick={logout}>Logout</button>
        </>
      ) : (
        <span>Please Login</span>
      )}
    </div>
  );
}
```

---

#  6. Industry-Level Auth (Context + Custom Hook)

In real projects:

👉 We use **Context API + Custom Hook**

### 📁 AuthContext.js

```js
import { createContext, useContext, useState, useEffect } from "react";

const AuthContext = createContext();

export const AuthProvider = ({ children }) => {
  const [user, setUser] = useState(null);

  useEffect(() => {
    const token = localStorage.getItem("token");
    if (token) {
      setUser({ token });
    }
  }, []);

  const login = (data) => {
    localStorage.setItem("token", data.token);
    setUser(data);
  };

  const logout = () => {
    localStorage.removeItem("token");
    setUser(null);
  };

  return (
    <AuthContext.Provider value={{ user, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
};

export const useAuth = () => {
  return useContext(AuthContext);
};
```

---

### 📁 Wrap App

```js
<AuthProvider>
  <App />
</AuthProvider>
```

---

#  7. Protected Route Example

```js
import { Navigate } from "react-router-dom";
import { useAuth } from "./AuthContext";

function PrivateRoute({ children }) {
  const { user } = useAuth();

  return user ? children : <Navigate to="/login" />;
}
```

---

#  8. Best Practices (Very Important for Interview)

✔ Store token in HTTP-only cookies (more secure)
✔ Use Axios interceptor to attach token
✔ Refresh token logic
✔ Handle 401 globally
✔ Never store sensitive data in localStorage

---

#  9. Axios Interceptor Example

```js
import axios from "axios";

const api = axios.create({
  baseURL: "http://localhost:5000",
});

api.interceptors.request.use((config) => {
  const token = localStorage.getItem("token");
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

export default api;
```

---

#  Short Interview Definition 

> A custom auth hook centralizes authentication logic like login, logout, and token handling. It improves code reusability, maintainability, and keeps components clean by separating business logic from UI.

---

# 🔐 Custom Hook + Authentication – Interview Questions & Answers

---

## 🟢 Basic Level

### 1️ What is a Custom Hook in React?

A custom hook is a reusable JavaScript function that starts with `use` and allows us to share logic using built-in hooks like `useState` and `useEffect`.

---

### 2️ Why do we create a custom hook for authentication?

To centralize login/logout logic, manage tokens, and avoid code duplication across components. It keeps UI clean and business logic separate.

---

### 3️ What is the difference between Context API and Custom Hook?

Context provides global state.
A custom hook is just a reusable function.
In auth, we usually combine both.

---

### 4️ Where should JWT be stored?

Best practice: HTTP-only cookies (secure).
Avoid storing sensitive tokens in localStorage due to XSS risk.

---

### 5️ What is JWT?

JWT (JSON Web Token) is a compact token used for secure authentication between client and server. It contains header, payload, and signature.

---

## 🟡 Intermediate Level

### 6️ How do you protect routes in React?

Use a PrivateRoute component that checks if the user/token exists. If not, redirect using `Navigate` from React Router.

---

### 7️ What is token expiration?

JWT contains an expiry time (`exp`). After expiration, the user must re-login or refresh the token.

---

### 8️ What is refresh token?

A long-lived token used to generate a new access token when the old one expires, without forcing user login again.

---

### 9️ How do you attach token to API requests?

Using Axios interceptors:

```js
axios.interceptors.request.use((config) => {
  config.headers.Authorization = `Bearer ${token}`;
  return config;
});
```

---

### 10 What happens if token expires during API call?

Server returns 401.
We catch it globally and either refresh token or logout user.

---

## 🔵 Advanced Level

### 1️1️ Why HTTP-only cookies are better than localStorage?

HTTP-only cookies are not accessible via JavaScript, so they prevent XSS attacks.

---

### 1️2️ How do you prevent XSS in authentication?

* Use HTTP-only cookies
* Sanitize user input
* Avoid dangerouslySetInnerHTML
* Implement CSP (Content Security Policy)

---

### 1️3️ What is role-based authentication?

Users are assigned roles (admin, user, editor).
Access is controlled based on role stored in token or database.

---

### 1️4️ How would you implement auth in Next.js?

* Store token in cookies
* Use middleware for protected routes
* Validate token on server side
* Use API routes for secure backend logic

---

### 1️5️ What is the difference between Authentication and Authorization?

Authentication → Who you are
Authorization → What you can access

---

# 🔥 Rapid Fire (Very Common)

* What is access token?
* What is refresh token?
* What is 401 vs 403?
* How to implement logout?
* How to persist login after page refresh?
* How to handle multiple tabs logout?
* How to secure REST APIs?
* How to implement OAuth login?
* What is stateless authentication?
* What is session-based authentication?

---

#  HR + Technical Combo Question

👉 “Explain your authentication flow in your project.”

Best Answer Structure:

1. User logs in
2. Backend validates credentials
3. JWT generated
4. Token stored securely
5. Token sent in headers
6. Protected routes check token
7. Logout clears token

---
