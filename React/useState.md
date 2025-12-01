### useSate complete lecture for beginner


#### What Exactly is `useState`?
`sate` means something that may change.
`useState` হলো React-এর built-in Hook যেটা **component-এর ভিতরে state তৈরি ও manage** করতে দেয়।

**React State = Data that changes over time & triggers re-render.**
UI = State এর reflection.

| Without state   | With state               |
| --------------- | ------------------------ |
| Data fixed থাকে | Data change হতে পারে     |
| UI static       | UI dynamically update হয় |
| No interaction  | Interaction possible     |



#### Why we need `useState`?

Because **UI should respond to changes.**

Example:
- Button চাপলে counter বাড়বে
- Input এ লিখলে live output দেখাবে
- Form submit হলে data দেখাবে
- Theme change হলে UI update হবে

এই dynamic behaviour এর জন্যই state লাগে → আর সেটা দেয় `useState`.


#### useState Syntax Breakdown

```js
const [value, setValue] = useState(initialValue);
```

Now break this slowly ↓

| Part           | Meaning                       |
| -------------- | ----------------------------- |
| `value`        | current state (UI দেখায় এটাই) |
| `setValue()`   | update করার function          |
| `initialValue` | প্রথমে state যা থাকবে         |

State শুধুমাত্র `setValue()` দিয়ে change করতে হবে।
Direct modify করলে UI update হবে না।

❌ Wrong: `value = 5`
✔ Correct: `setValue(5)`



#### How useState Works Internally

এটা অনেকেই জানে না — তাই তুমি এগিয়ে থাকবে।
React যখন component রেন্ডার করে, তখন প্রতিবার useState কে একটা internal slot ভাবে।

#### Think Like This (Mental Model)

```
Component Memory:
slot1 → useState(0) => count
slot2 → useState("Hi") => text
slot3 → useState([]) => todos
```

Component rerender হয় — কিন্তু slot same থাকে।
তাই পুরনো value lost হয় না।


#### Why React won't update UI immediately?

Because React batches & schedules updates — যাতে প্রতি পরিবর্তনে UI না ঝাঁকে।
তাই setState asynchronous behaviour দেখায়।


Example:

```js
setCount(count + 1);
setCount(count + 1);
setCount(count + 1);
```

তিনবার করলে count +3 আশা করি, কিন্তু result হয় +1 (কারণ একই old value use করে)।

Correct way ⬇

```js
setCount(prev => prev + 1);
setCount(prev => prev + 1);
setCount(prev => prev + 1);  // now +3
```

Rule: যদি previous state দরকার হয় → functional update ব্যবহার করবে।



#### Types of values allowed inside useState

| Type            | Example             |
| --------------- | ------------------- |
| Number          | Counter             |
| String          | Text change         |
| Boolean         | Toggle show/hide    |
| Array           | Todo list           |
| Object          | User data           |
| Function return | Lazy initialization |



##### Lazy Initialization (Important for Optimization)

Default value expensive হলে এভাবে use করো:

```js
const [data, setData] = useState(() => heavyCalculation());
```

React initial render এ *মাত্র একবার* function run করবে।
না হলে প্রত্যেক রেন্ডারে calculation হত।


### Every Pattern of useState You Must Know


#### Number State

```jsx
const [count, setCount] = useState(0);
<button onClick={() => setCount(count + 1)}>+</button>
```



#### String State

```jsx
const [msg, setMsg] = useState("Hello");
<button onClick={() => setMsg("Welcome!")}>Change</button>
```



#### Boolean Toggle

```jsx
const [show, setShow] = useState(true);
<button onClick={()=>setShow(prev=>!prev)}>
  {show ? "Hide" : "Show"}
</button>
```



#### Array Update

```jsx
setList([...list, "New Item"]);  // Add
setList(list.filter(x=>x !== item)); // Remove
```



#### Object Update (Most Misused)

❌ Wrong

```js
user.name = "Rahim"
setUser(user)
```

✔ Correct

```js
setUser({...user, name: "Rahim"})
```

Spread ছাড়া update করলে re-render detect করবে না।


#### When NOT to use useState?

| Situation                      | Better Alternative        |
| ------------------------------ | ------------------------- |
| Value UI তে দরকার নেই          | useRef                    |
| Heavy synchronous re-rendering | useReducer                |
| Global state needed            | Context / Zustand / Redux |
| Timer/interval ID store        | useRef better             |


#### Pitfalls You Must Know (Most Important)

| Mistake                                   | Why wrong          |
| ----------------------------------------- | ------------------ |
| state directly mutate                     | UI update হবে না   |
| async setState immediate value expect করা | delay behaviour    |
| loop ভেতরে uncontrolled state updates     | infinite re-render |
| too many state pieces unnecessarily       | performance drop   |


#### Best Practices (Industry-Level)

✔ Always update state using function when depends on previous value:

```js
setCount(c => c+1);
```

✔ Object/Array update → always create new copy

```js
setUser(prev => ({...prev, age: prev.age+1}));
```

✔ Heavy initial state → lazy initializer

```js
useState(()=>compute());
```

✔ Input form always use controlled components

```js
<input value={name} onChange={e=>setName(e.target.value)} />
```

✔ Multiple state combined → useReducer preferred



#### Final Mindset Summary

| Feature                  | Meaning              |
| ------------------------ | -------------------- |
| useState changes UI      | ALWAYS 💥            |
| setState async & batched | Not instant          |
| No direct modify         | Must use setter      |
| Arrays/Objects mutate নয় | New copy             |
| Previous value দরকার?    | Functional update    |
| Heavy initial value?     | Lazy init            |
| State too many হলে?      | useReducer / Context |



**useState কে interview perspective** থেকে সম্পূর্ণ বিশ্লেষণ—`useState` কীভাবে data মনে রাখে, কেন immutable, আর কবে কোন question আসতে পারে।


#### useState কি data মনে রাখে?

**Answer:** হ্যাঁ, `useState` **component এর memory তে state value রাখে**।

##### Internally:

* প্রতিটি functional component render হলে React একটি **state memory slot** allocate করে।
* useState যেটা return করে সেটা হচ্ছে **current value + update function**।
* যেকোনো re-render এ React পুরনো state **loss করে না**, কারণ সেই memory slot এখনো component instance এর সাথে সংযুক্ত থাকে।

**Mental Model:**

```text
Component Memory:
slot1 -> useState(0) => count
slot2 -> useState("Hello") => name
slot3 -> useState([]) => tasks
```

* রেন্ডার হবে → নতুন UI generate হবে
* কিন্তু `count`, `name`, `tasks` আগের value retain করবে।

তাই useState “data মনে রাখে” এবং next render এ ব্যবহার হয়।



#### useState কি immutable?

**Answer:** Hae, **state যে value useState এ রাখে, সেটা immutable ধরো**।

**কেন immutable?**

* React re-render detect করতে **state reference change দেখতে চায়**।
* যদি তুমি direct mutate করো (object/array), তখন **reference same থাকে**, আর React বুঝবে **কিছু change হয়নি** → UI update হবে না।

**Example (Wrong / Immutable rule violation)**

```js
const [user, setUser] = useState({name:"Salman", age:22});
user.age = 23;   // ❌ mutable change
setUser(user);   // React re-render detect করতে পারবে না
```

**Correct way (Immutable)**

```js
setUser({...user, age: 23});  // ✅ new object
```

* Array: `push` বা `pop` direct না, বরং spread / filter / slice ব্যবহার করো
* Primitive types (number, string, boolean) সাধারণত safe


#### Interview Perspective

**প্রশ্ন আসতে পারে:**

1. **How does useState store value between renders?**

   * React assigns a memory slot per component instance.
   * State persists in that slot across re-renders.

2. **Why is state in React immutable?**

   * Immutable state ensures React can detect changes via reference comparison.
   * Mutable changes break re-rendering.

3. **Difference between direct mutation & setState update?**

   ```js
   count = count + 1  // ❌
   setCount(count+1)  // ✅ triggers re-render
   ```

4. **Functional update usage? Why?**

   * When new state depends on previous state.

   ```js
   setCount(prev => prev + 1);
   ```

5. **What types of values can be stored in useState?**

   * Primitive (number, string, boolean)
   * Object
   * Array
   * Function return (lazy initialization)

6. **Lazy initialization importance?**

   * Heavy computation for initial value

   ```js
   const [data, setData] = useState(() => expensiveFunction());
   ```

   * Function runs only once → performance optimization



#### Extra Tips for Interviews

* “State is preserved between renders” → buzzword
* “State is immutable” → must explain why, reference vs value
* “Functional update” → show you understand async updates
* “Lazy initialization” → bonus point for optimization question
* Object / Array updates → spread / map / filter → always use new reference



💡 **Summary Table (Interview Ready)**

| Question                             | Answer                                                                                            |
| ------------------------------------ | ------------------------------------------------------------------------------------------------- |
| Does useState remember data?         | ✅ Yes, stored in memory slot of component instance                                                |
| Is state mutable?                    | ❌ No, state should be treated immutable                                                           |
| Why immutable?                       | React uses reference comparison to detect changes → re-render triggered only if reference changes |
| How to update previous state safely? | Functional update: `setState(prev => newValue)`                                                   |
| Can useState store object/array?     | ✅ Yes, but must update immutably (`...spread`)                                                    |
| Lazy initialization?                 | ✅ `useState(() => heavyComputation())`                                                            |

