# JavaScript Interview Prep: Promises (The Exhaustive Master Guide)

This guide covers the evolution of asynchronous JavaScript, from Callbacks to the technical internals of the Promise object.

---

## 1. Synchronous vs Asynchronous JS
* **Synchronous:** Code executes line-by-line. Each line waits for the previous one to finish.
* **Asynchronous:** Allows the program to start a long-running task and still be responsive to other events.
* **Golden Rule:** Each turn of the event loop runs synchronous code first; macrotasks (e.g. `setTimeout`) and microtasks (e.g. promise `.then`) are scheduled and run in defined order after the current stack clears.

---

## 2. The Callback Era & Callback Hell
A **Callback** is a function passed as an argument to another function.
* **Problem:** When you have multiple dependent async tasks, you end up with deeply nested code.
* **Callback Hell:** Also known as the "Pyramid of Doom." It makes code unreadable and unmaintainable.

---

## 3. Introduction to Promises
A Promise represents the eventual completion (or failure) of an asynchronous operation and its resulting value.

### **States of a Promise:**
1.  **Pending:** Initial state, neither fulfilled nor rejected.
2.  **Fulfilled (Resolved):** Operation completed successfully.
3.  **Rejected:** Operation failed.

### **Basic Syntax:**
```javascript
const sub = new Promise((resolve, reject) => {
  setTimeout(() => {
    const result = true;
    if (result) resolve("Success!");
    else reject(new Error("Failed!"));
  }, 2000);
});

sub.then((res) => console.log(res)).catch((err) => console.log(err));
```

---

## 4. Promise Chaining
To solve callback hell, we use **Promise Chaining**. If a `.then()` handler returns a promise, the next `.then()` will wait for it to resolve.
```javascript
fetchUser("123")
  .then((res) => {
    console.log(res);
    return fetchPosts(res.id); // returning another promise continues the chain
  })
  .then((res) => {
    console.log(res);
  })
  .catch((err) => console.log(err));
```

---

## 5. Promise Combinators
Used to handle multiple promises in parallel.

| Method | Behavior |
| :--- | :--- |
| **Promise.all** | Returns all results if **all** resolve. Fails if **any** reject. |
| **Promise.race** | Returns the result/error of the **first** promise to settle. |
| **Promise.allSettled** | Returns an array of objects describing the outcome of **all** promises. |
| **Promise.any** | Returns the **first resolved** promise. Fails only if all reject. |

---

## 6. Async / Await
Modern syntactic sugar for Promises. It makes async code look and behave like sync code.
* Uses `try...catch` for error handling.
* `await` can only be used inside an `async` function.

---

## 7. Critical Output-Based Questions

### **Q1: Sync vs Async Order**
```javascript
console.log("start");
const promise1 = new Promise((resolve, reject) => {
  console.log(1);
  resolve(2);
});
promise1.then((res) => {
  console.log(res);
});
console.log("end");
// Output: start, 1, end, 2
// Reason: Promise executor runs synchronously. .then() is asynchronous.
```

### **Q2: Resolve without Call**
**Question:** What if there is no `resolve()` called?
**Answer:** The `.then()` block will never be executed; the promise stays in a `pending` state.

### **Q3: Tricky Chaining**
**Question:** If a `.catch()` returns a string, is the next `.then()` called?
**Answer:** Yes. Returning anything from a `.catch()` (that isn't a thrown error) resolves the promise chain.

---

## 8. Technical Polyfills (Senior Interview)

### **Polyfill for Promise.all**
```javascript
function myPromiseAll(promises) {
  return new Promise((resolve, reject) => {
    let results = [];
    let pending = promises.length;
    if (pending === 0) resolve(results);
    
    promises.forEach((promise, index) => {
      Promise.resolve(promise).then((res) => {
        results[index] = res;
        pending--;
        if (pending === 0) resolve(results);
      }, reject);
    });
  });
}
```

### **Custom Promise implementation (Simplified)**
Key logic requires:
1.  Handling `fulfilled`, `rejected`, and `pending` states.
2.  Queuing callbacks if `.then()` is called before the promise resolves (Async case).
3.  Executing immediately if promise is already resolved (Sync case).

---

## 💡 Quick Revise Summary
- **Executor Function:** Runs immediately (synchronously).
- **.then() / .catch():** Always pushed to the Microtask Queue (asynchronous).
- **Microtask Queue** has higher priority than the **Task Queue** (setTimeout).
- Use **Promise.all** for parallel tasks where you need all data to proceed.