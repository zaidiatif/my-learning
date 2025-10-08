# Chapter 26: Web APIs

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

## 1.1 Fetch: AbortController, Timeouts, and Streaming

- Cancel requests and set timeouts with `AbortController`; stream responses via the Streams API.

```javascript
const ac = new AbortController();
const t = setTimeout(() => ac.abort(), 5000);
const res = await fetch('/data', { signal: ac.signal });
clearTimeout(t);

// Streaming NDJSON
const reader = res.body.getReader();
const decoder = new TextDecoder();
let buf = '';
for (;;) {
  const { value, done } = await reader.read();
  if (done) break;
  buf += decoder.decode(value, { stream: true });
  let i; while ((i = buf.indexOf('\n')) >= 0) { handle(buf.slice(0, i)); buf = buf.slice(i + 1); }
}
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

## 2.1 BroadcastChannel and PostMessage

- Communicate across tabs/windows/origin with `BroadcastChannel`; use `postMessage` for same-window cross-context.
```javascript
const bc = new BroadcastChannel('updates');
bc.onmessage = (e) => console.log(e.data);
bc.postMessage({ type: 'ping' });
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

## 3.1 Cache API and Service Workers (Brief)

- Cache requests for offline/fast repeat loads; integrate with a Service Worker fetch handler.
```javascript
// In SW
self.addEventListener('fetch', (event) => {
  event.respondWith((async () => {
    const cache = await caches.open('v1');
    const cached = await cache.match(event.request);
    if (cached) return cached;
    const res = await fetch(event.request);
    cache.put(event.request, res.clone());
    return res;
  })());
});
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

## 9. Clipboard and Web Share API

- Read/write clipboard (requires permissions); invoke native share sheets on supported devices.
```javascript
await navigator.clipboard.writeText('hello');
const text = await navigator.clipboard.readText();
if (navigator.share) await navigator.share({ title: 'My App', url: location.href });
```

---

## 10. Permissions API

- Query and react to permission state changes.
```javascript
const status = await navigator.permissions.query({ name: 'geolocation' });
console.log(status.state);
status.onchange = () => console.log('perm changed', status.state);
```

---

## 11. Web Crypto API

- Secure random, hashing, and key-based crypto.
```javascript
const bytes = crypto.getRandomValues(new Uint8Array(16));
const data = new TextEncoder().encode('hello');
const hash = await crypto.subtle.digest('SHA-256', data);
```

---

## 12. URL, URLSearchParams, and History API

```javascript
const url = new URL('https://example.com/page?x=1');
url.searchParams.set('q', 'test');
history.pushState({ q: 'test' }, '', url);
window.addEventListener('popstate', (e) => console.log('state', e.state));
```

---

## 13. Performance, Navigation Timing, and Long Tasks

```javascript
new PerformanceObserver((list) => {
  for (const e of list.getEntries()) console.log(e.entryType, e.name, e.duration);
}).observe({ type: 'longtask', buffered: true });
console.log(performance.getEntriesByType('navigation')[0]);
```

---

## 14. Web Locks API

- Coordinate resource access across tabs/workers.
```javascript
await navigator.locks.request('my-resource', async (lock) => {
  // exclusive section
});
```

---

## 15. Device and Sensor APIs (Brief)

- Battery, Network Information, Device Memory, Sensor APIs (availability varies).
```javascript
if (navigator.getBattery) {
  const b = await navigator.getBattery();
  console.log(b.level, b.charging);
}
```

---

By leveraging these Web APIs, developers can build rich, interactive web applications with enhanced functionality and seamless user experiences.
