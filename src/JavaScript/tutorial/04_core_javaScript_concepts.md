# Chapter 4: Core JavaScript Concepts

## **1. Syntax, Expressions, and Statements**

JavaScript syntax refers to the set of rules for writing code that the JavaScript engine can interpret. Understanding expressions and statements is essential for writing meaningful and functional JavaScript programs.

### **Syntax Basics**:

- **Case Sensitivity**: JavaScript is case-sensitive (`var` and `Var` are different).
- **Whitespace**: Ignored but used for readability.
- **Blocks**: Enclosed in curly braces `{}`.

### **Expressions**:

- Produces a value.
- Example:
  ```javascript
  let sum = 5 + 3; // Expression producing 8
  ```

### **Statements**:

- Executes actions and ends with a semicolon (`;`).
- Example:
  ```javascript
  console.log("Hello, World!"); // Statement
  ```

---

## **2. Variables (var, let)**

Variables store data values and allow them to be reused throughout a program.

### **Using `var`**:

- Function-scoped (accessible within the function where it is declared).
- Can be redeclared and updated.
- Example:
  ```javascript
  var name = "Alice";
  name = "Bob";
  ```

### **Using `let`**:

- Block-scoped (accessible only within the block where it is declared).
- Cannot be redeclared in the same scope, but can be updated.
- Example:
  ```javascript
  let age = 25;
  age = 30;
  ```

### **Differences Between `var` and `let`**:

| Feature           | `var`               | `let`                        |
| ----------------- | ------------------- | ---------------------------- |
| Scope             | Function            | Block                        |
| Redeclaration     | Allowed             | Not Allowed                  |
| Hoisting Behavior | Hoisted (undefined) | Hoisted (temporal dead zone) |

---

## **3. Constants and Immutable Values**

Constants are variables that cannot be reassigned after their initial declaration.

### **Using `const`**:

- Block-scoped, like `let`.
- Must be initialized during declaration.
- Cannot be updated or redeclared.
- Example:
  ```javascript
  const PI = 3.14;
  // PI = 3.15; // Error: Assignment to constant variable.
  ```

### **Immutable Values**:

- Primitive values (like numbers, strings) are inherently immutable.
- Objects and arrays can have their properties modified, even if declared with `const`.
  ```javascript
  const user = { name: "Alice" };
  user.name = "Bob"; // Allowed
  ```

---

## **4. Operators**

JavaScript provides a variety of operators to perform operations on data.

### **Assignment Operators**:

- Assign values to variables.
- Example:
  ```javascript
  let x = 10;
  x += 5; // x = x + 5
  ```

### **Arithmetic Operators**:

- Perform mathematical calculations.
- Examples:
  ```javascript
  let sum = 5 + 3; // Addition
  let diff = 10 - 2; // Subtraction
  let prod = 4 * 3; // Multiplication
  let div = 12 / 3; // Division
  let mod = 10 % 3; // Modulus (remainder)
  ```

### **Logical Operators**:

- Combine boolean values.
- Examples:
  ```javascript
  let isAdult = true && false; // Logical AND
  let canDrive = true || false; // Logical OR
  let notEligible = !true; // Logical NOT
  ```

### **Comparison Operators**:

- Compare values.
- Examples:
  ```javascript
  let isEqual = 5 == "5"; // true (loose equality)
  let isStrictEqual = 5 === "5"; // false (strict equality)
  let isGreater = 10 > 5; // true
  ```

### **Bitwise Operators**:

- Perform operations at the binary level.
- Examples:
  ```javascript
  let andResult = 5 & 1; // AND
  let orResult = 5 | 1; // OR
  let xorResult = 5 ^ 1; // XOR
  ```

---

## **5. Comments and Semicolons**

Comments improve code readability and help explain what the code does.

### **Types of Comments**:

- **Single-Line Comments**:

  ```javascript
  // This is a single-line comment
  console.log("Single-line comment example");
  ```

- **Multi-Line Comments**:
  ```javascript
  /*
  This is a
  multi-line comment
  */
  console.log("Multi-line comment example");
  ```

### **Semicolons**:

- Semicolons terminate statements in JavaScript.
- Optional in most cases but recommended for clarity and to avoid errors.
- Example:
  ```javascript
  let name = "Alice";
  console.log(name);
  ```

---

## **Conclusion**

Understanding JavaScript syntax, variables, constants, operators, and commenting practices is fundamental for writing clean and efficient code. Mastering these core concepts provides a strong foundation for tackling more advanced topics in JavaScript programming.
