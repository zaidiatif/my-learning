# Chapter 5: Data Types in JavaScript

JavaScript provides various data types to work with different kinds of values. Understanding these types is fundamental to programming in JavaScript.

---

## **1. Overview of Data Types**

JavaScript data types can be broadly classified into two categories:

### **Primitive Data Types**:

- **String**: Represents textual data.
- **Number**: Represents numerical values, including integers and floating-point numbers.
- **BigInt**: Represents integers of arbitrary size.
- **Boolean**: Represents true/false values.
- **Undefined**: Represents a variable that has been declared but not assigned a value.
- **Null**: Represents the intentional absence of any object value.
- **Symbol**: Represents a unique identifier.

### **Non-Primitive (Reference) Data Types**:

- **Object**: A collection of key-value pairs, including arrays and functions.

---

## **2. Primitive Data Types**

### **String**:

- Used to represent text.
- Can be defined using single quotes (`'`), double quotes (`"`), or backticks (`` ` ``).
- Example:
  ```javascript
  let name = "Alice";
  let greeting = `Hello, ${name}!`;
  console.log(greeting); // Output: Hello, Alice!
  ```

### **Number**:

- Includes integers and floating-point numbers.
- Example:
  ```javascript
  let age = 25;
  let price = 19.99;
  console.log(age, price);
  ```

### **BigInt**:

- For numbers beyond the safe integer limit (`2^53 - 1`).
- Example:
  ```javascript
  let largeNumber = BigInt(9007199254740991);
  console.log(largeNumber);
  ```

### **Boolean**:

- Represents `true` or `false`.
- Example:
  ```javascript
  let isAvailable = true;
  console.log(isAvailable); // Output: true
  ```

### **Undefined**:

- Default value of a variable that is declared but not initialized.
- Example:
  ```javascript
  let x;
  console.log(x); // Output: undefined
  ```

### **Null**:

- Explicitly represents no value.
- Example:
  ```javascript
  let result = null;
  console.log(result); // Output: null
  ```

### **Symbol**:

- Unique and immutable.
- Used for creating unique keys for objects.
- Example:
  ```javascript
  let id = Symbol("id");
  console.log(id);
  ```

---

## **3. Non-Primitive(Reference) Data Types**

### **Object**:

- A collection of properties, where each property is a key-value pair.
- Example:
  ```javascript
  let person = {
    name: "Alice",
    age: 25,
  };
  console.log(person.name); // Output: Alice
  ```

### **Array**:

- Special type of object used to store ordered collections.
- Example:
  ```javascript
  let numbers = [1, 2, 3, 4];
  console.log(numbers[2]); // Output: 3
  ```

### **Function**:

- A callable object.
- Example:
  ```javascript
  function greet() {
    console.log("Hello!");
  }
  greet();
  ```

### **Set**:

- A collection of unique values.
- Example:
  ```javascript
  let uniqueValues = new Set([1, 2, 3, 3]);
  console.log(uniqueValues); // Output: Set { 1, 2, 3 }
  ```

### **Map**:

- A collection of key-value pairs where keys can be of any type.
- Example:
  ```javascript
  let map = new Map();
  map.set("name", "Alice");
  console.log(map.get("name")); // Output: Alice
  ```

### **WeakSet**:

- Similar to Set but holds weak references to objects.
- Example:
  ```javascript
  let obj = { name: "Alice" };
  let weakSet = new WeakSet([obj]);
  console.log(weakSet.has(obj)); // Output: true
  ```

### **WeakMap**:

- Similar to Map but holds weak references to keys.
- Example:
  ```javascript
  let weakMap = new WeakMap();
  let key = {};
  weakMap.set(key, "value");
  console.log(weakMap.get(key)); // Output: value
  ```

---

## **4. Dynamic Typing in JavaScript**

JavaScript is dynamically typed, meaning a variable's type can change during execution.

Example:

```javascript
let data = 42; // Number
data = "Now a string"; // String
console.log(data);
```

---

## **5. Type Checking**

### **Using `typeof` Operator**:

- Determines the data type of a value.
- Example:
  ```javascript
  console.log(typeof 42); // Output: number
  console.log(typeof "hello"); // Output: string
  console.log(typeof null); // Output: object (legacy behavior)
  ```

### **Using `instanceof`**:

- Checks if an object is an instance of a specific class.
- Example:

  ```javascript
  let numbers = [1, 2, 3];
  console.log(numbers instanceof Array); // Output: true
  ```

  ### **Using `Array.isArray()`**:

- Determines if a value is an array.
- Example:
  ```javascript
  let numbers = [1, 2, 3];
  console.log(Array.isArray(numbers)); // Output: true
  ```

---

## **6. Type Conversion**

### **Implicit Conversion (Type Coercion)**:

- JavaScript converts values automatically in some cases.
- Example:
  ```javascript
  console.log("5" + 2); // Output: '52' (String concatenation)
  console.log("5" - 2); // Output: 3 (Numeric subtraction)
  ```

### **Explicit Conversion**:

- Converting data types manually.
- Examples:
  ```javascript
  let num = Number("42"); // String to Number
  let str = String(42); // Number to String
  let bool = Boolean(1); // Number to Boolean
  ```

---

## **Conclusion**

Understanding JavaScript data types is crucial for writing effective code. With a mix of primitive and non-primitive types, JavaScript offers the flexibility needed for dynamic and complex programming tasks.
