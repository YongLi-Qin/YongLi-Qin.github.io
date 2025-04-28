---
layout: post
title: "🎓 [Deep Dive] Practical useEffect Use Cases"
date: 2025-04-28 00:00:00 +1000
---

# 🎯 Why This Matters

In the previous post, we introduced the basic concepts behind `useEffect`.
Now, let's dive into practical examples to understand why we need it — and what issues can arise if we ignore it.

---

## 📌 Use Case 1: Fetching API Data

### ❌ Without useEffect (Wrong)

```js
function BadComponent() {
  const [data, setData] = useState(null);

  fetch('https://api.example.com/data')
    .then(res => res.json())
    .then(data => setData(data));

  return <div>{JSON.stringify(data)}</div>;
}
```

- Every render triggers a fetch

- setData() causes re-render → infinite loop

- App becomes slow, unstable, or crashes


---

### ✅ With useEffect (Correct)

```js
function GoodComponent() {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetch('https://api.example.com/data')
      .then(res => res.json())
      .then(data => setData(data));
  }, []);

  return <div>{JSON.stringify(data)}</div>;
}
```

- fetch runs only once on mount
- No repeated API calls


---

## 📌 Use Case 2: Setting up Event Listeners

### ❌ Without useEffect (Wrong)

```js
function BadResizeComponent() {
  window.addEventListener('resize', () => {
    console.log(window.innerWidth);
  });

  return <div>Resize the window!</div>;
}

```

- A new event listener is added on every render
- Leads to memory leak and performance drop


---

### ✅ With useEffect (Correct)

```js
function GoodResizeComponent() {
  useEffect(() => {
    const handleResize = () => {
      console.log(window.innerWidth);
    };

    window.addEventListener('resize', handleResize);

    return () => window.removeEventListener('resize', handleResize);
  }, []);

  return <div>Resize the window!</div>;
}
```

- Listener added once
- Listener cleaned up properly when component unmounts


---


## 📌 Use Case 3: Timers (setInterval / setTimeout)

### ❌ Without useEffect (Wrong)

``` js
function BadTimerComponent() {
  setInterval(() => {
    console.log('Tick');
  }, 1000);

  return <div>Timer running...</div>;
}
```

- Every render creates a new interval
- Leads to multiple timers ticking at once


---

### ✅ With useEffect (Correct)
``` js
function GoodTimerComponent() {
  useEffect(() => {
    const timer = setInterval(() => {
      console.log('Tick');
    }, 1000);

    return () => clearInterval(timer); // cleanup
  }, []);

  return <div>Timer running...</div>;
}
```

- Only one interval
- Cleared on unmount





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

