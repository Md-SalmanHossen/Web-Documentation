 **useRef এর complete guide** step-by-step, theory থেকে practical, interview perspective পর্যন্ত। 

---

#### useRef কী?

`useRef` হলো React-এর **built-in Hook**, যা ব্যবহার করা হয়:

1. **DOM element access করার জন্য**
2. **Mutable value store করার জন্য, যা re-render trigger করে না**
3. **Previous state/value track করার জন্য**

**Syntax:**

```js
const refName = useRef(initialValue);
```

* Current value access করতে হয়: `refName.current`



#### useRef vs useState

| Feature                      | useState  | useRef      |
| ---------------------------- | --------- | ----------- |
| Triggers re-render on change | ✅ Yes     | ❌ No        |
| Store primitive/object/array | ✅ Yes     | ✅ Yes       |
| Access DOM elements          | ❌ No      | ✅ Yes       |
| Track previous value         | Possible  | ✅ Best      |
| Functional updates needed?   | Sometimes | Usually not |



#### Common use cases

##### 1) DOM element access

```jsx
import { useRef } from "react";

function InputFocus() {
  const inputRef = useRef();

  const handleFocus = () => {
    inputRef.current.focus(); // direct DOM access
  }

  return (
    <div>
      <input ref={inputRef} type="text" placeholder="Type here" />
      <button onClick={handleFocus}>Focus Input</button>
    </div>
  );
}

export default InputFocus;
```
Button চাপলেই input auto-focus।


##### 2) Store mutable value without re-render

```jsx
import { useRef, useState } from "react";

function RefCounter() {
  const countRef = useRef(0);
  const [dummy, setDummy] = useState(0);

  const increase = () => {
    countRef.current += 1;
    console.log("CountRef:", countRef.current);
    // UI update করতে চাইলে setDummy ব্যবহার করতে হবে
    setDummy(dummy + 1);
  }

  return (
    <div>
      <h2>Check console for countRef</h2>
      <button onClick={increase}>Increase</button>
    </div>
  );
}

export default RefCounter;
```

countRef change হলেও UI automatic re-render হয় না।

---

##### 3) Track previous state

```jsx
import { useState, useEffect, useRef } from "react";

function PreviousValueTracker() {
  const [count, setCount] = useState(0);
  const prevCount = useRef(0);

  useEffect(() => {
    prevCount.current = count; // update previous value after each render
  }, [count]);

  return (
    <div>
      <h2>Current: {count}</h2>
      <h3>Previous: {prevCount.current}</h3>
      <button onClick={() => setCount(count + 1)}>Increase</button>
    </div>
  );
}

export default PreviousValueTracker;
```

Previous state track করা সহজ।


##### 4) Timer / Interval / Mutable ID storage

```jsx
import { useRef, useState } from "react";

function Timer() {
  const [time, setTime] = useState(0);
  const timerRef = useRef(null);

  const start = () => {
    timerRef.current = setInterval(() => {
      setTime(t => t + 1);
    }, 1000);
  }

  const stop = () => {
    clearInterval(timerRef.current);
  }

  return (
    <div>
      <h2>Time: {time}</h2>
      <button onClick={start}>Start</button>
      <button onClick={stop}>Stop</button>
    </div>
  );
}

export default Timer;
```

Timer ID store হচ্ছে useRef এ, re-render free।



#### Interview Perspective

**প্রশ্ন আসতে পারে:**

1. **What is useRef in React?**

   * Hook for storing mutable values and accessing DOM without causing re-render.

2. **Difference between useRef and useState?**

   * useState → triggers re-render
   * useRef → does NOT trigger re-render

3. **How to track previous state in functional components?**

   * useRef + useEffect combination

4. **When to use useRef for DOM?**

   * Focus input, scroll, animations, measuring element size

5. **Can useRef store objects/arrays?**

   * Yes, mutable, re-render-free



#### Tips / Best Practices

* DOM access only when necessary
* UseRef value change won’t trigger UI update
* For previous state tracking → useEffect + useRef
* For interval / timeout IDs → useRef is best
* Avoid overusing for state you want UI to reflect


**Summary Table**

| Feature                   | useState  | useRef      |
| ------------------------- | --------- | ----------- |
| Re-render on change       | ✅ Yes     | ❌ No        |
| Store value               | ✅ Yes     | ✅ Yes       |
| Access DOM                | ❌ No      | ✅ Yes       |
| Track previous value      | Possible  | ✅ Best      |
| Functional update needed? | Sometimes | Usually not |


