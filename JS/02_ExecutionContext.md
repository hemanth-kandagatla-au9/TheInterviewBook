# Execution Context (EC) in JavaScript

## What is an Execution Context?
An **Execution Context** is an abstract environment where JavaScript code is compiled, evaluated, and executed. It manages the scope, variables, and logic needed to run a piece of code. Execution contexts are stacked in the **call stack** to track program execution flow.

---

## Types of Execution Contexts
1. **Global Execution Context (GEC)**  
   - Created for **top-level code** (code not inside any function).  
   - There is exactly **one GEC** per JavaScript program.  

2. **Function Execution Context**  
   - Created **each time a function is called**.  
   - One EC per function invocation.  

3. **Eval Execution Context**  
   - Created for code executed via `eval()` (rarely used).  

---

## Components of an Execution Context
Every EC contains three core components:

### 1. Variable Environment
- **Stores**:  
  - `let`, `const`, and `var` declarations (with hoisting rules applied).  
  - Function declarations (fully hoisted).  
  - The `arguments` object (only in function ECs).  

### 2. Scope Chain
- A hierarchy of **lexical scopes** (parent-child relationships).  
- Determines accessibility of variables/functions (e.g., inner functions can access outer scope variables).  

### 3. `this` Binding
- The value of `this` depends on how the function is called:  
  - Global EC: `this` refers to the global object (`window` in browsers, `global` in Node.js).  
  - Function EC: `this` is determined by the call-site (unless using arrow functions).  

⚠️ **Arrow Functions**: Do NOT have their own `arguments` object or `this` binding. They inherit `this` from the closest enclosing regular function (lexical scoping).

---

## Call Stack Mechanics
- The **call stack** is a LIFO (Last-In-First-Out) data structure that manages execution contexts.  
- **Process**:  
  1. GEC is created and pushed to the stack when the script starts.  
  2. When a function is called, its EC is created and pushed on top.  
  3. When a function finishes, its EC is popped off the stack.  

**Example**:  
```javascript
function greet() {
  console.log("Hello!"); // New EC for greet() is created
}

greet(); // EC is popped after execution
console.log("Done"); // Runs in GEC