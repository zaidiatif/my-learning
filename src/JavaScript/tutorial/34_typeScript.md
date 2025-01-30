# Chapter 34: TypeScript

## **Introduction to TypeScript**

TypeScript is a strongly typed superset of JavaScript developed by Microsoft. It compiles to plain JavaScript, enabling developers to build scalable and robust applications with enhanced productivity and fewer runtime errors.

### **Key Features**

- **Static Typing**: Adds type annotations to JavaScript, helping identify errors during development.
- **Improved Tooling**: Provides autocompletion, refactoring, and inline documentation.
- **Type Inference**: Automatically infers types when possible, reducing boilerplate.
- **Compatibility**: Works with existing JavaScript code and libraries.

---

## **TypeScript vs. JavaScript**

TypeScript and JavaScript share many similarities, but TypeScript introduces additional features to improve developer experience and code quality.

### **1. Static Typing**

- **JavaScript**: Dynamically typed, meaning types are determined at runtime.
- **TypeScript**: Statically typed, allowing types to be defined at compile-time.

#### Example:

**JavaScript**

```javascript
let name = "John"; // Type inferred as string at runtime
name = 42; // No error until runtime
```

**TypeScript**

```typescript
let name: string = "John";
name = 42; // Compilation error: Type 'number' is not assignable to type 'string'.
```

### **2. Tooling and IDE Support**

- **JavaScript**: Basic IDE support for syntax highlighting and debugging.
- **TypeScript**: Enhanced tooling with autocomplete, inline error detection, and robust refactoring options.

### **3. Features**

- **JavaScript**: Limited to ES standard features.
- **TypeScript**: Adds interfaces, enums, decorators, and advanced type constructs.

### **4. Compatibility**

- **JavaScript**: Supported natively by browsers and runtime environments.
- **TypeScript**: Requires compilation to JavaScript for execution.

### **5. Error Handling**

- **JavaScript**: Runtime errors are only caught during execution.
- **TypeScript**: Compile-time errors catch issues before execution, reducing runtime bugs.

#### Example:

**JavaScript Runtime Error**

```javascript
function greet(user) {
  return user.toUpperCase(); // Error if user is undefined
}
```

**TypeScript Compile-Time Error**

```typescript
function greet(user: string) {
  return user.toUpperCase();
}

greet(undefined); // Error: Argument of type 'undefined' is not assignable to parameter of type 'string'.
```

---

## **Installing and Setting Up TypeScript**

### **1. Installing TypeScript**

TypeScript can be installed globally or as a project dependency using npm or yarn.

#### Global Installation:

```bash
npm install -g typescript
```

#### Local Installation:

```bash
npm install --save-dev typescript
```

### **2. TypeScript Configuration**

Create a `tsconfig.json` file to configure the TypeScript compiler.

#### Example `tsconfig.json`:

```json
{
  "compilerOptions": {
    "target": "ES6",
    "module": "CommonJS",
    "strict": true,
    "outDir": "./dist",
    "rootDir": "./src"
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

### **3. Compiling TypeScript**

Compile TypeScript files into JavaScript using the `tsc` command.

#### Example:

```bash
tsc
```

---

## **Core TypeScript Concepts**

### **1. Basic Types**

TypeScript extends JavaScript with static types like `string`, `number`, `boolean`, `any`, `void`, `unknown`, and `never`.

#### Example:

```typescript
let name: string = "TypeScript";
let age: number = 25;
let isActive: boolean = true;
```

### **2. Interfaces**

Interfaces define the shape of an object.

#### Example:

```typescript
interface User {
  id: number;
  name: string;
}

const user: User = {
  id: 1,
  name: "Alice",
};
```

### **3. Classes**

TypeScript supports object-oriented programming with classes.

#### Example:

```typescript
class Animal {
  constructor(public name: string) {}

  makeSound(): void {
    console.log(`${this.name} makes a sound.`);
  }
}

const dog = new Animal("Dog");
dog.makeSound();
```

### **4. Generics**

Generics provide a way to create reusable components.

#### Example:

```typescript
function identity<T>(value: T): T {
  return value;
}

const num = identity<number>(42);
const str = identity<string>("Hello");
```

---

## **Advanced TypeScript Features**

### **1. Union and Intersection Types**

- **Union Types**: Combine multiple types.
- **Intersection Types**: Merge multiple types.

#### Example:

```typescript
type ID = string | number;
type Person = {
  name: string;
};

type Employee = Person & {
  id: ID;
};

const employee: Employee = {
  name: "Jane",
  id: 123,
};
```

### **2. Enums**

Enums represent a group of constant values.

#### Example:

```typescript
enum Direction {
  Up,
  Down,
  Left,
  Right,
}

let move: Direction = Direction.Up;
```

### **3. Decorators**

Decorators are metadata annotations for classes and methods.

#### Example:

```typescript
function Log(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
  const original = descriptor.value;
  descriptor.value = function (...args: any[]) {
    console.log(`Calling ${propertyKey} with`, args);
    return original.apply(this, args);
  };
}

class Calculator {
  @Log
  add(a: number, b: number): number {
    return a + b;
  }
}

const calc = new Calculator();
calc.add(2, 3);
```

---

## **TypeScript with Modern Frameworks**

### **1. TypeScript with React**

TypeScript integrates seamlessly with React for typing props and state.

#### Example:

```typescript
import React from "react";

type ButtonProps = {
  label: string;
  onClick: () => void;
};

const Button: React.FC<ButtonProps> = ({ label, onClick }) => (
  <button onClick={onClick}>{label}</button>
);

export default Button;
```

### **2. TypeScript with Node.js**

TypeScript enhances Node.js applications by adding type safety to server-side code.

#### Example:

```typescript
import express, { Request, Response } from "express";

const app = express();

app.get("/", (req: Request, res: Response) => {
  res.send("Hello, TypeScript with Node.js!");
});

app.listen(3000, () => console.log("Server running on port 3000"));
```

---

## **Best Practices**

1. **Use Strict Mode**: Enable `strict` in `tsconfig.json` to catch common errors early.
2. **Leverage Type Inference**: Let TypeScript infer types when possible.
3. **Modular Code**: Split code into small, reusable modules.
4. **Avoid Any**: Use `any` sparingly to maintain type safety.
5. **Update Regularly**: Keep TypeScript and its dependencies updated.

---

## **Conclusion**

TypeScript empowers developers to write cleaner, more maintainable code with confidence. By combining JavaScript's flexibility with static typing, TypeScript is a powerful tool for building modern applications at scale.
