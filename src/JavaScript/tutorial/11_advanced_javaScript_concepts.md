# Chapter 11: Advanced JavaScript Concepts

In this chapter, we dive deeper into advanced topics in JavaScript that enable developers to write more powerful and efficient code.

---

## **1. Prototypes and Inheritance**

JavaScript uses prototypal inheritance to enable objects to inherit properties and methods from other objects.

### **1.1 Understanding Prototypes**:

Every JavaScript object has an internal property called `[[Prototype]]`, which points to another object.

```javascript
const person = {
  greet() {
    console.log("Hello!");
  },
};

const john = Object.create(person);
john.greet(); // Output: Hello!
```

### **1.2 Prototype Chain**:

If a property or method is not found in the object itself, JavaScript looks up the prototype chain.

---

## **2. ES6 Classes and Static Methods**

Classes in JavaScript are syntactic sugar over the prototypal inheritance model.

### **2.1 Defining a Class**:

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} makes a noise.`);
  }
}

const dog = new Animal("Dog");
dog.speak();
```

### **2.2 Inheritance with Classes**:

```javascript
class Dog extends Animal {
  speak() {
    console.log(`${this.name} barks.`);
  }
}

const myDog = new Dog("Buddy");
myDog.speak(); // Output: Buddy barks.
```

### **2.3 Static Methods**:

Static methods are called on the class itself, not on instances of the class.

```javascript
class MathUtil {
  static add(a, b) {
    return a + b;
  }
}

console.log(MathUtil.add(5, 3)); // Output: 8
```

---

## **3. Modules: import, export, default exports**

Modules allow developers to organize code into reusable pieces.

### **3.1 Exporting and Importing**:

```javascript
// math.js
export function add(a, b) {
  return a + b;
}

// main.js
import { add } from "./math.js";
console.log(add(2, 3));
```

### **3.2 Default Exports**:

```javascript
// greet.js
export default function greet(name) {
  return `Hello, ${name}!`;
}

// main.js
import greet from "./greet.js";
console.log(greet("Alice"));
```

---

## **4. Advanced Array and Object Techniques**

### **4.1 Destructuring**:

```javascript
const [a, b] = [1, 2];
const { name, age } = { name: "Alice", age: 25 };
```

### **4.2 Spread and Rest Operators**:

```javascript
const arr = [1, 2, 3];
const newArr = [...arr, 4];

function sum(...numbers) {
  return numbers.reduce((a, b) => a + b, 0);
}
```

---

## **5. Template Literals, String Interpolation, Multiline Strings, and Tagged Templates**

### **5.1 Template Literals**:

Allows embedding expressions within strings using backticks (`).

```javascript
const name = "Alice";
console.log(`Hello, ${name}!`); // Output: Hello, Alice!
```

### **5.2 Multiline Strings**:

```javascript
const message = `This is a 
multiline string.`;
console.log(message);
```

### **5.3 Tagged Templates**:

```javascript
function tag(strings, ...values) {
  console.log(strings);
  console.log(values);
}

tag`Hello, ${name}!`;
```

---

## **6. Dynamic Imports and Lazy Loading**

Dynamic imports allow modules to be loaded on demand, reducing initial load time.

```javascript
async function loadModule() {
  const { add } = await import("./math.js");
  console.log(add(4, 5));
}

loadModule();
```

---

## **7. Comparing Classes with Prototypes**

Classes provide a cleaner syntax for creating objects and handling inheritance compared to prototypes, though both ultimately use the same underlying system.

### **7.1 Using Prototypes**:

```javascript
function Animal(name) {
  this.name = name;
}

Animal.prototype.speak = function () {
  console.log(`${this.name} makes a noise.`);
};

const dog = new Animal("Dog");
dog.speak();
```

### **7.2 Using Classes**:

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} makes a noise.`);
  }
}

const dog = new Animal("Dog");
dog.speak();
```

---

## **8. WeakMap and WeakSet**

WeakMap and WeakSet hold weak references to objects, allowing them to be garbage collected when no longer in use.

### **8.1 WeakMap**:

```javascript
const wm = new WeakMap();
const obj = {};
wm.set(obj, "value");
console.log(wm.get(obj)); // Output: value
```

### **8.2 WeakSet**:

```javascript
const ws = new WeakSet();
const obj = {};
ws.add(obj);
console.log(ws.has(obj)); // Output: true
```

---

## **9. Symbols, Iterators, and Iterables**

### **9.1 Symbols**:

Unique and immutable values used as object keys.

```javascript
const sym = Symbol("unique");
const obj = { [sym]: "value" };
console.log(obj[sym]);
```

### **9.2 Iterators and Iterables**:

Enable objects to be iterable.

```javascript
const iterable = {
  *[Symbol.iterator]() {
    yield 1;
    yield 2;
    yield 3;
  },
};

for (const value of iterable) {
  console.log(value);
}
```

---

## **10. Generators**

Generators are functions that can pause execution and yield multiple values.

```javascript
function* generatorFunction() {
  yield 1;
  yield 2;
  yield 3;
}

const gen = generatorFunction();
console.log(gen.next().value); // Output: 1
console.log(gen.next().value); // Output: 2
```

---

## **Conclusion**

Understanding these advanced JavaScript concepts is crucial for developing complex applications and mastering the language. By incorporating these techniques, developers can write more efficient and maintainable code.
