# Chapter 3: Basic JavaScript Execution

## **1. Writing the First JavaScript Program**

Starting with JavaScript can be as simple as creating a file and running a few lines of code. Below are the steps to write your first program.

### **Steps to Write Your First Program**:

1. Open a code editor or browser console.
2. Write the following JavaScript code:
   ```javascript
   console.log("Hello, World!");
   ```
3. Save and run the program:
   - In a code editor, save the file as `script.js` and link it to an HTML file.
   - In the browser console, type the code directly and press Enter.

### **Example with HTML Integration**:

1. Create an HTML file named `index.html`:
   ```html
   <!DOCTYPE html>
   <html lang="en">
     <head>
       <meta charset="UTF-8" />
       <meta name="viewport" content="width=device-width, initial-scale=1.0" />
       <title>My First JavaScript Program</title>
     </head>
     <body>
       <script src="script.js"></script>
     </body>
   </html>
   ```
2. Save the JavaScript code in `script.js` and open `index.html` in your browser.

---

## **2. Using the Browser Console**

The browser console is a quick and efficient tool for testing JavaScript code without any setup.

### **How to Access the Console**:

- Open Developer Tools in your browser:
  - **Chrome/Edge**: `Ctrl+Shift+I` (Windows/Linux) or `Cmd+Option+I` (Mac).
  - **Firefox**: `Ctrl+Shift+I` (Windows/Linux) or `Cmd+Option+I` (Mac).
  - **Safari**: Enable Developer Mode in Preferences and press `Cmd+Option+I`.
- Navigate to the **Console** tab.

### **Example**:

- Type the following in the console:
  ```javascript
  let greeting = "Hello, Console!";
  console.log(greeting);
  ```
- Observe the output directly in the console.

---

## **3. Using Online JavaScript Playgrounds**

Online JavaScript playgrounds provide a collaborative environment to write, test, and share JavaScript code without installing any software.

### **Popular Platforms**:

#### **JSFiddle**

- URL: [https://jsfiddle.net](https://jsfiddle.net)
- Features: Real-time HTML, CSS, and JavaScript editing with a preview of results.

#### **CodePen**

- URL: [https://codepen.io](https://codepen.io)
- Features: Showcasing and sharing code snippets.

#### **Replit**

- URL: [https://replit.com](https://replit.com)
- Features: Full IDE experience with JavaScript and other language support.

### **Example**:

1. Open any online platform (e.g., JSFiddle).
2. Enter the following JavaScript code:
   ```javascript
   alert("Welcome to JavaScript Playgrounds!");
   ```
3. Run the code and observe the output in the preview pane.

---

## **4. console.log() and Output**

The `console.log()` method is a developer's best friend for debugging and displaying information during development.

### **Usage Examples**:

#### **Basic Output**:

```javascript
console.log("Hello, World!");
```

#### **Variable Logging**:

```javascript
let name = "Alice";
console.log("Name:", name);
```

#### **Debugging Objects**:

```javascript
let user = { name: "Alice", age: 25 };
console.log("User Info:", user);
```

#### **String Interpolation**:

```javascript
let age = 25;
console.log(`User age is ${age}`);
```

---

## **Conclusion**

This chapter has introduced foundational skills for writing JavaScript code. Whether using a code editor, the browser console, or online playgrounds, you now have multiple ways to experiment with and run JavaScript. Using `console.log()` effectively will help you debug and improve your code as you progress.
