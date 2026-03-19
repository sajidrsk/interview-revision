# JavaScript Interview Prep: Event Propagation (Exhaustive Guide)

Event propagation determines the order in which event handlers are executed when one element is nested inside another and both have handlers for the same event.

---

## 1. What is Event Propagation?
When you click a button inside a form, you are technically clicking the form and the surrounding div as well. Event propagation is the process of deciding the direction (top-to-bottom or bottom-to-top) in which these events are fired.

---

## 2. Event Bubbling
By default, events **bubble up** from the innermost element to the outermost element.
* **Order:** Child → Parent → Ancestor → Global.
* **Analogy:** Bubbles rise from the bottom of a pool to the top.
* **Example:** Clicking a button inside a div triggers the button's event first, then the div's.

**Note:** Some events do NOT bubble, such as `focus`, `blur`, `mouseenter`, and `mouseleave`.

---

## 3. Event Capturing (Trickling)
Events are executed from the outermost element down to the target element.
* **Mechanism:** To enable capturing, pass `{ capture: true }` as the third argument in `addEventListener`.
* **Order:** Global → Ancestor → Parent → Child.

---

## 4. Key Terminology: Target vs. CurrentTarget

| Property | Definition | Value during Bubbling |
| :--- | :--- | :--- |
| **event.target** | The element that actually triggered the event (where the click started). | Remains constant (the origin element). |
| **event.currentTarget** | The element to which the current event listener is attached. | Changes as the event bubbles up. |
| **this** | In a standard function, it points to the element the listener is attached to. | Same as `event.currentTarget`. |

---

## 5. Stopping Propagation
If you only want the event to stop bubbling to ancestors, use `stopPropagation()`:
```javascript
element.addEventListener("click", (e) => {
  e.stopPropagation();
  alert("Parent handlers won't run for this bubble phase.");
});
```
`stopImmediatePropagation()` goes further: other listeners registered on the **same** element for the same phase are not run either.

`preventDefault()` is different—it cancels the browser’s default action (e.g. following a link), not propagation.

---

## 6. Event Delegation
The most high-value interview topic. Instead of adding an event listener to 100 individual list items (which is bad for memory), you add **one** listener to the parent element.

### **Implementation Strategy:**
1. Attach listener to the parent container.
2. Use `event.target` to identify which child was clicked.
3. Check the `tagName` or `className` to trigger specific logic.

**Benefits:**
* Lower memory usage.
* Handles dynamically added elements automatically.

---

## 7. Senior Level Challenge: Modal Closing
**Requirement:** Create a modal that closes when you click the "negative space" (the background) but stays open if you click inside the modal content.

### **The Solution logic:**
Do not just add a close listener to the container, because bubbling will cause a click *inside* the modal to trigger the container's close event.
```javascript
modalContainer.addEventListener("click", (e) => {
  if (e.target.className === "modal-container") {
    closeModal();
  }
});
```
**Logic:** By checking if `e.target` is exactly the container, we ensure clicks on the nested modal content are ignored.

---

## 💡 Quick Revise Checklist
- [ ] **Bubbling:** Child to Parent (Default).
- [ ] **Capturing:** Parent to Child (Set `capture: true`).
- [ ] **Delegation:** Listener on parent + `e.target` check.
- [ ] **e.stopPropagation():** Stops the event from reaching other elements (bubbling/capturing).
- [ ] **e.stopImmediatePropagation():** Like above, and skips other listeners on the same target.
- [ ] **e.preventDefault():** Stops default browser behavior (like form submission or link following).