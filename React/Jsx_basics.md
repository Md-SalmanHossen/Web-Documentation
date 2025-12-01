

### JSX — What is it?

**JSX = JavaScript + XML**

* React এ আমরা যেই code লিখি UI এর জন্য, সেটা JSX।
* HTML এর মতো structure, কিন্তু JavaScript logic embed করা যায়।
* Browser সরাসরি JSX read করতে পারে না → React **Babel** দিয়ে convert করে normal JS এ।

```jsx
const element = <h1>Hello World</h1>;
```


### 1) JSX Rules / Syntax

1. **Single Parent Element**

```jsx
// ❌ Wrong
const element = <h1>Hello</h1><p>World</p>;

// ✅ Correct
const element = (
  <div>
    <h1>Hello</h1>
    <p>World</p>
  </div>
);
```

2. **JavaScript inside JSX → {}**

```jsx
const name = "Salman";
const element = <h1>Hello {name}</h1>;
```

3. **Attributes → camelCase**

```jsx
<img src="logo.png" alt="Logo" />
<input type="text" placeholder="Enter Name" />
```

* `class` → `className`
* `for` → `htmlFor`

```jsx
<div className="container">Hello</div>
<label htmlFor="input">Name</label>
```

4. **JSX supports expressions**

```jsx
const a = 10, b = 20;
const element = <h1>{a + b}</h1>; // 30
```


#### 2) Conditional Rendering

1. **Ternary Operator**

```jsx
const isLoggedIn = true;
const element = <h1>{isLoggedIn ? "Welcome" : "Please login"}</h1>;
```

2. **Logical AND**

```jsx
const showMessage = true;
const element = <div>{showMessage && <p>Hello User</p>}</div>;
```


#### 3) Rendering Lists (Array)

```jsx
const skills = ["React", "Node", "MongoDB"];
const element = (
  <ul>
    {skills.map((skill, index) => <li key={index}>{skill}</li>)}
  </ul>
);
```

`key` is mandatory for dynamic lists


#### 4) Embedding Components in JSX

```jsx
function Welcome({name}) {
  return <h1>Hello {name}</h1>;
}

const element = <Welcome name="Salman" />;
```

* Component names **must start with Capital letter**
* Props pass data to child component

---

#### 5) Inline Styles in JSX

```jsx
const style = {color:"blue", fontSize:"20px"};
const element = <h1 style={style}>Styled Text</h1>;
```

or inline directly:

```jsx
const element = <h1 style={{color:"red", fontWeight:"bold"}}>Hello</h1>;
```

#### 6) JSX supports Events

```jsx
function handleClick() {
  alert("Clicked!");
}

const element = <button onClick={handleClick}>Click Me</button>;
```

* Events are camelCase → `onClick`, `onChange`, `onSubmit`

---

#### 7) JSX Comments

```jsx
const element = (
  <div>
    {/* This is a JSX comment */}
    <h1>Hello</h1>
  </div>
);
```

---

#### 8) Fragments in JSX

* Use `<> ... </>` or `<React.Fragment> ... </>` to avoid extra div

```jsx
const element = (
  <>
    <h1>Heading</h1>
    <p>Paragraph</p>
  </>
);
```


#### 9) Nesting & Expressions

```jsx
function App(){
  const user = {name:"Salman", age:22};
  return (
    <div>
      <h1>{`Hello ${user.name}, Age: ${user.age}`}</h1>
      {user.age > 18 ? <p>Adult</p> : <p>Minor</p>}
    </div>
  );
}
```


#### 10) JSX Best Practices

1. Always **wrap multiple elements** in a parent element or Fragment
2. Use **camelCase** for attributes
3. Dynamic data → `{}`
4. Keep **component name capitalized**
5. Use **keys** for lists


#### Summary Table

| Concept       | JSX Example                      |
| ------------- | -------------------------------- |
| Single Parent | `<div><h1></h1></div>`           |
| Expression    | `{a + b}`                        |
| Conditional   | `{isLoggedIn ? "Yes" : "No"}`    |
| List          | `{arr.map((i)=> <li>{i}</li>)}`  |
| Component     | `<Welcome name="Salman"/>`       |
| Event         | `<button onClick={handleClick}>` |
| Style         | `<h1 style={{color:"red"}}>`     |
| Fragment      | `<> ... </>`                     |
| Comment       | `{/* comment */}`                |


