# Chapter 21: Event Loop and Concurrency

JavaScript's event loop is the cornerstone of its concurrency model. By managing asynchronous operations and ensuring non-blocking behavior, the event loop enables JavaScript to handle multiple tasks efficiently. This chapter delves into the mechanics of the event loop, its role in concurrency, and strategies for managing asynchronous code.

---

## 1. The JavaScript Concurrency Model

JavaScript operates on a single-threaded model, meaning it can execute only one task at a time. To manage multiple tasks efficiently, it uses an event-driven, non-blocking approach powered by the event loop.

### Key Features:

- **Single Threaded**: JavaScript executes code in a single call stack.
- **Asynchronous Execution**: Tasks that take time (e.g., I/O, timers) are deferred to avoid blocking the main thread.
- **Callbacks and Promises**: Enable handling of asynchronous operations.

---

## 2. Understanding the Event Loop

The event loop continuously checks for and processes tasks from different queues. It ensures asynchronous operations are executed without blocking the main thread, enabling JavaScript to remain responsive.

## 2.1. Anatomy of the Event Loop

The event loop continuously checks for and processes tasks from different queues. The main components include:

### Call Stack

Holds functions to be executed. Functions are added to the stack when called and removed once completed.

### Task Queue

Contains tasks (e.g., `setTimeout` callbacks) that are scheduled to run after the current stack is cleared.

### Microtask Queue

A higher-priority queue for tasks like Promise resolutions and `MutationObserver` callbacks. Microtasks are processed before moving to the task queue.

---

## 2.2. Browser vs Node.js Event Loop (High Level)

- Browser: tasks (macrotasks) → microtasks checkpoint → rendering → next task.
- Node.js (libuv): phases (timers, pending callbacks, idle/prepare, poll, check, close) with microtask checkpoints between phases; `process.nextTick` runs before other microtasks.

```javascript
// Node ordering (typical): nextTick > Promise microtask > setImmediate (check) > setTimeout(0) (timers)
setTimeout(() => console.log('timeout'), 0);
setImmediate(() => console.log('immediate'));
Promise.resolve().then(() => console.log('promise')); 
process.nextTick?.(() => console.log('nextTick'));
```

---

## 3. Execution Context and the Call Stack

The call stack keeps track of function calls and execution contexts. Each time a function is invoked, it is added to the stack. Once the function completes, it is removed from the stack. Understanding the stack is crucial for debugging and performance optimization.

---

## 4. Microtasks, Macrotasks and Task Queues

### Task Queues:

- **Macrotasks**: Include callbacks from `setTimeout`, `setInterval`, and I/O operations.
- **Microtasks**: Include resolved Promises and `queueMicrotask` tasks. Processed immediately after the current execution context.

### Example:

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Macrotask");
}, 0);

Promise.resolve().then(() => {
  console.log("Microtask");
});

console.log("End");
```

**Output:**

```
Start
End
Microtask
Macrotask
```

### Promises

Promises represent a value that may be available now, or in the future, or never. They simplify callback-based asynchronous code.

### Async/Await

Built on Promises, `async` and `await` provide a syntax resembling synchronous code for better readability and maintainability.

---

## 4.1 Microtask Utilities: queueMicrotask, MessageChannel, postMessage

- `queueMicrotask`: schedule after current turn, before next macrotask.
- `MessageChannel`/`postMessage`: create a macrotask without minimum timer clamping.

```javascript
queueMicrotask(() => console.log('microtask'));

const ch = new MessageChannel();
ch.port1.onmessage = () => console.log('macrotask via MessageChannel');
ch.port2.postMessage(null);
```

---

## 5. Non-blocking I/O and Asynchronous Execution

JavaScript's non-blocking I/O model ensures that I/O operations, such as reading files or making network requests, do not block the main thread. This is achieved using asynchronous APIs like callbacks, Promises, and `async/await`.

---

## 6. Potential Pitfalls in Concurrency

### Common Issues:

- **Callback Hell**: Nested callbacks lead to unreadable and hard-to-maintain code.
- **Blocking Code**: Long-running synchronous tasks block the event loop.
- **Uncaught Rejections**: Failing to handle Promise rejections can crash applications.

### Solutions:

- Use Promises or async/await to manage callbacks.
- Avoid blocking the event loop by offloading intensive tasks to Web Workers.
- Always handle Promise rejections with `.catch()` or `try/catch`.

---

## 7. Web Workers and Multithreading

Web Workers enable JavaScript to perform multithreaded operations by offloading heavy computations to separate threads. This keeps the main thread responsive.

### Features:

- Communication between the main thread and workers occurs via `postMessage` and message events.
- Workers do not have access to the DOM for security and performance reasons.

---

## 8. Shared Memory (SharedArrayBuffer, Atomics)

### SharedArrayBuffer:

Enables threads to share memory, allowing faster communication and data processing.

### Atomics:

Provides atomic operations to prevent race conditions when multiple threads access shared memory.

### Example:

```javascript
const sharedBuffer = new SharedArrayBuffer(1024);
const sharedArray = new Uint8Array(sharedBuffer);

Atomics.store(sharedArray, 0, 42);
console.log(Atomics.load(sharedArray, 0)); // 42
```

---

## 8.1 Atomics.wait/notify (Coordinating Workers)

```javascript
// Main
const buf = new SharedArrayBuffer(4);
const ia = new Int32Array(buf);
const w = new Worker('w.js');
w.postMessage(buf);
// signal work ready
Atomics.store(ia, 0, 1);
Atomics.notify(ia, 0, 1);

// w.js
onmessage = ({ data: buf }) => {
  const ia = new Int32Array(buf);
  // wait until value != 0
  while (Atomics.wait(ia, 0, 0) === 'ok') {
    // process...
    Atomics.store(ia, 0, 0);
  }
};
```

---

## 9. Debugging Event Loop Issues

### Tools:

- **Chrome DevTools**: Provides tools to monitor async operations and identify bottlenecks.
- **Node.js Inspector**: Offers debugging capabilities for server-side JavaScript.

### Tips:

- Log execution order to understand task scheduling.
- Use `Performance` APIs to measure delays and response times.

---

## 10. Rendering Ticks and Scheduling Work

- Use `requestAnimationFrame` for visual updates; `requestIdleCallback` for idle tasks.
- Break long tasks; yield via `setTimeout(0)` or `MessageChannel`.

```javascript
function chunk(items, fn) {
  let i = 0;
  function step() {
    const start = performance.now();
    while (i < items.length && performance.now() - start < 8) fn(items[i++]);
    if (i < items.length) setTimeout(step, 0);
  }
  step();
}
```

---

## 11. Detecting Long Tasks

```javascript
new PerformanceObserver((list) => {
  for (const e of list.getEntries()) console.warn('Long task', e.duration);
}).observe({ type: 'longtask', buffered: true });
```

---

## 12. Priority and Cooperative Scheduling (Patterns)

- Prioritize user input and rendering; defer low-priority work.
- Simple cooperative scheduler:
```javascript
const tasks = { high: [], normal: [], low: [] };
function schedule(priority, fn) { tasks[priority].push(fn); }
function run() {
  const q = tasks.high.length ? tasks.high : tasks.normal.length ? tasks.normal : tasks.low;
  if (!q.length) return;
  (q.shift())();
  queueMicrotask(run);
}
run();
```

---

By mastering these concepts, you can write more efficient, responsive, and robust JavaScript applications while effectively managing concurrency and leveraging multithreading.

The event loop is at the heart of JavaScript's ability to handle asynchronous operations. By mastering its mechanics, you can write more efficient and responsive applications, effectively managing concurrency and avoiding common pitfalls.
