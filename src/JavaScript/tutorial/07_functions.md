---

[<< Chapter 6](./06_control_flow_structures.md) | [Chapter 8 >>](./08_advanced_functions.md)

---

# Chapter 7: Functions

Functions are one of the most important building blocks in JavaScript. They allow developers to write reusable, modular, and efficient code.

---

## **1. Introduction to Functions**

A function is a block of code designed to perform a specific task. Functions are executed when they are invoked (called).

### **Syntax**:

```javascript
function functionName(parameters) {
  // Code to be executed
}
```

- **functionName**: The name of the function.
- **parameters**: Input values the function can accept.
- **return**: Sends a value back to the caller.

Example:

```javascript
function greet(name) {
  return `Hello, ${name}!`;
}
console.log(greet("Alice")); // Output: Hello, Alice!
```

---

## **2. Types of Functions**

### **Function Declarations**

- Defined using the `function` keyword.
- Hoisted to the top of their scope.
- Example:
  ```javascript
  function add(a, b) {
    return a + b;
  }
  console.log(add(2, 3)); // Output: 5
  ```

### **Function Expressions**

- Functions assigned to variables.
- Not hoisted.
- Example:
  ```javascript
  const multiply = function (a, b) {
    return a * b;
  };
  console.log(multiply(2, 3)); // Output: 6
  ```

### **Arrow Functions**

- A concise way to write functions introduced in ES6.
- Do not bind their own `this`.
- Syntax:
  ```javascript
  const functionName = (parameters) => expression;
  ```
  Example:
  ```javascript
  const subtract = (a, b) => a - b;
  console.log(subtract(5, 3)); // Output: 2
  ```

### **Anonymous Functions**

- Functions without a name, often used as arguments.
- Example:
  ```javascript
  setTimeout(function () {
    console.log("This runs after 2 seconds");
  }, 2000);
  ```

### **Immediately Invoked Function Expressions (IIFE)**

- Functions executed as soon as they are defined.
- Syntax:
  ```javascript
  (function () {
    console.log("IIFE executed");
  })();
  ```

---

## **3. Parameters and Arguments**

### **Default Parameters**

- Allow initializing parameters with default values.
- Example:
  ```javascript
  function greet(name = "Guest") {
    return `Hello, ${name}!`;
  }
  console.log(greet()); // Output: Hello, Guest!
  ```

### **Rest Parameters**

- Collects all remaining arguments into an array.
- Example:
  ```javascript
  function sum(...numbers) {
    return numbers.reduce((acc, num) => acc + num, 0);
  }
  console.log(sum(1, 2, 3, 4)); // Output: 10
  ```

### **Spread Operator**

- Expands an array into individual elements.
- Example:
  ```javascript
  function maxOfNumbers(a, b, c) {
    return Math.max(a, b, c);
  }
  const nums = [1, 2, 3];
  console.log(maxOfNumbers(...nums)); // Output: 3
  ```

### **`arguments` Object**

- An array-like object accessible inside regular functions (not arrow functions).
- Example:
  ```javascript
  function showArguments() {
    console.log(arguments);
  }
  showArguments(1, 2, 3); // Output: [1, 2, 3]
  ```

---

## **5. Scope,Hoisting and Closures**

### **Scope**

- Determines the accessibility of variables.
  - **Global Scope**: Accessible anywhere in the code.
  - **Local Scope**: Accessible only within a function.

Example:

```javascript
let globalVar = "I am global";
function checkScope() {
  let localVar = "I am local";
  console.log(globalVar); // Accessible
  console.log(localVar); // Accessible
}
checkScope();
console.log(globalVar); // Accessible
console.log(localVar); // Error: localVar is not defined
```

### **Closures**

- A function that retains access to its outer scope even after the outer function has executed.
- Example:
  ```javascript
  function outerFunction(outerVar) {
    return function innerFunction(innerVar) {
      console.log(`Outer: ${outerVar}, Inner: ${innerVar}`);
    };
  }
  const closureFunc = outerFunction("outside");
  closureFunc("inside"); // Output: Outer: outside, Inner: inside
  ```

### **Hoisting**

- JavaScript moves declarations to the top of their scope before code execution.
- Example:
  ```javascript
  console.log(a); // Output: undefined
  var a = 5;
  ```
  In the above example, `var a` is hoisted, but the initialization `a = 5` is not.

---

## **6. Higher-Order Functions**

Higher-order functions take other functions as arguments or return them as results. They are a key feature of functional programming.

- Example:
  ```javascript
  function higherOrderFunction(callback) {
    callback();
  }
  higherOrderFunction(() => console.log("Callback executed"));
  ```

### **Common Examples**:

- **Array Methods**: `map`, `filter`, `reduce`
  ```javascript
  const numbers = [1, 2, 3, 4];
  const doubled = numbers.map((num) => num * 2);
  console.log(doubled); // Output: [2, 4, 6, 8]
  ```

---

## **7. Arrow Function Details**

- Arrow functions do not have their own `this`, `arguments`, `super`, or `new.target` and cannot be used as constructors.
- To return an object literal implicitly, wrap it in parentheses.
- Examples:
  ```javascript
  const getObj = () => ({ id: 1 });
  // const x = new (() => {})(); // TypeError: not constructible
  ```

## **8. Parameter Destructuring and Defaults**

- Destructure parameters for clarity and provide defaults directly in the signature.
- Example:
  ```javascript
  function createUser({ name, role = "user" }) {
    return { name, role };
  }
  console.log(createUser({ name: "Alice" })); // { name: 'Alice', role: 'user' }
  ```

## **9. `arguments` vs Rest Parameters**

- `arguments` is array-like and only available in non-arrow functions.
- Rest parameters (`...args`) produce a real array and are preferred.
- Example:
  ```javascript
  function oldWay() {
    const args = Array.from(arguments);
    return args.join(",");
  }
  const newWay = (...args) => args.join(",");
  console.log(oldWay(1, 2, 3)); // "1,2,3"
  console.log(newWay(1, 2, 3)); // "1,2,3"
  ```

## **10. Generators**

- Generator functions (`function*`) can pause and resume execution using `yield`.
- Useful for iterators, lazy evaluation, and controlling async flows (with libraries).
- Example:
  ```javascript
  function* idGen() {
    let i = 0;
    while (true) yield i++;
  }
  const g = idGen();
  console.log(g.next().value); // 0
  console.log(g.next().value); // 1
  ```

## **11. Pitfalls and Best Practices**

- Closures in loops: prefer `let` over `var` to capture per-iteration values.
- Prefer early returns to reduce nesting and improve readability.
- Example (closure in loops):

  ```javascript
  const fns = [];
  for (var i = 0; i < 3; i++) {
    fns.push(() => i);
  }
  console.log(fns[0]()); // 3 (single shared binding)

  const fns2 = [];
  for (let j = 0; j < 3; j++) {
    fns2.push(() => j);
  }
  console.log(fns2[0]()); // 0
  ```

## **12. Useful Function Metadata**

- `Function.name`: the function’s name; `Function.length`: number of declared parameters.
- Example:
  ```javascript
  function sample(a, b, c) {}
  console.log(sample.name); // 'sample'
  console.log(sample.length); // 3
  ```

## **Conclusion**

Functions are a cornerstone of JavaScript programming, enabling code reusability, modularity, and cleaner design. Understanding the various types of functions, scopes, closures, and higher-order functions is essential for writing efficient and effective JavaScript code.

---

[<< Chapter 6](./06_control_flow_structures.md) | [Chapter 8 >>](./08_advanced_functions.md)

---
