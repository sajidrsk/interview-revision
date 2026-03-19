# JavaScript Interview Prep: Objects (Exhaustive Guide)

Objects are the building blocks of JavaScript. This guide covers every property nuance, memory referencing logic, and output-based question from the series.

---

## 1. Object Basics & Deletion
* **Definition:** An object is a collection of key-value pairs.
* **Access:** Dot notation (`obj.key`) or Square brackets (`obj["key"]`).

### **The delete Keyword Gotcha**
**Question:** What is the output of the following IIFE?
```javascript
const func = (function(a) {
  delete a;
  return a;
})(5);
console.log(func); // Output: 5
```
**Logic:** The `delete` keyword is used to delete properties from an **object**, not local variables. Since `a` is a local variable (parameter), `delete` does nothing.

---

## 2. Dynamic & Computed Properties
If you have a variable and want to use its value as an object key:
```javascript
const property = "firstName";
const name = "Piyush";

const user = {
  [property]: name // Computed property
};
console.log(user.firstName); // "Piyush"
```

---

## 3. Iterating Through Objects
Use the `for...in` loop to iterate through keys.
```javascript
for (let key in user) {
  console.log(key); // Prints keys
  console.log(user[key]); // Prints values
}
```

---

## 4. Duplicate Keys Logic
**Question:** What happens if an object has two keys with the same name?
```javascript
const obj = { a: "one", b: "two", a: "three" };
console.log(obj); // Output: { a: "three", b: "two" }
```
**Logic:** The first declaration determines the **position** of the key, but the last declaration determines the **value**.

---

## 5. Coding Task: multiplyNumeric
**Question:** Create a function that multiplies all numeric property values of an object by 2.
```javascript
function multiplyByTwo(obj) {
  for (let key in obj) {
    if (typeof obj[key] === "number") {
      obj[key] *= 2;
    }
  }
}
```

---

## 6. Objects as Keys (The [object Object] Trap)
**Question:** What is the output?
```javascript
const a = {};
const b = { key: "b" };
const c = { key: "c" };

a[b] = 123;
a[c] = 456;

console.log(a[b]); // Output: 456
```
**Logic:** JavaScript cannot use an object as a key directly. It converts the object to a string. Both `b` and `c` stringify to `"[object Object]" `. Therefore, `a["[object Object]"]` is first set to 123 and then overwritten by 456.

---

## 7. JSON Methods (Stringify & Parse)
* **stringify:** Converts object to string (e.g., for LocalStorage).
* **parse:** Converts string back to object.

### **Advanced Stringify Filter**
**Question:** How can you stringify only specific properties?
```javascript
const settings = { username: "Piyush", level: 19, health: 90 };
const data = JSON.stringify(settings, ["level", "health"]);
console.log(data); // '{"level":19, "health":90}'
```

---

## 8. 'this' in Objects (Normal vs Arrow)
**Question:** What is the output?
```javascript
const shape = {
  radius: 10,
  diameter() { return this.radius * 2; },
  perimeter: () => 2 * Math.PI * this.radius
};
console.log(shape.diameter()); // 20
console.log(shape.perimeter()); // NaN
```
**Logic:** `diameter` is a regular function, so `this` is `shape`. `perimeter` is an arrow function: it has no own `this` and closes over the enclosing scope’s `this`. In a browser script at the top level that is often `window`, so `window.radius` is `undefined` → `NaN`. In **strict mode** or **ES modules**, top-level `this` can be `undefined`, which also yields `NaN`.

---

## 9. Destructuring & Nesting
* **Renaming:** `const { name: myName } = user;`
* **Nested:** `const { fullName: { first } } = user;`

---

## 10. Spread vs Rest Operators
* **Spread:** `const combined = { ...obj1, ...obj2 };`
* **Rest Parameter:** Must be the **last** argument in a function.

---

## 11. Object Referencing & Equality
* **Assignment:** `let d = c;` sharing the same memory address. Changing `d` changes `c`.
* **Equality:** `console.log({a:1} == {a:1})` is `false` because they point to different memory locations.

### **Reference Reassignment**
**Question:** Does changing an object inside a function affect the original?
```javascript
function changeAge(person) {
  person.age = 25; // Modifies original
  person = { name: "John", age: 50 }; // Reassigns local variable, original remains unchanged
  return person;
}
```

---

## 12. Deep Copy vs Shallow Copy
* **Shallow Copy:** Copies top-level properties; nested objects still share references.
  - `Object.assign({}, obj)`
  - `{ ...obj }`
* **Deep Copy:** Completely clones the entire tree.
  - `JSON.parse(JSON.stringify(obj))`

---

## 💡 Quick Revise Summary
- Objects are referenced, not copied.
- Arrow functions ignore object context for `this`.
- `delete` only works on object properties.
- Use `JSON.parse(JSON.stringify())` for simple deep clones.