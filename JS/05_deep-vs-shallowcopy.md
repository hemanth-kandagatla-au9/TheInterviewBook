# Deep Copy vs Shallow Copy in JavaScript

## 1. Shallow Copy

### Definition
A **shallow copy** duplicates only the top-level properties of an object. Nested objects/arrays are **referenced**, not cloned. Changes to nested properties in the copy will affect the original object.

### How to Create

#### For Objects:
```javascript
const originalObj = { a: 1, nested: { b: 2 } };

// Method 1: Object.assign()
const shallowCopy1 = Object.assign({}, originalObj);

// Method 2: Spread Operator (...)
const shallowCopy2 = { ...originalObj };
```

#### For Arrays:
```javascript
const originalArr = [1, [2, 3]];

// Method 1: slice()
const shallowArr1 = originalArr.slice();

// Method 2: Spread Operator (...)
const shallowArr2 = [...originalArr];
```

### Example of Shared Reference
```javascript
shallowCopy1.nested.b = 99;
console.log(originalObj.nested.b); // Output: 99 (Original modified!)
```

## 2. Deep Copy

### Definition
A **deep copy** recursively clones all nested objects/arrays, creating a fully independent copy. Changes to the copy do not affect the original.

### How to Create
```javascript
const original = { a: 1, nested: { b: 2 }, date: new Date() };

// Method 1: JSON.parse(JSON.stringify()) (Limited)
const deepCopy1 = JSON.parse(JSON.stringify(original));
// ❗ Fails for functions, Dates, undefined, and circular references.

// Method 2: structuredClone() (Modern Browsers/Node.js ≥17)
const deepCopy2 = structuredClone(original); // ✅ Handles Dates but not functions.

// Method 3: Lodash (Third-Party Library)
const _ = require('lodash');
const deepCopy3 = _.cloneDeep(original); // ✅ Handles most cases.
```

### Example of Independence
```javascript
deepCopy2.nested.b = 99;
console.log(original.nested.b); // Output: 2 (Original unchanged)
```

## 3. Key Differences

| Aspect | Shallow Copy | Deep Copy |
|--------|-------------|-----------|
| Nested Objects | Shared by reference | Recursively cloned |
| Performance | Faster (minimal memory) | Slower (high memory usage) |
| Use Case | Top-level data immutability | Full object independence |
| Tools | Object.assign(), spread operator | structuredClone, lodash.cloneDeep |

## 4. Common Pitfalls

### Pitfall 1: Accidental Mutation of Original
```javascript
const original = { items: ['apple'] };
const copy = { ...original };
copy.items.push('banana');
console.log(original.items); // ['apple', 'banana'] 😱
```

### Pitfall 2: JSON Method Limitations
```javascript
const obj = { 
  date: new Date(), 
  fn: () => console.log('Hi'), 
  undef: undefined 
};
const jsonClone = JSON.parse(JSON.stringify(obj));
// Result: { date: "2024-03-16T12:00:00.000Z" } (Date → string, fn/undef dropped)
```

### Pitfall 3: Circular References Crash JSON
```javascript
const obj = { a: 1 };
obj.self = obj; // Circular reference
JSON.stringify(obj); // ❌ Error: Circular structure
```

## 5. Best Practices

### Use Shallow Copies When:
- Working with simple objects (no nested data).
- Performance is critical.

### Use Deep Copies When:
- Managing state in libraries like Redux.
- Cloning nested objects/arrays.

### Recommended Tools:
- `structuredClone()` for modern apps.
- `lodash.cloneDeep()` for legacy systems or complex objects.
- **Immer** for immutable state updates (e.g., `produce` function).

### Avoid:
- Using JSON methods for objects with functions/Dates.
- Mutating nested properties of shallow copies.
