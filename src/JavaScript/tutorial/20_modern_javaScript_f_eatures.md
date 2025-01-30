# Chapter 20: Modern JavaScript Features

Modern JavaScript (ECMAScript) introduces a wide range of features that simplify development, enhance performance, and improve code readability. Staying up-to-date with these features is essential for writing efficient and maintainable code. This chapter explores the most important additions to JavaScript in recent versions.

---

## 1. ES6 and Beyond: A Brief Overview

ES6 (ECMAScript 2015) was a major milestone for JavaScript, introducing many foundational features. Subsequent updates (ES7, ES8, etc.) built upon this foundation, adding new syntax, APIs, and capabilities.

---

## 2. Arrow Functions and Default Parameters

### Arrow Functions:

Arrow functions provide a concise syntax for writing functions and automatically bind the `this` keyword.

```javascript
const add = (a, b) => a + b;
console.log(add(2, 3)); // 5
```

### Default Parameters:

Specify default values for function parameters.

```javascript
function greet(name = "Guest") {
  return `Hello, ${name}!`;
}
console.log(greet()); // Hello, Guest!
```

---

## 3. Template Literals and Destructuring

### Template Literals:

Simplify string interpolation and multi-line strings.

```javascript
const name = "Alice";
console.log(`Welcome, ${name}!`); // Welcome, Alice!
```

### Destructuring:

Extract values from arrays or objects into variables.

```javascript
const [a, b] = [1, 2];
const { name, age } = { name: "Bob", age: 25 };
console.log(a, b, name, age); // 1 2 Bob 25
```

---

## 4. Modules and Import/Export

Modules enable better code organization by splitting functionality into separate files.

```javascript
// math.js
export const add = (a, b) => a + b;

// main.js
import { add } from "./math.js";
console.log(add(3, 4)); // 7
```

---

## 5. Promises, Async/Await, and Generators

### Promises:

Handle asynchronous operations with a cleaner syntax than callbacks.

```javascript
fetch("https://api.example.com")
  .then((response) => response.json())
  .then((data) => console.log(data));
```

### Async/Await:

Provide a synchronous-like syntax for asynchronous code.

```javascript
async function fetchData() {
  const response = await fetch("https://api.example.com");
  const data = await response.json();
  console.log(data);
}
```

### Generators:

Create functions that can be paused and resumed.

```javascript
function* generateNumbers() {
  yield 1;
  yield 2;
  yield 3;
}

for (const num of generateNumbers()) {
  console.log(num); // 1, 2, 3
}
```

---

## 6. New Data Structures: Maps, Sets, WeakMaps, WeakSets

### Maps and Sets:

Efficiently store unique values and key-value pairs.

```javascript
const map = new Map([["key", "value"]]);
const set = new Set([1, 2, 3]);
```

### WeakMaps and WeakSets:

Store objects with weak references, allowing garbage collection.

---

## 7. Enhanced Object Literals and Spread/Rest Operators

### Object Enhancements:

```javascript
const name = "Eve";
const person = {
  name,
  greet() {
    return `Hi, ${name}!`;
  },
};
```

### Spread/Rest:

```javascript
const arr = [1, 2, 3];
const newArr = [...arr, 4]; // [1, 2, 3, 4]

function sum(...nums) {
  return nums.reduce((a, b) => a + b, 0);
}
```

## 9. Iterators, Iterables, and the `for...of` Loop

Iterators allow custom iteration logic, and the `for...of` loop simplifies working with iterable objects like arrays and strings.

```javascript
const arr = [10, 20, 30];
for (const num of arr) {
  console.log(num);
}
```

---

## 1. Optional Chaining (?.) and Nullish Coalescing (??)

### Optional Chaining:

Safely access nested properties without having to check each level manually.

```javascript
const user = {};
console.log(user?.profile?.name); // undefined
```

### Nullish Coalescing:

Provide a fallback for `null` or `undefined` values.

```javascript
const value = null;
console.log(value ?? "Default"); // Default
```

---

## 2. Promises: Promise.all(), Promise.race(), Promise.allSettled()

### Promise.all():

Waits for all promises to resolve or rejects if any promise fails.

```javascript
Promise.all([fetch("/api1"), fetch("/api2")])
  .then((results) => console.log(results))
  .catch((error) => console.error(error));
```

### Promise.race():

Returns the result of the first promise to settle (resolve or reject).

```javascript
Promise.race([fetch("/api1"), fetch("/api2")]).then((result) =>
  console.log(result)
);
```

### Promise.allSettled():

Returns a result for all promises regardless of whether they resolved or rejected.

```javascript
Promise.allSettled([fetch("/api1"), fetch("/api2")]).then((results) =>
  console.log(results)
);
```

---

## 3. Understanding Concurrency in JavaScript and Node.js

JavaScript employs an event-driven concurrency model, with the **event loop** and **task queues** ensuring non-blocking execution.

### Key Concepts:

- **Microtasks** (e.g., resolved Promises) are executed before macrotasks.
- **Macrotasks** (e.g., setTimeout, setInterval) are processed after the current execution context.

In Node.js, the event loop integrates with I/O operations to handle concurrency effectively.

---

## 4. BigInt and Meta-programming with Proxy/Reflect

### BigInt:

Support for arbitrary-precision integers.

```javascript
const largeNumber = 12345678901234567890n;
console.log(largeNumber * 2n);
```

## 10. Proxies and Reflect API

### Proxies:

Intercept and redefine operations on objects.

```javascript
const handler = {
  get: (obj, prop) => (prop in obj ? obj[prop] : "Not Found"),
};
const proxy = new Proxy({ name: "Proxy" }, handler);
console.log(proxy.name, proxy.age); // Proxy, Not Found
```

### Reflect API:

Provide methods for object manipulation.

```javascript
Reflect.set(obj, "prop", value);
```

---

## 5. Internationalization (Intl)

The `Intl` object provides tools for formatting dates, numbers, and strings according to locale-specific conventions.

### Examples:

#### Date Formatting:

```javascript
const date = new Date();
const formatter = new Intl.DateTimeFormat("en-US", { dateStyle: "long" });
console.log(formatter.format(date));
```

#### Number Formatting:

```javascript
const number = 1234567.89;
const numberFormatter = new Intl.NumberFormat("de-DE", {
  style: "currency",
  currency: "EUR",
});
console.log(numberFormatter.format(number)); // 1.234.567,89 €
```

---

Modern JavaScript features enhance productivity, reduce boilerplate, and provide powerful new tools for development. By mastering these features, you can write cleaner, more efficient, and future-proof code.
