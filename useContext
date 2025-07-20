Instead of passing the announcement (data) from one person to another through a long chain, anyone in the group can just check the chat directly. In React, `useContext` lets components access shared data without passing it through every parent and child.

---

### Why Use `useContext`?
- **Saves Time**: No need to pass props through every component.
- **Cleaner Code**: Reduces clutter from prop passing.
- **Perfect for Global Data**: Great for things like themes, user authentication, or app-wide settings.

---

### Step-by-Step Guide to Using `useContext`

Here’s how to set up and use the `useContext` hook in a React app, step by step, with a practical example of a theme toggle (light/dark mode).

#### Step 1: Create a Context

You need to create a Context to store the shared data. This is like setting up the group chat.

```javascript
// ThemeContext.js
import React from 'react';

// Create a Context
const ThemeContext = React.createContext();

export default ThemeContext;
```

- `React.createContext()` creates a Context object.
- You can optionally provide a default value (e.g., `React.createContext('light')`) for cases when a component uses the Context outside a Provider.
- It’s good practice to put this in a separate file (e.g., `ThemeContext.js`) for reusability.

#### Step 2: Set Up the Provider

The **Provider** is a component that wraps part of your app and provides the data to all components inside it. This is like sending the announcement to the group chat.

In your main app file (e.g., `App.js`), set up the Provider and pass the data you want to share.

```javascript
// App.js
import React, { useState } from 'react';
import ThemeContext from './ThemeContext';
import Header from './Header';
import Main from './Main';

function App() {
  // State for the theme (data to share)
  const [theme, setTheme] = useState('light');

  // Function to toggle theme
  const toggleTheme = () => {
    setTheme(theme === 'light' ? 'dark' : 'light');
  };

  return (
    // Wrap components in the Provider and pass the value
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      <Header />
      <Main />
    </ThemeContext.Provider>
  );
}

export default App;
```

- **What’s Happening**:
  - We use `useState` to manage the `theme` (light or dark) and a `toggleTheme` function to switch it.
  - The `ThemeContext.Provider` wraps the components (`Header` and `Main`) that need access to the theme.
  - The `value` prop passes an object `{ theme, toggleTheme }` to the Context, making it available to all components inside.

#### Step 3: Consume the Context with `useContext`

Now, any component inside the `Provider` can use the `useContext` hook to access the shared data (the theme and toggle function).

Create a `Header` component that uses the theme and allows toggling it:

```javascript
// Header.js
import React, { useContext } from 'react';
import ThemeContext from './ThemeContext';

function Header() {
  // Use useContext to grab the data from ThemeContext
  const { theme, toggleTheme } = useContext(ThemeContext);

  return (
    <div
      style={{
        background: theme === 'light' ? '#fff' : '#333',
        color: theme === 'light' ? '#000' : '#fff',
        padding: '10px',
      }}
    >
      <h1>Current Theme: {theme}</h1>
      <button onClick={toggleTheme}>Toggle Theme</button>
    </div>
  );
}

export default Header;
```

Create a `Main` component that also uses the theme:

```javascript
// Main.js
import React, { useContext } from 'react';
import ThemeContext from './ThemeContext';

function Main() {
  // Use useContext to grab the theme
  const { theme } = useContext(ThemeContext);

  return (
    <p
      style={{
        background: theme === 'light' ? '#fff' : '#333',
        color: theme === 'light' ? '#000' : '#fff',
        padding: '10px',
      }}
    >
      This is the main content in {theme} mode.
    </p>
  );
}

export default Main;
```

- **What’s Happening**:
  - Import `useContext` and the `ThemeContext`.
  - Call `useContext(ThemeContext)` to get the data (`theme` and `toggleTheme`) from the Context.
  - Use the `theme` to style the components and display the current mode.
  - In `Header`, the `toggleTheme` function is used to switch between light and dark modes when the button is clicked.

#### Step 4: Run the App

When you run this app:
- The `App` component provides the `theme` and `toggleTheme` function to the Context.
- The `Header` component shows the current theme and has a button to toggle it.
- The `Main` component also shows the current theme.
- Clicking the "Toggle Theme" button in `Header` updates the `theme` state in `App`, and both `Header` and `Main` automatically update to reflect the new theme (light or dark).

The components don’t need to pass props back and forth—the Context handles it all!

---

### Full Code Example

**ThemeContext.js**:
```javascript
import React from 'react';

const ThemeContext = React.createContext();

export default ThemeContext;
```

**App.js**:
```javascript
import React, { useState } from 'react';
import ThemeContext from './ThemeContext';
import Header from './Header';
import Main from './Main';

function App() {
  const [theme, setTheme] = useState('light');

  const toggleTheme = () => {
    setTheme(theme === 'light' ? 'dark' : 'light');
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      <Header />
      <Main />
    </ThemeContext.Provider>
  );
}

export default App;
```

**Header.js**:
```javascript
import React, { useContext } from 'react';
import ThemeContext from './ThemeContext';

function Header() {
  const { theme, toggleTheme } = useContext(ThemeContext);

  return (
    <div
      style={{
        background: theme === 'light' ? '#fff' : '#333',
        color: theme === 'light' ? '#000' : '#fff',
        padding: '10px',
      }}
    >
      <h1>Current Theme: {theme}</h1>
      <button onClick={toggleTheme}>Toggle Theme</button>
    </div>
  );
}

export default Header;
```

**Main.js**:
```javascript
import React, { useContext } from 'react';
import ThemeContext from './ThemeContext';

function Main() {
  const { theme } = useContext(ThemeContext);

  return (
    <p
      style={{
        background: theme === 'light' ? '#fff' : '#333',
        color: theme === 'light' ? '#000' : '#fff',
        padding: '10px',
      }}
    >
      This is the main content in {theme} mode.
    </p>
  );
}

export default Main;
```

---
---

### Key Points to Understand
- **Functional Components Only**: `useContext` works only in functional components, not class components.
- **Provider Requirement**: Components must be inside a `Provider` to access the Context. If they’re outside, they’ll get the default value (if set) or `undefined`.
- **Dynamic Data**: When the value in the `Provider` changes (e.g., `theme` updates), all components using `useContext` re-render with the new value.
- **Best Use Cases**: Use `useContext` for global data like themes, user info, or app settings. For local data (used by only a few components), props or `useState` are often better.

---
---
---

### Summary
The `useContext` hook simplifies sharing data across your React app. You:
1. Create a Context with `React.createContext()`.
2. Wrap components in a `Provider` and pass the data via the `value` prop.
3. Use `useContext` in any component inside the `Provider` to access the data.
