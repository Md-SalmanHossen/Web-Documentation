
### Zod কী?

**Zod** হলো **JavaScript/TypeScript এর জন্য একটি schema validation library।**

* সহজ কথায়, এটা তোমাকে **ডাটা চেক এবং validate** করতে সাহায্য করে।
* যেমন: তোমার ফর্মে age দিলে সেটা number কিনা, email ঠিক আছে কিনা, বা object structure ঠিক আছে কিনা তা Zod দিয়ে validate করা যায়।
* **TypeScript এর সাথে perfect compatibility** আছে, তাই type safety আসে।

**উদাহরণঃ**

```js
import { z } from "zod";

const ageSchema = z.number().min(18).max(60); // 18-60 এর মধ্যে number
ageSchema.parse(25); //  ঠিক আছে
ageSchema.parse(15); //  Error: Number must be greater than or equal to 18
```


#### Zod কেন ব্যবহার করা হয়?

* **Form Validation**: ফর্ম ডাটা চেক করার জন্য
* **API Validation**: API থেকে আসা data validate করার জন্য
* **Type Safety**: TypeScript users-এর জন্য ডাটা টাইপ ensure করার জন্য
* **Runtime Check**: JS-এ type safety compile-time ছাড়া runtime-এ verify করতে


#### Zod কিভাবে কাজ করে (কনসেপ্ট)

1. তুমি একটা **schema** define করবে।
2. সেই schema অনুযায়ী ডাটা validate হবে।
3. যদি ডাটা ঠিক থাকে → ডাটা return হবে।
4. যদি ভুল থাকে → Error throw করবে।

**Step by Step Example:**

```js
import { z } from "zod";

//  Schema define
const userSchema = z.object({
  name: z.string().min(2),          // name অবশ্যই string, 2 অক্ষরের বেশি
  age: z.number().min(18),          // age number & >=18
  email: z.string().email(),        // valid email
});

//  Valid data
const user = {
  name: "Salman",
  age: 25,
  email: "salman@example.com"
};

const parsedUser = userSchema.parse(user); // ঠিক আছে
console.log(parsedUser);

//  Invalid data
const invalidUser = {
  name: "S",
  age: 15,
  email: "not-an-email"
};

// parse করলে error দিবে
// userSchema.parse(invalidUser);
```


#### Zod এর ধরণ (Types / Methods)

| Type                                | ব্যাবহার                  |
| ----------------------------------- | ------------------------- |
| `z.string()`                        | string validate করতে      |
| `z.number()`                        | number validate করতে      |
| `z.boolean()`                       | true/false validate করতে  |
| `z.object({})`                      | object validate করতে      |
| `z.array(z.string())`               | array validate করতে       |
| `z.enum(["A","B"])`                 | enum value validate করতে  |
| `z.optional()`                      | optional field চেক করতে   |
| `z.nullable()`                      | null allow করতে           |
| `z.union([z.string(), z.number()])` | multiple type accept করতে |

**Example:**

```js
const productSchema = z.object({
  name: z.string(),
  price: z.number().min(0),
  tags: z.array(z.string()).optional(),
});

productSchema.parse({
  name: "Laptop",
  price: 500,
  tags: ["electronics", "computer"]
});
```


#### Zod এর কিছু Advanced Feature

1. **Custom Error Message**

```js
z.string().min(3, "Name should be at least 3 characters")
```

2. **Transform Data**

```js
z.string().transform((val) => val.toUpperCase());
```

3. **Refine / Super Custom Validation**

```js
z.number().refine((val) => val % 2 === 0, {
  message: "Age must be even",
});
```

4. **Nested Objects**

```js
const schema = z.object({
  user: z.object({
    name: z.string(),
    age: z.number(),
  }),
});
```


#### Zod কোথায় ব্যবহার হয়?

* **React / Next.js Form Validation**
* **API Request/Response Validation** (backend)
* **Config validation** (যেমন .env file check)
* **Any data input validation** (files, JSON, external APIs)


#### Quick Start (Installation)

```bash
npm install zod
```

React বা Node project-এ import:

```js
import { z } from "zod";
```


💡 **Tip:**

* Zod ব্যবহার করলে তুমি **ডাটা safe + error-proof + type-safe** রাখো।
* Beginner হিসাবে আগে **string, number, object, array** types master করা ভালো।

### **React Hook Form + Zod** 


#### Prerequisites

1. React project (create-react-app / Vite)
2. React Hook Form
3. Zod

**Install packages:**

```bash
npm install react-hook-form zod @hookform/resolvers
```

* `react-hook-form` → form handle করার জন্য
* `zod` → validation schema তৈরি করার জন্য
* `@hookform/resolvers` → React Hook Form কে Zod এর সাথে connect করতে



#### Basic React Form with Zod

**Example:** Signup Form → name, email, password

```jsx
import React from "react";
import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";

// 1 Zod schema define
const signupSchema = z.object({
  name: z.string().min(2, "Name must be at least 2 characters"),
  email: z.string().email("Invalid email address"),
  password: z.string().min(6, "Password must be at least 6 characters"),
});

// 2 TypeScript users: Infer type from Zod
// type SignupData = z.infer<typeof signupSchema>;

export default function SignupForm() {
  //3 useForm setup with Zod
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm({
    resolver: zodResolver(signupSchema),
  });

  // 4 onSubmit function
  const onSubmit = (data) => {
    console.log("Form Data:", data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)} style={{ maxWidth: "400px" }}>
      <div>
        <label>Name</label>
        <input {...register("name")} />
        {errors.name && <p style={{ color: "red" }}>{errors.name.message}</p>}
      </div>

      <div>
        <label>Email</label>
        <input {...register("email")} />
        {errors.email && <p style={{ color: "red" }}>{errors.email.message}</p>}
      </div>

      <div>
        <label>Password</label>
        <input type="password" {...register("password")} />
        {errors.password && <p style={{ color: "red" }}>{errors.password.message}</p>}
      </div>

      <button type="submit">Signup</button>
    </form>
  );
}
```

**এই Form-এ Zod কাজ করছে এভাবে:**

* User name কম দিলে error দেখাবে
* Email invalid হলে error দেখাবে
* Password কম length হলে error দেখাবে
* সব ঠিক থাকলে console-এ data print হবে


#### 3 Advantages of React + Zod

1. **Single Source of Truth** → Zod schema দিয়ে সব validation centralized
2. **Type Safe** → TS users জন্য auto type inference
3. **Readable Errors** → error messages সহজে handle করা যায়
4. **Scalable** → nested objects, arrays, custom validation সহজ


#### 4️ Advanced Example: Nested Object Validation

```jsx
const profileSchema = z.object({
  username: z.string().min(3),
  email: z.string().email(),
  address: z.object({
    city: z.string().min(2),
    zip: z.string().min(5).max(5)
  })
});
```

React Hook Form-এ use করার সময় ঠিক একইভাবে handle করবে।


**Tip:**

* `zodResolver` ব্যবহার না করলে Zod নিজে parse() দিয়ে manual validation করতে হবে।
* React Hook Form + Zod combination হলো **best practice** modern React projects-এ।

