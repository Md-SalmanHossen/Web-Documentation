
---

## **Side Effect মানে কী?**

React একটি UI rendering framework — তার কাজ **state/props → UI update করা।**
এখন UI render ছাড়াও অনেক কাজ থাকে যেগুলো React এর control এর বাইরে — এসব কাজকে বলে **Side Effect**।

### Side Effect বলতে বোঝায় —

> Component render হওয়ার পর React এর বাইরে যেসব কাজ execute হয়, সেগুলো Side Effect।

### Example of Side Effects

| কাজ                         | কেন Side Effect?                      |
| --------------------------- | ------------------------------------- |
| API call                    | Data fetch Browser/Network handle করে |
| setTimeout / setInterval    | Asynchronous browser timer            |
| event listener add/remove   | DOM এ direct attach                   |
| localStorage read/write     | External I/O                          |
| console logging (render পর) | UI render এর বাইরে                    |
| WebSocket/SSE connect       | External live stream                  |

সহজ ভাষায় —
**যেকোনো কাজ যা UI draw না করে React-এর বাইরে বিশ্বে কিছু করে, সেটা Side Effect।**

---

## **Mount, Update, Unmount — Lifecycle সহজ ভাষায়**

React component জীবনের ৩টা phase থাকে:

| Phase       | মানে                                      | কখন ঘটে         |
| ----------- | ----------------------------------------- | --------------- |
| **Mount**   | Component প্রথমবার screen এ আসা           | initial render  |
| **Update**  | state/props change হয়ে পুনরায় render হওয়া | re-render       |
| **Unmount** | UI থেকে Component চিরতরে রিমুভ হওয়া       | disappear/close |

### সহজ Example:

* Component প্রথম show হলো → **Mount**
* তুমি button চাপলে count বাড়লো → UI আবার আঁকল → **Update**
* Page বদলে component আর দেখা যাচ্ছে না → **Unmount**

---

## UseEffect আসলে কেন দরকার?

React component যখন render হয়, natural কাজ UI update করা।
কিন্তু আমাদের দরকার হয় —

✔ Render এর পরে কোড চালানো
✔ External API call
✔ Event listener attach
✔ Memory clean করা
✔ Async কাজ

তাই React বলল — “তুমি side effect manage করতে চাইলে useEffect ব্যবহার করো।”

> **useEffect = React lifecycle-এর সাথে side effect চালানোর tool**

---

## এখন মূল কথা 💥

### `useEffect` এর এক কথায় theory সংজ্ঞা:

> **UI render এর পরে যেকোনো Side Effect execute করার জন্য ব্যবহৃত React Hook হলো useEffect — যা Mount, State/Prop Update এবং Unmount অবস্থায় effect চালাতে ও cleanup করতে সাহায্য করে।**

আরো small version:

> **useEffect = Side Effect Runner + Lifecycle Controller**

আরো short — exam এর জন্য perfect:

> **useEffect = component mount, update এবং unmount এ side-effect handle করার hook।**

---

## Theory Summary (Final Ready-to-Remember Sheet)

| Concept     | এক লাইনে Meaning                           |
| ----------- | ------------------------------------------ |
| Side Effect | UI render ছাড়া বাহিরের কাজ                 |
| Mount       | Component প্রথম render                     |
| Update      | state/prop পরিবর্তনে rerender              |
| Unmount     | component UI থেকে remove                   |
| useEffect   | lifecycle অনুযায়ী side effect চালানোর hook |

---

### useEffect Lifecycle Examples (Mount → Update → Unmount)

এটাই সেই part যেখানে তুমি practically দেখবে —

| State       | কখন ঘটে                      | useEffect কীভাবে react করে          |
| ----------- | ---------------------------- | ----------------------------------- |
| **Mount**   | Component first time render  | `useEffect(..., [])` execute        |
| **Update**  | state/prop change → rerender | dependency array অনুযায়ী effect run |
| **Unmount** | component remove/disappear   | `return()` cleanup run              |

---

---

# 1) **Mount (Component first time আসা)**

```jsx
useEffect(() => {
  console.log("🟢 Component Mounted");
}, []);
```

Explanation:
`[]` empty dependency → শুধুমাত্র প্রথম render-এ run
📌 Mostly used for API call, event listener add.

---

---

# 🔄 2) **Update (state/props change হলে)**

```jsx
useEffect(() => {
  console.log("🔄 Count Updated:", count);
}, [count]);   // count পরিবর্তন হলেই effect আবার run
```

Flow:

1. component প্রথম render → useEffect run
2. count পরিবর্তন → component re-render
3. effect আবার trigger হয়

📌 Useful for search filter, data reload, UI update logic.

---

---

# 3) Mount + Update একসাথে

```jsx
useEffect(() => {
  console.log("Runs on mount & on every count change");
}, [count]);
```

📌 প্রথম render + যখন count update হয়।

---

---

# 4) **Unmount (component remove হলে Cleanup run)**

```jsx
useEffect(() => {
  console.log("Component mounted");

  return () => {
    console.log("🔴 Component Unmounted");
  };
}, []);
```

Unount কখন হয়?
→ route change হলে, conditional rendering হলে, tab close etc.

📌 Used for cleanup:
✔ interval clear
✔ event listener remove
✔ subscription disconnect

---

---

# ⚡ Full Lifecycle Example Combined

```jsx
function Example(){
  const [count,setCount] = useState(0);

  useEffect(() => {
    console.log("🟢 Mounted");

    return () => {
      console.log("🔴 Unmounted");
    };
  }, []);

  useEffect(() => {
    console.log("🔄 Updated: count =", count);
  }, [count]);

  return (
    <div>
      <h2>{count}</h2>
      <button onClick={() => setCount(count+1)}>Increase</button>
    </div>
  );
}
```

Output order:

| Action                         | Console Output       |
| ------------------------------ | -------------------- |
| component show                 | 🟢 Mounted           |
| user clicked + updated state   | 🔄 Updated count = X |
| navigate away / hide component | 🔴 Unmounted         |

---

---

# One Line Cheat Sheet

| Lifecycle      | How in useEffect                   |
| -------------- | ---------------------------------- |
| Mount          | `useEffect(()=>{},[])`             |
| Update         | `useEffect(()=>{},[state/props])`  |
| Unmount        | `useEffect(()=> return ()=>{},[])` |
| Mount + Update | `useEffect(()=>{},[deps])`         |

---
**useEffect এর Basic Practical Examples** দেবো – যেখানে API fetch, event handle, timer, localStorage, dependency change সব দেখানো হবে।
---

# 📌 Example 1 — API Fetch Only First Render এ (Very Basic)

```jsx
import { useEffect, useState } from "react";

function Users() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/users")
      .then(res => res.json())
      .then(data => setUsers(data));
  }, []); 

  return (
    <div>
      <h2>Users List</h2>
      {users.map(u => <p key={u.id}>{u.name}</p>)}
    </div>
  );
}
```

### এখানে কী হল?

| Code       | Meaning                                   |
| ---------- | ----------------------------------------- |
| `[]`       | Component first time mount হলে effect run |
| `fetch()`  | API থেকে data আনা                         |
| `setUsers` | Data state এ save → UI তে দেখানো          |

📌 Real use — component load হলে API থেকে ডাটা এনে দেখাতে ব্যবহার হয়।

---

# 📌 Example 2 — Button click হলে data fetch

```jsx
function App() {
  const [id, setId] = useState(1);
  const [post, setPost] = useState({});

  useEffect(() => {
    fetch(`https://jsonplaceholder.typicode.com/posts/${id}`)
      .then(r => r.json())
      .then(data => setPost(data));
  }, [id]); // id change হলেই API পুনরায় hit হবে

  return (
    <>
      <button onClick={() => setId(id+1)}>Load Next Post</button>
      <h3>{post.title}</h3>
      <p>{post.body}</p>
    </>
  );
}
```

### Key Concept:

✔ প্রথম render এ run
✔ কিন্তু **id পরিবর্তন হলেই আবার run** → dependency array

📌 Useful for pagination, filtering, dropdown change.

---

# 📌 Example 3 — Timer (setInterval) + Cleanup

```jsx
function Timer(){
  const [count, setCount] = useState(0);

  useEffect(()=>{
     const timer = setInterval(()=>{
        setCount(c => c + 1);
     },1000);

     return ()=> clearInterval(timer); // Cleanup on unmount
  },[]);

  return <h1>Timer: {count}</h1>;
}
```

### এখানে শিখলে কী?

| Feature                     | কেন দরকার                       |
| --------------------------- | ------------------------------- |
| `setInterval`               | প্রতি সেকেন্ডে count +1         |
| `return()=>clearInterval()` | component remove হলে timer বন্ধ |

📌 Very important — cleanup না দিলে memory leak হবে।

---

# 📌 Example 4 — Window Resize Listener (Event add/remove)

```jsx
function WindowSize(){
  const [width,setWidth] = useState(window.innerWidth);

  useEffect(()=>{
    const handleResize = () => setWidth(window.innerWidth);
    window.addEventListener("resize",handleResize);

    return () => window.removeEventListener("resize",handleResize);
  },[]);

  return <h2>Screen width: {width}px</h2>
}
```

📌 Mount এ listener add করা হয়
📌 Unmount এ remove করা হয়

---

# 📌 Example 5 — LocalStorage Save & Load

```jsx
function App(){
  const [name,setName]=useState(() => {
    return localStorage.getItem("user") || "";
  });

  useEffect(()=>{
    localStorage.setItem("user",name);
  },[name]);

  return(
    <input 
      value={name} 
      onChange={e=>setName(e.target.value)} 
      placeholder="Enter Name" 
    />
  );
}
```

### What happens?

✔ input change → name change
✔ useEffect run → value localStorage এ save
✔ reload করলেও value থেকে যায়

---

# 📌 Example 6 — Loading + Error Handling with API

```jsx
function Example(){
  const [data,setData] = useState(null);
  const [loading,setLoading] = useState(true);
  const [error,setError] = useState(null);

  useEffect(()=>{
    fetch("https://api.example.com/data")
      .then(r=>r.json())
      .then(res=>{
        setData(res);
        setLoading(false);
      })
      .catch(err=>{
        setError(err.message);
        setLoading(false);
      });
  },[]);

  if(loading) return <h3>Loading..</h3>
  if(error) return <h3>Error: {error}</h3>

  return <pre>{JSON.stringify(data,null,2)}</pre>
}
```

এটা হলো **perfect production pattern।**
✔ loading
✔ error
✔ data state

---

---

# এক কথায় Summary (Basic + Practical)

| Goal                     | Example                       |
| ------------------------ | ----------------------------- |
| First render এ data আনতে | `useEffect(()=>{fetch()},[])` |
| State change হলে action  | `useEffect(()=>{},[state])`   |
| Timer/Interval           | Cleanup সহ useEffect          |
| Event listener           | Add → Cleanup remove          |
| LocalStorage sync        | `[value]` dependency ব্যবহার  |

---