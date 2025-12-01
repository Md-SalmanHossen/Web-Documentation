

# ⚛️ React `useEffect` Hook: সম্পূর্ণ ডকুমেন্টেশন এবং ব্যবহারবিধি

## I. 💥 Side Effect কী এবং কেন দরকার?

React একটি **UI Rendering Framework**—এর প্রাথমিক কাজ হল **state/props** অনুযায়ী UI (User Interface) আপডেট করা।

কিন্তু UI রেন্ডারিং-এর বাইরেও অনেক কাজ থাকে যেগুলো React-এর রেন্ডার চক্রের (Render Cycle) বাইরে থেকে ব্রাউজার বা পরিবেশ (Environment) দ্বারা পরিচালিত হয়। এই কাজগুলোকেই **Side Effect** বলা হয়।

### Side Effect-এর সংজ্ঞা

> Component **render** হওয়ার পর **React-এর বাইরে** যেসব কাজ execute হয়, সেগুলোই **Side Effect**।

সহজ ভাষায়, **যেকোনো কাজ যা UI draw না করে React-এর বাইরের বিশ্বে কিছু পরিবর্তন করে, সেটাই Side Effect।**

### 📋 Side Effect-এর উদাহরণ

| কাজ                         | কেন Side Effect?                      |
| :-------------------------- | :------------------------------------- |
| **API Call**                | Data fetch **Browser/Network** handle করে |
| `setTimeout` / `setInterval` | Asynchronous browser timer            |
| `event listener` add/remove | **DOM** এ direct attach                |
| `localStorage` read/write   | **External I/O** (Input/Output)        |
| `WebSocket`/`SSE` connect   | External live stream                  |

-----

## II. ⏳ Component Lifecycle (জীবনচক্র)

React Component তার জীবদ্দশায় ৩টি প্রধান ধাপ অতিক্রম করে। `useEffect` Hook এই ধাপগুলোর সাথে Side Effect-কে Sync করার জন্য ব্যবহৃত হয়।

| Phase       | অর্থ                                      | কখন ঘটে         |
| :---------- | :---------------------------------------- | :-------------- |
| **Mount**   | Component প্রথমবার DOM-এ প্রবেশ করে (screen এ আসে) | initial render  |
| **Update**  | `state`/`props` পরিবর্তন হয়ে পুনরায় render হওয়া | re-render       |
| **Unmount** | UI থেকে Component চিরতরে রিমুভ হওয়া       | disappear/close |

### Lifecycle-এর সরলীকরণ

1.  Component প্রথম show হলো → **Mount**
2.  `state` (যেমন button click) পরিবর্তন হলো → UI পুনরায় আঁকল → **Update**
3.  Page পরিবর্তন হলো/Component আর দেখা যাচ্ছে না → **Unmount**

-----

## III. ✨ `useEffect` Hook-এর প্রয়োজনীয়তা

React-এ, আমাদের প্রায়শই দরকার হয়:

  * ✔ Render-এর **পরে** কোড চালানো (UI আপডেট নিশ্চিত করার পর)
  * ✔ External API call বা অ্যাসিঙ্ক্রোনাস কাজ পরিচালনা করা
  * ✔ Event listener অ্যাটাচ ও ডিসপোজ করা
  * ✔ Component রিমুভ হওয়ার সময় **Memory Cleanup** করা

`useEffect` Hook এই প্রয়োজনগুলো মেটায়।

> **`useEffect` = React lifecycle-এর সাথে Side Effect চালানোর এবং নিয়ন্ত্রণ করার টুল।**

### `useEffect`-এর সংক্ষিপ্ত সংজ্ঞা

> **UI render এর পরে যেকোনো Side Effect execute করার জন্য ব্যবহৃত React Hook হলো `useEffect` — যা Mount, State/Prop Update এবং Unmount অবস্থায় effect চালাতে ও cleanup করতে সাহায্য করে।**

অন্যভাবে: **`useEffect` = Side Effect Runner + Lifecycle Controller**

-----

## IV. ⚙️ `useEffect` Lifecycle Example এবং ব্যবহারবিধি

`useEffect` একটি কলব্যাক ফাংশন এবং একটি ডিপেন্ডেন্সি অ্যারে (Dependency Array) নেয়। এই অ্যারের উপরই এর কার্যকারিতা নির্ভর করে।

| State       | কখন ঘটে                      | Dependency Array       | Output Action                                 |
| :---------- | :---------------------------- | :--------------------- | :------------------------------------------- |
| **Mount**   | Component first time render  | `[]` (Empty Array)     | Effect শুধুমাত্র একবার run হয়।                |
| **Update**  | state/prop change → rerender | `[state/props]`        | ডিপেন্ডেন্সি পরিবর্তন হলেই effect আবার run হয়।  |
| **Unmount** | component remove/disappear   | `return () => {}`      | Cleanup ফাংশনটি run হয়।                      |

### 1\) 🟢 Mount (Component প্রথমবার DOM-এ আসা)

```jsx
// [] Empty Dependency Array
useEffect(() => {
  console.log("🟢 Component Mounted: API call, initial setup");
}, []);
```

**ব্যাখ্যা:** `[]` থাকার কারণে, Effect টি **শুধুমাত্র একবার**, প্রথম render-এর পরে run হয়। এটি API call বা ইভেন্ট লিসেনার যোগ করার জন্য আদর্শ।

-----

### 2\) 🔄 Update (state/props change হলে)

```jsx
// [count] Dependency Array
useEffect(() => {
  console.log("🔄 Count Updated:", count);
}, [count]);
```

**ফ্লো:**

1.  Component প্রথম render হলে Effect run হয়।
2.  `count` পরিবর্তন হলে Component **re-render** হয়।
3.  Effect আবার **trigger** হয় কারণ `count` পরিবর্তিত হয়েছে।

**ব্যবহার:** Search filter, data reload বা UI আপডেট লজিকের জন্য উপযোগী।

-----

### 3\) 🔴 Unmount এবং Cleanup

```jsx
// return() => cleanup function
useEffect(() => {
  const timer = setInterval(() => { /* ... */ }, 1000);
  console.log("Component setup done.");

  return () => {
    // এই কোডটি Component Unmount হওয়ার আগে run হবে
    clearInterval(timer); // Cleanup (Memory Leak এড়ানো)
    console.log("🔴 Component Unmounted: Timer cleared");
  };
}, []);
```

**Unmount কখন হয়?** Route change, Conditional rendering বা Component hide হলে।

**গুরুত্বপূর্ণ:** Cleanup-এর জন্য ব্যবহৃত হয়: `setInterval` clear, `event listener` remove, বা subscription disconnect করা।

-----

## V. ⚡ Full Lifecycle Example Combined

```jsx
function Example(){
  const [count,setCount] = useState(0);

  // Mount/Unmount Logic
  useEffect(() => {
    console.log("🟢 Component Mounted");
    return () => {
      console.log("🔴 Unmounted");
    };
  }, []); // Mount Only

  // Update Logic
  useEffect(() => {
    console.log("🔄 Updated: count =", count);
  }, [count]); // Runs on Mount & when count updates

  return (
    <div>
      <h2>Count: {count}</h2>
      <button onClick={() => setCount(count+1)}>Increase</button>
    </div>
  );
}
```

**Console Output Order:**

| Action                         | Console Output       |
| :------------------------------ | :-------------------- |
| 1. Component show (Initial Load) | `🟢 Mounted`          |
| 2. User clicked, state updated   | `🔄 Updated count = 1` |
| 3. Navigate away / hide component| `🔴 Unmounted`        |

-----

## VI. 📌 Basic Practical Use Cases

### Example 1: API Fetch (Mount Only)

**Goal:** Component Load হলে API থেকে User data আনা।

```jsx
import { useEffect, useState } from "react";

function Users() {
  const [users, setUsers] = useState([]);

  useEffect(() => {
    // ⚡ Side Effect: Data Fetching
    fetch("https://jsonplaceholder.typicode.com/users")
      .then(res => res.json())
      .then(data => setUsers(data));
  }, []); // Mount Only
  /* ... return JSX ... */
}
```

### Example 2: Dynamic Data Fetch (Dependency Update)

**Goal:** Button click-এ `id` পরিবর্তন হলে সেই `id`-এর নতুন post fetch করা।

```jsx
function App() {
  const [id, setId] = useState(1);
  const [post, setPost] = useState({});

  useEffect(() => {
    // ⚡ Side Effect: [id] পরিবর্তন হলে fetch run হবে
    fetch(`https://jsonplaceholder.typicode.com/posts/${id}`)
      .then(r => r.json())
      .then(data => setPost(data));
  }, [id]); // id change হলেই effect run
  /* ... return JSX ... */
}
```

### Example 3: Error Handling ও Loading State

**Goal:** API Call করার সময় `loading` এবং `error` state ম্যানেজ করা।

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
  },[]); // Mount Only

  if(loading) return <h3>Loading..</h3>
  if(error) return <h3>Error: {error}</h3>
  /* ... return data display ... */
}
// 💡 এটি Perfect Production Pattern
```

### Example 4: Window Event Listener + Cleanup

**Goal:** Window Resize ইভেন্ট ট্র্যাক করা এবং Unmount-এর সময় লিসেনার রিমুভ করা।

```jsx
function WindowSize(){
  const [width,setWidth] = useState(window.innerWidth);

  useEffect(()=>{
    const handleResize = () => setWidth(window.innerWidth);

    // ⚡ Side Effect: Event Listener Attach
    window.addEventListener("resize",handleResize);

    return () => window.removeEventListener("resize",handleResize); // 🧹 Cleanup: Remove listener
  },[]); // Mount/Unmount Only
  /* ... return JSX ... */
}
```

-----

## VII. 🔑 One Line Cheat Sheet (Final Review)

| Goal (লক্ষ্য)               | Dependency Array (নির্ভরশীলতা অ্যারে)               |
| :------------------------ | :------------------------------------------------- |
| **Mount** (প্রথমবার কাজ)     | `useEffect(()=>{...}, [])`                          |
| **Update** (State/Prop Change) | `useEffect(()=>{...}, [state/props])`                |
| **Unmount** (Cleanup)      | `useEffect(()=> return ()=>{ Cleanup Logic }, [])`  |
| **Mount + Every Re-render** | `useEffect(()=>{...})` (দ্বিতীয় আর্গুমেন্ট বাদ দিলে)       |

-----