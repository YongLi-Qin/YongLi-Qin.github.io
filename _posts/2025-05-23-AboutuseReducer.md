---
layout: post
title: "🎓 [Deep Dive] About useReducer"
date: 2025-05-26 12:00:00 +1000
---

# 📝 About useReducer

Understand What It Is, Why It Matters, and How It Works

---

## ✅ Introduction

`useReducer` is a React Hook that is an alternative to `useState`, especially useful when:

- State logic is complex or involves multiple sub-values  
- The next state depends on the previous state  
- You want to keep state transitions predictable and testable  

It is conceptually similar to how **Redux reducers** work — you dispatch an action, and a reducer function determines the next state based on that action.

---

## 🔁 Core Concepts

| **Concept** | **Description** |
|-------------|------------------|
| `state`     | The current state of your component |
| `dispatch`  | A function to send actions to the reducer |
| `reducer`   | A pure function that returns the new state based on the current state and the dispatched action |

Syntax:
```js
const [state, dispatch] = useReducer(reducerFn, initialState);
```

---


## ✅ When to Use useReducer

- When your state includes multiple related values

- When state transitions are complex

- When you want a clearer structure for managing state

- When you're building form state, multi-step wizards, or state machines


---


## 🔍 useReducer in Practice
### 🧪 Example: Toggle Button with useState
```js
const [isOn, setIsOn] = useState(false);
```

### 🔁 With useReducer
```js
function toggleReducer(state, action) {
  switch (action.type) {
    case 'TOGGLE':
      return { isOn: !state.isOn };
    default:
      return state;
  }
}

const [state, dispatch] = useReducer(toggleReducer, { isOn: false });

<button onClick={() => dispatch({ type: 'TOGGLE' })}>
  {state.isOn ? 'ON' : 'OFF'}
</button>
```

This becomes especially useful as the logic gets more complicated (e.g., also tracking number of toggles, timestamps, etc.).


## ✅ Summary
- useReducer helps manage complex state transitions in React components

- It brings structure and predictability by centralizing update logic

- It is ideal when multiple pieces of state need to be updated in coordination

- A great step toward Redux-style thinking — but within a single component

🎯 Use useReducer when useState starts getting messy, and your component logic will thank you.
