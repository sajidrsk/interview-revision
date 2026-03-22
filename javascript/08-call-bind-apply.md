# JavaScript Interview Prep: Call, Bind, and Apply (Exhaustive Guide)

**Source:** [RoadsideCoder - Call, Bind and Apply](https://www.youtube.com/watch?v=VkmUOktYDAU)

Explicit binding allows us to set the `this` keyword independently of how a function is called. These methods are available to every JavaScript function through the function prototype.

---

## 1. Core Definitions

### **Call**
Invokes a function immediately with a specified `this` context and arguments provided individually (comma-separated).
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
Does not invoke the function immediately. Instead, it returns a **new reusable function** with the `this` context permanently bound. You can provide arguments later when calling the new function.
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
console.log(Math.min.apply(null, numbers)); // 1
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
**Logic:** `apply` breaks down the `elements` array and passes them as individual arguments to `push`.

### **Q3: Looping Over an Array of Objects**
**Question:** Use `call` to print all animals in the object.
```javascript
const animals = [
  { species: "Lion", name: "King" },
  { species: "Whale", name: "Queen" }
];

function printAnimals(i) {
  this.print = function() {
    console.log("#" + i + " " + this.species + ": " + this.name);
  };
  this.print();
}

for (let i = 0; i < animals.length; i++) {
  printAnimals.call(animals[i], i);
}
```

---

## 3. Critical Output-Based Questions

### **Q1: Call vs Bind Execution**
**Question:** What is the output?
```javascript
const person = { name: "Piyush" };
function sayHi(age) {
  return `${this.name} is ${age}`;
}

console.log(sayHi.call(person, 24)); // Output: "Piyush is 24"
console.log(sayHi.bind(person, 24)); // Output: [Function: bound sayHi]
```
**Logic:** `call` immediately executes the function, whereas `bind` returns a bound function.

### **Q2: Call with Function Inside Object**
**Question:** What is the output?
```javascript
const age = 10;
var person = {
  name: "Piyush",
  age: 20,
  getAge: function() {
    return this.age;
  }
};
var person2 = { age: 24 };

console.log(person.getAge.call(person2)); // Output: 24
```
**Logic:** By using `call(person2)`, we overwrite the context from `person` to `person2`. The global `age = 10` is a distractor.

### **Q3: Context in Async Callbacks**
**Question:** What happens to `this` when passing a context manually inside a `setTimeout` arrow function?
```javascript
var status = "😎";

setTimeout(() => {
  const status = "😍";
  const data = {
    status: "🥑",
    getStatus() { return this.status; }
  };

  console.log(data.getStatus()); // Output: "🥑"
  console.log(data.getStatus.call(this)); // Output: "😎"
}, 0);
```
**Logic:** `data.getStatus()` uses implicit binding, printing `"🥑"`. In `data.getStatus.call(this)`, `this` inside an arrow function points to the outer scope (the `window` object). The global `var status` is `"😎"`.

### **Q4: The Bind Chaining Rule**
**Question:** What is the output?
```javascript
function f() { console.log(this.name); }
f = f.bind({ name: "John" }).bind({ name: "Ann" });
f(); // Output: "John"
```
**Logic:** Chaining `bind` does not work. Once a function is bound to an object, it cannot be rebound to another object.

### **Q5: Calling a Bound Function from an Object**
**Question:** What is the output?
```javascript
function checkContext() { console.log(this); }
let user = {
  g: checkContext.bind(null)
};
user.g(); // Output: Window object
```
**Logic:** Even though `user.g` is called as an object method, the function was explicitly hard-bound to `null` (which defaults to `window` in non-strict mode).

### **Q6: Arrow Functions and Explicit Binding**
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
**Logic:** Arrow functions completely ignore `call`, `bind`, and `apply`. Their `this` is lexically bound to their parent scope.

---

## 4. Practical Bind Applications

### **Fixing Lost Context in Callbacks**
**Question:** How do you fix line 10 so the password check works?
```javascript
function checkPassword(success, failed) {
  let password = prompt("Password?", "");
  if (password == "Roadside Coder") success();
  else failed();
}

let user = {
  name: "Piyush",
  loginSuccessful() { console.log(`${this.name} logged in`); },
  loginFailed() { console.log(`${this.name} failed to log in`); },
};

// Error: When success() is called, 'this' is lost.
// checkPassword(user.loginSuccessful, user.loginFailed);

// Fix:
checkPassword(user.loginSuccessful.bind(user), user.loginFailed.bind(user));
```

### **Partial Application with Bind**
**Question:** Modify the above code to use a single `login(result)` function.
```javascript
let user = {
  name: "Piyush",
  login(result) {
    console.log(this.name + (result ? " logged in" : " failed to log in"));
  }
};

checkPassword(user.login.bind(user, true), user.login.bind(user, false));
```
**Logic:** We can pre-fill arguments (`true` and `false`) using `bind`, which returns a function that `checkPassword` can execute later.

---

## 5. Advanced Polyfills (The Senior Level Part)

### **Polyfill for Call**
```javascript
Function.prototype.myCall = function(context = {}, ...args) {
  if (typeof this !== "function") {
    throw new Error(this + " is not callable");
  }
  
  context.fn = this; // Assign function to the context object
  let result = context.fn(...args); // Call it (implicit binding sets 'this')
  delete context.fn; // Clean up
  return result;
};
```

### **Polyfill for Apply**
```javascript
Function.prototype.myApply = function(context = {}, args = []) {
  if (typeof this !== "function") {
    throw new Error(this + " is not callable");
  }
  if (!Array.isArray(args)) {
    throw new TypeError("CreateListFromArrayLike called on non-object");
  }
  
  context.fn = this;
  let result = context.fn(...args);
  delete context.fn;
  return result;
};
```

### **Polyfill for Bind**
```javascript
Function.prototype.myBind = function(context = {}, ...args) {
  if (typeof this !== "function") {
    throw new Error(this + " cannot be bound as it's not callable");
  }
  
  context.fn = this;
  return function(...newArgs) {
    // Spread both initial arguments and newly provided arguments
    return context.fn(...args, ...newArgs); 
  };
};
```

---

## 💡 Quick Revise Summary
- **Call**: Immediate execution, comma-separated arguments.
- **Apply**: Immediate execution, array-based arguments.
- **Bind**: Delayed execution, returns a new bound function.
- **Arrow Functions**: Context cannot be changed by Call, Bind, or Apply.
- **Irreversibility**: A bound function (`bind`) cannot be rebound to a new context.