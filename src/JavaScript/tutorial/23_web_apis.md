# Chapter 23: Web APIs

Web APIs are essential tools that allow developers to interact with the browser and extend application functionality. This chapter explores a variety of Web APIs, their usage, and best practices for integration in modern web applications.

---

## 1. Introduction to Web APIs

Web APIs provide pre-defined interfaces for interacting with browser functionalities and other web resources. They simplify tasks like making network requests, storing data, or accessing device features.

Example:

```javascript
// Using the console API
console.log("Hello, Web APIs!");
```

---

## 1. XMLHttpRequest, Fetch API, and Axios

### XMLHttpRequest

The XMLHttpRequest object is an older way to make HTTP requests in JavaScript.

```javascript
const xhr = new XMLHttpRequest();
xhr.open("GET", "https://api.example.com/data", true);
xhr.onload = function () {
  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.responseText));
  }
};
xhr.send();
```

### Fetch API

Fetch is a modern, promise-based way to make HTTP requests.

```javascript
fetch("https://api.example.com/data")
  .then((response) => response.json())
  .then((data) => console.log(data))
  .catch((error) => console.error("Error:", error));
```

### Axios

Axios is a popular third-party library for making HTTP requests.

```javascript
axios
  .get("https://api.example.com/data")
  .then((response) => console.log(response.data))
  .catch((error) => console.error("Error:", error));
```

---

## 2. WebSockets, Server-Sent Events, and WebRTC

### WebSockets

WebSockets enable bidirectional, real-time communication between the browser and the server.

```javascript
const socket = new WebSocket("ws://example.com/socket");

socket.onopen = () => console.log("WebSocket connection opened");
socket.onmessage = (event) => console.log("Message from server:", event.data);
socket.onclose = () => console.log("WebSocket connection closed");

socket.send("Hello, server!");
```

### Server-Sent Events (SSE)

SSE allows servers to push updates to the browser.

```javascript
const eventSource = new EventSource("/events");
eventSource.onmessage = (event) => console.log("Update:", event.data);
```

### WebRTC for Peer-to-Peer Communication

WebRTC allows browsers to establish peer-to-peer connections for real-time video, audio, and data sharing.

```javascript
const peerConnection = new RTCPeerConnection();
peerConnection.onicecandidate = (event) => {
  if (event.candidate) {
    console.log("New ICE candidate:", event.candidate);
  }
};
peerConnection
  .createOffer()
  .then((offer) => peerConnection.setLocalDescription(offer));
```

---

## 3. Working with Web Storage (IndexedDB, localStorage, sessionStorage, Cookies)

### localStorage and sessionStorage

The Web Storage API provides mechanisms for storing key-value
pairs in the browser. Use **localStorage** for persistent
data and **sessionStorage** for session-scoped data.

```javascript
localStorage.setItem("key", "value");
console.log(localStorage.getItem("key"));
sessionStorage.setItem("sessionKey", "sessionValue");
```

### Cookies

```javascript
document.cookie = "user=John; expires=Fri, 31 Dec 2023 23:59:59 GMT";
console.log(document.cookie);
```

### IndexedDB

```javascript
const request = indexedDB.open("MyDatabase", 1);
request.onupgradeneeded = (event) => {
  const db = event.target.result;
  db.createObjectStore("store", { keyPath: "id" });
};
```

---

## 4. Canvas API (2D/3D Graphics) for Drawing Graphics

The Canvas API provides a way to draw graphics and animations directly in the browser using JavaScript.

```javascript
const canvas = document.getElementById("myCanvas");
const ctx = canvas.getContext("2d");
ctx.fillStyle = "green";
ctx.fillRect(10, 10, 100, 100);
```

---

## 5. Web Audio API

The Web Audio API allows developers to process and synthesize audio in web applications.

```javascript
const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
const oscillator = audioCtx.createOscillator();
oscillator.type = "sine";
oscillator.frequency.setValueAtTime(440, audioCtx.currentTime);
oscillator.connect(audioCtx.destination);
oscillator.start();
```

---

## 6. Geolocation API

The Geolocation API allows web applications to access the geographical location of the user. User permission is required for access.

```javascript
navigator.geolocation.getCurrentPosition(
  (position) => {
    console.log("Latitude:", position.coords.latitude);
    console.log("Longitude:", position.coords.longitude);
  },
  (error) => {
    console.error("Error:", error);
  }
);
```

---

## 7. Notifications and Push API

The Notifications API sends system-level notifications to the user. The Push API enables sending notifications from the server to the client.

```javascript
Notification.requestPermission().then((permission) => {
  if (permission === "granted") {
    new Notification("Hello, Notifications!");
  }
});
```

---

## 8. File API and File Handling

The File API allows users to read and manipulate files.

```javascript
const fileInput = document.getElementById("fileInput");
fileInput.addEventListener("change", (event) => {
  const file = event.target.files[0];
  const reader = new FileReader();
  reader.onload = (e) => console.log(e.target.result);
  reader.readAsText(file);
});
```

---

By leveraging these Web APIs, developers can build rich, interactive web applications with enhanced functionality and seamless user experiences.
