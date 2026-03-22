# JavaScript Interview Prep: Promises (The Exhaustive Master Guide)

**Source:** [RoadsideCoder - Promises, Callbacks, Async/await, Polyfills](https://www.youtube.com/watch?v=HaJdoFp2OEc)

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
| **Promise.all** | Returns an array of results if **all** resolve. Fails completely if **any** reject. |
| **Promise.race** | Returns the result/error of the **first** promise to settle (either resolve or reject). |
| **Promise.allSettled** | Returns an array of objects describing the outcome (fulfilled/rejected) of **all** promises. |
| **Promise.any** | Returns the **first resolved** promise. Fails only if **all** promises reject. |

---

## 6. Async / Await
Modern syntactic sugar for Promises. It makes async code look and behave like sync code.
* Uses `try...catch` for error handling.
* `await` can only be used inside an `async` function.

### **Rewriting Promises with Async/Await**
**Question:** Rewrite the following `.then/.catch` logic using `async/await`.
```javascript
function loadJson(url) {
  return fetch(url).then((response) => {
    if (response.status == 200) {
      return response.json();
    } else {
      throw new Error(response.status);
    }
  });
}
```
**Answer:**
```javascript
async function loadJson(url) {
  const response = await fetch(url);
  if (response.status === 200) {
    return await response.json();
  } else {
    throw new Error(response.status);
  }
}
```

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
**Question:** What if there is no `resolve()` or `reject()` called inside the executor?
**Answer:** The `.then()` block will never be executed; the promise stays in a `pending` state permanently.

### **Q3: Tricky Chaining with Catch**
**Question:** What is the output?
```javascript
const promise = new Promise((resolve, reject) => {
  reject(Error("Some Error"));
});

promise
  .catch(err => console.log(err.message))
  .then(() => console.log("Success 4"));
```
**Answer:**
```text
Some Error
Success 4
```
**Logic:** Returning anything implicitly from `.catch()` (that isn't a thrown error) returns a resolved promise, continuing the chain to the next `.then()`.

### **Q4: Advanced Promise Chaining**
**Question:** What is the output?
```javascript
function job(state) {
  return new Promise(function(resolve, reject) {
    if (state) {
      resolve("success");
    } else {
      reject("error");
    }
  });
}

job(true)
  .then(function(data) {
    console.log(data);
    return job(true);
  })
  .then(function(data) {
    if (data !== "victory") {
      throw "Defeat";
    }
    return job(true);
  })
  .then(function(data) {
    console.log(data);
  })
  .catch(function(error) {
    console.log(error);
    return "Error caught";
  })
  .then(function(data) {
    console.log(data);
    return new Error("test");
  })
  .then(function(data) {
    console.log("Success:", data.message);
  });
```
**Output:**
```text
success
Defeat
Error caught
Success: test
```
**Logic:** 
1. `job(true)` resolves, prints "success", and returns `job(true)`.
2. Second `.then` gets "success". Since `"success" !== "victory"`, it throws `"Defeat"`.
3. The thrown error skips the next `.then` and goes straight to the `.catch`.
4. The `.catch` prints "Defeat" and returns "Error caught" (a resolved string).
5. The next `.then` prints "Error caught" and returns a `new Error` object (Note: it is returned, not thrown, so it does not trigger a rejection).
6. The final `.then` prints the error object's message: "Success: test".

---

## 8. Coding Tasks

### **Task 1: Resolve Promises Recursively**
**Question:** Implement a function that takes an array of promises and resolves them recursively.
```javascript
function promRecurse(promises) {
  if (promises.length === 0) return;

  const currentPromise = promises.shift();
  
  currentPromise
    .then((res) => console.log(res))
    .catch((err) => console.error(err))
    .finally(() => promRecurse(promises)); // Recursively call with remaining
}
```

---

## 9. Technical Polyfills (Senior Interview)

### **Polyfill for Promise.all**
```javascript
function myPromiseAll(promises) {
  return new Promise((resolve, reject) => {
    let results = [];
    let pending = promises.length;
    if (pending === 0) {
      resolve(results);
      return;
    }
    
    promises.forEach((promise, index) => {
      Promise.resolve(promise).then((res) => {
        results[index] = res;
        pending--;
        if (pending === 0) resolve(results);
      }).catch(reject); // Reject immediately if any fails
    });
  });
}
```

### **Custom Promise implementation (Simplified)**
**Question:** How would you build your own basic Promise polyfill handling `.then` and `resolve` asynchronously?
```javascript
function PromisePolyfill(executor) {
  let onResolve, onReject;
  let isFulfilled = false, isRejected = false, isCalled = false, value;

  function resolve(val) {
    isFulfilled = true;
    value = val;
    if (typeof onResolve === "function") {
      onResolve(val);
      isCalled = true;
    }
  }

  function reject(val) {
    isRejected = true;
    value = val;
    if (typeof onReject === "function") {
      onReject(val);
      isCalled = true;
    }
  }

  this.then = function (callback) {
    onResolve = callback;
    if (isFulfilled && !isCalled) {
      isCalled = true;
      onResolve(value);
    }
    return this;
  };

  this.catch = function (callback) {
    onReject = callback;
    if (isRejected && !isCalled) {
      isCalled = true;
      onReject(value);
    }
    return this;
  };

  try {
    executor(resolve, reject);
  } catch (error) {
    reject(error);
  }
}
```

---

## 💡 Quick Revise Summary
- **Executor Function:** Runs immediately (synchronously) upon initialization.
- **.then() / .catch():** Always pushed to the Microtask Queue (asynchronous) when the state changes.
- **Microtask Queue** has higher priority than the **Task Queue** (setTimeout).
- Use **Promise.all** for parallel tasks where you need all data to proceed.
- Errors thrown inside `.then()` automatically go to the nearest `.catch()`. 
- Returning normal data (or Error objects) from `.catch()` continues execution normally in the next `.then()`.