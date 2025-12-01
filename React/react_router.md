

### **React Router v7+ Complete Lecture (Beginner Friendly)**



#### React Router কি?

* **React Router** হল একটি library যা React apps এ **page routing** handle করে।
* React default হিসেবে **single-page application (SPA)** তাই page reload হয় না।
* React Router use করে তুমি **different URLs এ different components render** করতে পারো।

**উদাহরণ:**

* `/` → Home page
* `/about` → About page
* `/contact` → Contact page


#### কিভাবে install করবো

```bash
npm install react-router-dom
```

React Router v7+ সব modern React version support করে।


#### Basic Setup

```jsx
import { BrowserRouter, Routes, Route } from "react-router-dom";
import Home from "./Home";
import About from "./About";

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </BrowserRouter>
  );
}

export default App;
```

**Explanation:**

* `BrowserRouter` → app কে routing context দেয়
* `Routes` → সব route গুলো wrap করে
* `Route` → একটা path assign করে component render করার জন্য
* `element` → যেই component render হবে



#### Navigation Between Pages

React Router এ navigation করার জন্য **Link component** use করি:

```jsx
import { Link } from "react-router-dom";

function Navbar() {
  return (
    <nav>
      <Link to="/">Home</Link> | 
      <Link to="/about">About</Link>
    </nav>
  );
}
export default Navbar;
```

**Important:**

* `<a>` use করলে page reload হবে → React SPA এর advantage হারাবে।
* Always use `<Link>`.


#### Nested Routes (Layout System)

**Scenario:** সব page এ same navbar + footer রাখতে চাই।

```jsx
import { Outlet, Link } from "react-router-dom";

function Layout() {
  return (
    <div>
      <nav>
        <Link to="/">Home</Link> | 
        <Link to="/about">About</Link>
      </nav>
      <hr />
      <Outlet /> {/* Nested route content এখানে render হবে */}
      <footer>© 2025 My App</footer>
    </div>
  );
}

// App.js
<BrowserRouter>
  <Routes>
    <Route path="/" element={<Layout />}>
      <Route index element={<Home />} />       {/* Default nested route */}
      <Route path="about" element={<About />} />
    </Route>
  </Routes>
</BrowserRouter>
```

**Explanation:**

* `Layout` component → Navbar + footer define করে
* `Outlet` → nested routes এর content render করে
* `index` → default nested route

---

#### Dynamic Routes

**Scenario:** user profile page `/user/:id`

```jsx
import { useParams } from "react-router-dom";

function UserProfile() {
  const { id } = useParams();
  return <h2>User ID: {id}</h2>;
}
```

**Route:**

```jsx
<Route path="/user/:id" element={<UserProfile />} />
```

* URL: `/user/123` → shows **User ID: 123**


#### Programmatic Navigation

* Sometimes আমরা button click এ navigate করতে চাই
* Use `useNavigate` hook:

```jsx
import { useNavigate } from "react-router-dom";

function Home() {
  const navigate = useNavigate();

  const goToAbout = () => {
    navigate("/about");
  };

  return <button onClick={goToAbout}>Go to About</button>;
}
```


#### Loader & Action (React Router v7+ feature)

* **Loader** → route এ data fetch করে component load করার আগে
* **Action** → form submit handle করে

```jsx
// loader example
export async function userLoader() {
  const res = await fetch("https://jsonplaceholder.typicode.com/users");
  return res.json();
}

// route
<Route path="/users" element={<Users />} loader={userLoader} />
```

* `useLoaderData()` hook use করে component এ data access করো

```jsx
import { useLoaderData } from "react-router-dom";

function Users() {
  const users = useLoaderData();
  return (
    <ul>
      {users.map(u => <li key={u.id}>{u.name}</li>)}
    </ul>
  );
}
```


#### 404 Page (Catch All Route)

```jsx
<Route path="*" element={<h1>Page Not Found</h1>} />
```

* সব undefined route capture করে 404 show করে


##### Summary (Beginner Cheatsheet)

| Concept          | Example                                                                                   |
| ---------------- | ----------------------------------------------------------------------------------------- |
| Basic Route      | `<Route path="/about" element={<About />} />`                                             |
| Nested Route     | `<Route path="/" element={<Layout />}><Route path="about" element={<About />} /></Route>` |
| Navigation       | `<Link to="/about">About</Link>`                                                          |
| Dynamic Route    | `/user/:id` + `useParams()`                                                               |
| Programmatic Nav | `useNavigate()`                                                                           |
| Loader           | Fetch data before render + `useLoaderData()`                                              |
| Catch-all 404    | `<Route path="*" element={<NotFound />} />`                                               |



