# Chapter 10: Working with Objects and Arrays

This chapter explores how to effectively work with objects and arrays in JavaScript, two fundamental data structures in the language.

---

## **1. Object Literals, Properties, and Methods**

### **1.1 Creating Objects**:

Objects are key-value pairs that store data.

```javascript
const person = {
  name: "Alice",
  age: 25,
  greet: function () {
    console.log("Hello, " + this.name);
  },
};
```

### **1.2 Accessing Properties**:

- **Dot Notation**: `person.name`
- **Bracket Notation**: `person["name"]`

### **1.3 Adding and Modifying Properties**:

```javascript
person.job = "Developer";
person.age = 26;
```

### **1.4 Deleting Properties**:

```javascript
delete person.age;
```

---

## **2. Arrays and Common Array Methods**

### **2.1 Creating Arrays**:

Arrays store ordered lists of values.

```javascript
const fruits = ["Apple", "Banana", "Cherry"];
```

### **2.2 Accessing Elements**:

```javascript
console.log(fruits[0]); // Apple
```

### **2.3 Adding and Removing Elements**:

- **`push()`**: Adds to the end.
- **`pop()`**: Removes from the end.
- **`unshift()`**: Adds to the beginning.
- **`shift()`**: Removes from the beginning.

### **Example**:

```javascript
fruits.push("Durian");
fruits.pop();
```

### **2.4 Iterating Over Arrays**:

- **`for` Loop**
- **`for...of` Loop**

### **Example**:

```javascript
fruits.forEach((fruit) => console.log(fruit));
```

### **2.5 Iterating Over Arrays**:

- **`map()`**: Transforms elements and returns a new array.
- **`filter()`**: Returns elements that meet a condition.
- **`reduce()`**: Reduces an array to a single value.
- **`forEach()`**: Executes a function for each array element.
- **`some()` and `every()`**: Check conditions on array elements.

### **Example**:

```javascript
const numbers = [1, 2, 3, 4, 5];
const squares = numbers.map((num) => num ** 2);
console.log(squares); // [1, 4, 9, 16, 25]
```

---

## **3. Combining Objects and Arrays**

### **3.1 Arrays of Objects**:

```javascript
const users = [
  { name: "Alice", age: 25 },
  { name: "Bob", age: 30 },
];
```

### **3.2 Objects with Array Properties**:

```javascript
const company = {
  name: "TechCorp",
  employees: ["Alice", "Bob", "Charlie"],
};
```

---

## **4. Destructuring and Spread/Rest Operators**

### **4.1 Destructuring**:

Simplifies extracting values from arrays or objects.

```javascript
const [first, second] = fruits;
const { name, job } = person;
```

### **4.2 Spread Operator**:

Allows expanding arrays or objects.

```javascript
const allFruits = [...fruits, "Mango"];
```

### **4.3 Rest Operator**:

Groups the rest of the elements.

```javascript
const [firstFruit, ...otherFruits] = fruits;
```

---

## **5. Understanding and Using JSON**

### **5.1 What is JSON?**:

JSON (JavaScript Object Notation) is a format for data exchange.

```json
{
  "name": "Alice",
  "age": 25
}
```

### **5.2 Handling JSON in JavaScript**:

- **`JSON.parse()`**: Converts a JSON string into an object.
- **`JSON.stringify()`**: Converts an object into a JSON string.

### **Example**:

```javascript
const jsonString = '{"name": "Alice", "age": 25}';
const user = JSON.parse(jsonString);
console.log(user.name); // Alice

const jsonData = JSON.stringify(user);
console.log(jsonData); // {"name":"Alice","age":25}
```

---

## **6. Best Practices**

1. **Use Descriptive Keys**: Choose meaningful names for object properties.
2. **Avoid Sparse Arrays**: Ensure array indices are contiguous.
3. **Leverage Built-in Methods**: Use methods like `Object.keys`, `Object.values`, and `Object.entries` for object manipulation.
4. **Use `const` for Arrays and Objects**: Prevent reassignment while allowing modifications to elements or properties.
5. **Validate JSON Data**: When parsing JSON from external sources.

---

## **7. Property Descriptors and Immutability**

- Control property characteristics with `Object.defineProperty`.
- Freeze or seal objects to prevent mutations.
- Examples:
  ```javascript
  const user = {};
  Object.defineProperty(user, 'id', { value: 1, writable: false, enumerable: true });
  // user.id = 2; // TypeError in strict mode

  const cfg = Object.freeze({ mode: 'prod' }); // no adds/removes/changes
  const semi = Object.seal({ a: 1 }); // no adds/removes, can change existing
  ```

---

## **8. Prototypes and Inheritance Basics**

- Create objects with a specific prototype using `Object.create`.
- Check own keys vs prototype keys with `Object.hasOwn`.
- Examples:
  ```javascript
  const proto = { greet() { return `hi ${this.name}`; } };
  const alice = Object.create(proto);
  alice.name = 'Alice';
  console.log(alice.greet()); // hi Alice
  console.log(Object.hasOwn(alice, 'greet')); // false
  ```

---

## **9. Copying and Merging (Shallow vs Deep)**

- Shallow clone: spread or `Object.assign`.
- Deep clone: `structuredClone` (where available) or libraries for complex cases.
- Examples:
  ```javascript
  const a = { x: { y: 1 } };
  const shallow = { ...a }; // shares nested refs
  const deep = typeof structuredClone === 'function' ? structuredClone(a) : JSON.parse(JSON.stringify(a));
  ```

---

## **10. Advanced Array Operations**

- `slice` (non-mutating) vs `splice` (mutating).
- `find`/`findIndex`, `includes`, `some`/`every`, `flat`/`flatMap`.
- `sort` with a comparator; avoid default lexicographic pitfalls.
- Examples:
  ```javascript
  const arr = [3, 1, 10];
  console.log(arr.slice(0, 2)); // [3,1]
  arr.splice(1, 1); // arr = [3,10]
  console.log([1, [2, 3]].flat()); // [1,2,3]
  console.log(arr.sort((a, b) => a - b)); // numeric sort
  ```

---

## **11. Sets and Maps**

- Use `Set` for uniqueness and `Map` for key-value with non-string keys.
- Examples:
  ```javascript
  const unique = [...new Set([1, 2, 2, 3])]; // [1,2,3]
  const counts = new Map();
  for (const n of [1,1,2]) counts.set(n, (counts.get(n) || 0) + 1);
  ```

---

## **12. Iterating Objects Safely**

- Prefer `Object.keys/values/entries` over `for...in` for own keys.
- Convert pairs back to objects with `Object.fromEntries`.
- Examples:
  ```javascript
  const obj = { a: 1, b: 2 };
  for (const [k, v] of Object.entries(obj)) console.log(k, v);
  const mapped = Object.fromEntries(Object.entries(obj).map(([k, v]) => [k, v * 2]));
  ```

---

## **13. JSON Gotchas**

- `JSON.stringify` drops functions, `undefined`, `Symbol`, and loses `Date` types.
- Use replacer/reviver for custom handling.
- Example:
  ```javascript
  const reviver = (k, v) => (k === 'createdAt' ? new Date(v) : v);
  const user = { createdAt: new Date() };
  const s = JSON.stringify(user);
  const parsed = JSON.parse(s, reviver);
  ```

---

## **14. Optional Chaining and Nullish Coalescing**

- Safely access deep properties and set defaults only for `null`/`undefined`.
- Examples:
  ```javascript
  const city = person?.address?.city ?? 'Unknown';
  const [head, ...tail] = (fruits ?? []);
  ```

---

## **15. Performance Notes**

- Avoid sparse arrays; prefer contiguous indices.
- Pre-allocate when size is known; minimize intermediate arrays in hot paths.
- Consider Typed Arrays for numeric, fixed-size data.
- Example:
  ```javascript
  const buf = new Uint8Array(1024); // fixed-size numeric storage
  ```

---

## **Conclusion**

Understanding how to effectively use objects and arrays is crucial for working with JavaScript. These data structures provide the foundation for organizing and manipulating data in most JavaScript applications.
