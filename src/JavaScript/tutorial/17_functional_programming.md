# Chapter 17: Functional Programming

Functional programming is a programming paradigm that treats computation as the evaluation of mathematical functions and avoids changing state or mutable data. JavaScript supports functional programming principles, making it a versatile language for both object-oriented and functional paradigms.

---

## **1. Key Concepts of Functional Programming**

### **1.1 Pure Functions**

A pure function is a function that always produces the same output for the same input and has no side effects.

```javascript
function add(a, b) {
  return a + b; // No side effects, always produces the same result for the same input
}
```

### **1.2 Immutability**

Avoid modifying existing data; instead, create new data structures.

```javascript
const arr = [1, 2, 3];
const newArr = [...arr, 4]; // Original array remains unchanged
```

### **1.3 First-Class Functions**

Functions are treated as first-class citizens in JavaScript, meaning they can be assigned to variables, passed as arguments, and returned from other functions.

```javascript
const greet = (name) => `Hello, ${name}!`;
console.log(greet("Alice"));
```

### **1.4 Higher-Order Functions**

Functions that take other functions as arguments or return functions.

```javascript
const multiply = (factor) => (number) => number * factor;
const double = multiply(2);
console.log(double(5)); // Output: 10
```

### **1.5 Side Effects and Referential Transparency**

A function has a side effect if it modifies some state or interacts with the outside world. Referential transparency ensures the output of a function depends only on its input values.

Minimizing side effects helps make functions predictable and easier to test.

```javascript
// Function with side effects
let count = 0;
function increment() {
  count++; // Modifies external state
}

// Referentially transparent function
function pureIncrement(count) {
  return count + 1; // No side effects
}
```

---

## **2. Advanced Functional Techniques**

### **2.1 Currying**

Breaking down a function that takes multiple arguments into a series of functions that each take a single argument.

```javascript
const add = (a) => (b) => a + b;
const addFive = add(5);
console.log(addFive(3)); // Output: 8
```

### **2.2 Partial Application**

Creating a new function by pre-filling some arguments of the original function.

```javascript
function multiply(a, b) {
  return a * b;
}
const double = multiply.bind(null, 2);
console.log(double(5)); // Output: 10
```

### **2.3 Recursion**

A function that calls itself to solve smaller instances of a problem.

```javascript
const factorial = (n) => {
  if (n === 0) return 1;
  return n * factorial(n - 1);
};
console.log(factorial(5)); // Output: 120
```

### **2.4 Map, Filter, and Reduce**

- **`map`**: Transforms an array by applying a function to each element.
- **`filter`**: Filters elements based on a condition.
- **`reduce`**: Reduces an array to a single value by applying a function.

```javascript
const numbers = [1, 2, 3, 4];
const squared = numbers.map((n) => n * n);
const evens = numbers.filter((n) => n % 2 === 0);
const sum = numbers.reduce((total, n) => total + n, 0);

console.log(squared); // [1, 4, 9, 16]
console.log(evens); // [2, 4]
console.log(sum); // 10
```

### **2.5 Pipelines and Function Composition**

Combining multiple functions to create a new function.

#### **Pipelines (Chaining)**

Pipelines allow chaining of functions for cleaner, sequential processing.

```javascript
const pipeline =
  (...fns) =>
  (value) =>
    fns.reduce((acc, fn) => fn(acc), value);
const increment = (x) => x + 1;
const double = (x) => x * 2;
const process = pipeline(increment, double);
console.log(process(3)); // Output: 8
```

#### **Function Composition**

Combining multiple functions such that the output of one function becomes the input of the next.

Combining multiple functions to create a new function.

```javascript
const compose = (f, g) => (x) => f(g(x));
const addOne = (x) => x + 1;
const double = (x) => x * 2;
const addOneAndDouble = compose(double, addOne);
console.log(addOneAndDouble(3)); // Output: 8
```

---

## **3. Libraries and Tools for Functional Programming**

JavaScript has several libraries to facilitate functional programming:

- **Lodash**: A utility library with functional programming helpers.
- **Ramda**: A library designed specifically for functional programming.
- **Immutable.js**: Ensures immutability for data structures.

---

## **4. Practical Immutability Techniques**

- Shallow vs deep freeze; structural sharing to avoid copying whole trees.
- Examples:
  ```javascript
  // Shallow freeze
  const cfg = Object.freeze({ env: 'prod', limits: { max: 5 } });
  // cfg.env = 'dev'; // TypeError in strict mode

  // Structural sharing (non-mutating nested update)
  const state = { user: { name: 'Alice', tags: ['a'] } };
  const next = {
    ...state,
    user: { ...state.user, tags: [...state.user.tags, 'b'] }
  };
  ```

---

## **5. Point-free Style and Data-last APIs**

- Prefer data-last functions for easy composition.
- Example:
  ```javascript
  const map = (fn) => (arr) => arr.map(fn);
  const filter = (pred) => (arr) => arr.filter(pred);
  const pipe = (...fns) => (x) => fns.reduce((v, f) => f(v), x);

  const isEven = (n) => n % 2 === 0;
  const double = (n) => n * 2;
  const process = pipe(filter(isEven), map(double));
  console.log(process([1,2,3,4])); // [4,8]
  ```

---

## **6. Functors and Maybes (Lightweight Option Type)**

- A Functor supports `map`; a Maybe avoids null checks.
- Example:
  ```javascript
  const Just = (v) => ({
    map: (f) => Just(f(v)),
    fold: (_, g) => g(v)
  });
  const Nothing = () => ({
    map: () => Nothing(),
    fold: (f) => f()
  });
  const fromNullable = (v) => (v == null ? Nothing() : Just(v));

  const result = fromNullable(user.email)
    .map((s) => s.toLowerCase())
    .fold(() => 'no-email', (s) => s);
  ```

---

## **7. Either for Error Handling (Functional Try)**

- Represent success/ failure without throwing.
- Example:
  ```javascript
  const Left = (e) => ({ map: () => Left(e), chain: () => Left(e), fold: (f) => f(e) });
  const Right = (v) => ({ map: (f) => Right(f(v)), chain: (f) => f(v), fold: (_, g) => g(v) });
  const tryCatch = (fn) => {
    try { return Right(fn()); } catch (e) { return Left(e); }
  };

  const parsed = tryCatch(() => JSON.parse('{"a":1}'))
    .map((o) => o.a)
    .fold(() => 0, (a) => a);
  ```

---

## **8. Lenses (Immutable Nested Updates)**

- Read/update deeply nested data immutably.
- Example (simple lens):
  ```javascript
  const lens = (getter, setter) => ({ get: (s) => getter(s), set: (v, s) => setter(v, s) });
  const prop = (k) => lens((s) => s[k], (v, s) => ({ ...s, [k]: v }));

  const userLens = prop('user');
  const nameLens = lens(
    (s) => s.user.name,
    (v, s) => ({ ...s, user: { ...s.user, name: v } })
  );
  const state = { user: { name: 'Alice' } };
  const next = nameLens.set('Bob', state); // { user: { name: 'Bob' } }
  ```

---

## **9. Transducers (Map/Filter without Intermediate Arrays)**

- Compose transformations and apply once to a collection.
- Example:
  ```javascript
  const mapT = (f) => (reducer) => (acc, x) => reducer(acc, f(x));
  const filterT = (p) => (reducer) => (acc, x) => (p(x) ? reducer(acc, x) : acc);
  const composeT = (...ts) => ts.reduce((a, b) => (r) => a(b(r)));

  const xf = composeT(mapT((x) => x * 2), filterT((x) => x % 3 === 0));
  const push = (acc, x) => (acc.push(x), acc);
  const result = [1,2,3,4,5,6].reduce(xf(push), []);
  console.log(result); // [6, 12]
  ```

---

## **10. Lazy Evaluation with Generators**

- Use generators to build lazy pipelines on large/streaming data.
- Example:
  ```javascript
  function* mapG(iter, f) { for (const x of iter) yield f(x); }
  function* filterG(iter, p) { for (const x of iter) if (p(x)) yield x; }
  const range = function* (n) { for (let i = 0; i < n; i++) yield i; };

  const pipeline = filterG(mapG(range(10), (x) => x * 2), (x) => x % 3 === 0);
  console.log([...pipeline]); // [0, 6, 12, 18]
  ```

---

## **11. TypeScript Tips for FP**

- Prefer generics and data-last signatures for inference.
- Model optional/error flows with unions (e.g., `string | undefined`, discriminated unions) or `Result` types.
- Example:
  ```ts
  export type Result<T, E> = { ok: true; value: T } | { ok: false; error: E };
  export const ok = <T>(value: T): Result<T, never> => ({ ok: true, value });
  export const err = <E>(error: E): Result<never, E> => ({ ok: false, error });
  ```

---

## **Conclusion**

Functional programming in JavaScript enables developers to write clean, maintainable, and predictable code. By embracing principles like immutability, pure functions, and higher-order functions, you can create robust applications that are easier to debug and scale.
