# Chapter 14: JavaScript Design Patterns

In this chapter, we explore common design patterns in JavaScript. Design patterns are reusable solutions to common problems in software design, helping to write cleaner, more efficient, and maintainable code.

---

## **1. Creational Patterns**

### **1.1 Singleton Pattern**

Ensures only one instance of a class exists.

```javascript
const Singleton = (function () {
  let instance;

  function createInstance() {
    return { value: "I am the only instance" };
  }

  return {
    getInstance() {
      if (!instance) {
        instance = createInstance();
      }
      return instance;
    },
  };
})();

const singleA = Singleton.getInstance();
const singleB = Singleton.getInstance();
console.log(singleA === singleB); // true
```

### **1.2 Factory Pattern**

A factory function creates and returns objects dynamically.

```javascript
function createPerson(name, age) {
  return {
    name,
    age,
    introduce() {
      console.log(`Hi, I'm ${this.name} and I'm ${this.age} years old.`);
    },
  };
}

const person = createPerson("Alice", 30);
person.introduce();
```

### **1.3 Constructor Pattern**

Defines a blueprint for creating objects using the `new` keyword.

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
  this.introduce = function () {
    console.log(`Hi, I'm ${this.name} and I'm ${this.age} years old.`);
  };
}

const person = new Person("Bob", 25);
person.introduce();
```

---

## **2. Structural Patterns**

### **2.1 Module Pattern**

Encapsulates related functions and variables in a single object, promoting code organization.

```javascript
const calculator = (function () {
  let result = 0;

  return {
    add(x) {
      result += x;
    },
    subtract(x) {
      result -= x;
    },
    getResult() {
      return result;
    },
  };
})();

calculator.add(5);
calculator.subtract(2);
console.log(calculator.getResult()); // 3
```

### **2.2 Proxy Pattern**

Provides a surrogate or placeholder object to control access to another object.

```javascript
const handler = {
  get(target, prop) {
    return prop in target ? target[prop] : `Property ${prop} does not exist`;
  },
};

const target = { name: "Proxy Example" };
const proxy = new Proxy(target, handler);
console.log(proxy.name); // Proxy Example
console.log(proxy.age); // Property age does not exist
```

### **2.3 Event Emitter**

Implements a publish-subscribe mechanism for events.

```javascript
class EventEmitter {
  constructor() {
    this.events = {};
  }

  on(event, listener) {
    if (!this.events[event]) {
      this.events[event] = [];
    }
    this.events[event].push(listener);
  }

  emit(event, ...args) {
    if (this.events[event]) {
      this.events[event].forEach((listener) => listener(...args));
    }
  }
}

const emitter = new EventEmitter();
emitter.on("greet", (name) => console.log(`Hello, ${name}`));
emitter.emit("greet", "Alice");
```

### **2.4 Decorator Pattern**

Enhances an object’s behavior without modifying its structure.

```javascript
function addTimestamp(obj) {
  obj.timestamp = new Date();
  return obj;
}

const user = { name: "John" };
const decoratedUser = addTimestamp(user);
console.log(decoratedUser);
```

### **2.5 Composite Pattern**

Combines objects into tree structures to represent part-whole hierarchies.

```javascript
class Component {
  constructor(name) {
    this.name = name;
    this.children = [];
  }

  add(child) {
    this.children.push(child);
  }

  display(depth = 0) {
    console.log("-".repeat(depth) + this.name);
    this.children.forEach((child) => child.display(depth + 2));
  }
}

const root = new Component("Root");
const child1 = new Component("Child1");
const child2 = new Component("Child2");
root.add(child1);
root.add(child2);
root.display();
```

### **2.6 Dependency Injection and Inversion of Control**

Promotes loosely coupled code by injecting dependencies.

```javascript
class Logger {
  log(message) {
    console.log(`LOG: ${message}`);
  }
}

class App {
  constructor(logger) {
    this.logger = logger;
  }

  run() {
    this.logger.log("App is running...");
  }
}

const logger = new Logger();
const app = new App(logger);
app.run();
```

---

## **3. Behavioral Patterns**

### **3.1 Observer Pattern**

Allows objects to subscribe to events and get notified when events occur.

```javascript
class Subject {
  constructor() {
    this.observers = [];
  }

  subscribe(observer) {
    this.observers.push(observer);
  }

  notify(data) {
    this.observers.forEach((observer) => observer.update(data));
  }
}

class Observer {
  update(data) {
    console.log(`Received update: ${data}`);
  }
}

const subject = new Subject();
const observer1 = new Observer();
const observer2 = new Observer();

subject.subscribe(observer1);
subject.subscribe(observer2);

subject.notify("Event occurred");
```

### **3.2 Strategy Pattern**

Defines a family of algorithms, encapsulates each, and makes them interchangeable.

```javascript
class PaymentStrategy {
  pay(amount) {
    throw new Error("pay() must be implemented.");
  }
}

class CreditCardPayment extends PaymentStrategy {
  pay(amount) {
    console.log(`Paid ${amount} using credit card.`);
  }
}

class PayPalPayment extends PaymentStrategy {
  pay(amount) {
    console.log(`Paid ${amount} using PayPal.`);
  }
}

function processPayment(strategy, amount) {
  strategy.pay(amount);
}

processPayment(new CreditCardPayment(), 100);
processPayment(new PayPalPayment(), 200);
```

### **3.3 Command Pattern**

Encapsulates a request as an object, allowing you to parameterize methods.

```javascript
class Command {
  execute() {
    throw new Error("execute() must be implemented.");
  }
}

class Light {
  turnOn() {
    console.log("Light is ON");
  }

  turnOff() {
    console.log("Light is OFF");
  }
}

class LightOnCommand extends Command {
  constructor(light) {
    super();
    this.light = light;
  }

  execute() {
    this.light.turnOn();
  }
}

const light = new Light();
const lightOn = new LightOnCommand(light);
lightOn.execute();
```

### **3.4 Iterator Pattern**

Provides a way to access elements of an aggregate object sequentially.

```javascript
class Iterator {
  constructor(items) {
    this.items = items;
    this.index = 0;
  }

  next() {
    return this.index < this.items.length
      ? { value: this.items[this.index++], done: false }
      : { done: true };
  }
}

const iterator = new Iterator(["a", "b", "c"]);
console.log(iterator.next()); // { value: 'a', done: false }
console.log(iterator.next()); // { value: 'b', done: false }
console.log(iterator.next()); // { value: 'c', done: false }
console.log(iterator.next()); // { done: true }
```

### **3.5 State Pattern**

Allows an object to alter its behavior when its internal state changes.

```javascript
class Light {
  constructor() {
    this.state = new OffState();
  }

  setState(state) {
    this.state = state;
  }

  pressButton() {
    this.state.pressButton(this);
  }
}

class OnState {
  pressButton(light) {
    console.log("Turning off the light...");
    light.setState(new OffState());
  }
}

class OffState {
  pressButton(light) {
    console.log("Turning on the light...");
    light.setState(new OnState());
  }
}

const light = new Light();
light.pressButton(); // Turning on the light...
light.pressButton(); // Turning off the light...
```

---

## **4. Combining Patterns**

Many patterns can work together to create robust applications. For instance, the module pattern can encapsulate a singleton object, or the observer pattern can be combined with factories to manage event-driven systems dynamically.

---

## **5. Additional Patterns (Concise Guide + Examples)**

### **5.1 Adapter (Structural)**
Converts one interface to another expected by clients.
```javascript
class OldApi { getUser(id) { return { id, name: 'Alice' }; } }
class NewApi { fetchUser(id) { return Promise.resolve({ id, fullName: 'Alice' }); } }
class Adapter {
  constructor(newApi) { this.newApi = newApi; }
  async getUser(id) { const u = await this.newApi.fetchUser(id); return { id: u.id, name: u.fullName }; }
}
```

### **5.2 Facade (Structural)**
Simplifies a complex subsystem with a unified API.
```javascript
function buildReport({ fetchData, transform, render }) {
  return async function run() { const raw = await fetchData(); const view = transform(raw); return render(view); };
}
```

### **5.3 Bridge (Structural)**
Decouple abstraction from implementation to vary independently.
```javascript
class Shape { constructor(renderer) { this.renderer = renderer; } draw() { this.renderer.draw(this); } }
class Circle extends Shape { constructor(renderer, r) { super(renderer); this.r = r; } }
class CanvasRenderer { draw(shape) { /* draw on <canvas> */ } }
class SvgRenderer { draw(shape) { /* draw as <svg> */ } }
```

### **5.4 Flyweight (Structural)**
Share intrinsic state to reduce memory.
```javascript
class IconFactory {
  constructor() { this.cache = new Map(); }
  get(name) { if (!this.cache.has(name)) this.cache.set(name, loadIcon(name)); return this.cache.get(name); }
}
```

### **5.5 Builder (Creational)**
Step-by-step construction for complex objects.
```javascript
class Query { constructor({ select = [], where = [] } = {}) { Object.assign(this, { select, where }); } }
class QueryBuilder {
  constructor() { this.state = { select: [], where: [] }; }
  select(...fields) { this.state.select.push(...fields); return this; }
  where(clause) { this.state.where.push(clause); return this; }
  build() { return new Query(this.state); }
}
```

### **5.6 Prototype (Creational)**
Clone existing objects to create new ones.
```javascript
const proto = { greet() { return `hi ${this.name}`; } };
const user = Object.assign(Object.create(proto), { name: 'Alice' });
```

### **5.7 Mediator (Behavioral)**
Centralize complex communications between objects.
```javascript
class Mediator { constructor() { this.handlers = {}; }
  on(evt, fn) { (this.handlers[evt] ||= []).push(fn); }
  emit(evt, payload) { (this.handlers[evt] || []).forEach(fn => fn(payload)); }
}
```

### **5.8 Memento (Behavioral)**
Capture and restore object state (undo).
```javascript
class Caretaker { constructor() { this.stack = []; }
  save(state) { this.stack.push(JSON.stringify(state)); }
  restore() { return JSON.parse(this.stack.pop()); }
}
```

### **5.9 Visitor (Behavioral)**
Separate operations from object structure.
```javascript
function visit(node, visitor) {
  if (Array.isArray(node.children)) node.children.forEach(c => visit(c, visitor));
  visitor(node);
}
```

### **5.10 Chain of Responsibility (Behavioral)**
Pass a request along a chain until handled.
```javascript
const withAuth = next => ctx => ctx.user ? next(ctx) : { status: 401 };
const withLogging = next => ctx => (console.log('req'), next(ctx));
const handler = ctx => ({ status: 200 });
const pipeline = withAuth(withLogging(handler));
```

### **5.11 Template Method (Behavioral)**
Define a skeleton algorithm, letting subclasses override steps.
```javascript
class Task { run() { this.before(); this.execute(); this.after(); }
  before() {} execute() { throw new Error('implement'); } after() {} }
```

---

## **6. Choosing Patterns and Avoiding Overengineering**

- Prefer simple functions and modules first; introduce patterns when duplication or complexity appears.
- Composition over inheritance: build pipelines/middlewares instead of deep hierarchies.
- Make dependencies explicit (constructor injection); avoid hidden singletons.
- For async workflows, consider Strategy + Chain or Command + Queue.
- Testability: small pure functions and DI ease unit testing.

---

## **7. TypeScript and Documentation Tips**

- Use interfaces for strategies and commands; narrow types on boundaries.
- Prefer readonly fields for immutable configuration; expose minimal public surface.
- Document invariants and extension points; provide small, runnable examples.

---

## **Conclusion**

Understanding design patterns in JavaScript equips developers to tackle complex problems with efficient, reusable solutions. By mastering these patterns, you can write scalable and maintainable code, ensuring a better development experience.
