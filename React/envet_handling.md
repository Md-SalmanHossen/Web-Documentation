**React Event Handling** সম্পূর্ণ, step-by-step, 100% theoretical + practical explain 


#### 1) Introduction to React Event Handling

* React e **events handle করা হয় JSX এর মাধ্যমে**
* React uses **Synthetic Events** → cross-browser wrapper of native events
* Syntax **camelCase** → `onClick`, `onChange`, `onSubmit`
* Difference from HTML: **No quotes for functions**

```jsx
<button onClick={handleClick}>Click Me</button>
```


#### 2) Basic onClick Event

#### Functional Component

```jsx
function App() {
  function handleClick() {
    alert("Button Clicked!");
  }

  return <button onClick={handleClick}>Click Me</button>;
}
```

Note:

* **Do not call function with () in JSX** → `onClick={handleClick()}` ❌
* Correct → `onClick={handleClick}` ✔



#### 3) Event with Parameters

```jsx
function App() {
  function greet(name) {
    alert(`Hello ${name}`);
  }

  return <button onClick={() => greet("Salman")}>Greet</button>;
}
```

* Arrow function needed to pass arguments
* Without arrow → function executes immediately


#### 4) Synthetic Event Object

All event handlers get **event object** by default

```jsx
function App() {
  function handleClick(event) {
    console.log(event); // SyntheticEvent object
    console.log(event.target); // the element clicked
  }

  return <button onClick={handleClick}>Click</button>;
}
```

* `event.preventDefault()` → prevent default behavior
* `event.stopPropagation()` → stop event bubbling


#### 5) Handling Form Events

##### Input Change

```jsx
function App() {
  const [name, setName] = React.useState("");

  function handleChange(event) {
    setName(event.target.value);
  }

  return (
    <>
      <input type="text" value={name} onChange={handleChange} />
      <p>Hello {name}</p>
    </>
  );
}
```

* `value` + `onChange` = **controlled component**
* React prefers controlled components for form inputs


#### Form Submit

```jsx
function App() {
  const [text, setText] = React.useState("");

  function handleSubmit(event) {
    event.preventDefault();
    alert(`Submitted: ${text}`);
  }

  return (
    <form onSubmit={handleSubmit}>
      <input value={text} onChange={e => setText(e.target.value)} />
      <button type="submit">Submit</button>
    </form>
  );
}
```

* **Important**: default HTML submit reloads page → `preventDefault()`


#### 6) Mouse Events

```jsx
function App() {
  function handleMouseEnter() {
    console.log("Mouse Entered!");
  }

  function handleMouseLeave() {
    console.log("Mouse Left!");
  }

  return (
    <div onMouseEnter={handleMouseEnter} onMouseLeave={handleMouseLeave} style={{padding:"20px", background:"lightgray"}}>
      Hover Me
    </div>
  );
}
```

* Other mouse events: `onMouseDown`, `onMouseUp`, `onMouseMove`, `onClick`

---

#### 7) Keyboard Events

```jsx
function App() {
  function handleKeyDown(e) {
    console.log("Key pressed:", e.key);
  }

  return <input type="text" onKeyDown={handleKeyDown} />;
}
```

* `onKeyDown`, `onKeyUp`, `onKeyPress`
* `e.key` gives key pressed, `e.code` gives code

---

#### 8) Passing Event + Props

Child → Parent communication using event + props

```jsx
function Child({onCustomClick}) {
  return <button onClick={() => onCustomClick("Data from Child")}>Click Me</button>;
}

function Parent() {
  function handleChildClick(data) {
    alert(data);
  }

  return <Child onCustomClick={handleChildClick} />;
}
```

* Most common **React pattern** in real apps


#### Conditional / Dynamic Event Handling

```jsx
function App() {
  const [active, setActive] = React.useState(false);

  return (
    <button onClick={() => setActive(!active)} className={`px-4 py-2 ${active ? "bg-green-600" : "bg-red-600"}`}>
      {active ? "Active" : "Inactive"}
    </button>
  );
}
```

* Boolean state → toggle UI + event together


#### 10) Event Pooling (Important React Concept)

* SyntheticEvent is **pooled** → properties may become null in async code

```jsx
function App() {
  function handleClick(e) {
    setTimeout(() => console.log(e.target), 1000); // ❌ may be null
  }
  return <button onClick={handleClick}>Click</button>;
}
```

* Fix: `e.persist()`

```jsx
function handleClick(e) {
  e.persist();
  setTimeout(() => console.log(e.target), 1000); // works
}
```


#### 11) Common React Event Types

| Event     | Example                                            |
| --------- | -------------------------------------------------- |
| Mouse     | onClick, onDoubleClick, onMouseEnter, onMouseLeave |
| Keyboard  | onKeyDown, onKeyUp, onKeyPress                     |
| Form      | onChange, onSubmit, onFocus, onBlur                |
| Focus     | onFocus, onBlur                                    |
| Clipboard | onCopy, onCut, onPaste                             |
| Drag      | onDragStart, onDragOver, onDrop                    |



#### 12) Best Practices

1. Event handler functions **defined outside JSX**
2. Use **arrow functions only when passing params**
3. Use **controlled components** for form inputs
4. Avoid **inline functions** in big lists → performance issue
5. Remember **camelCase events** in JSX
6. Always **preventDefault** when needed


#### Summary — React Event Handling

1. **onClick/onChange/onSubmit** → basic events
2. **SyntheticEvent** → cross-browser wrapper
3. **Pass parameters** → use arrow function
4. **Forms** → controlled components
5. **Child → Parent** → function props callback
6. **Dynamic events + state** → UI update
7. **Event Pooling** → e.persist() if async needed
8. **Mouse/Keyboard/Form/Focus/Clipboard/Drag** → almost everything handled



💡 Pro Tip:
**Every interactive UI in React uses these patterns** — buttons, forms, toggles, modals, notifications, dynamic lists — all use event handling + state + props.

