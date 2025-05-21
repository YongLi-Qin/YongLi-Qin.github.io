---
layout: post
title: "🎓 [Deep Dive] About Custom Hook"
date: 2025-05-21
---

## 📘 What is Custom Hook?

A Custom Hook in React is a reusable function that starts with the prefix use and leverages one or more built-in React Hooks (like useState, useEffect, etc.) to encapsulate and share stateful logic between components, without dealing with any UI rendering.


---

## ⚠️ Key characteristics:
- Must start with use (e.g., useFetch, useForm, useTimer)
- Can call other Hooks inside
- Returns values or functions (not JSX)
- Encourages logic reuse and separation of concerns

## ❓ Why we need to use Custom Hook?

Custom Hooks are used to encapsulate reusable logic in React. Instead of copying and pasting the same useEffect, useState, or fetch code across multiple components, you write it once as a function — a Custom Hook — and reuse it everywhere.

---

## ✅ Benefits of Custom Hooks:

- Avoid code duplication: You don’t have to repeat the same logic in multiple components.

- Improve readability: Your components focus only on rendering UI, not logic.

- Encourage separation of concerns: Logic lives in hooks, UI lives in components.

- Easier to test: You can test the logic of your hook without involving the UI.

- Composable: You can even combine multiple hooks into one complex behavior.

 
---

##  📘 Example: Why Custom Hooks Are Helpful

Instead of writing this in every component:

```js
useEffect(() => {
  setLoading(true);
  fetch('/api/data')
    .then(res => res.json())
    .then(setData)
    .catch(setError)
    .finally(() => setLoading(false));
}, []);
```

You can extract it into a reusable hook:
```js
// useFetch.js
import { useState, useEffect } from 'react';

export function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    setLoading(true);
    fetch(url)
      .then(res => res.json())
      .then(setData)
      .catch(setError)
      .finally(() => setLoading(false));
  }, [url]);

  return { data, loading, error };
}
```

Then in any component:


```js
import { useFetch } from '../hooks/useFetch';

function MyComponent() {
  const { data, loading, error } = useFetch('/api/data');

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error occurred</p>;

  return <div>{data.name}</div>;
}
```


