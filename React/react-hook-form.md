**React Hook Form (formHook) কীভাবে manage করে** 


### **1️ Form Hook কীভাবে manage করে – থিওরিটিকাল**

React Hook Form মূলত **uncontrolled inputs** এবং **hook-based state management** এর উপর কাজ করে।

##### **Core Idea**

1. প্রতিটি input field কে `register()` দিয়ে register করা হয়।
2. Input এর value **DOM এ থাকে**, React state এ নয়।
3. Input change, blur, validation সব automatically track হয়।
4. Submit হলে `handleSubmit()` validation করে এবং `onSubmit(data)` এ validated data পাঠায়।
5. `formState.errors` থেকে error messages show করা যায়।


##### **Form Flow (Conceptual Diagram)**

```
Input (DOM) 
   │
   ▼
register() → Hook tracks value & validation
   │
   ▼
watch() → Real-time value (optional)
   │
   ▼
handleSubmit() → Validation
   │
   ├── valid → onSubmit(data)
   └── invalid → formState.errors
```


##### **Hook Methods & Responsibilities**

| Method/Hook        | Role                            |
| ------------------ | ------------------------------- |
| `useForm()`        | Form create & state management  |
| `register()`       | Input hook এর সাথে bind করা     |
| `handleSubmit()`   | Submit & validation handle করা  |
| `formState.errors` | Validation errors track করা     |
| `watch()`          | Real-time input value track করা |
| `reset()`          | Form clear করা                  |
| `setValue()`       | Programmatically value set করা  |
| `getValues()`      | Form values read করা            |

💡 **Important:** React Hook Form **re-render কমায়**, কারণ input DOM এ থাকে, React state এ নয়।


### **2️ Practical Example Step by Step**

#### **Step 1: Install**

```bash
npm install react-hook-form
```


#### **Step 2: Basic Form Setup**

```jsx
import React from "react";
import { useForm } from "react-hook-form";

export default function App() {
  const { register, handleSubmit, formState: { errors }, reset, watch } = useForm();

  // Step 4: onSubmit
  const onSubmit = (data) => {
    console.log("Form Data:", data);
    reset(); // Form reset after submit
  };

  // Step 3: Watch a field (optional)
  const nameValue = watch("name");

  return (
    <div>
      <h2>React Hook Form Example</h2>
      <form onSubmit={handleSubmit(onSubmit)}>

        {/* Name input */}
        <input 
          type="text" 
          placeholder="Name" 
          {...register("name", { required: "Name is required" })} 
        />
        {errors.name && <p>{errors.name.message}</p>}

        {/* Email input */}
        <input 
          type="email" 
          placeholder="Email"
          {...register("email", { required: "Email required" })}
        />
        {errors.email && <p>{errors.email.message}</p>}

        {/* Password input */}
        <input 
          type="password" 
          placeholder="Password" 
          {...register("password", { 
            required: "Password needed", 
            minLength: { value: 6, message: "Min 6 characters" }
          })}
        />
        {errors.password && <p>{errors.password.message}</p>}

        <button type="submit">Submit</button>
      </form>

      {/* Real-time watch */}
      <p>Typing Name: {nameValue}</p>
    </div>
  );
}
```

#### **Step 3: What Happens Internally (Management)**

1. `register("name")` → Name input DOM এর সাথে Hook bind করে।
2. User typing → value DOM এ update, React re-render কম হয়।
3. Submit → `handleSubmit(onSubmit)` validate করে `formState.errors` check করে।
4. Valid → `onSubmit(data)` call হয়।
5. Invalid → `formState.errors` তে error message থাকে।
6. `watch("name")` → Name value real-time দেখায়।
7. `reset()` → সব input clear করে।


#### **Step 4: Advantages Shown in Example**

* **Less Re-render** → performance better
* **Validation** built-in → errors easily shown
* **Watch** → dynamic display possible
* **Reset / Programmatic control** → full management


