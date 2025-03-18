# JavaScript Garbage Collection

## Overview
JavaScript is a **garbage-collected language**, meaning it automatically manages memory allocation and deallocation. Developers don’t manually free memory, reducing risks like memory leaks.

---

## Mark-and-Sweep Algorithm (Primary GC Algorithm)

### 1. Mark Phase
- Starts from roots (global variables, currently executing function’s variables).
- Traverses and marks all reachable objects.

### 2. Sweep Phase
- Deletes unmarked (unreachable) objects.

---

## How Garbage Collection Works

### 1. **Memory Allocation**  
Objects/variables are stored in the **heap** during execution.

```javascript
function createData() {
  const data = new Array(100000).fill('📦'); // Allocates heap memory
}

createData(); // Data becomes unreachable after function finishes
```

- After the `createData` function finishes, the `data` variable becomes unreachable.
- The garbage collector identifies it as unreachable and frees the memory during the sweep phase.

---

This process helps prevent memory leaks by ensuring that memory used by unreachable objects is reclaimed automatically.

