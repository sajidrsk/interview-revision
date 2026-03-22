# React JS Interview: Pagination (Complete Implementation Guide)

Pagination allows developers to present large datasets in manageable chunks. This guide explores two primary ways to implement it.

---

## 1. Problem Statement & Requirements [00:01:12]
The interviewer asks you to fetch products from a dummy API (e.g., `dummyjson.com`) and display them in a grid.
* **Junior Goal:** Fetch all data once and paginate it in the frontend.
* **Senior Goal:** Use API parameters (`limit` and `skip`) to fetch only the required data for each page change.

---

## 2. Approach A: Frontend-Driven Pagination [00:10:49]
In this method, we fetch the entire dataset (e.g., 100 items) into local state and then slice it based on the current page.

### **The Slicing Logic [00:11:20]**
To show 10 items per page, we calculate the start and end index:
```javascript
// Page 1: (1 * 10 - 10) = 0 to (1 * 10) = 10
// Page 2: (2 * 10 - 10) = 10 to (2 * 10) = 20
{products.slice(page * 10 - 10, page * 10).map(prod => (
  <span key={prod.id}>{prod.title}</span>
))}
```

---

## 3. Approach B: Backend-Driven Pagination [00:20:10]
This is more performant as it prevents the browser from loading massive JSON files. We update the API URL based on the `page` state.

### **Dynamic API Fetching [00:22:42]**
```javascript
const fetchProducts = async () => {
  const skip = page * 10 - 10;
  const res = await fetch(`https://dummyjson.com/products?limit=10&skip=${skip}`);
  const data = await res.json();
  setProducts(data.products);
  setTotalPages(data.total / 10);
};

// Crucial: Run fetch whenever 'page' changes [00:24:03]
useEffect(() => {
  fetchProducts();
}, [page]);
```

---

## 4. Building the UI: Components & Styling [00:13:10]

### **Pagination Controls**
We generate numbers 1 through 10 dynamically:
```javascript
{[...Array(products.length / 10)].map((_, i) => (
  <span 
    className={page === i + 1 ? "selected" : ""} 
    onClick={() => setPage(i + 1)}
  >
    {i + 1}
  </span>
))}
```

### **Prev / Next Buttons [00:17:33]**
* **Prev:** `setPage(page - 1)`. Hidden/disabled if `page === 1`.
* **Next:** `setPage(page + 1)`. Hidden/disabled if `page === totalPages`.

---

## 5. Interviewer's Evaluation Criteria (Pro-Tips)
1.  **Semantic HTML:** Use `alt` tags for images and meaningful class names. [00:07:22]
2.  **BEM Convention:** The tutor suggests using the `products__single` naming pattern. [00:07:53]
3.  **Key Props:** Always provide a unique `key` (like `prod.id`) when mapping to assist the Virtual DOM. [00:08:16]
4.  **UX Details:** Add `cursor: pointer` to clickable spans and show a "Selected" state for the current page number. [00:09:36]

---

## 6. Logic "Gotchas"
* **API Response Format:** Remember that the data from `dummyjson` comes as an object containing a `products` array, not as a raw array. Check for `data && data.products` before setting state. [00:05:10]
* **String to Number:** API params in the URL should be numbers. Ensure your page calculation doesn't accidentally concatenate strings.

---

## 💡 Quick Revise Summary
- **Frontend Pagination:** Fast navigation but high initial load time. Good for small datasets.
- **Backend Pagination:** Scalable for millions of records. Requires an API call on every page change.
- **Dependency Array:** In the backend approach, the `page` variable MUST be in the `useEffect` dependency array.
- **Total Count:** The backend must return the total record count (e.g., `data.total`) so the frontend knows how many page numbers to render.