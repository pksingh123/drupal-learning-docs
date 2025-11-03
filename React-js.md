# What is Hooks?
Hooks are functions that allow functional components to manage state, perform side effects, and use React features.

# Example: 
```jsx
import React, { useState, useEffect } from 'react';

function Counter() {
  // useState Hook to manage state
  const [count, setCount] = useState(0);

  // useEffect Hook to perform side effects
  useEffect(() => {
    console.log(`You clicked ${count} times`);
  }, [count]); // runs when count changes

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click Me</button>
    </div>
  );
}

# List of hooks

# useState()	
#Adds state to a functional component
# Example

import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0); // initial value = 0

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increase</button>
    </div>
  );
}

# useEffect()	
#Used to perform side effects — actions that affect something outside the component (like fetching data, timers, or DOM updates).
# Why Used
#Fetch data from an API
#Set up subscriptions or timers
#Update the DOM manually

# Example:
import React, { useState, useEffect } from 'react';

function Timer() {
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => setSeconds(s => s + 1), 1000);
    return () => clearInterval(interval); // cleanup
  }, []); // empty array = run once

  return <p>Seconds passed: {seconds}</p>;
}

# useContext()	
Accesses global data from React Context
# useRef()	
Accesses or stores DOM elements and values between renders
# useMemo()	
Optimizes performance by memoizing values
# useCallback()	
Memoizes functions to prevent unnecessary re-renders
# useReducer()	
Manages complex state logic (alternative to useState)
# useLayoutEffect()
