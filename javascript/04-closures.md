# JavaScript Interview: Closures (The Complete Guide)

Concepts, code examples, and common interview output questions on closures.

---

## 1. Lexical Scope
Before understanding closures, you must understand the environment they are born in.
* **Scope:** Refers to the current context of your code (Global, Local, or Block).
* **Lexical Scope:** A variable defined outside a function can be accessible inside another function defined after that variable declaration. 
* **The Rule:** Inner functions have access to outer variables, but outer functions **cannot** access inner variables.

---

## 2. What is a Closure?
A **Closure** is a function bundled together with references to its surrounding state (the lexical environment). 
* It gives you access to an outer function's scope from an inner function.
* In JavaScript, closures are created **every time a function is created** at function creation time.

### **Example: Basic Closure**
```javascript
function makeFunc() {
  var name = "Mozilla";
  function displayName() {
    console.log(name);
  }
  return displayName;
}
var myFunc = makeFunc();
myFunc(); // Output: "Mozilla"
```
*Even though `makeFunc()` has finished executing, `myFunc` still remembers `name` because of the closure.*

---

## 3. Closure Scope Chain
Every closure has access to three levels of scope:
1. **Local Scope** (Own scope)
2. **Outer Function Scope**
3. **Global Scope**

### **Output Question: Scope Chain Logic**
```javascript
var e = 10;
function sum(a) {
  return function(b) {
    return function(c) {
      return function(d) {
        return a + b + c + d + e;
      };
    };
  };
}
console.log(sum(1)(2)(3)(4)); // Output: 20
```
*The innermost function has access to a, b, c, d (outer scopes) and e (global scope).*

---

## 4. Interview Output Questions

### **Q1: Shadowing and Block Scope**
```javascript
let count = 0;
(function printCount() {
  if (count === 0) {
    let count = 1; // Shadowing
    console.log(count); // Output: 1
  }
  console.log(count); // Output: 0
})();
```

### **Q2: Writing a Function for: addSix(10)**
```javascript
function createBase(baseNumber) {
  return function(num) {
    return baseNumber + num;
  };
}
var addSix = createBase(6);
console.log(addSix(10)); // 16
console.log(addSix(21)); // 27
```

### **Q3: Time Optimization using Closures**
Using a closure to cache a heavy loop (like an array of 1 million items) so it doesn't re-run every time you search for an index.

### **Q4: The SetTimeout Loop Challenge**
**The Problem:**
```javascript
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1000);
} 
// Output: 3, 3, 3 (Because var has no block scope)
```
**The Solution (Closure Fix):**
```javascript
for (var i = 0; i < 3; i++) {
  function inner(value) {
    setTimeout(() => console.log(value), 1000);
  }
  inner(i);
} 
// Output: 0, 1, 2
```

---

## 5. Advanced Concepts & Polyfills

### **Private Counter (Encapsulation)**
```javascript
function counter() {
  var _counter = 0;
  function add(increment) { _counter += increment; }
  function retrieve() { return "Counter = " + _counter; }
  return { add, retrieve };
}
const c = counter();
c.add(5);
console.log(c.retrieve()); // "Counter = 5"
```

### **Module Pattern**
A pattern that uses closures to define private and public methods. Public methods can access private methods, but users cannot access private methods directly.

### **Polyfill: Once Function**
```javascript
function once(func, context) {
  let ran;
  return function() {
    if (func) {
      ran = func.apply(context || this, arguments);
      func = null;
    }
    return ran;
  };
}
const hello = once(() => console.log("Hello!"));
hello(); // "Hello!"
hello(); // (Nothing happens)
```

### **Polyfill: Memoize (Caching)**
```javascript
function myMemoize(fn) {
  const res = {};
  return function(...args) {
    var argsCache = JSON.stringify(args);
    if (!res[argsCache]) {
      res[argsCache] = fn.call(this, ...args);
    }
    return res[argsCache];
  };
}
```

---

## 💡 Summary for Quick Revision
* **Closure vs Scope:** Scope defines what variables you have access to; Closure is the function itself that "remembers" its outer variables.
* **Encapsulation:** Closures are the only way to achieve true private variables in older JS versions.
* **Memory Management:** Closures can lead to higher memory usage because variables in the outer scope are not garbage collected while the closure exists.