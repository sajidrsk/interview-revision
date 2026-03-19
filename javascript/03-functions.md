# JavaScript Interview: Functions (Deep Dive)

Comprehensive revision notes on function types, scoping, hoisting, and ES6 Arrow functions.

---

## 1. Function Types & Definitions

### **Function Declaration (Statement)**
A normal way to define a function. These are hoisted completely.
```javascript
function square(num) {
  return num * num;
}
```

### **Function Expression**
When a function is assigned to a variable. These are **not** hoisted like declarations.
```javascript
const square = function (num) {
  return num * num;
};
```

### **Anonymous Function**
A function without a name. Usually used in function expressions or as callbacks.

### **First Class Functions**
In JS, functions can be treated like variables. They can be passed into other functions, manipulated, and returned.

---

## 2. Immediately Invoked Function Expressions (IIFE)
A function that is executed immediately after it is created.

### **Output Question: IIFE & Scoping**
```javascript
(function(x) {
  return (function(y) {
    console.log(x); // Output: 1
  })(2);
})(1);
```
**Logic:** The inner function searches for `x` in its own scope. Since it doesn't find it, it looks in the parent scope (Closure).

---

## 3. Function Scope & Shadowing
Variables defined inside a function are not accessible outside. Inner functions can access variables from outer scopes.

### **Output Question: For Loop with SetTimeout**
```javascript
for (let i = 0; i < 5; i++) {
  setTimeout(function() {
    console.log(i);
  }, i * 1000);
}
// Output: 0, 1, 2, 3, 4 (because 'let' creates a new block scope for each iteration)
```
**Interview Twist:** If you use `var i`, the output is `5, 5, 5, 5, 5` because `var` has no block scope.

---

## 4. Function Hoisting
Functions are hoisted completely. This means the entire function body is moved to the top of the scope during the creation phase.

### **The Hoisting "Gotcha"**
```javascript
var x = 21;
var fun = function() {
  console.log(x);
  var x = 20;
};
fun();
// Output: undefined
```
**Logic:** Inside `fun()`, a local variable `x` is hoisted to the top of the local scope as `undefined`. It "shadows" the global `x`.

---

## 5. Params vs. Arguments & Operators

* **Params:** The labels (`num1, num2`) in the function definition.
* **Arguments:** The real values (`5, 10`) passed during the call.

### **Spread vs. Rest Operators**
* **Rest:** Collects arguments into an array: `function fn(...nums) {}`.
* **Spread:** Expands array into elements: `fn(...arr)`.
* **Rule:** The Rest parameter **must** be the last parameter in the list.

---

## 6. Callback Functions
A function passed into another function as an argument, which is then invoked inside the outer function.
**Examples:** `map`, `setTimeout`, `addEventListener`.

---

## 7. Arrow Functions vs. Regular Functions

1. **Syntax:** Arrow functions are shorter and allow implicit returns.
2. **Arguments Object:** Regular functions have a built-in `arguments` object; arrows do not.
3. **Implicit Return:** `const add = (a, b) => a + b;`
4. **'this' Keyword (Crucial):**
   * **Regular Function:** `this` points to the object calling the function.
   * **Arrow Function:** `this` points to the lexical scope (it inherits `this` from its parent).

### **Output Question: 'this' context**
```javascript
let user = {
  username: "Alice",
  rc1: () => console.log(this.username),
  rc2() { console.log(this.username); }
};
user.rc1(); // Output: undefined (arrow uses lexical `this`, often global/window in scripts)
user.rc2(); // Output: "Alice"
```

---

## 💡 Quick Revise Summary
- Function Declarations are fully hoisted; expressions are not.
- IIFEs help with data privacy and scoping.
- Closures allow functions to access variables from their outer lexical environment.
- Arrow functions are NOT suitable for object methods if you need to access `this`.