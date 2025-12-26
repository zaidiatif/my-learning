---

[<< Chapter 5](./05_data_types_in_javaScript.md) | [Chapter 7 >>](./07_functions.md)

---

# Chapter 6: Control Flow Structures

Control flow structures in JavaScript allow developers to determine the flow of program execution based on conditions and loops. These structures are fundamental to building dynamic and interactive applications.

---

## **1. Conditional Statements**

### **if Statement**

- Executes a block of code if a specified condition evaluates to `true`.
- Example:
  ```javascript
  let age = 18;
  if (age >= 18) {
    console.log("You are eligible to vote.");
  }
  ```

### **if...else Statement**

- Provides an alternative block of code if the condition evaluates to `false`.
- Example:
  ```javascript
  let age = 16;
  if (age >= 18) {
    console.log("You are eligible to vote.");
  } else {
    console.log("You are not eligible to vote.");
  }
  ```

### **if...else if...else Statement**

- Used to test multiple conditions.
- Example:
  ```javascript
  let score = 85;
  if (score >= 90) {
    console.log("Grade: A");
  } else if (score >= 75) {
    console.log("Grade: B");
  } else {
    console.log("Grade: C");
  }
  ```

### **Switch Statement**

- Evaluates an expression and matches the value to a case.
- Example:
  ```javascript
  let day = 3;
  switch (day) {
    case 1:
      console.log("Monday");
      break;
    case 2:
      console.log("Tuesday");
      break;
    case 3:
      console.log("Wednesday");
      break;
    default:
      console.log("Invalid day");
  }
  ```

### **Ternary Operator**

- A compact alternative to `if...else` for simple conditions.
- Syntax:
  ```javascript
  condition ? expressionIfTrue : expressionIfFalse;
  ```
- Example:
  ```javascript
  let age = 18;
  let eligibility = age >= 18 ? "Eligible" : "Not Eligible";
  console.log(eligibility);
  ```

---

## **2. Loops**

### **for Loop**

- Executes a block of code a specific number of times.
- Example:
  ```javascript
  for (let i = 0; i < 5; i++) {
    console.log(i);
  }
  ```

### **while Loop**

- Executes a block of code as long as the condition evaluates to `true`.
- Example:
  ```javascript
  let i = 0;
  while (i < 5) {
    console.log(i);
    i++;
  }
  ```

### **do...while Loop**

- Executes the code block at least once before checking the condition.
- Example:
  ```javascript
  let i = 0;
  do {
    console.log(i);
    i++;
  } while (i < 5);
  ```

### **for...in Loop**

- Iterates over the properties of an object.
- Example:
  ```javascript
  let person = { name: "Alice", age: 25 };
  for (let key in person) {
    console.log(`${key}: ${person[key]}`);
  }
  ```

### **for...of Loop**

- Iterates over iterable objects like arrays, strings, or maps.
- Example:
  ```javascript
  let numbers = [1, 2, 3];
  for (let number of numbers) {
    console.log(number);
  }
  ```

---

## **3. Control Flow Interruptions**

### **break Statement**

- Exits a loop or switch statement prematurely.
- Example:
  ```javascript
  for (let i = 0; i < 10; i++) {
    if (i === 5) break;
    console.log(i);
  }
  ```

### **continue Statement**

- Skips the current iteration and continues with the next one.
- Example:
  ```javascript
  for (let i = 0; i < 10; i++) {
    if (i % 2 === 0) continue;
    console.log(i);
  }
  ```

### **Labeled Statements**

- Allows naming a loop or block of code for reference.
- Example:
  ```javascript
  outerLoop: for (let i = 0; i < 3; i++) {
    for (let j = 0; j < 3; j++) {
      if (i === j) continue outerLoop;
      console.log(`i = ${i}, j = ${j}`);
    }
  }
  ```

### **return Statement**

- Exits from a function and optionally returns a value.
- Example:
  ```javascript
  function square(num) {
    return num * num;
  }
  console.log(square(4)); // Output: 16
  ```

---

## **4. Advanced Control Flow Topics**

### **Truthy/Falsy and Strict Equality**

- JavaScript treats certain values as falsy: `false`, `0`, `-0`, `0n`, `""`, `null`, `undefined`, and `NaN`. Everything else is truthy.
- Prefer `===` and `!==` over `==` and `!=` to avoid implicit type coercion.
- Example:
  ```javascript
  if ("0") console.log("truthy"); // strings are truthy
  console.log(0 == false); // true (coercion)
  console.log(0 === false); // false (no coercion)
  ```

### **Switch Fallthrough and Grouping Cases**

- You can intentionally group cases by omitting `break` to handle multiple values identically.
- Example:
  ```javascript
  const code = "B";
  switch (code) {
    case "A":
    case "B":
      console.log("Group A/B");
      break;
    case "C":
      console.log("C");
      break;
    default:
      console.log("Unknown");
  }
  ```

### **Nullish Coalescing (`??`) vs Logical OR (`||`)**

- `||` falls back on any falsy value; `??` falls back only on `null` or `undefined`.
- Example:
  ```javascript
  const count = 0;
  const withOr = count || 10; // 10 (0 is falsy)
  const withNullish = count ?? 10; // 0 (only null/undefined trigger fallback)
  ```

### **Looping Arrays and Objects Correctly**

- Use `for...of` for arrays and other iterables; use `for...in` for object keys.
- When using `for...in`, guard with `Object.prototype.hasOwnProperty.call` to skip inherited keys.
- Utilities: `Object.keys`, `Object.values`, `Object.entries`.
- Examples:

  ```javascript
  const arr = [1, 2, 3];
  for (const n of arr) console.log(n); // good for arrays

  const obj = { a: 1, b: 2 };
  for (const key in obj) {
    if (Object.prototype.hasOwnProperty.call(obj, key)) {
      console.log(key, obj[key]);
    }
  }
  // or:
  Object.entries(obj).forEach(([k, v]) => console.log(k, v));
  ```

### **Async/Await in Loops**

- `for...of` works well with `await` for sequential async operations.
- Avoid relying on `Array.prototype.forEach` with `async` callbacks because the outer function won’t await them.
- Example:
  ```javascript
  const ids = [1, 2, 3];
  for (const id of ids) {
    const data = await fetchItem(id); // sequential and awaited
    console.log(data);
  }
  // Avoid: ids.forEach(async id => await fetchItem(id)); // forEach doesn't await
  ```

### **Error Handling as Control Flow**

- Use `try...catch...finally` and `throw` for exceptional conditions, not for normal branching.
- Example:
  ```javascript
  function parseNumber(s) {
    const n = Number(s);
    if (Number.isNaN(n)) throw new Error("Invalid number");
    return n;
  }
  try {
    const n = parseNumber("abc");
  } catch (e) {
    console.error(e.message);
  } finally {
    console.log("Done");
  }
  ```

### **Infinite Loop Safeguards**

- Always ensure loops have a clear termination condition and that relevant state changes inside the loop.
- Example:
  ```javascript
  let attempts = 0;
  while (true) {
    if (++attempts > 3) break; // guard to prevent infinite loop
  }
  ```

### **Guidance on Labeled Statements**

- Labels can be useful but are rarely necessary. Prefer refactoring into functions or breaking complex loops.

---

## **Conclusion**

Control flow structures form the backbone of decision-making and repetitive operations in JavaScript. By mastering conditional statements, loops, and control flow interruptions, you can create dynamic, efficient, and logical code.

---

[<< Chapter 5](./05_data_types_in_javaScript.md) | [Chapter 7 >>](./07_functions.md)

---
