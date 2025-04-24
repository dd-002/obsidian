__________
## useState() hook :

`useState` lets you add **state** to your **function components**. State is like a memory for your component — it remembers values across renders.
`const [state, setState] = useState(initialValue);`

```jsx
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0); // starts at 0

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click Me</button>
    </div>
  );
}
```
##### Functional Update --
```jsx
setCount(prevCount => prevCount + 1); //functional update
setCount(count + 1); //direct update
```
When state updates happen **quickly** or **multiple times in a row**, the value of `count` might be **stale** — it hasn’t updated yet when you try to use it again.

React **batches** updates to improve performance, which means your `count` might not reflect the latest value _yet_. The fix is:
`setCount(prevCount => prevCount + 1);` 
**Example:**
```jsx
const [user, setUser] = useState({
  name: '',
  age: 25,
  email: 'example@email.com',
});
setUser(prev => ({ ...prev, name: 'Alice' }));
```
Explanation:
Here's how it works:
- `prev` is the current value of `user`.
- `{ ...prev }` copies all the existing properties.
- `name: 'Alice'` **overwrites** the `name` field.

#### Rendering And Re-rendering in useState hook




_____

## useEffect() hook:

`useEffect` lets us **run code after a component renders**.

```jsx
useEffect(() => {
  // ✅ Code that runs after render

  return () => {
    // 🧼 Cleanup (optional)
  };
}, [dependencies]);
```
The callback runs after the component renders. The `dependencies` array controls **when it runs** again. *useEffect runs once on mount, and again if dependency changes and not if the component gets re rendered(but no change in dependency)*. The `return` function runs on **cleanup** (before the next effect or when the component unmounts).


Ways of using useEffect():

1. Run on component mount
```jsx
useEffect(() => {
  console.log('Component mounted!');
}, []); // empty array = run once when component is mounted
```

2. Run when state changes
```jsx
useEffect(() => {
  console.log(`Count changed: ${count}`);
}, [count]); //runs when state of count changes
```

3. Fetch data from an API
```jsx
import { useState, useEffect } from 'react';

function UserList() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/users')
      .then(res => res.json())
      .then(data => setUsers(data));
  }, []); // run only once

  return (
    <ul>
      {users.map(user => <li key={user.id}>{user.name}</li>)}
    </ul>
  );
}
```

4. If we omit dependency array
```jsx
useEffect(() => {
  console.log('Runs after every render!');
}); //This runs **after every render** (initial + updates)
```

Example :
```jsx
useEffect(() => {
  const handleResize = () => {
    setWidth(window.innerWidth);
  };

  window.addEventListener('resize', handleResize);

  return () => window.removeEventListener('resize', handleResize);
}, []);
```
return happens when the component unmounts [[React Rendering]].
Understanding the example: useEffect runs once when the component gets mounted, also the reason we dont mention width in side [] is because , otherwise we will keep on adding event listeners everytime width state changes. 
***Note: > changing state does not unmount and remount the component. Instead, React does a re-render, not a full unmount/remount.***




