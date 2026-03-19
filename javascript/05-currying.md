# JavaScript Interview Prep: Currying (Exhaustive Guide)

Currying is a fundamental concept in functional programming that is frequently used to test a candidate's understanding of closures and higher-order functions.

---

## 1. What is Currying?
Currying is a function transformation where a function that takes multiple arguments is converted into a series of functions that each take a **single argument** at a time.

* **Callable Transformation:** It transforms a function from `f(a, b, c)` into `f(a)(b)(c)`.
* **Mechanism:** It is constructed by chaining closures—immediately returning an inner function that maintains access to the previous arguments.

---

## 2. Basic Implementation: sum(a)(b)(c)
### **Question:** How do you implement a function that sums three numbers using currying?

**Normal Function:**
```javascript
function sum(a, b, c) {
    return a + b + c;
}
```

**Curried Implementation:**
```javascript
function sum(a) {
    return function (b) {
        return function (c) {
            return a + b + c;
        };
    };
}
console.log(sum(2)(6)(1)); // Output: 9
```

---

## 3. Why Use Currying?
1.  **Avoid Redundancy:** Avoid passing the same variable repeatedly.
2.  **Higher-Order Functions:** Helps in creating specialized functions from generic ones.
3.  **Pure Functions:** Makes functions more modular and easier to test.
4.  **Error Prevention:** Reduces the chance of missing arguments during complex logic.

---

## 4. Output Question: The "evaluate" Function
### **Question:** Create a curried function `evaluate` that performs sum, multiply, divide, and subtract.

```javascript
function evaluate(operation) {
    return function (a) {
        return function (b) {
            if (operation === "sum") return a + b;
            else if (operation === "multiply") return a * b;
            else if (operation === "divide") return a / b;
            else if (operation === "subtract") return a - b;
            else return "Invalid Operation";
        };
    };
}

// Reusability Example:
const mul = evaluate("multiply");
console.log(mul(3)(5)); // 15
console.log(mul(2)(6)); // 12
```
**Logic:** By currying, we initialize the "operation" once and reuse that specialized function multiple times without re-declaring the operation type.

---

## 5. Infinite Currying
### **Question:** Implement a function that can take 'n' number of arguments: `add(1)(2)(3)...(n)()`.

```javascript
function add(a) {
    return function (b) {
        if (b) return add(a + b); // Recursive call if 'b' exists
        return a; // Base case: return total if 'b' is empty
    };
}

console.log(add(5)(2)(4)(8)()); // Output: 19
```
**Edge case:** `if (b)` treats `0` as falsy, so `add(1)(0)()` can break this pattern—production versions use `arguments.length` or an explicit “terminal” value instead of truthiness.

**Step-by-Step Logic:**
1.  `add(5)` returns a function expecting `b`.
2.  Passing `2` makes `b` truthy, so it returns `add(5 + 2)`, which is `add(7)`.
3.  This continues until we call it with empty parentheses `()`.
4.  In the final call, `b` is undefined (falsy), so it hits the base case and returns the accumulated `a`.

---

## 6. Currying vs. Partial Application
* **Arity:** The number of arguments or operands a function receives.
* **Currying:** Transforms a function into a chain of functions that **always take exactly one argument**.
* **Partial Application:** Transforms a function into another function with **smaller arity** (it can take multiple arguments at once).

**Example of Partial Application:**
```javascript
function sum(a) {
    return function (b, c) {
        return a + b + c;
    };
}
// Here, the inner function takes TWO arguments (b, c). This is Partial Application, NOT Currying.
```

---

## 7. Real-World Use Case: DOM Manipulation
### **Question:** How can currying optimize updating UI elements?

```javascript
function updateElementText(id) {
    return function (content) {
        document.querySelector("#" + id).textContent = content;
    };
}

// Initialize once with the element ID
const updateHeader = updateElementText("heading");

// Use multiple times to update content
updateHeader("Welcome");
updateHeader("Updated headline");
```
**Benefit:** We don't have to perform the expensive `querySelector` operation every time we want to change the text.

---

## 8. Advanced: The Curry Converter (Converter Polyfill)
### **Question:** Write a function that converts any normal function into a curried version.

```javascript
function curry(func) {
    return function curriedFunc(...args) {
        // Check if current arguments are enough for the original function
        if (args.length >= func.length) {
            return func(...args);
        } else {
            // Otherwise, return a function to collect more arguments
            return function (...next) {
                return curriedFunc(...args, ...next);
            };
        }
    };
}

const sum = (a, b, c, d) => a + b + c + d;
const totalSum = curry(sum);

console.log(totalSum(1)(6)(5)(8)); // Output: 20
```
**Logic:** This uses `func.length` to determine how many arguments the original function expects. Until the collected `args` match that length, it continues returning a new function.

---

## 💡 Quick Revise Summary
* **Definition:** f(a,b) → f(a)(b).
* **Mechanism:** Chaining closures.
* **Infinite Currying:** Uses recursion and an empty call as a base case.
* **Key Advantage:** Reusability and function specialization.