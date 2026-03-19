# JavaScript Interview: Map, Filter, & Reduce (Deep Dive)

Comprehensive revision notes on array transformations, polyfills, and complex chaining.

---

## 1. Core Array Methods

### **Map()**
* **Purpose:** Creates a new array by applying a callback function to every element of the original array.
* **Callback Parameters:** `(currentElement, index, array)`.
* **Key Characteristic:** Always returns an array of the same length as the original.

### **Filter()**
* **Purpose:** Creates a new array with only the elements that satisfy a specific condition.
* **Logic:** If the callback returns `true`, the element is pushed to the new array; if `false`, it is skipped.

### **Reduce()**
* **Purpose:** Reduces an array of values down to a **single value**.
* **Parameters:** Takes a callback and an `initialValue`.
* **Callback Parameters:** `(accumulator, currentValue, index, array)`.
* **Accumulator:** Stores the result of the previous computation.

---

## 2. Writing Polyfills (Manual Implementation)
Interviewers use this to check your understanding of the `Array.prototype` and loop logic.

### **Map Polyfill**
```javascript
Array.prototype.myMap = function(cb) {
  let temp = [];
  for (let i = 0; i < this.length; i++) {
    temp.push(cb(this[i], i, this));
  }
  return temp;
};
```

### **Filter Polyfill**
```javascript
Array.prototype.myFilter = function(cb) {
  let temp = [];
  for (let i = 0; i < this.length; i++) {
    if (cb(this[i], i, this)) {
      temp.push(this[i]);
    }
  }
  return temp;
};
```

### **Reduce Polyfill**
```javascript
Array.prototype.myReduce = function(cb, initialValue) {
  var accumulator = initialValue;
  for (let i = 0; i < this.length; i++) {
    // If no initialValue, first element becomes accumulator
    accumulator = accumulator !== undefined ? cb(accumulator, this[i], i, this) : this[i];
  }
  return accumulator;
};
```

---

## 3. Map vs ForEach
* **Return Value:** `map` returns a new array; `forEach` returns `undefined`.
* **Chaining:** You can chain other methods (like `.filter()`) after a `map`, but not after a `forEach`.
* **Original Array:** Both do not change the original array by default, but `forEach` is typically used when you want to perform "side effects" (e.g., logging or saving to a DB).

---

## 4. Complex Output Questions (The Student Array)
*Scenario: An array of objects containing `name`, `rollNumber`, and `marks`.*

### **Q1: Return names in Capital Letters**
```javascript
const names = students.map((stu) => stu.name.toUpperCase());
```

### **Q2: Return details of students scoring > 60**
```javascript
const highScorers = students.filter((stu) => stu.marks > 60);
```

### **Q3: More than 60 marks AND Roll Number > 15**
```javascript
const filtered = students.filter((stu) => stu.marks > 60 && stu.rollNumber > 15);
```

### **Q4: Sum of marks of all students**
```javascript
const totalMarks = students.reduce((acc, curr) => acc + curr.marks, 0);
```

### **Q5: Return only names of students who scored > 60**
```javascript
const namesOnly = students.filter((stu) => stu.marks > 60).map((stu) => stu.name);
```

### **Q6: The "Grace Marks" Challenge**
**Goal:** Return total marks for students who scored > 60 **after** adding 20 marks to those who scored < 60.
```javascript
const finalMarks = students
  .map((stu) => {
    if (stu.marks < 60) {
      stu.marks += 20;
    }
    return stu;
  })
  .filter((stu) => stu.marks > 60)
  .reduce((acc, curr) => acc + curr.marks, 0);
// Depends on input data; the map step mutates each student object—prefer copying if you need immutability.
```

**Immutable variant (same idea):**
```javascript
const finalMarks = students
  .map((stu) => ({
    ...stu,
    marks: stu.marks < 60 ? stu.marks + 20 : stu.marks,
  }))
  .filter((stu) => stu.marks > 60)
  .reduce((acc, curr) => acc + curr.marks, 0);
```

---

## 💡 Key Revision Takeaways
1. **Chaining order matters:** Filtering before mapping can save processing time.
2. **Initial Value in Reduce:** Always provide a `0` or `[]` as an initial value to avoid errors with empty arrays.
3. **Immutability:** Map and Filter promote functional programming by returning new copies rather than mutating data.