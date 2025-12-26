---

[<< Chapter 12](./12_asynchronous_javaScript.md) | [Chapter 14 >>](./14_accessibility_in_javascript.md)

---

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

## **7. Performance: Reflow/Repaint, Batching, and Fragments**

- Read layout once, then write to avoid layout thrashing.
- Use `DocumentFragment` to batch inserts and minimize reflows.
- Defer visual updates to the next frame when appropriate.

```javascript
// Batch DOM updates with a fragment
const ul = document.querySelector("ul");
const frag = document.createDocumentFragment();
for (let i = 0; i < 1000; i++) {
  const li = document.createElement("li");
  li.textContent = `Item ${i}`;
  frag.appendChild(li);
}
ul.appendChild(frag);

// Defer heavy updates
requestAnimationFrame(() => {
  ul.classList.add("hydrated");
});
```

---

## **8. Observers: Mutation, Intersection, Resize**

- Observe DOM changes, element visibility, and size changes efficiently.

```javascript
// MutationObserver: watch subtree changes
const mo = new MutationObserver((mutations) => {
  for (const m of mutations) console.log("mutation", m.type);
});
mo.observe(document.body, { childList: true, subtree: true });

// IntersectionObserver: lazy-load images
const io = new IntersectionObserver((entries, obs) => {
  for (const e of entries)
    if (e.isIntersecting) {
      const img = e.target;
      img.src = img.dataset.src;
      obs.unobserve(img);
    }
});
document.querySelectorAll("img[data-src]").forEach((img) => io.observe(img));

// ResizeObserver: respond to element size changes
const ro = new ResizeObserver((entries) => {
  for (const e of entries) console.log("size", e.contentRect.width);
});
ro.observe(document.querySelector("#sidebar"));
```

---

## **9. Event Listener Options and Delegation Tips**

- Use `{ passive: true }` for scroll/touch listeners to improve responsiveness.
- Use `{ once: true }` for one-time handlers; `{ capture: true }` for capture phase when needed.

```javascript
window.addEventListener("scroll", onScroll, { passive: true });
button.addEventListener("click", handle, { once: true });

// Robust delegation with closest()
document.addEventListener("click", (e) => {
  const item = e.target.closest("[data-item]");
  if (!item) return;
  console.log("clicked item id", item.dataset.item);
});
```

---

## **10. Safe HTML Handling**

- Prefer `textContent` over `innerHTML` to avoid XSS.
- When injecting HTML from untrusted sources, sanitize first.

```javascript
// Safe text insertion
title.textContent = userInput;

// If HTML is required, sanitize (example assumes a sanitizer is available)
// const clean = DOMPurify.sanitize(untrustedHtml);
// container.innerHTML = clean;
```

---

## **11. Accessibility: ARIA, Focus, and Keyboard**

- Manage focus order and visibility with `tabindex`, and announce updates with ARIA.
- Provide keyboard equivalents for interactive elements.

```javascript
// Move focus to dialog
const dialog = document.querySelector("#dialog");
dialog.setAttribute("role", "dialog");
dialog.setAttribute("aria-modal", "true");
dialog.querySelector("[data-initial-focus]")?.focus();

// Keyboard activation
document.addEventListener("keydown", (e) => {
  if (e.key === "Enter" && e.target.matches('[role="button"]'))
    e.target.click();
});
```

---

## **12. Datasets and Attribute Management**

- Use `dataset` for custom data attributes and `classList` for state.

```javascript
const card = document.querySelector(".card");
card.dataset.id = "42";
card.classList.toggle("selected", true);
```

---

## **Conclusion**

DOM manipulation is a powerful feature of JavaScript that allows developers to create dynamic and interactive web applications. By mastering the techniques of traversing, modifying, and interacting with the DOM, you can enhance the functionality and user experience of your websites.

---

[<< Chapter 12](./12_asynchronous_javaScript.md) | [Chapter 14 >>](./14_accessibility_in_javascript.md)

---
