# JavaScript Interview Prep: Call, Bind, and Apply (Exhaustive Guide)

Explicit binding allows us to set the `this` keyword independently of how a function is called. These methods are available to every JavaScript function through the function prototype.

---

## 1. Core Definitions

### **Call**
Invokes a function immediately with a specified `this` context and arguments provided individually.
```javascript
function sayHello(age) {
  return "Hello " + this.name + " is " + age;
}
const obj = { name: "Piyush" };
console.log(sayHello.call(obj, 24)); // "Hello Piyush is 24"
```

### **Apply**
Identical to `call`, but arguments are passed as an **array**.
```javascript
console.log(sayHello.apply(obj, [24])); // "Hello Piyush is 24"
```

### **Bind**
Does not invoke the function immediately. Instead, it returns a **new function** with the `this` context permanently bound.
```javascript
const bindFunc = sayHello.bind(obj);
console.log(bindFunc(24)); // "Hello Piyush is 24"
```

---

## 2. Real-World Use Cases & Tricky Scenarios

### **Q1: Function Borrowing with Math Methods**
**Question:** How do you find the min/max of an array using `apply`?
```javascript
const numbers = [1, 2, 3, 4, 5];
console.log(Math.max.apply(null, numbers)); // 5
```
**Logic:** `Math.max` expects a list of arguments, not an array. `apply` spreads the array into arguments for us.

### **Q2: Appending Arrays**
**Question:** Append one array to another without using `concat` (which creates a new array).
```javascript
const array = ["a", "b"];
const elements = [0, 1, 2];
array.push.apply(array, elements);
console.log(array); // ["a", "b", 0, 1, 2]
```

---

## 3. Critical Output-Based Questions

### **Q1: The Bind Chaining Rule**
**Question:** What is the output?
```javascript
function f() { console.log(this.name); }
f = f.bind({ name: "John" }).bind({ name: "Ann" });
f();
// Output: "John"
```
**Logic:** Chaining `bind` does not work. Once a function is bound to an object, it cannot be rebound to another object.

### **Q2: Context in Async Callbacks**
**Question:** What happens to `this` when a method is used in `setTimeout`?
```javascript
const user = {
  status: "🥑",
  getStatus() { return this.status; }
};
console.log(user.getStatus.call(user)); // "🥑"
setTimeout(() => {
  console.log(user.getStatus()); // "🥑"
}, 1000);
```
**Note:** If you pass `user.getStatus` directly as a callback, `this` will point to the global object. You must wrap it or use `bind`.

### **Q3: Arrow Functions and CBA**
**Question:** Can you change the context of an arrow function using `call`?
```javascript
const age = 10;
var person = {
  name: "Piyush",
  getAgeArrow: () => console.log(this.age),
  getAgeNormal: function() { console.log(this.age); }
};
var person2 = { age: 24 };
person.getAgeArrow.call(person2); // Undefined (points to window)
person.getAgeNormal.call(person2); // 24
```
**Logic:** Arrow functions ignore `call`, `bind`, and `apply`. They strictly follow Lexical Scoping.

---

## 4. Advanced Polyfills (The Senior Level Part)

### **Polyfill for Call**
```javascript
Function.prototype.myCall = function(context = {}, ...args) {
  if (typeof this !== "function") throw new Error(this + " is not callable");
  context.fn = this; // Assign function to the context object
  let result = context.fn(...args); // Call it (implicit binding sets 'this')
  delete context.fn; // Clean up
  return result;
};
```

### **Polyfill for Apply**
```javascript
Function.prototype.myApply = function(context = {}, args = []) {
  if (typeof this !== "function") throw new Error("Not callable");
  if (!Array.isArray(args)) throw new TypeError("CreateListFromArrayLike called on non-object");
  context.fn = this;
  let result = context.fn(...args);
  delete context.fn;
  return result;
};
```

### **Polyfill for Bind**
```javascript
Function.prototype.myBind = function(context = {}, ...args) {
  if (typeof this !== "function") throw new Error("Not callable");
  context.fn = this;
  return function(...newArgs) {
    return context.fn(...args, ...newArgs);
  };
};
```

---

## 💡 Quick Revise Summary
- **Call**: Immediate, Comma-separated.
- **Apply**: Immediate, Array-based.
- **Bind**: Delayed, Permanent.
- **Arrow Functions**: Context cannot be changed by CBA.
- **Irreversibility**: A bound function (`bind`) cannot be rebound.