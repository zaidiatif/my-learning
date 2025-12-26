---

[<< Chapter 11](./11_advanced_javaScript_concepts.md) | [Chapter 13 >>](./13_dom_manipulation.md)

---

# Chapter 12: Asynchronous JavaScript

In this chapter, we explore the core concepts of asynchronous programming in JavaScript and the tools available to handle asynchronous operations effectively.

---

## **1. Introduction to Asynchronous Programming**

JavaScript is a single-threaded, non-blocking language that uses an event loop to handle asynchronous tasks. Asynchronous programming allows multiple tasks to run independently without waiting for others to complete.

### **1.1 Synchronous vs. Asynchronous**:

- **Synchronous**: Code executes line by line, blocking subsequent operations until the current task is completed.
- **Asynchronous**: Code execution doesn't wait; tasks are delegated to the event loop, allowing the program to continue running other operations.

---

## **2. Callbacks and Callback Hell**

Callbacks are functions passed as arguments to other functions and executed after an asynchronous operation completes.

### **2.1 Example**:

```javascript
function fetchData(callback) {
  setTimeout(() => {
    console.log("Data fetched.");
    callback();
  }, 2000);
}

fetchData(() => {
  console.log("Callback executed.");
});
```

### **2.2 Callback Hell**:

Nested callbacks can lead to complex and unreadable code, known as "callback hell."

```javascript
setTimeout(() => {
  console.log("Step 1");
  setTimeout(() => {
    console.log("Step 2");
    setTimeout(() => {
      console.log("Step 3");
    }, 1000);
  }, 1000);
}, 1000);
```

---

## **3. Promises (then, catch, finally)**

Promises provide a cleaner way to handle asynchronous operations and avoid callback hell.

### **3.1 Creating a Promise**:

```javascript
const promise = new Promise((resolve, reject) => {
  const success = true;
  if (success) {
    resolve("Operation successful");
  } else {
    reject("Operation failed");
  }
});

promise
  .then((message) => console.log(message))
  .catch((error) => console.error(error))
  .finally(() => console.log("Promise settled"));
```

### **3.2 Chaining Promises**:

```javascript
fetch("https://api.example.com/data")
  .then((response) => response.json())
  .then((data) => console.log(data))
  .catch((error) => console.error(error))
  .finally(() => console.log("Fetch operation completed"));
```

---

## **4. async/await and Error Handling**

`async` and `await` provide a more readable way to work with asynchronous code, making it appear synchronous.

### **4.1 Using Async/Await**:

```javascript
async function fetchData() {
  try {
    const response = await fetch("https://api.example.com/data");
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error(error);
  }
}

fetchData();
```

### **4.2 Error Handling**:

Errors in `async` functions are caught using `try...catch`.

### **4.3 for await...of**:

Used to iterate over asynchronous iterables.

```javascript
async function processItems(items) {
  for await (const item of items) {
    console.log(item);
  }
}

const asyncIterable = {
  [Symbol.asyncIterator]() {
    let i = 0;
    return {
      next() {
        if (i < 3) {
          return Promise.resolve({ value: i++, done: false });
        }
        return Promise.resolve({ done: true });
      },
    };
  },
};

processItems(asyncIterable);
```

---

## **5. Handling Multiple Async Operations**

### **5.1 Parallel Execution with `Promise.all`**:

Wait for multiple promises to resolve.

```javascript
const promise1 = Promise.resolve("First");
const promise2 = Promise.resolve("Second");

Promise.all([promise1, promise2])
  .then((values) => console.log(values)) // Output: ["First", "Second"]
  .catch((error) => console.error(error));
```

### **5.2 Handling Race Conditions with `Promise.race`**:

Returns the result of the first resolved/rejected promise.

```javascript
const promise1 = new Promise((resolve) => setTimeout(resolve, 1000, "One"));
const promise2 = new Promise((resolve) => setTimeout(resolve, 500, "Two"));

Promise.race([promise1, promise2])
  .then((value) => console.log(value)) // Output: "Two"
  .catch((error) => console.error(error));
```

---

## **6. setTimeout(), setInterval(), and Delays**

### **6.1 setTimeout**:

Schedules a task to run after a specified delay.

```javascript
setTimeout(() => {
  console.log("Executed after 2 seconds");
}, 2000);
```

### **6.2 setInterval**:

Runs a task repeatedly at specified intervals.

```javascript
const intervalId = setInterval(() => {
  console.log("Repeating task every 1 second");
}, 1000);

// Clear the interval after 5 seconds
setTimeout(() => clearInterval(intervalId), 5000);
```

---

## **7. Throttling, Debouncing Techniques, and requestAnimationFrame()**

### **7.1 Throttling**:

Limits the execution of a function to once every specified interval.

```javascript
function throttle(func, limit) {
  let lastFunc;
  let lastRan;
  return function (...args) {
    const context = this;
    if (!lastRan) {
      func.apply(context, args);
      lastRan = Date.now();
    } else {
      clearTimeout(lastFunc);
      lastFunc = setTimeout(() => {
        if (Date.now() - lastRan >= limit) {
          func.apply(context, args);
          lastRan = Date.now();
        }
      }, limit - (Date.now() - lastRan));
    }
  };
}
```

### **7.2 Debouncing**:

Ensures a function executes only after a specified time has elapsed since its last invocation.

```javascript
function debounce(func, delay) {
  let timeout;
  return function (...args) {
    clearTimeout(timeout);
    timeout = setTimeout(() => func.apply(this, args), delay);
  };
}
```

### **7.3 requestAnimationFrame**:

Optimizes animations by aligning them with the browser's refresh rate.

```javascript
function animate() {
  console.log("Animating frame...");
  requestAnimationFrame(animate);
}

requestAnimationFrame(animate);
```

---

## **8. The Event Loop**

The event loop is a mechanism that handles asynchronous operations in JavaScript.

### **8.1 How It Works**:

1. The **call stack** executes synchronous code.
2. The **web APIs** handle asynchronous tasks (e.g., `setTimeout`, `fetch`).
3. The **callback queue** queues tasks for execution.
4. The event loop moves tasks from the callback queue to the call stack when it's empty.

### **8.2 Example**:

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Timeout");
}, 0);

console.log("End");
// Output: Start -> End -> Timeout
```

---

## **9. Best Practices**

1. **Avoid Nested Callbacks**: Use promises or `async/await` instead.
2. **Error Handling**: Always handle errors in asynchronous code.
3. **Use Linting Tools**: Ensure consistent use of asynchronous constructs.
4. **Optimize Performance**: Use `Promise.all` for parallel execution when appropriate.
5. **Graceful Degradation**: Handle scenarios where asynchronous resources fail.

---

## **10. Promise Combinators and Patterns**

- `Promise.allSettled` waits for all outcomes; `Promise.any` fulfills on the first success.
- Build timeouts and races for resilience.

```javascript
const results = await Promise.allSettled([fetch(a), fetch(b)]);
const firstOk = await Promise.any([fetch("/a"), fetch("/b")]);

function withTimeout(promise, ms) {
  return Promise.race([
    promise,
    new Promise((_, rej) => setTimeout(() => rej(new Error("Timeout")), ms)),
  ]);
}
```

---

## **11. Concurrency Control (Pooling/Semaphore)**

Limit concurrent async work to avoid resource spikes.

```javascript
async function runPool(tasks, limit = 3) {
  const results = [];
  const executing = new Set();
  for (const task of tasks) {
    const p = Promise.resolve()
      .then(task)
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

## **12. Cancellation and Timeouts with AbortController**

Use `AbortController` to cancel fetches and propagate cancellation to your APIs.

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

## **13. Async Generators and Streams**

Produce values over time and consume with `for await...of`.

```javascript
async function* stream(ids) {
  for (const id of ids) {
    const res = await fetch(`/api/${id}`);
    yield res.json();
  }
}
// for await (const item of stream([1, 2, 3])) console.log(item);
```

---

## **14. Microtasks: queueMicrotask vs setTimeout**

Microtasks run before the next macrotask; useful for deferring but prioritizing work.

```javascript
console.log("A");
setTimeout(() => console.log("timeout"), 0); // macrotask
queueMicrotask(() => console.log("micro")); // microtask
console.log("B");
// Order: A, B, micro, timeout
```

---

## **15. Web Workers (Offloading CPU)**

Run heavy computations off the main thread.

```javascript
// main.js
const worker = new Worker("worker.js");
worker.postMessage({ n: 1e7 });
worker.onmessage = (e) => console.log("result", e.data);

// worker.js
self.onmessage = (e) => {
  const { n } = e.data;
  let sum = 0;
  for (let i = 0; i < n; i++) sum += i;
  self.postMessage(sum);
};
```

---

## **Conclusion**

Asynchronous programming is essential for creating responsive and efficient JavaScript applications. By mastering callbacks, promises, `async/await`, and understanding the event loop, developers can handle asynchronous operations effectively and write cleaner, more maintainable code.

---

[<< Chapter 11](./11_advanced_javaScript_concepts.md) | [Chapter 13 >>](./13_dom_manipulation.md)

---
