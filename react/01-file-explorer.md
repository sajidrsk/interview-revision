# React JS Interview: File Explorer (Official Problem Statement & Solution)

This guide covers the exact requirements provided by the tutor for both junior and senior-level React interviews.

---

## 1. The Given Problem (Interviewer's Requirements) [00:00:33]

The interviewer asks you to build a **VS Code Sidebar Clone**.

### **Junior Requirements (The Basics):**

- **Data Rendering:** Take a provided recursive JSON object and render it as a tree.
- **Infinite Nesting:** The UI must support folders nested inside folders infinitely.
- **Expand/Collapse:** Clicking on a folder should toggle the visibility of its internal items.

### **Senior Requirements (The Advanced Add-on):**

- **Insert Functionality:** Add "Plus Folder" and "Plus File" buttons next to folder names.
- **Dynamic Inputs:** Clicking a button should reveal an input field specifically inside that folder.
- **Item Creation:** Pressing "Enter" should add the new item to the data structure at the correct location.
- **Event Handling:** Handle clicking outside the input (\`onBlur\`) and prevent event bubbling (\`stopPropagation\`).

---

## 2. Project Architecture & Setup [00:02:11]

The tutor emphasizes that **clean coding abilities** are measured. Suggested file structure:

- `/src/data/folderData.js`: Stores the initial JSON tree.
- `/src/components/Folder.js`: The recursive UI component.
- `/src/hooks/useTraverseTree.js`: The logic for tree manipulation.
- `/src/App.js`: The root container.

---

## 3. The Data Structure (JSON Tree) [00:03:01]

A recursive object where each node has an `id`, `name`, `isFolder` boolean, and an `items` array.

```javascript
const explorer = {
  id: "1",
  name: "root",
  isFolder: true,
  items: [
    {
      id: "2",
      name: "public",
      isFolder: true,
      items: [{ id: "3", name: "index.html", isFolder: false, items: [] }],
    },
  ],
};
export default explorer;
```

---

## 4. Initial Rendering & Recursion [00:06:51]

### **Step 1: The Basic Folder Component**

We pass the `explorer` data as a **Prop**.

> **Interview Tip:** What is the difference between State and Props?
> **Answer:** State is local data managed _within_ a component; Props are read-only data passed _down_ from a parent.

### **Step 2: Conditional Rendering**

Check if the item is a folder to display the 📁 icon and children; otherwise, show the 📄 icon.

```javascript
if (explorer.isFolder) {
  return (
    <div className="folderContainer">
      <div className="folder" onClick={() => setExpand(!expand)}>
        <span>📁 {explorer.name}</span>
      </div>
      <div style={{ display: expand ? "block" : "none", paddingLeft: 25 }}>
        {explorer.items.map((exp) => (
          <Folder key={exp.id} explorer={exp} /> // Recursive Call
        ))}
      </div>
    </div>
  );
}
```

---

## 5. Advanced UI: Adding Folders/Files [00:13:28]

### **The stopPropagation Gotcha**

The "Add" buttons are inside the clickable folder header. Clicking them would trigger the "Expand/Collapse" logic.
**Fix:**

```javascript
const handleNewItem = (e, isFolder) => {
  e.stopPropagation(); // Prevents parent folder from toggling
  setExpand(true); // Ensure folder is open when adding
  setShowInput({ visible: true, isFolder });
};
```

### **The Input Box Logic [00:16:50]**

- **autoFocus:** Ensures the user can start typing immediately without clicking the box.
- **onBlur:** If the user clicks away, the input disappears (`visible: false`).
- **onKeyDown:** Handles the "Enter" key (`keyCode === 13`).

---

## 6. The Algorithm: Custom Hook (DFS) [00:22:11]

The tutor uses **Depth First Search (DFS)** to traverse the tree.

> **Pro Tip:** When people say "DS/Algo isn't used in real work," show them this. We MUST use DFS to find a specific nested folder ID.

### **useTraverseTree Hook Implementation**

```javascript
const useTraverseTree = () => {
  const insertNode = (tree, folderId, item, isFolder) => {
    // Base Case: We found the target folder
    if (tree.id === folderId && tree.isFolder) {
      tree.items.unshift({
        id: new Date().getTime(),
        name: item,
        isFolder,
        items: [],
      });
      return tree;
    }

    // Recursive Step: Map through children and call insertNode again
    let latestNode = tree.items.map((obj) => {
      return insertNode(obj, folderId, item, isFolder);
    });

    return { ...tree, items: latestNode };
  };

  return { insertNode };
};
```

---

## 7. Tutor's Specific Guidelines & Best Practices

1.  **Unique IDs:** Use `new Date().getTime()` for quick ID generation during an interview, but mention `uuid` for production.
2.  **State Management:** Always update the state immutably by spreading the old data (`{...tree}`).
3.  **Scalability:** Pass functions like `handleInsertNode` through props so the logic resides at the root (`App.js`).

---

## 8. Tutor Suggestions (Homework) [00:33:55]

To make this application "Production Ready," the tutor suggests implementing:

1.  **Delete Node:** Create a `deleteNode` function in the hook to filter out items by ID.
2.  **Update Node:** Add an edit icon to rename files or folders.
3.  **Dynamic Programming Optimization:** Optimize the search so it doesn't re-traverse branches that were already checked.

---

## 💡 Quick Revise Summary

- **Component Design:** Use recursive self-calling.
- **Tree Traversal:** DFS is the core algorithm for nested updates.
- **UX:** Use `autoFocus` and `e.stopPropagation()` for a polished interface.
- **Folder vs File:** Manage logic conditionally based on the `isFolder` boolean.
