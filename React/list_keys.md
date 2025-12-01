React.js এ **List** মানে হচ্ছে একাধিক data কে loop করে UI-তে দেখানো। আর **Keys** হচ্ছে list render করার সময় প্রতিটি element কে React-এর কাছে আলাদা করে চেনানোর জন্য একটি unique identifier।

#### What are Lists in React?

React এ যখন তোমার কাছে একটি array থাকে এবং তুমি সেটিকে UI-তে বারবার repeat করে দেখাতে চাও, তখন **List Rendering** ব্যবহার করা হয়।

সাধারণত `.map()` function দিয়ে list render করা হয়।

### উদাহরণ:

```jsx
const numbers = [1, 2, 3, 4, 5];

function NumberList(){
  return (
    <ul>
      {numbers.map(num => <li>{num}</li>)}
    </ul>
  );
}
```

উপরে `numbers` array থেকে প্রতিটি element `li` হিসেবে দেখানো হয়েছে।


#### What are Keys in React?

List render করার সময় React-কে প্রতিটি element কে চেনার জন্য একটি unique **key** দিতে হয়।

🔹 কেন key লাগে?

* React কে বোঝাতে কোন item পরিবর্তন হয়েছে
* কোন element add/remove/update হয়েছে তা tracking করতে
* Re-render performance উন্নত হয়
* Warning avoid করা যায়



#### Wrong Example → Key নেই

```jsx
const items = ["Apple", "Banana", "Mango"];

function Fruits(){
  return (
    <ul>
      {items.map(fruit => <li>{fruit}</li>)}
    </ul>
  );
}
```

এতে React console-এ warning দেবে:
 *"Each child in a list should have a unique key..."*


#### Correct Example → With Key

```jsx
const items = ["Apple", "Banana", "Mango"];

function Fruits(){
  return (
    <ul>
      {items.map((fruit, index) => (
        <li key={index}>{fruit}</li>
      ))}
    </ul>
  );
}
```


#### Best Practice for Keys

| Case                                           | Best Key                                               |
| ---------------------------------------------- | ------------------------------------------------------ |
| Data comes from DB / has unique id             | `id` ব্যবহার করবে (Best)                               |
| No unique field                                | index use করা যাবে, but ক্ষেত্র বিশেষে not recommended |
| Sorting, filtering, reordering হওয়ার সম্ভাবনা | index ব্যবহার কোরো না                                  |


### Best Example (with id)

```jsx
const users = [
  { id: 1, name: "Salman" },
  { id: 2, name: "Rafi" },
  { id: 3, name: "Amina" }
];

function UserList(){
  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```


#### Summary

| Concept      | Meaning                                                 |
| ------------ | ------------------------------------------------------- |
| List         | Array loop করে UI-তে elements দেখানো                    |
| Key          | প্রতিটি element কে React-এর কাছে unique করে চেনানোর tag |
| Why use key? | Fast re-render, correct update tracking                 |
| Best key?    | `id` based unique key                                   |




##### Props সহ Dynamic List Rendering

Props দিয়ে parent থেকে data pass করে list render করতে পারো:

```jsx
// Parent Component
const users = [
  { id: 1, name: "Salman" },
  { id: 2, name: "Rafi" },
  { id: 3, name: "Amina" }
];

function App() {
  return <UserList data={users} />;
}

// Child Component
function UserList({ data }) {
  return (
    <ul>
      {data.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

- এখানে `UserList` dynamic, কারণ props এর data অনুযায়ী render হবে।
- Key ব্যবহার করা হয়েছে `user.id` দিয়ে।

##### Filtering + Searching + Sorting with Key

```jsx
const users = [
  { id: 1, name: "Salman" },
  { id: 2, name: "Rafi" },
  { id: 3, name: "Amina" }
];

function App() {
  const searchTerm = "a"; // simple search/filter
  const filtered = users
    .filter(user => user.name.toLowerCase().includes(searchTerm.toLowerCase()))
    .sort((a, b) => a.name.localeCompare(b.name)); // sort alphabetically

  return (
    <ul>
      {filtered.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

- Filtered + searched + sorted list
- Key দিয়ে React কে unique element চেনানো হয়েছে



### Summary

1. **Props**: Parent থেকে data pass করে child এ dynamic list render
2. **Key**: সব list item কে unique করে চেনানো
3. **Filter/Search/Sort**: `.filter()`, `.sort()`, `.map()` combo


