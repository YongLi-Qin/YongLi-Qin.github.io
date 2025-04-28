---
layout: post
title: "🎓 [Deep Dive] About useEffect"
date: 2025-04-25 00:00:00 +1000
---

# 📝 Why Do We Need useEffect in React?

Understand What It Is, Why It Matters, and How It Works

---

## ✅ Introduction

React's function components are clean and powerful, but they come with a twist — no lifecycle methods like `componentDidMount`. So how do we fetch data, set up timers, or listen to events?

That’s where `useEffect` comes in.


---

## 🔍 What is useEffect?


`useEffect` is a React Hook that lets you perform __side effects__ in function components.


---

### 💻 Common side effects include:

- Fetching data from APIs
- Setting up subscriptions or timers
- Listening to window resize or scroll events
- Manually updating the DOM


---


## 🛠️ Basic Syntax

```js
useEffect(() => {
  // your side effect logic here
}, [dependencies]);
```


---

## 📈 Example:
```js
import React, { useState, useEffect } from 'react';

function ExampleComponent() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log('Count changed:', count);
  }, [count]); // runs when `count` changes

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>
        Add +1
      </button>
    </div>
  );
}
```


---


## 🔁 Three types of the dependency array

- [] → run once (on mount)
- [count] → run when count changes
- no array → run on every render

---

## 💥 What happens if you don’t use useEffect?


### ❌ Fetching data directly inside the component body
```js
function BadComponent() {
  const [data, setData] = useState(null);

  // ❌ This will run every render — bad!
  fetch('https://api.example.com/data')
    .then(res => res.json())
    .then(data => setData(data));

  return <div>{JSON.stringify(data)}</div>;
}
```
---

### 🚨 What goes wrong?
- `fetch()` runs → `setData()` updates state

- State update → triggers re-render

- Re-render the whole component and it will run `fetch()` again → another state update...

- 🔁 Infinite loop of requests → bad for performance and may crash the app- 

---

### ✅ Fetching data with useEffect

```javascript
function GoodComponent() {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetch('https://api.example.com/data')
  }, []); 


  return (
      return <div>{JSON.stringify(data)}</div>;
  );
}
```

- In this case, we use 
```javascript
useEffect(() => {
    fetch('https://api.example.com/data')
  }, []); 
  ```
  - Only runs once when the component mounts (first render)
  - Never re-runs, even if state/props change
  - So we avoid the Infinite loop of requests like above code


## ❓ Q&A: Common Questions About useEffect


#### 1. What is the purpose of the cleanup function in useEffect?
In useEffect, you can return a function — called the cleanup function — that React will call when the component unmounts or before re-running the effect due to dependency changes.


The purpose of cleanup is to remove or cancel any ongoing side effects that should no longer be active, such as:
- Removing event listeners
- Clearing timers (`setInterval` or `setTimeout`)
- Unsubscribing from subscriptions
- Closing WebSocket connections


Without proper cleanup, you risk memory leaks, unexpected behaviors, and performance issues.

```js
useEffect(() => {
  function handleResize() {
    console.log('Window resized:', window.innerWidth);
  }

  window.addEventListener('resize', handleResize);

  return () => {
    window.removeEventListener('resize', handleResize);
  };
}, []);

```


---


#### 2. Can you have multiple useEffect hooks in the same component?

✅ Yes, absolutely!


You can (and often should) use multiple `useEffect` hooks within a single component.
Each `useEffect` should ideally manage one specific side effect.


This makes your code cleaner, easier to read, and easier to maintain.

Example:

```js
useEffect(() => {
  console.log('Effect for count');
}, [count]);

useEffect(() => {
  console.log('Effect for name');
}, [name]);

```

Each effect operates independently, depending on its specific dependencies.


---


#### 3. If there are multiple useEffect, what is their execution order?
React runs multiple `useEffect` hooks in the order they appear in your code — top to bottom.

Even if different effects depend on different variables, the order is determined by their position in the component.


Example:

```js
useEffect(() => {
  console.log('First Effect');
}, []);

useEffect(() => {
  console.log('Second Effect');
}, []);

useEffect(() => {
  console.log('Third Effect');
}, []);

```


Output after first render:

```js
First Effect
Second Effect
Third Effect


```

Cleanup functions, if any, are called in reverse order before running the new effects.





<!-- 
### 🧪 Understanding useEffect Comparison and Triggering

###  💼 How Dependency Comparison Works?

React's `useEffect` hook uses **shallow comparison** for its dependency array:


---

### 🧐 Primitive Values (number, string, boolean)
```javascript
useEffect(() => {
  // Runs when userId changes
}, [userId]); // Compares with `===`
```

- Uses strict equality (===) comparison
- Effect runs only when the value actually changes


---

### 🤔 When useEffect Triggers?

- First Render (Mount)
- Always runs after initial render


---

#### 💡 Subsequent Renders

- Runs after render completes

-Only if shallow comparison detects changes in dependencies

---


#### ⭕️ The Render Cycle

- Component renders (due to state/props change)

- React compares each dependency with previous values

- Effect executes if any dependency changed

---

#### 🔑 Key Points
- useEffect runs asynchronously after render completes

- Parent re-renders will cause child effects to re-evaluate

- State updates (useState) trigger re-renders which may trigger effects

---
 -->
