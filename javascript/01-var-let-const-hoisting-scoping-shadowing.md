# JavaScript Interview Prep: Var, Let, & Const (Deep Dive)

Comprehensive revision notes covering scoping, shadowing, hoisting, and the Execution Context.

---

## 1. Scope in JavaScript
Scope is a specific region of a program where a defined variable exists and can be recognized.

* **Global Scope:** Variables defined outside any function or block.
* **Function Scope (`var`):** Variables are confined to the function they are declared in. They ignore block boundaries.
* **Block Scope (`let`, `const`):** Variables are confined to the nearest curly braces `{}` (e.g., `if` blocks, `for` loops).

### **Output Question: Scope Comparison**
```javascript
{
  var a = 5;
  let b = 10;
  const c = 15;
}
console.log(a); // Output: 5 (var ignores block scope)
console.log(b); // ReferenceError: b is not defined
console.log(c); // ReferenceError: c is not defined
```

---

## 2. Variable Shadowing
Shadowing occurs when a variable in an inner scope has the same name as a variable in an outer scope.

### **The Shadowing Rule**
```javascript
function test() {
  let a = "Hello";
  if (true) {
    let a = "Hi"; // Inner 'a' shadows outer 'a'
    console.log(a); // Output: "Hi"
  }
  console.log(a); // Output: "Hello"
}
```

### **Illegal Shadowing**
You can shadow `var` with `let`, but you **cannot** shadow `let` with `var` within the same block scope.
```javascript
function test() {
  var a = "Hello";
  let b = "Bye";
  if (true) {
    let a = "Hi"; // Legal: shadowing var with let
    var b = "Goodbye"; // Illegal: SyntaxError: Identifier 'b' has already been declared
  }
}
```

---

## 3. Declaration & Initialization Summary

| Feature | `var` | `let` | `const` |
| :--- | :--- | :--- | :--- |
| **Redeclaration** | ✅ Allowed in same scope | ❌ SyntaxError | ❌ SyntaxError |
| **Re-initialization** | ✅ Allowed | ✅ Allowed | ❌ TypeError |
| **Initial Value Required** | ❌ No (defaults to undefined) | ❌ No | ✅ Yes (SyntaxError if missing) |

---

## 4. JavaScript Execution Context
To understand Hoisting, you must understand the two phases of how JS runs code.

### **Phase 1: Creation Phase**
1. Creates the **Global/Window Object**.
2. Sets up a **Memory Heap**.
3. **Variable Allocation:**
   * `var` is allocated memory and initialized as `undefined`.
   * `let` and `const` are allocated memory but are **not initialized** (placed in the TDZ).
4. **Function Allocation:** Stores function declarations in their entirety.

### **Phase 2: Execution Phase**
* Executes code line-by-line.
* Assigns actual values to variables and executes function calls.

---

## 5. Hoisting & Temporal Dead Zone (TDZ)

### **Output Question: Simple Hoisting**
```javascript
console.log(count);
var count = 1; 
// Output: undefined (memory allocated, but value not yet assigned)
```

### **Output Question: Let/Const Hoisting**
```javascript
console.log(count);
let count = 1; 
// Output: ReferenceError: Cannot access 'count' before initialization
```
*Note: `let` and `const` ARE hoisted, but they reside in the TDZ.*

### **Temporal Dead Zone (TDZ) Definition**
The time between the start of the scope and the actual variable declaration where the variable is in memory but cannot be accessed.

---

## 6. Advanced Interview Scenarios

### **Scenario A: Functional Hoisting**
```javascript
function abc() {
  console.log(a); // undefined
  var a = 10;
}
abc();
```

### **Scenario B: The TDZ Crash**
```javascript
function abc() {
  console.log(a, b, c);
  var a = 10;
  let b = 20;
  const c = 30;
}
abc();
// Result: ReferenceError for 'b'. The script stops execution immediately.
```

---

## 💡 Quick Revise Checklist
- [ ] `var` is function-scoped; `let`/`const` are block-scoped.
- [ ] Illegal Shadowing: You cannot shadow `let` using `var`.
- [ ] `var` allows redeclaration; `let`/`const` do not.
- [ ] `const` must be initialized immediately at declaration.
- [ ] Execution Context has two phases: **Creation** and **Execution**.
- [ ] `let`/`const` are in the **Temporal Dead Zone** until declaration.