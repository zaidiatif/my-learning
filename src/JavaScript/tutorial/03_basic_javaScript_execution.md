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

## **2. Development Environments**

### **Browser Developer Tools**

Modern web browsers come equipped with powerful developer tools that are essential for JavaScript development and debugging.

#### **Accessing Developer Tools**:

- **Chrome/Edge**: Press `F12` or `Ctrl+Shift+I` (Windows/Linux) or `Cmd+Option+I` (Mac)
- **Firefox**: Press `F12` or `Ctrl+Shift+I` (Windows/Linux) or `Cmd+Option+I` (Mac)
- **Safari**: Enable Developer Mode in Preferences, then press `Cmd+Option+I`

#### **Console Tab Features**:

- **Execute JavaScript**: Type JavaScript code directly and press Enter
- **View Output**: See results of `console.log()`, errors, and warnings
- **Interactive Testing**: Test code snippets without creating files

**Example**:
```javascript
// Type this directly in the browser console
let message = "Hello from the console!";
console.log(message);
```

#### **Sources Tab**:

- **Set Breakpoints**: Click on line numbers to pause execution
- **Step Through Code**: Use F10 (step over) and F11 (step into)
- **Watch Variables**: Monitor variable values during execution

### **Local Development Setup**

#### **Creating a Local Project**:

1. **Create Project Folder**:
   ```bash
   mkdir my-javascript-project
   cd my-javascript-project
   ```

2. **Create HTML File**:
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>JavaScript Project</title>
   </head>
   <body>
       <h1>My JavaScript Project</h1>
       <script src="app.js"></script>
   </body>
   </html>
   ```

3. **Create JavaScript File** (`app.js`):
   ```javascript
   console.log("Project loaded successfully!");
   ```

4. **Open in Browser**: Double-click the HTML file or use a local server

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

## **4. Basic Output and Debugging**

### **console.log() and Console Methods**

The `console.log()` method is a developer's best friend for debugging and displaying information during development.

#### **Usage Examples**:

**Basic Output**:
```javascript
console.log("Hello, World!");
```

**Variable Logging**:
```javascript
let name = "Alice";
console.log("Name:", name);
```

**Debugging Objects**:
```javascript
let user = { name: "Alice", age: 25 };
console.log("User Info:", user);
```

**String Interpolation**:
```javascript
let age = 25;
console.log(`User age is ${age}`);
```

**Multiple Console Methods**:
```javascript
console.log("Regular message");
console.info("Informational message");
console.warn("Warning message");
console.error("Error message");
console.table([{name: "Alice", age: 25}, {name: "Bob", age: 30}]);
```

### **Alert and Prompt**

#### **alert()**:
Displays a modal dialog with a message and an OK button.

```javascript
alert("This is an alert message!");
```

#### **prompt()**:
Displays a modal dialog with a message, input field, and OK/Cancel buttons.

```javascript
let userName = prompt("What is your name?");
console.log("Hello, " + userName + "!");
```

#### **confirm()**:
Displays a modal dialog with a message and OK/Cancel buttons, returns true/false.

```javascript
let userConfirmed = confirm("Do you want to continue?");
if (userConfirmed) {
    console.log("User clicked OK");
} else {
    console.log("User clicked Cancel");
}
```

### **Document Write**

**document.write()** writes content directly to the HTML document.

```javascript
document.write("<h1>Hello from JavaScript!</h1>");
document.write("<p>This text was added by JavaScript.</p>");
```

**Note**: `document.write()` should be used sparingly as it can overwrite the entire document if called after the page loads.

---

## **5. Running JavaScript**

### **Browser Runtime**

JavaScript runs in the browser using the browser's JavaScript engine (V8 in Chrome, SpiderMonkey in Firefox).

#### **Features Available in Browser**:
- DOM manipulation
- Browser APIs (localStorage, fetch, etc.)
- Window object
- Document object

**Example**:
```javascript
// Browser-specific code
document.getElementById("myButton").addEventListener("click", function() {
    alert("Button clicked!");
});
```

### **Node.js Runtime**

Node.js allows JavaScript to run on the server-side using the V8 engine.

#### **Running JavaScript with Node.js**:

1. **Create a file** (`server.js`):
   ```javascript
   console.log("Hello from Node.js!");
   console.log("Current directory:", process.cwd());
   ```

2. **Run the file**:
   ```bash
   node server.js
   ```

3. **Interactive Mode**:
   ```bash
   node
   # Then type JavaScript code directly
   ```

#### **Node.js vs Browser Differences**:

| Feature | Browser | Node.js |
|---------|---------|---------|
| Global Object | `window` | `global` |
| DOM Access | Yes | No |
| File System | No | Yes (fs module) |
| Process Info | Limited | Yes (process object) |

### **Different Execution Contexts**

#### **Global Context**:
```javascript
// Variables declared here are global
var globalVar = "I'm global";
let globalLet = "I'm also global";
```

#### **Function Context**:
```javascript
function myFunction() {
    // Variables declared here are local to the function
    var localVar = "I'm local";
    console.log(globalVar); // Can access global variables
}
```

#### **Block Context**:
```javascript
if (true) {
    let blockVar = "I'm block-scoped";
    const blockConst = "I'm also block-scoped";
}
// blockVar and blockConst are not accessible here
```

---

## **6. Debugging Basics**

### **Common JavaScript Errors**

#### **Syntax Errors**:
```javascript
// Missing semicolon or bracket
let name = "Alice"
console.log(name); // SyntaxError: Unexpected token
```

#### **Reference Errors**:
```javascript
console.log(undefinedVariable); // ReferenceError: undefinedVariable is not defined
```

#### **Type Errors**:
```javascript
let number = 42;
number.toUpperCase(); // TypeError: number.toUpperCase is not a function
```

### **Debugging Techniques**

#### **Using console.log() for Debugging**:
```javascript
function calculateTotal(price, tax) {
    console.log("Price:", price);
    console.log("Tax:", tax);
    
    let total = price + (price * tax);
    console.log("Total:", total);
    
    return total;
}
```

#### **Using Breakpoints**:
1. Open Developer Tools (F12)
2. Go to Sources tab
3. Click on line number to set breakpoint
4. Refresh page or trigger the code
5. Use Step Over (F10) and Step Into (F11) to debug

#### **Error Handling**:
```javascript
try {
    let result = riskyOperation();
    console.log("Success:", result);
} catch (error) {
    console.error("Error occurred:", error.message);
}
```

---

## **7. Enhanced Examples**

### **Interactive Example**:
```html
<!DOCTYPE html>
<html>
<head>
    <title>Interactive JavaScript</title>
</head>
<body>
    <h1>JavaScript Calculator</h1>
    <input type="number" id="num1" placeholder="First number">
    <input type="number" id="num2" placeholder="Second number">
    <button onclick="calculate()">Calculate Sum</button>
    <p id="result"></p>

    <script>
        function calculate() {
            let num1 = parseFloat(document.getElementById("num1").value);
            let num2 = parseFloat(document.getElementById("num2").value);
            
            if (isNaN(num1) || isNaN(num2)) {
                alert("Please enter valid numbers!");
                return;
            }
            
            let sum = num1 + num2;
            document.getElementById("result").textContent = `Sum: ${sum}`;
            console.log(`Calculated: ${num1} + ${num2} = ${sum}`);
        }
    </script>
</body>
</html>
```

### **Node.js Example**:
```javascript
// file: calculator.js
const readline = require('readline');

const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
});

rl.question('Enter first number: ', (num1) => {
    rl.question('Enter second number: ', (num2) => {
        let sum = parseFloat(num1) + parseFloat(num2);
        console.log(`Sum: ${sum}`);
        rl.close();
    });
});
```

Run with: `node calculator.js`

---

## **Conclusion**

This chapter has introduced foundational skills for writing and executing JavaScript code. You've learned multiple ways to run JavaScript (browser, Node.js, online playgrounds), different output methods (console.log, alert, prompt, document.write), and basic debugging techniques. Understanding execution contexts and error handling will help you write more robust JavaScript applications as you progress through the curriculum.
