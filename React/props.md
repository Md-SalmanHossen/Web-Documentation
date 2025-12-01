
### Props — React Theory and Practical Explained Deeply

#### 1. Props কী?

React এ component দু’ধরনের data handle করে:

1. **State** → component এর ভেতরের data
2. **Props** → component এর বাইরে থেকে পাওয়া data

**Props = “External Input to Component”**

যেভাবে function parameter নেয়, ঠিক সেইভাবে React Component **props গ্রহণ করে**।

> Component = function
> Props = function parameter

---

#### 2. কেন Props দরকার?

ধরি React এ ১টা card বানালে ভালো দেখায়। কিন্তু ১০টা card লাগবে।
তুমি যদি প্রতিটা card এ আলাদা মান (name, image, price) দিতে চাও, সেগুলো component এ individually hardcode করলে reusable থাকা যায় না।

- তাই parent component data পাঠায়
- child সেই data ব্যবহার করে UI তৈরি করে

**Props = Reusability + Dynamic UI**


#### 3. Props একদিকেই চলে → Uni-directional Data Flow

React এ data goes only:

```
Parent → Child
```

Child component props modify করতে পারে না।

কারণ:

| Props               | State                     |
| ------------------- | ------------------------- |
| Read-only           | Mutable (change allowed)  |
| External            | Internal                  |
| Parent → Child flow | Component নিজে handle করে |

> তাই Props একমুখী রাস্তা।
> Data নিচে নামে, উপরে ওঠে না।


#### 4. Props এর মূল উদ্দেশ্য

| উদ্দেশ্য            | ব্যাখ্যা                               |
| ------------------- | -------------------------------------- |
| Component reuse     | এক component বহু data দিয়ে use করা যায় |
| Data communication  | Parent থেকে child এ তথ্য পাঠানো        |
| Dynamic UI          | Website এ flexible content দেখানো      |
| Controlled behavior | Function, event child e পাঠানো যায়     |


#### 5. Props কীভাবে কাজ করে? (Mentally Imagine)

ধরি component হলো machine
আর props হলো সেই machine এ input দেওয়া র raw material.

```
Input → Process → Output
props → component → UI rendering
```

React component নিজে props তৈরি করে না — শুধুমাত্র **গ্রহণ করে**।

**Props never store data**
 শুধু UI-তে দেখানোর জন্য use হয়


#### 6. Props Immutable কেন?

ধরি তুমি বন্ধুর কাছ থেকে একটা বই ধার নিলে।
তোমার কাজ পড়া — বদলানো নয়।

Child component props **ব্যবহার করতে পারে**
কিন্তু **change করতে পারে না**

কারণ:

* React predictable UI maintain করতে চায়।
* Props change করা মানে data origin override করা।
* Data consistency নষ্ট হয়।


#### 7. Props Drilling — Theory

যখন একটি data বহু স্তরের component এ নিয়ে যেতে হয়:

```
App → A → B → C → D
```

Child component শুধুই data forward করে, নিজে দরকার না হলেও।

এটা ঝামেলা বাড়ায় → maintainability কমায়।

সমাধান:
✔ Context API
✔ Redux
✔ Zustand
(কিন্তু props না বুঝে এগুলো শেখা ভুল!)


#### 8. Functions as Props — Conceptual Explanation

Child component parent থেকে function receive করতে পারে।

এই function child event থেকে call হলে, data **উল্টা parent এ ফিরতে পারে**।

এটা হলো একমাত্র উপায় যেটা দিয়ে child 👉 parent এ information পাঠাতে পারে।

```
Parent sends function ↓
Child triggers function ↑
Parent receives child data
```

এটাকে বলে **Callback Communication Using Props**.


##### 9. Props.children — Theory Explanation

ধরি তুমি container বানালে
এখন container এর মধ্যে তুমি ইচ্ছা মতো text, card, button পাঠাবে।

**Props.children = component এর ভিতরে you-can-place-anything area**
যেন componentটা একটা খালি বাক্স — তুমি ভিতরে content ঢোকাচ্ছো।


### Props Summary 

| Concept        | One-line Meaning                        |
| -------------- | --------------------------------------- |
| Props          | External data passed to component       |
| Direction      | Parent → Child                          |
| Mutability     | Read-only                               |
| Purpose        | Reusability + Dynamic UI                |
| children       | Component এর ভিতরে JSX পাঠানোর ব্যবস্থা |
| function props | Child → parent communication            |
| drilling       | অনেক স্তর পার হয়ে data পাঠানো           |



Ebar **React Props Practical Deep Practice Guide** dicchi — jekhane তুমি practically সব শিখবেঃ

- Array props
- Object props
- Function props
- CSS props
- Conditional props
- children props
- Default props
- PropTypes → শেষের অংশে Fully deep theoretical + practica- 


#### 1. Basic Props Practical

#### `Parent.jsx`

```jsx
import Child from "./Child";

export default function Parent(){
  return <Child name="Salman" institute="UIU" age={22} />;
}
```

#### `Child.jsx`

```jsx
export default function Child({name, institute, age}) {
  return (
    <>
      <h2>Name: {name}</h2>
      <h3>Institute: {institute}</h3>
      <h4>Age: {age}</h4>
    </>
  );
}
```

Mindset: Component je value receive kore seta শুধু display kore। Change করতে পারে না।


#### 2. Array Props Practice

Often used for list rendering (users, posts, products)

#### `Parent.jsx`

```jsx
const skills = ["React", "Node", "MongoDB", "Express"];

export default function Parent(){
  return <Skills skills={skills} />;
}
```

#### `Skills.jsx`

```jsx
export default function Skills({skills}) {
  return (
    <ul>
      {skills.map(s => <li key={s}>{s}</li>)}
    </ul>
  );
}
```

-  Array আসলে loop/mapping er মাধ্যমে UI তে দেখায়।
-  `.map()` use is must for dynamic lists.



#### 3. Object Props Practice

Often used for user profile, product details

#### `Parent.jsx`

```jsx
const user = {
  name:"Salman",
  dept:"CSE",
  address:{ city:"Dhaka", country:"Bangladesh" }
};

export default function Parent(){
  return <Profile info={user} />;
}
```

#### `Profile.jsx`

```jsx
export default function Profile({info}) {
  return (
    <>
      <h2>{info.name}</h2>
      <h3>{info.dept}</h3>
      <p>{info.address.city}, {info.address.country}</p>
    </>
  );
}
```

Object props → dot notation দিয়ে access।


#### 4. Function Props (Most Important for jobs)

Parent function → child call → parent data receives

#### `Parent.jsx`

```jsx
function Parent(){
  const handleClick = (data)=>{
    alert("Child sent: " + data);
  }

  return <Button sendData={handleClick}/>;
}

export default Parent;
```

#### `Button.jsx`

```jsx
export default function Button({sendData}) {
  return (
    <button onClick={()=> sendData("Hello from Child!")}>
      Send to Parent
    </button>
  );
}
```

📌 Interview crack concept:
**Only way child → parent data = function props**


#### 5. Props with CSS Styling (Dynamic UI)

#### `Parent.jsx`

```jsx
export default function Parent(){
  return <Box color="purple" padding="20px" round={true}/>;
}
```

#### `Box.jsx`

```jsx
export default function Box({color, padding, round}) {
  return (
    <div style={{
      background: color,
      padding: padding,
      borderRadius: round ? "15px" : "0px"
    }}>
      Dynamic Styled Box
    </div>
  );
}
```

- CSS props → style object e directly use
- Boolean props → conditionally style change করা যায়



**props অনুযায়ী Tailwind CSS change**:

```jsx
export default function Button({variant="primary", size="md", children}) {
  const styles = {
    primary: "bg-blue-600 text-white",
    secondary: "bg-gray-600 text-white",
    danger: "bg-red-500 text-white"
  };
  const sizes = {
    sm: "text-sm px-2 py-1",
    md: "text-base px-4 py-2",
    lg: "text-lg px-6 py-3"
  };
  return (
    <button className={`${styles[variant]} ${sizes[size]} rounded`}>
      {children}
    </button>
  );
}
```

Usage:

```jsx
<Button variant="primary" size="md">Save</Button>
<Button variant="danger" size="lg">Delete</Button>
<Button variant="secondary" size="sm">Cancel</Button>
```

Key idea:

* `variant` props → color/style control
* `size` props → padding/text-size control
* Tailwind class dynamically `${}` দিয়ে attach করো

#### 6. Props.children Practical

Wrap content inside a reusable container

#### `Card.jsx`

```jsx
export default function Card({children}) {
  return (
    <div style={{border:"1px solid gray", padding:"15px", margin:"10px"}}>
      {children}
    </div>
  );
}
```

#### Use:

```jsx
<Card><h2>Title Inside Card</h2></Card>
<Card><p>Another card with text</p></Card>
```

children = জায়গা যেখানে তুমি যেকোনো JSX পাঠাতে পারো।


#### 7. Conditional Rendering Props

#### `Parent.jsx`

```jsx
<Status online={true}/>
```

#### `Status.jsx`

```jsx
export default function Status({online}) {
  return (
    <p style={{color: online ? "green" : "red"}}>
      {online ? "User Active" : "User Offline"}
    </p>
  );
}
```

Boolean props → UI state update.

---

#### 8. Default Props (No value passed হলে fallback)

```jsx
function Avatar({name="Unknown", size=50}) {
  return <img style={{width:size, height:size}} alt={name} />;
}
```

- Avoid undefined crash
- Good for large production apps


#### PropTypes Deep Discussion

React বড় application → ভুল props দিলে bug হয়।
PropTypes মূলত **props validation layer**.


#### Install

```
npm i prop-types
```


#### Basic PropTypes

```jsx
import PropTypes from 'prop-types';

function User({name, age}) {
  return <h1>{name} - {age}</h1>;
}

User.propTypes = {
  name: PropTypes.string,
  age: PropTypes.number
};
```

#### Required props (must pass)

```jsx
User.propTypes = {
  name: PropTypes.string.isRequired
};
```

 না দিলে console এ warning দিবে
UX safer & predictable



#### One of allowed values

```jsx
Button.propTypes = {
  type: PropTypes.oneOf(["primary","secondary","danger"])
}
```

predefined rules maintain করে development smooth।


#### Array & Object validation

```jsx
Skills.propTypes = {
  skills: PropTypes.arrayOf(PropTypes.string)
}

Profile.propTypes = {
  info: PropTypes.shape({
    name: PropTypes.string,
    age: PropTypes.number,
    address: PropTypes.shape({
      city: PropTypes.string,
      zip: PropTypes.number
    })
  })
}
```

Complex nested object validation = production grade coding


#### Function PropTypes

```jsx
Child.propTypes = {
  sendData: PropTypes.func.isRequired
}
```

 mandatory for parent→child callbacks.


#### PropTypes Summary Table

| Usage                   | Syntax                      |
| ----------------------- | --------------------------- |
| String                  | PropTypes.string            |
| Number                  | PropTypes.number            |
| Boolean                 | PropTypes.bool              |
| Array                   | PropTypes.array             |
| Object                  | PropTypes.object            |
| Any type                | PropTypes.any               |
| Required                | PropTypes.string.isRequired |
| One value from set      | PropTypes.oneOf([])         |
| Custom object structure | PropTypes.shape({})         |
| Array specific type     | PropTypes.arrayOf(type)     |
| Function                | PropTypes.func              |

This is **industry-standard dynamic Tailwind component pattern**.
