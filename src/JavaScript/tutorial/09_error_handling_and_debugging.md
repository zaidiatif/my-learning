---

[<< Chapter 8](./08_advanced_functions.md) | [Chapter 10 >>](./10_working_with_objects_and_arrays.md)

---

# Chapter 9: Error Handling and Debugging

In this chapter, we explore techniques for managing errors and debugging JavaScript code effectively.

---

## **1. Types of Errors**

JavaScript errors can be categorized into three main types:

### **1.1 Syntax Errors**:

Occur when the JavaScript parser encounters invalid syntax.

```javascript
console.log("Hello World" // Missing closing parenthesis
```

### **1.2 Runtime Errors**:

Occur during code execution, often due to invalid operations.

```javascript
console.log(nonExistentVariable); // ReferenceError: nonExistentVariable is not defined
```

### **1.3 Logical Errors**:

Occur when the code runs without errors but produces incorrect results.

```javascript
const sum = (a, b) => a - b; // Incorrect operator
console.log(sum(5, 3)); // Output: 2 (expected: 8)
```

---

## **2. Error Handling**

### **2.1 Using `try...catch`**:

The `try...catch` block is used to handle runtime errors gracefully.

```javascript
try {
  console.log(nonExistentVariable);
} catch (error) {
  console.error("An error occurred:", error.message);
}
```

### **2.2 Throwing Custom Errors**:

You can throw custom errors using the `throw` statement.

```javascript
function divide(a, b) {
  if (b === 0) {
    throw new Error("Division by zero is not allowed.");
  }
  return a / b;
}

try {
  console.log(divide(10, 0));
} catch (error) {
  console.error(error.message);
}
```

### **2.3 `finally` Block**:

The `finally` block executes regardless of whether an error was thrown or not.

```javascript
try {
  console.log("Trying something risky...");
} catch (error) {
  console.error("An error occurred.");
} finally {
  console.log("This will always run.");
}
```

---

## **3. Debugging Tools and Techniques**

### **3.1 Using `console` Methods**:

- **`console.log()`**: Logs general information.
- **`console.error()`**: Logs errors.
- **`console.warn()`**: Logs warnings.
- **`console.table()`**: Displays tabular data.

### **Example**:

```javascript
const data = [
  { name: "Alice", age: 25 },
  { name: "Bob", age: 30 },
];
console.table(data);
```

### **3.2 Breakpoints**:

Set breakpoints in your code to pause execution at specific lines and inspect program state.

### **3.3 Inspecting Variables**:

Use developer tools to monitor variables and check their values during execution.

### **3.4 Watch Expressions**:

Watch expressions in developer tools allow you to track specific variables or expressions in real-time.

### **3.5 Debugging Async Code**:

Use asynchronous debugging features in developer tools, such as async stack traces, to debug promises and async/await code effectively.

### **3.6 Debugger Statement**:

The `debugger` statement pauses code execution, allowing inspection in developer tools.

```javascript
function testDebugger() {
  const x = 10;
  debugger; // Execution pauses here
  console.log(x);
}

testDebugger();
```

---

### **3.7 Linting Tools**:

Use linting tools like ESLint to catch syntax and logical errors early.

---

## **4. Best Practices for Error Handling**

1. **Validate Input**: Always validate user input to avoid unexpected errors.

   ```javascript
   function greet(name) {
     if (typeof name !== "string") {
       throw new TypeError("Name must be a string.");
     }
     return `Hello, ${name}!`;
   }
   ```

2. **Use Meaningful Error Messages**: Provide clear and specific error messages to help with debugging.

3. **Graceful Degradation**: Ensure your application continues to function despite minor errors.

4. **Log Errors**: Use logging mechanisms to track and analyze errors in production environments.

---

## **5. Advanced Error Handling**

### **5.1 Async/Await and Promise Rejections**

- Wrap `await` calls with `try...catch` and handle non-OK responses.
- Always handle promise rejections with `.catch` or `try...catch`.
- Example:
  ```javascript
  async function fetchJson(url) {
    const res = await fetch(url);
    if (!res.ok) throw new Error(`HTTP ${res.status}`);
    return res.json();
  }
  try {
    const data = await fetchJson("/api/users");
    console.log(data);
  } catch (e) {
    console.error("Request failed:", e.message);
  }
  ```

### **5.2 Creating Custom Error Classes**

- Define semantic error types to distinguish operational cases.
- Example:
  ```javascript
  class ValidationError extends Error {
    constructor(message, field) {
      super(message);
      this.name = "ValidationError";
      this.field = field;
    }
  }
  function createUser(input) {
    if (typeof input.name !== "string") {
      throw new ValidationError("Name must be a string", "name");
    }
    return { id: 1, ...input };
  }
  ```

### **5.3 Retries with Backoff and Cancellation**

- Use exponential backoff for transient failures and `AbortController` to cancel.
- Example:
  ```javascript
  async function withRetry(fn, { retries = 3, baseMs = 200, signal } = {}) {
    let attempt = 0;
    for (;;) {
      try {
        return await fn({ signal });
      } catch (e) {
        if (attempt++ >= retries || (signal && signal.aborted)) throw e;
        const delay = baseMs * 2 ** (attempt - 1);
        await new Promise((r, rej) => {
          const t = setTimeout(r, delay);
          signal?.addEventListener(
            "abort",
            () => {
              clearTimeout(t);
              rej(new DOMException("Aborted", "AbortError"));
            },
            { once: true }
          );
        });
      }
    }
  }
  ```

### **5.4 Do Not Swallow Errors**

- Avoid empty `catch` blocks; log or rethrow as appropriate.
- Keep `try` blocks small and specific; rethrow unexpected errors.

---

## **6. Global Error Hooks**

### **6.1 Browser**

- Handle uncaught exceptions and unhandled promise rejections for telemetry.
  ```javascript
  window.addEventListener("error", (e) => {
    console.error("Uncaught error:", e.message, e.filename, e.lineno);
  });
  window.addEventListener("unhandledrejection", (e) => {
    console.error("Unhandled rejection:", e.reason);
  });
  ```

### **6.2 Node.js**

- Log and fail fast for uncaught exceptions; decide policy for unhandled rejections.
  ```javascript
  process.on("uncaughtException", (err) => {
    console.error("Uncaught exception", err);
    process.exit(1);
  });
  process.on("unhandledRejection", (reason) => {
    console.error("Unhandled rejection", reason);
  });
  ```

---

## **7. Debugging Tips**

- Use source maps to map minified code back to sources.
- Structure logs with `console.group`/`groupEnd` and add context.
- Add performance markers for timing critical paths.
- Example:
  ```javascript
  console.group("User fetch");
  console.time("fetch-users");
  try {
    const users = await fetchJson("/api/users");
    console.table(users.slice(0, 3));
  } finally {
    console.timeEnd("fetch-users");
    console.groupEnd();
  }
  ```

---

## **Conclusion**

Mastering error handling and debugging techniques is essential for building robust and reliable JavaScript applications. By understanding error types, using debugging tools, and following best practices, developers can identify and fix issues efficiently.

---

[<< Chapter 8](./08_advanced_functions.md) | [Chapter 10 >>](./10_working_with_objects_and_arrays.md)

---
