---

[<< Chapter 7](./07_functions.md) | [Chapter 9 >>](./09_error_handling_and_debugging.md)

---

# Chapter 8: Advanced Functions

In this chapter, we delve into more sophisticated aspects of JavaScript functions, enabling you to write more powerful and optimized code.

---

## **1. Closures and Encapsulation**

A closure is created when a function retains access to its outer scope, even after the outer function has executed. Closures are commonly used for data encapsulation and private variables.

### **Example**:

```javascript
// Example 1
function outerFunction(outerVariable) {
  return function innerFunction(innerVariable) {
    console.log(`Outer: ${outerVariable}, Inner: ${innerVariable}`);
  };
}

const newFunction = outerFunction("outside");
newFunction("inside");
// Output: Outer: outside, Inner: inside
// Example 2
function createCounter() {
  let count = 0;
  return function () {
    count++;
    return count;
  };
}

const counter = createCounter();
console.log(counter()); // Output: 1
console.log(counter()); // Output: 2
```

Closures are commonly used for:

- Data encapsulation
- Function factories
- Maintaining state in asynchronous operations

---

## **2. Recursion**

Recursion occurs when a function calls itself, allowing tasks to be broken into smaller, repeatable steps.

### **Example**:

```javascript
function factorial(n) {
  if (n === 0) return 1;
  return n * factorial(n - 1);
}

console.log(factorial(5)); // Output: 120
```

### **Use Cases**:

- Solving mathematical problems (e.g., factorial, Fibonacci)
- Traversing data structures (e.g., trees, graphs)

---

## **3. Function Currying**

Currying transforms a function with multiple arguments into a series of functions that each take a single argument.

### **Example**:

```javascript
function multiply(a) {
  return function (b) {
    return a * b;
  };
}

const multiplyByTwo = multiply(2);
console.log(multiplyByTwo(5)); // Output: 10
```

---

## **4. Function Composition**

Function composition is the process of combining two or more functions to produce a new function.

### **Example**:

```javascript
const add = (x) => x + 1;
const multiply = (x) => x * 2;

const compose = (f, g) => (x) => f(g(x));
const addThenMultiply = compose(multiply, add);

console.log(addThenMultiply(5)); // Output: 12
```

---

## **5. Asynchronous Functions**

Asynchronous functions allow non-blocking operations, crucial for tasks like fetching data or working with timers.

### **Promises**

```javascript
const fetchData = () => {
  return new Promise((resolve, reject) => {
    setTimeout(() => resolve("Data fetched"), 2000);
  });
};

fetchData().then((data) => console.log(data)); // Output: Data fetched
```

### **Async/Await**

```javascript
async function fetchDataAsync() {
  const data = await fetchData();
  console.log(data);
}

fetchDataAsync();
```

---

## **6. Memoization**

Memoization is an optimization technique to store the results of expensive function calls and return the cached result for the same inputs.

### **Example**:

```javascript
function memoize(fn) {
  const cache = {};
  return function (...args) {
    const key = JSON.stringify(args);
    if (cache[key]) return cache[key];
    const result = fn(...args);
    cache[key] = result;
    return result;
  };
}

const slowFunction = (n) => {
  console.log("Computing...");
  return n * 2;
};

const fastFunction = memoize(slowFunction);
console.log(fastFunction(5)); // Computing... Output: 10
console.log(fastFunction(5)); // Output: 10 (from cache)
```

---

## **7. `this` Keyword in Different Contexts**

The value of `this` in JavaScript depends on how a function is called.

### **Global Context**:

In the global scope, `this` refers to the global object (`window` in browsers).

```javascript
console.log(this); // Output: Window (in browsers)
```

### **Object Context**:

When a function is called as a method of an object, `this` refers to that object.

```javascript
const obj = {
  value: 42,
  getValue() {
    return this.value;
  },
};

console.log(obj.getValue()); // Output: 42
```

### **Arrow Functions**:

Arrow functions do not have their own `this` context and inherit it from their surrounding scope.

```javascript
const obj = {
  value: 42,
  getArrowFunction: () => this.value,
};

console.log(obj.getArrowFunction()); // Output: undefined (in browsers)
```

---

## **8. Function Bindings (`bind`, `call`, `apply`)**

### **`bind` Method**:

Creates a new function with `this` bound to the specified object.

```javascript
const obj = { value: 42 };
function getValue() {
  return this.value;
}

const boundGetValue = getValue.bind(obj);
console.log(boundGetValue()); // Output: 42
```

### **`call` Method**:

Calls a function with a specified `this` value and arguments provided individually.

```javascript
const obj = { value: 42 };
function getValue(arg) {
  return `${arg}: ${this.value}`;
}

console.log(getValue.call(obj, "Value")); // Output: Value: 42
```

### **`apply` Method**:

Similar to `call`, but arguments are provided as an array.

```javascript
console.log(getValue.apply(obj, ["Value"])); // Output: Value: 42
```

---

## **9. Iterators and Iterables**

JavaScript supports the iterable protocol via `Symbol.iterator`. Custom iterables can be consumed by `for...of`, spread syntax, and `Array.from`.

```javascript
const range = (start, end) => ({
  [Symbol.iterator]() {
    let i = start;
    return {
      next() {
        return i <= end ? { value: i++, done: false } : { done: true };
      },
    };
  },
});
for (const n of range(3, 5)) console.log(n); // 3,4,5
```

---

## **10. Advanced Generators and Async Generators**

Generators can delegate with `yield*`, receive values, and signal completion via `return`. Async generators work with `for await...of`.

```javascript
function* child() {
  yield 2;
  return 3;
}
function* parent() {
  const x = yield 1; // receive value from next()
  const y = yield* child(); // delegate to child generator
  yield x + y; // 10 + 3 => 13
}
const it = parent();
console.log(it.next()); // { value: 1, done: false }
console.log(it.next(10)); // { value: 2, done: false }
console.log(it.next()); // { value: 13, done: false }
console.log(it.next()); // { value: undefined, done: true }
```

```javascript
// Async generator and for-await-of
async function* stream(ids) {
  for (const id of ids) {
    const res = await fetch(`/api/${id}`);
    yield res.json();
  }
}
// for await (const item of stream([1, 2, 3])) {
//   console.log(item);
// }
```

---

## **11. Promise Utilities and Concurrency Patterns**

Use built-in combinators for parallelism and robust error handling. Limit concurrency to avoid resource spikes.

```javascript
// Built-ins
await Promise.all([a(), b()]);
await Promise.allSettled([a(), b()]);
await Promise.race([a(), b()]);
await Promise.any([a(), b()]); // first fulfillment

// Concurrency pool
async function runPool(fns, limit = 3) {
  const results = [];
  const executing = new Set();
  for (const fn of fns) {
    const p = Promise.resolve()
      .then(fn)
      .then((r) => {
        executing.delete(p);
        return r;
      });
    results.push(p);
    executing.add(p);
    if (executing.size >= limit) await Promise.race(executing);
  }
  return Promise.all(results);
}
```

---

## **12. Cancellation with AbortController**

Use `AbortController` to cancel fetches and pass `AbortSignal` through your async APIs.

```javascript
const ac = new AbortController();
setTimeout(() => ac.abort(), 200);
try {
  await fetch("/slow", { signal: ac.signal });
} catch (e) {
  if (e.name === "AbortError") console.log("Cancelled");
}
```

---

## **13. Enhanced Memoization Strategies**

Prefer `Map`/`WeakMap` for object keys and consider multi-arg keys and invalidation.

```javascript
function memoize(fn) {
  const primitiveCache = new Map();
  const objectCache = new WeakMap();
  return function memoized(...args) {
    if (args.length === 1 && typeof args[0] === "object" && args[0] !== null) {
      if (objectCache.has(args[0])) return objectCache.get(args[0]);
      const res = fn(...args);
      objectCache.set(args[0], res);
      return res;
    }
    const key = JSON.stringify(args);
    if (primitiveCache.has(key)) return primitiveCache.get(key);
    const res = fn(...args);
    primitiveCache.set(key, res);
    return res;
  };
}
```

---

## **14. Recursion Safety (Iterative/Trampoline)**

Tail-call optimization is not generally available; prefer iterative solutions or trampolines for deep recursion.

```javascript
function factorialIter(n) {
  let res = 1;
  for (let i = 2; i <= n; i++) res *= i;
  return res;
}
```

---

## **15. Functional Utilities: Pipe/Compose and Partial Application**

```javascript
const pipe =
  (...fns) =>
  (x) =>
    fns.reduce((v, f) => f(v), x);
const compose =
  (...fns) =>
  (x) =>
    fns.reduceRight((v, f) => f(v), x);

const add = (a, b) => a + b;
const add5 = add.bind(null, 5); // simple partial via bind
console.log(
  pipe(
    (x) => x * 2,
    (x) => x + 1
  )(3)
); // 7
```

---

## **16. Debounce and Throttle**

Control the rate of function execution for performance-sensitive events.

```javascript
const debounce = (fn, ms) => {
  let t;
  return (...args) => {
    clearTimeout(t);
    t = setTimeout(() => fn(...args), ms);
  };
};
const throttle = (fn, ms) => {
  let last = 0,
    t;
  return (...args) => {
    const now = Date.now();
    if (now - last >= ms) {
      last = now;
      fn(...args);
    } else {
      clearTimeout(t);
      t = setTimeout(() => {
        last = Date.now();
        fn(...args);
      }, ms - (now - last));
    }
  };
};
```

---

## **Conclusion**

Understanding advanced function concepts like closures, recursion, currying, the `this` keyword, and function bindings is vital for writing dynamic and maintainable JavaScript code. These techniques enhance your ability to create complex, reusable functions that adapt to various contexts.

---

[<< Chapter 7](./07_functions.md) | [Chapter 9 >>](./09_error_handling_and_debugging.md)

---
