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

## **Conclusion**

Control flow structures form the backbone of decision-making and repetitive operations in JavaScript. By mastering conditional statements, loops, and control flow interruptions, you can create dynamic, efficient, and logical code.
