

## useEffect — Complete Guide in Bangla 

React মূলত UI render করে। কিন্তু application এর ভিতরে শুধুই UI থাকে না — API request, timer চালানো, browser events, localStorage, socket connection ইত্যাদি অনেক কাজ থাকে।
এই React-বাইরের কাজগুলোকেই বলে **Side Effect**।
আর Side Effect control করার জন্য React দেয় **useEffect() hook**।



### Side Effect কী?

UI update এর বাইরে browser বা external system যেসব কাজ করে — সেগুলো Side Effect।

উদাহরণ:

| কাজ                       | কেন Side Effect?             |
| ------------------------- | ---------------------------- |
| API Fetch                 | Browser request handle করে   |
| setTimeout / setInterval  | Timer browser চালায়          |
| add/remove event listener | DOM browser control করে      |
| localStorage              | storage React নয় browser করে |
| WebSocket / server IO     | external connection প্রয়োজন  |

অর্থাৎ **UI ছাড়া বাইরের কাজ মানেই Side Effect।**



## React Component Lifecycle (Basic Idea)

| Stage   | কী হয়                                |
| ------- | ------------------------------------ |
| Mount   | Component প্রথম render হয়            |
| Update  | State/Props পরিবর্তনে নতুন render হয় |
| Unmount | Component UI থেকে মুছে যায়           |

useEffect এই তিনটা lifecycle moment-এ কাজ চালাতে পারে।



### useEffect কী করে?

* render হওয়ার পরে কোড execute করে
* dependency অনুযায়ী update হলে আবার run করে
* unmount হলে cleanup চালাতে পারে

এক লাইনে:
**useEffect = Side Effect handler + Lifecycle controller**



### useEffect Syntax

```jsx
useEffect(() => {
  // Side effect actions

  return () => {
    // Cleanup (optional)
  };
}, [dependencies]);
```



# useEffect এর ৩ ধরণ

### 1) শুধু Mount এ চলবে

```jsx
useEffect(() => {
  console.log("Runs once");
}, []);
```

ব্যবহার:

* প্রথমবার API call
* one-time setup
* initial load tasks


### 2) dependency change হলে চলবে

```jsx
useEffect(() => {
  console.log("count changed:", count);
}, [count]);
```

ব্যবহার:

* search input পরিবর্তন → নতুন data load
* id বদলালে নতুন API call
* calculated values update



### 3) Cleanup + Unmount Handling

```jsx
useEffect(() => {
  const timer = setInterval(() => console.log("tick"), 1000);

  return () => clearInterval(timer); // Cleanup
}, []);
```

ব্যবহার:

* timer/interval বন্ধ করা
* event listener remove করা
* socket/websocket disconnect করা



# Full Lifecycle Example

```jsx
function Example(){
  const [count,setCount] = useState(0);

  useEffect(() => {
    console.log("Mounted");
    return () => console.log("Unmounted");
  }, []);

  useEffect(() => {
    console.log("count updated:", count);
  }, [count]);

  return <button onClick={() => setCount(count+1)}>Increase</button>;
}
```



# Practical Examples (Beginner Friendly)

### 1) Mount এ শুধু একবার API লোড

```jsx
useEffect(() => {
  fetch("/api/users")
    .then(res => res.json())
    .then(data => setUsers(data));
}, []);
```



### 2) Dependency অনুযায়ী data reload

```jsx
useEffect(() => {
  fetch(`/api/post/${id}`)
    .then(res => res.json())
    .then(data => setPost(data));
}, [id]);
```

-

### 3) Loading + Error সহ API handling

```jsx
useEffect(() => {
  setLoading(true);
  fetch("/api/data")
    .then(res => res.json())
    .then(data => { setData(data); setLoading(false); })
    .catch(err => { setError(err.message); setLoading(false); });
}, []);
```



### 4) Window Resize Listener + Cleanup

```jsx
useEffect(() => {
  const handle = () => setWidth(window.innerWidth);
  window.addEventListener("resize", handle);

  return () => window.removeEventListener("resize", handle);
}, []);
```



# Final Quick Cheatsheet

| লক্ষ্য                   | Pattern                               |
| ------------------------ | ------------------------------------- |
| Just once run (on mount) | `useEffect(..., [])`                  |
| Value change এ run       | `useEffect(..., [value])`             |
| Every render             | `useEffect(...)` (কোন dependency নেই) |
| Cleanup on unmount       | `return ()=>{}`                       |
| API fetch                | Mount / dependency ভিত্তিক useEffect  |
| Timer/Event Listener     | useEffect + Cleanup                   |



### Bottom Summary

* UI দেখাবে useState
* UI ছাড়া বাকি কাজ সামলাবে useEffect
* Dependency থাকলে update এ run হবে
* Cleanup unmount এ কাজ করবে

#### বোঝার শর্ট ট্রিক:
কবে useEffect লাগবে?	মনে রাখো
Screen first load	only once → []
কিছু change observe করতে চাই	[dependency] দেবে
Cleanup দরকার	return ()=>{}
API load করতে চাই	সবসময় [] inside useEffect


**useEffect এর Basic Practical Examples** – যেখানে API fetch, event handle, timer, localStorage, dependency change দেখানো হবে।



#### Example 1 — API Fetch Only First Render এ (Very Basic)

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

##### এখানে কী হল?

| Code       | Meaning                                   |
| ---------- | ----------------------------------------- |
| `[]`       | Component first time mount হলে effect run |
| `fetch()`  | API থেকে data আনা                         |
| `setUsers` | Data state এ save → UI তে দেখানো          |

Real use — component load হলে API থেকে ডাটা এনে দেখাতে ব্যবহার হয়।



#### Example 2 — Button click হলে data fetch

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

##### Key Concept:

✔ প্রথম render এ run
✔ কিন্তু **id পরিবর্তন হলেই আবার run** → dependency array

Useful for pagination, filtering, dropdown change.



#### Example 3 — Timer (setInterval) + Cleanup

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

#### এখানে শিখলে কী?

| Feature                     | কেন দরকার                       |
| --------------------------- | ------------------------------- |
| `setInterval`               | প্রতি সেকেন্ডে count +1         |
| `return()=>clearInterval()` | component remove হলে timer বন্ধ |

 Very important — cleanup না দিলে memory leak হবে।



#### 📌 Example 4 — Window Resize Listener (Event add/remove)

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

- Mount এ listener add করা হয়
- Unmount এ remove করা হয়


##### 📌 Example 5 — LocalStorage Save & Load

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

##### What happens?

- input change → name change
- useEffect run → value localStorage এ save
- reload করলেও value থেকে যায়



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
- loading
- error
- data state


###### 🎯 এক কথায় Summary 
| Goal                     | Example                       |
| ------------------------ | ----------------------------- |
| First render এ data আনতে | `useEffect(()=>{fetch()},[])` |
| State change হলে action  | `useEffect(()=>{},[state])`   |
| Timer/Interval           | Cleanup সহ useEffect          |
| Event listener           | Add → Cleanup remove          |
| LocalStorage sync        | `[value]` dependency ব্যবহার  |

---

