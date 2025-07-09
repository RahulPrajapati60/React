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
