# Chapter 22: Modern JavaScript Features

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

## 5.1 Top-level await (ESM)

- In ES modules, `await` is allowed at the top level.
```javascript
// module.mjs
const data = await fetch('/data.json').then(r => r.json());
export { data };
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

## 7. Class Fields, Private Members, and Static Blocks

- Public fields, `#private` fields/methods, and `static {}` initialization.
```javascript
class Counter {
  #count = 0;
  static registry = new Map();
  static { Counter.registry.set('default', new Counter()); }
  inc() { this.#count++; }
  get value() { return this.#count; }
}
```

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

## 8. Logical Assignment Operators and Nullish Coalescing

- `&&=` assign if truthy, `||=` assign if falsy, `??=` assign if nullish.
```javascript
obj.enabled ||= true;       // set if falsy
opts.timeout ??= 500;       // set only if null/undefined
state.ready &&= checkReady();
```

---

## 9. Iterators, Iterables, and the `for...of` Loop

Iterators allow custom iteration logic, and the `for...of` loop simplifies working with iterable objects like arrays and strings.

```javascript
const arr = [10, 20, 30];
for (const num of arr) {
  console.log(num);
}
```

---

## 10. Optional Chaining (?.) and Nullish Coalescing (??)

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

## 11. Promises: Promise.all(), Promise.race(), Promise.allSettled(), Promise.any()

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

### Promise.any():

Resolves with the first fulfillment; rejects with `AggregateError` if all reject.
```javascript
Promise.any([fetch('/fast'), fetch('/slow')])
  .then(console.log)
  .catch((e) => console.error(e instanceof AggregateError));
```

---

## 12. Error Cause

- Attach a cause to errors for better debugging.
```javascript
try { doThing(); }
catch (e) { throw new Error('Failed to doThing', { cause: e }); }
```

---

## 13. Understanding Concurrency in JavaScript and Node.js

JavaScript employs an event-driven concurrency model, with the **event loop** and **task queues** ensuring non-blocking execution.

### Key Concepts:

- **Microtasks** (e.g., resolved Promises) are executed before macrotasks.
- **Macrotasks** (e.g., setTimeout, setInterval) are processed after the current execution context.

In Node.js, the event loop integrates with I/O operations to handle concurrency effectively.

---

## 14. BigInt and Meta-programming with Proxy/Reflect

### BigInt:

Support for arbitrary-precision integers.

```javascript
const largeNumber = 12345678901234567890n;
console.log(largeNumber * 2n);
```

## 15. Proxies and Reflect API

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

## 16. Useful Additions and APIs

### Numeric Separators:
```javascript
const million = 1_000_000;
```

### `globalThis`:
```javascript
console.log(globalThis === window || globalThis === global); // true in respective envs
```

### Array and Object Helpers:
```javascript
arr.at(-1);                 // last element
arr.flatMap(x => [x, x]);   // flat + map
Object.fromEntries([['a',1]]);
Object.hasOwn(obj, 'prop');
```

### String Helpers:
```javascript
'a-b-c'.replaceAll('-', '_');
for (const m of 'a1b2'.matchAll(/(\w)(\d)/g)) console.log(m[0]);
```

### Import Assertions and JSON Modules (ESM):
```javascript
import data from './data.json' assert { type: 'json' };
```

---

## 17. Internationalization (Intl)

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

### RelativeTimeFormat, ListFormat, Segmenter:
```javascript
new Intl.RelativeTimeFormat('en', { numeric: 'auto' }).format(-1, 'day'); // yesterday
new Intl.ListFormat('en', { style: 'short', type: 'conjunction' }).format(['a','b','c']);
const seg = new Intl.Segmenter('en', { granularity: 'word' });
for (const s of seg.segment('Hello world')) console.log(s.segment);
```

---

Modern JavaScript features enhance productivity, reduce boilerplate, and provide powerful new tools for development. By mastering these features, you can write cleaner, more efficient, and future-proof code.
