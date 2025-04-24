_______
https://youtube.com/playlist?list=PLC3y8-rFHvwg7czgqpQIBEAHn8D6l530t&si=2avOjkqaaX5CJOp3



**Rendering** is the process of **turning your components (JSX) into actual DOM elements** in the browser.

React components are **functions** (or classes) that return JSX.  
When React renders, it:

1. **Calls the component function**
2. **Evaluates the JSX**
3. **Updates the DOM** (if needed)



### 🧠 Two Types of Renders

 1. Initial Render
	- Happens **once** when a component is first loaded on the page.
	- React builds the DOM for the first time from your JSX.
 2. Re-render
	Happens when:
  - **State** changes via `useState`, `useReducer`, etc.
  - **Props** passed to a component change
  - **Context** changes (when using `useContext`)
  - React calls the component **again**, generates new JSX, and **compares it** to the previous one.
 3. 🔍 Virtual DOM & Reconciliation
	React doesn’t update the real DOM immediately. Instead:
	- It uses a **Virtual DOM** – a lightweight copy of the real DOM.
	- On re-render, React compares the new virtual DOM with the old one (called **diffing**).
	- It calculates the **minimal number of changes** (called **reconciliation**).
	- Then it updates the real DOM efficiently.


![[Pasted image 20250423075353.png]]

![[Pasted image 20250423075702.png]]

#### Unmounting: 
When a component **is removed from the UI**, React **unmounts** it.  
It stops rendering, deletes the DOM nodes, and runs any cleanup logic.
Unmounting happens when:
1. Conditional Rendering: When `isVisible` goes from `true` → `false`, the `<Popup />` component is **unmounted**.
2. Navigation (React Router)
3. Key Changes with Dynamic Lists: If an item is **removed** from the list, React unmounts the corresponding `<Item />`.
	etc.
**Also its important to note, State change ≠ unmount.**


## ⚙️ What Triggers a Render?

| Trigger                        | Causes Render? |
|-------------------------------|----------------|
| `useState` value changes      | ✅ Yes         |
| `useReducer` dispatch         | ✅ Yes         |
| Parent component re-renders   | ✅ Yes         |
| Context value changes         | ✅ Yes         |
| `useRef` update               | ❌ No          |
| `console.log()` or variable   | ❌ No          |

---

## 🔄 Render Flow Example

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  console.log('render');

  return (
    <button onClick={() => setCount(count + 1)}>
      Clicked {count} times
    </button>
  );
}
```

