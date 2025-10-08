# Chapter 46: TypeScript

## 1 **Introduction to TypeScript**

TypeScript is a strongly typed superset of JavaScript developed by Microsoft that compiles to plain JavaScript. It adds optional static typing, interfaces, and advanced type features, enabling developers to write robust, scalable, and maintainable applications.
Since its release in 2012, TypeScript has become a standard for enterprise-scale development and is now the backbone of modern frameworks like Angular, Next.js, and NestJS.

### 1.1 **Why TypeScript?**

JavaScript’s flexibility is both a strength and a weakness. While it allows rapid prototyping, it often leads to runtime errors that could have been caught earlier. TypeScript addresses this problem by shifting error detection from runtime to compile-time, improving developer productivity and reliability.

### 1.2 **Key Features**

- **Static Typing**: Adds type annotations to JavaScript, helping identify errors during development.
- **Intelligent Tooling**: Provides autocompletion, refactoring, real-time linting, and inline documentation.
- **Type Inference**: Automatically infers types when possible, reducing boilerplate.
- **Compatibility**: Works with existing JavaScript code and libraries or framework.
- **Configurable Compilation:** — Customize transpilation for different targets and environments.
- **Predictable Codebase:** — Enforces structure and reduces ambiguity in large projects.

---

## 2 **TypeScript vs. JavaScript**

TypeScript and JavaScript share many similarities, but TypeScript introduces additional features to improve developer experience and code quality.

| Feature | Javascript | Typescript |
|:--- |:--- |:--- |
| Typing	| Dynamic	 | Static (optional) |
| Error Detection	| Runtime	| Compile-time |
| Tooling Support	| Basic	| Advanced (IntelliSense, linting, etc.) |
| Language Features	| ES6/ESNext	| Includes ESNext + Types, Enums, Generics, Decorators |
| Execution	| Interpreted	| Compiled to JavaScript |

### 2.1 Example:

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
### 2.2 Example: Safer Coding with TypeScript

```typescript
function calculateArea(radius: number): number {
  return Math.PI * radius ** 2;
}

console.log(calculateArea(10)); // ✅ Works fine
console.log(calculateArea("ten")); // ❌ Error at compile time

```

In JavaScript, this would only fail during execution. In TypeScript, the compiler prevents it before the code runs.

### 2.2 Example:

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
## 3 Setting Up TypeScript

### 3.1 Installation

```bash
npm install -g typescript
```

For a project-specific installation:

```bash
npm install --save-dev typescript
```

### 3.1.2 Initializing Configuration

```bash
tsc --init
```

This creates a tsconfig.json file — the heart of every TypeScript project.

#### Example tsconfig.json:
```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "CommonJS",
    "strict": true,
    "esModuleInterop": true,
    "outDir": "./dist",
    "rootDir": "./src",
    "sourceMap": true,
    "skipLibCheck": true
  },
  "include": ["src/**/*"]
}
```

### 3.1.3. Compiling Code

```bash
tsc
```

This converts `.ts` files in `src/` into `.js` files in `dist/`.

---

## 4 **Core TypeScript Concepts**

### 4.1 **Basic Types**

TypeScript provides a rich type system that covers primitives and advanced constructs.

TypeScript extends JavaScript with static types like `string`, `number`, `boolean`, `any`, `void`, `unknown`, and `never`.

#### Example:

```typescript
let name: string = "TypeScript";
let age: number = 25;
let isActive: boolean = true;
let data: any = "can be anything";
let ids: number[] = [1, 2, 3];
```

### 4.2 **Interfaces**

Interfaces define contracts for object shapes, ensuring consistency.

OR

Interfaces define the shape of an object.

#### Example:

```typescript
interface Product {
  id: number;
  name: string;
  price: number;
  description?: string; // optional property
}

const item: Product = { id: 1, name: "Laptop", price: 1200 };
```

### 4.3 **Classes and Inheritance**

TypeScript supports object-oriented programming with classes.

#### Example:

```typescript
class Vehicle {
  constructor(public brand: string) {}

  start() {
    console.log(`${this.brand} engine started.`);
  }
}

class Car extends Vehicle {
  drive() {
    console.log(`${this.brand} is driving.`);
  }
}

const car = new Car("Tesla");
car.start();
car.drive();
```

### 4.4 **Generics**

Generics enable reusable and type-safe components.

#### Example:

```typescript
function wrap<T>(value: T): { item: T } {
  return { item: value };
}

const wrapped = wrap("Book"); // { item: "Book" }
```

---

## 5 **Advanced TypeScript Features**

### 5.1 **Union and Intersection Types**

- **Union Types**: Combine multiple types.
- **Intersection Types**: Merge multiple types.

#### Example:

```typescript
// 1
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

// 2
type ID = string | number;

type Admin = { role: "admin"; accessLevel: number };
type User = { name: string; email: string };

type AdminUser = Admin & User;

const admin: AdminUser = {
  role: "admin",
  accessLevel: 10,
  name: "Atif",
  email: "atif@example.com"
};
```
### 5.2 **Type Aliases and Literal Types**

```typescript
type Status = "pending" | "approved" | "rejected";

let orderStatus: Status = "approved";

```

### 5.3 **Enums**

Enums represent a group of constant values.

#### Example:

```typescript
// 1
enum Direction {
  Up,
  Down,
  Left,
  Right,
}

let move: Direction = Direction.Up;

// 2
enum LogLevel {
  Info,
  Warning,
  Error,
}

function log(level: LogLevel, message: string) {
  console.log(`[${LogLevel[level]}]: ${message}`);
}

log(LogLevel.Error, "Something went wrong!");

```

### 5.4 **Decorators**

Decorators are metadata annotations for classes and methods.

OR

Decorators add metadata to classes or methods — commonly used in frameworks like `Angular` or `NestJS`.

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

## 6 **TypeScript with Modern Frameworks**

### 6.1 **TypeScript with React**

TypeScript integrates seamlessly with React for typing props, state and hooks.

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

### 6.2 **TypeScript with Node.js**

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

### 6.3 With Next.js or NestJS

- **Next.js** uses TypeScript for type-safe React server components and API routes.
- **NestJS** leverages decorators, interfaces, and dependency injection for scalable backends.

---

## 7 Tooling and Ecosystem

| Tool	| Purpose |
|:--- |:--- |
| ts-node	| Run TypeScript files without compiling manually |
| ESLint + Prettier	| Code linting and formatting |
| TypeORM/Prisma	| ORM libraries with TypeScript typings |
| Jest/Vitest	| Testing frameworks with built-in TypeScript support |
| SWC/esbuild	| High-performance TypeScript transpilers for modern builds |

---

### 8 Compiler Internals

TypeScript’s compiler (`tsc`) performs three key stages:
- **Parsing** — Converts code into an abstract syntax tree (AST).
- **Type Checking** — Validates data types and identifies mismatches.
- **Emission** — Generates JavaScript code targeting the specified ECMAScript version.

You can also use the TypeScript Compiler API to create custom transformers, code analyzers, or documentation generators.

---

## 8 Performance and Optimization Tips

- Enable "skipLibCheck": true to speed up compilation.
- Use "incremental": true for faster rebuilds.
- Prefer type inference where possible to reduce redundancy.
- Split code into smaller modules for faster type-checking.
- Use isolatedModules in large monorepos to simplify incremental builds.

---
## 9 **Best Practices**

1. **Use Strict Mode**: Enable `strict` in `tsconfig.json` to catch common errors early, Ensures maximum type safety.
2. **Avoid Any**: Use `any` sparingly to maintain type safety or Use unknown or generics instead.
3. **Organize Types:** Keep interfaces and types in dedicated files.
4. **Regularly Update Type Definitions:** Keep `@types/*` packages current.
5. **Prefer Composition over Inheritance:** Easier to maintain.
6. **Leverage Type Inference**: Let TypeScript infer types when possible.
7. **Modular Code**: Split code into small, reusable modules.
8. **Write Declaration Files:** For third-party JS libraries without types.
9. **Use ESLint with TypeScript Plugin:** For static analysis and clean code.

---

## 10 **Conclusion**

TypeScript transforms JavaScript development into a `structured`, `predictable`, and `maintainable` process. By introducing `static types`, `interfaces`, `generics`, and `advanced tooling`, it reduces runtime errors and improves collaboration in large teams.

With seamless integration across `React`, `Node.js`, `Angular`, and `Next.js`, TypeScript is now an industry standard for modern, large-scale application development — bridging the gap between flexibility and reliability.

**In short:** TypeScript empowers developers to write robust, scalable, and future-proof applications with confidence.
