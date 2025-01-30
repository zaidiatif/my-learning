# Chapter 13: DOM Manipulation

In this chapter, we delve into the Document Object Model (DOM), a programming interface for web documents, and learn how to manipulate it using JavaScript to create dynamic and interactive web pages.

---

## **1. Introduction to the DOM**

The DOM (Document Object Model) represents the structure of an HTML document as a tree of nodes, where each element, attribute, and text is a node. JavaScript can be used to traverse, modify, and interact with these nodes. It provides a way for JavaScript to interact with and manipulate the content, structure, and styles of a web page.

### **Key Components of the DOM:**

- **Document**: Represents the entire HTML document.
- **Element Nodes**: Represent HTML elements like `<div>`, `<p>`, `<button>`.
- **Attribute Nodes**: Represent attributes of elements like `class`, `id`, `src`.
- **Text Nodes**: Represent the text content inside elements.

---

## **2. Selecting and Manipulating DOM Elements**

### **2.1 Selecting Elements**

JavaScript provides various methods to access DOM elements:

#### **Examples**:

```javascript
// Select by ID
const header = document.getElementById("header");

// Select by Class
const buttons = document.getElementsByClassName("btn");

// Query Selector
const mainSection = document.querySelector("#main");
const allParagraphs = document.querySelectorAll("p");
```

### **2.2 Navigating Nodes**:

- **Parent Node**: `parentNode`
- **Child Nodes**: `childNodes`, `firstChild`, `lastChild`
- **Sibling Nodes**: `previousSibling`, `nextSibling`

#### **Example**:

```javascript
const parent = document.getElementById("child").parentNode;
const firstChild = parent.firstChild;
const nextSibling = firstChild.nextSibling;
```

### **2.3 Element-Specific Traversal**:

- **Children**: `children`
- **First Element Child**: `firstElementChild`
- **Last Element Child**: `lastElementChild`
- **Next/Previous Sibling**: `nextElementSibling`, `previousElementSibling`

#### **Example**:

```javascript
const list = document.querySelector("ul");
const firstItem = list.firstElementChild;
const lastItem = list.lastElementChild;
```

### **2.4 Manipulating Elements**

#### **Modifying Attributes**

- **Set Attribute**: `setAttribute`
- **Get Attribute**: `getAttribute`
- **Remove Attribute**: `removeAttribute`

#### **Example**:

```javascript
const link = document.querySelector("a");
link.setAttribute("href", "https://example.com");
console.log(link.getAttribute("href"));
link.removeAttribute("target");
```

#### **Modifying Styles**

- Use the `style` property to change inline styles.

```javascript
const box = document.querySelector(".box");
box.style.backgroundColor = "blue";
box.style.color = "white";
```

#### **Working with Classes**

- **Add**: `classList.add`
- **Remove**: `classList.remove`
- **Toggle**: `classList.toggle`
- **Check**: `classList.contains`

#### **Example**:

```javascript
const button = document.querySelector("button");
button.classList.add("active");
if (button.classList.contains("active")) {
  button.classList.remove("active");
} else {
  button.classList.toggle("highlight");
}
```

#### **Changing Content**

- **InnerHTML**: Modifies the HTML content.
- **TextContent**: Modifies the text content only.

#### **Example**:

```javascript
const heading = document.querySelector("h1");
heading.innerHTML = "<span>Welcome!</span>"; // Sets HTML content
heading.textContent = "Welcome!"; // Sets plain text content
```

---

## **3. Event Handling**

### **3.1 Adding Event Listeners**

Use `addEventListener` to bind events to elements.

```javascript
const button = document.querySelector("button");
button.addEventListener("click", () => {
  alert("Button clicked!");
});
```

### **3.2 Event Propagation**

- **Event Bubbling**: Events propagate from the target element to its ancestors.
- **Event Capturing**: Events propagate from ancestors to the target element.

```javascript
const parent = document.querySelector("#parent");
const child = document.querySelector("#child");

parent.addEventListener("click", () => console.log("Parent clicked"));
child.addEventListener("click", () => console.log("Child clicked"));
```

### **3.3 Stopping Propagation**

Use `stopPropagation()` to stop the event from propagating further.

```javascript
child.addEventListener("click", (event) => {
  event.stopPropagation();
  console.log("Propagation stopped.");
});
```

---

## **4. Creating, Inserting, and Removing DOM Elements**

### **4.1 Creating Elements**

Use `document.createElement` to create new elements.

```javascript
const newDiv = document.createElement("div");
newDiv.textContent = "I am a new div!";
```

### **4.2 Inserting Elements**

- **Append to Parent**: `appendChild`
- **Insert Before**: `insertBefore`
- **Modern Methods**: `append`, `prepend`

```javascript
const parent = document.querySelector("#container");
const child = document.createElement("p");
child.textContent = "Hello!";
parent.appendChild(child);

const newChild = document.createElement("span");
parent.insertBefore(newChild, child);
```

### **4.3 Removing Elements**

- Use `removeChild` or the modern `remove` method to delete elements.

```javascript
const unwantedElement = document.querySelector(".old");
unwantedElement.remove();
```

---

## **5. Event Delegation Techniques**

Event delegation leverages event bubbling to attach a single event listener to a parent element, which handles events for its child elements.

### **Example**:

```javascript
const list = document.querySelector("ul");
list.addEventListener("click", (event) => {
  if (event.target.tagName === "LI") {
    console.log(`You clicked on ${event.target.textContent}`);
  }
});
```

---

## **6. Best Practices for DOM Manipulation**

1. **Minimize Repaints and Reflows**: Batch DOM changes to avoid performance issues.
2. **Use Modern Methods**: Prefer `querySelector` and `querySelectorAll` over older methods like `getElementById`.
3. **Avoid Inline Scripts**: Use external JavaScript files for better maintainability.
4. **Optimize Event Listeners**: Use event delegation when dealing with multiple similar elements.

---

## **Conclusion**

DOM manipulation is a powerful feature of JavaScript that allows developers to create dynamic and interactive web applications. By mastering the techniques of traversing, modifying, and interacting with the DOM, you can enhance the functionality and user experience of your websites.
