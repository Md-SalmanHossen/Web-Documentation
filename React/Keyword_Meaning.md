

### **Mutable**

* মানে: **পরিবর্তনযোগ্য**
* একবার তৈরি হওয়ার পরে value direct পরিবর্তন করা যায়।
* Example (JavaScript object/array):

```js
let arr = [1,2,3];
arr.push(4);  // mutable, আগের array পরিবর্তন হলো
```

### **Immutable**

* মানে: **অপরিবর্তনীয়**
* একবার তৈরি হলে value direct পরিবর্তন করা যায় না।
* পরিবর্তন করতে হলে **নতুন copy** তৈরি করতে হবে।
* Example (React useState object/array):

```js
const [user, setUser] = useState({name:"Salman"});
setUser({...user, name:"Rahim"}); // immutable, নতুন object তৈরি হলো
```


**Rule of Thumb in React:**

* **Primitive** types (number, string, boolean) → safe
* **Object/Array** → always treat immutable, use spread/map/filter → UI re-render ঠিকঠাক হবে।


