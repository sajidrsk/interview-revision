# JavaScript Interview Prep: Debouncing & Throttling (Exhaustive Guide)

Debouncing and Throttling are two essential techniques used to optimize event handling performance by limiting the rate at which a function is executed.

---

## 1. What is Debouncing?

Debouncing ensures that a function is only executed after a certain period of "silence" (no events triggered).

- **Real-World Example:** A search bar on an e-commerce site like Flipkart. It shouldn't make an API call on every keystroke. It waits until the user stops typing for, say, 400ms before fetching results.
- **Core Goal:** To limit the number of expensive operations (like API calls) triggered by rapid user actions.

---

## 2. What is Throttling?

Throttling ensures that a function is executed at most once every specified time interval, regardless of how many times the event is triggered.

- **Real-World Example:** Twitter infinite scroll. As you scroll, the scroll event fires hundreds of times. Throttling limits the API call for new posts to fire only once every 500ms.
- **Core Goal:** To maintain consistent function execution during a continuous stream of events.

---

## 3. Comparison Table

| Feature      | Debouncing                                         | Throttling                                    |
| :----------- | :------------------------------------------------- | :-------------------------------------------- |
| **Logic**    | Executes after the delay since the _last_ trigger. | Executes once every fixed interval.           |
| **Timer**    | Resets every time the event is triggered.          | Ignores triggers until the interval passes.   |
| **Best For** | Search bars, window resizing, auto-save.           | Scroll events, mouse movements, game actions. |

---

## 4. Technical Polyfills (Manual Implementation)

### **Polyfill for Debounce**

This version includes the logic to handle arguments and context (this).

```javascript
function myDebounce(cb, delay) {
  let timer;

  return function (...args) {
    const ctx = this;
    if (timer) clearTimeout(timer); // Reset timer if called again before delay finishes

    timer = setTimeout(() => {
      cb.apply(ctx, args);
    }, delay);
  };
}
```
*Note:* Preserving `this` matters when the debounced function is used as a method (e.g. `obj.method()`).

### **Polyfill for Throttle**

Leading-edge throttle: first call runs immediately, then a **cooldown** flag ignores further calls until `delay` ms have passed. (Another common variant tracks `last` time with `Date.now()` for trailing-edge behavior.)

```javascript
function myThrottle(cb, delay) {
  let wait = false;

  return function (...args) {
    const ctx = this;
    if (wait) return;

    cb.apply(ctx, args);
    wait = true;

    setTimeout(() => {
      wait = false;
    }, delay);
  };
}
```

---

## 5. Coding Task: UI Implementation

**Question:** Create a button UI where pressing the button increments a "Pressed" counter immediately, but increments a "Triggered" counter using Debounce/Throttle.

### **The Logic Flow:**

1.  **Pressing the button:** Fires the `click` event.
2.  **Regular Counter:** Increments on every click.
3.  **Debounced Counter:** Only increments after the user stops clicking for 800ms.
4.  **Throttled Counter:** Increments at most once every 800ms, even if the user clicks 20 times.

---

## 6. Interview Logic "Gotchas"

### **ClearTimeout in Debounce**

**Question:** Why do we need `clearTimeout(timer)`?
**Answer:** Without it, every single keystroke would eventually trigger its own API call after the delay. `clearTimeout` cancels the previous scheduled call so only the _very last_ one survives.

### **Arguments and Context**

**Question:** Why do we return an anonymous function inside the polyfill?
**Answer:** To maintain the original function's closure, arguments, and scope, ensuring that the debounced/throttled function behaves exactly like the original.

---

## 💡 Quick Revise Summary

- **Debounce:** "Wait until I'm done." (Search)
- **Throttle:** "Only once every X ms." (Scroll)
- **Debounce Implementation:** Uses `setTimeout` + `clearTimeout`.
- **Throttle Implementation:** Often a “cooldown” flag with `setTimeout`, or a `last`-time / timestamp check so the handler runs at most once per interval.
