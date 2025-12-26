---

[<< Chapter 29](./29_webAssembly.md) | [Chapter 31 >>](./31_static_site_generation_jamstack.md)

---

# Chapter 30: Building Progressive Web Apps (PWAs)

Progressive Web Apps (PWAs) combine the best features of web and mobile applications, offering users a seamless, reliable, and engaging experience. PWAs are designed to be fast, installable, and capable of working offline or on low-quality networks. This chapter covers the key concepts, technologies, and steps for building PWAs.

---

## 1. Introduction to PWAs

Progressive Web Apps aim to provide a native-like experience on the web. They leverage modern web technologies to bridge the gap between websites and mobile apps.

### Key Benefits of PWAs:

- **Cross-Platform Compatibility**: Works on all modern browsers and devices.
- **Offline Access**: Operates even when there is no internet connection.
- **App-Like Feel**: Mimics the look and feel of a native app.
- **Installable**: Users can add PWAs to their home screen.

---

## 2. Core PWA Features: Fast, Reliable, and Engaging

### Fast

PWAs load quickly and provide a smooth user experience by minimizing resource usage and prioritizing performance.

### Reliable

Service workers enable reliable performance even in unreliable network conditions.

### Engaging

PWAs support push notifications, offline functionality, and an immersive full-screen mode.

---

## 3. Service Workers: Enabling Offline Capabilities and Caching Strategies

A service worker is a JavaScript file that runs in the background and intercepts network requests. It is key to enabling offline capabilities and caching.

Service workers and caching strategies form the backbone of PWA performance optimization. Proper caching ensures fast loading and reliable offline functionality.

### Cache-Control, ETag, and Last-Modified

- **Cache-Control**: Specifies caching policies for the browser and intermediate caches.
- **ETag**: Provides a unique identifier for resource versions, allowing validation and conditional requests.
- **Last-Modified**: Indicates the last time the resource was modified to validate cache freshness.

### Registering a Service Worker:

```javascript
if ("serviceWorker" in navigator) {
  navigator.serviceWorker
    .register("/service-worker.js")
    .then(() => console.log("Service Worker registered successfully."))
    .catch((error) =>
      console.error("Service Worker registration failed:", error)
    );
}
```

### Example Service Worker with Caching:

```javascript
self.addEventListener("install", (event) => {
  event.waitUntil(
    caches.open("my-pwa-cache").then((cache) => {
      return cache.addAll(["/", "/index.html", "/styles.css", "/app.js"]);
    })
  );
});

self.addEventListener("fetch", (event) => {
  event.respondWith(
    caches.match(event.request).then((response) => {
      return response || fetch(event.request);
    })
  );
});
```

---

## 4. Caching Strategies for PWAs

Caching strategies determine how resources are stored and retrieved to optimize performance.

### Common Strategies:

- **Cache First**: Serve resources from the cache, falling back to the network if not available.
- **Network First**: Fetch resources from the network, falling back to the cache if offline.
- **Stale While Revalidate**: Serve from the cache while fetching an updated version in the background.

---

## 4.1 Runtime Caching Recipes (Practical)

- Cache-first for static assets; Network-first for HTML/API; Stale-While-Revalidate for images/CSS.

```javascript
self.addEventListener("fetch", (event) => {
  const req = event.request;
  if (req.destination === "document") {
    // Network-first for documents
    event.respondWith(
      (async () => {
        try {
          const res = await fetch(req);
          const cache = await caches.open("pages");
          cache.put(req, res.clone());
          return res;
        } catch {
          const cache = await caches.open("pages");
          return cache.match(req) || cache.match("/offline.html");
        }
      })()
    );
  }
});
```

Include a minimal `/offline.html` in your app and pre-cache it during `install`:

```html
<!DOCTYPE html>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Offline</title>
<h1>You are offline</h1>
<p>Please check your connection. Some content may be unavailable.</p>
```

---

## 5. The Web App Manifest: Making Your App Installable

The Web App Manifest is a JSON file that provides metadata about your application, enabling it to be installed on a user’s device.

### Example Manifest:

```json
{
  "name": "My PWA",
  "short_name": "PWA",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#000000",
  "icons": [
    {
      "src": "/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "/icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}
```

---

## 6. Offline-First Strategy

The offline-first strategy ensures that PWAs function effectively even without an active internet connection. This approach prioritizes serving cached resources while falling back to network requests only when necessary.

### Benefits:

- Improved user experience in poor network conditions.
- Reduced server load due to reliance on cached resources.

### Implementation Example:

```javascript
self.addEventListener("fetch", (event) => {
  event.respondWith(
    caches.match(event.request).then((cachedResponse) => {
      return cachedResponse || fetch(event.request);
    })
  );
});
```

---

## 7. Push Notifications and Background Sync

### Push Notifications

Push notifications allow PWAs to re-engage users with timely updates and information.

#### Example:

```javascript
self.addEventListener("push", (event) => {
  const options = {
    body: event.data.text(),
    icon: "/icon-192x192.png",
  };
  event.waitUntil(
    self.registration.showNotification("Notification Title", options)
  );
});
```

### Background Sync

Background sync ensures essential tasks, such as form submissions or data uploads, are completed when the user is back online.

#### Example:

```javascript
self.addEventListener("sync", (event) => {
  if (event.tag === "sync-data") {
    event.waitUntil(syncData());
  }
});

function syncData() {
  // Logic to sync data with the server
}
```

---

## 7.1 Periodic Background Sync and Background Fetch (where available)

```javascript
// Periodic background sync registration
if ("periodicSync" in registration) {
  try {
    await registration.periodicSync.register("content-sync", {
      minInterval: 24 * 60 * 60 * 1000,
    });
  } catch {}
}
```

---

## 8. Best Practices for Building PWAs

- Use HTTPS to ensure secure communication.
- Optimize for performance and accessibility.
- Regularly update service workers to manage changes.
- Test PWAs on different devices and browsers.

### Lighthouse and Web Vitals

- Use Lighthouse PWA audit; track LCP, CLS, INP. Improve with preloading, code-splitting, image optimization.

### Offline Fallbacks

- Provide `/offline.html` and cache it; handle navigation fallback in the service worker.

### Installability and PWA UI

- Handle `beforeinstallprompt`; provide an install button; tune manifest icons and display modes.

```javascript
let deferredPrompt;
window.addEventListener("beforeinstallprompt", (e) => {
  e.preventDefault();
  deferredPrompt = e;
  installButton.hidden = false;
});

installButton.addEventListener("click", async () => {
  installButton.hidden = true;
  if (!deferredPrompt) return;
  deferredPrompt.prompt();
  const { outcome } = await deferredPrompt.userChoice;
  console.log("PWA install:", outcome);
  deferredPrompt = null;
});
```

### Security and Privacy

- Ensure HTTPS, proper `Permissions-Policy`, and limit data persisted offline.

---

Progressive Web Apps are a powerful tool for delivering fast, reliable, and engaging user experiences. By implementing service workers, leveraging caching strategies, and adopting an offline-first approach, developers can create robust applications that function seamlessly in various network conditions.

---

[<< Chapter 29](./29_webAssembly.md) | [Chapter 31 >>](./31_static_site_generation_jamstack.md)

---
