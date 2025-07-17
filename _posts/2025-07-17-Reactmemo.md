---
layout: post
title: "🎓 [Deep Dive] Understanding React.memo"
date: 2025-06-14 12:00:00 +1000
---

# 🧠 What is React.memo?
- React.memo is a higher-order component (HOC) that lets you memoize a React functional component.
If the component receives the same props, React will skip re-rendering and reuse the last rendered result.

- You can think of it as a cache layer for functional components — useful when your component doesn’t need to re-render every time its parent does.




---



## 🧰 Syntax and Usage
```js
const MemoizedComponent = React.memo(MyComponent);
```
Or with inline function:
```js
const Greeting = React.memo(({ name }) => {
  return <h1>Hello, {name}</h1>;
});
```
- React performs a shallow comparison on props.

- If props are unchanged, rendering is skipped.

- You can pass a custom comparison function as the second argument:
```js
React.memo(Component, (prevProps, nextProps) => {
  // Return true to skip re-rendering
});
```



---


# 🔍 When Do You Need React.memo?
✅ Use React.memo when:
- ✅ Your component is pure (output only depends on props)
- ✅ Parent re-renders frequently, but props don’t change
- ✅ The component is expensive to render (e.g., contains heavy computation or a big tree)
- ✅ Static information
⚠️ Avoid using React.memo when:
- Your component renders quickly — memoization adds overhead
- Props change frequently — memoization won’t help
- Props include new object/function references each time (unless wrapped with `useMemo` / `useCallback`)


---

## 📦 Real-World Use Case: Avoid Re-rendering on Parent Update
```js
const Greeting = React.memo(({ name }) => {
  console.log("Greeting rendered");
  return <h2>Hello, {name}</h2>;
});

function Parent() {
  const [count, setCount] = useState(0);
  return (
    <>
      <Greeting name="Alice" />
      <button onClick={() => setCount(count + 1)}>Click {count}</button>
    </>
  );
}
```
Even if Parent re-renders on every button click, Greeting will not re-render unless the name prop changes.


---




### 🧾 Summary: When and Why to Use React.memo
- Use React.memo to prevent unnecessary re-renders of functional components.

- It's ideal when:

- Props are stable

- Component rendering is costly

- You care about performance


---




