# Chapter 28: WebXR and WebVR in JavaScript

## 1 Introduction

The web has evolved from static pages to `immersive 3D worlds`.
`WebXR` (Web Extended Reality) is the W3C standard API that lets you build `VR (Virtual Reality)` and `AR (Augmented Reality)` experiences that run right in your browser — no app installation required.

### Key Concepts

| Term	| Description |
|:--- |:--- |
| VR (Virtual Reality)	| Fully immersive 3D environments that replace the real world (e.g., VR games). |
| AR (Augmented Reality)	| Digital elements overlaid on the real world (e.g., AR furniture preview). |
| WebXR API	| JavaScript API for interacting with AR/VR hardware (Oculus, Vive, HoloLens, etc.). |
| WebVR (Legacy)	| Early API for VR, now replaced by WebXR. |
| WebGL	| Low-level graphics API that WebXR uses to render 3D scenes. |

## 2 Why WebXR Matters

- Runs in the browser — no native apps or SDKs required.
- Cross-platform — works with phones, headsets, and desktops.
- Integrates with JavaScript frameworks (React, Vue, etc.).
- Ideal for e-commerce previews, 3D education, data visualization, and metaverse experiences.

## 3 Getting Started with WebXR

### Basic Feature Detection
```js
if (navigator.xr) {
  navigator.xr.isSessionSupported("immersive-vr").then((supported) => {
    console.log(supported ? "WebXR is supported!" : "WebXR not available.");
  });
} else {
  console.log("WebXR not supported in this browser.");
}
```

This checks whether your browser supports immersive VR mode.

## 4 Creating Your First VR Scene with A-Frame

`A-Frame` is a declarative, HTML-based framework for creating WebXR experiences using minimal code.

## Installation
```html
<script src="https://aframe.io/releases/1.5.0/aframe.min.js"></script>
```

#### Example: Simple VR Scene
```html
<a-scene>
  <a-box position="-1 0.5 -3" rotation="0 45 0" color="#4CC3D9"></a-box>
  <a-sphere position="0 1.25 -5" radius="1.25" color="#EF2D5E"></a-sphere>
  <a-cylinder position="1 0.75 -3" radius="0.5" height="1.5" color="#FFC65D"></a-cylinder>
  <a-plane position="0 0 -4" rotation="-90 0 0" width="10" height="10" color="#7BC8A4"></a-plane>
  <a-sky color="#ECECEC"></a-sky>
</a-scene>
```

Open this in a browser → You can enter VR mode and explore the 3D world.

## 5 Interactive 3D Experience with Three.js + WebXR

Three.js is the most widely used 3D library for JavaScript and provides native WebXR support.

#### Installation
```bash
npm install three
```

#### Basic WebXR Example
```js
import * as THREE from "three";
import { VRButton } from "three/examples/jsm/webxr/VRButton.js";

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.setSize(window.innerWidth, window.innerHeight);
renderer.xr.enabled = true;
document.body.appendChild(renderer.domElement);
document.body.appendChild(VRButton.createButton(renderer));

const geometry = new THREE.BoxGeometry();
const material = new THREE.MeshStandardMaterial({ color: 0x00aaff });
const cube = new THREE.Mesh(geometry, material);
scene.add(cube);

const light = new THREE.DirectionalLight(0xffffff, 1);
light.position.set(5, 5, 5).normalize();
scene.add(light);

camera.position.z = 2;

function animate() {
  cube.rotation.x += 0.01;
  cube.rotation.y += 0.01;
  renderer.render(scene, camera);
}
renderer.setAnimationLoop(animate);
```

#### Output:
A rotatable 3D cube in a WebXR-enabled VR environment.

## 6 Building an AR Experience

WebXR also supports `AR sessions` on compatible devices.

#### Example: Real-World 3D Object (AR Mode)
```js
navigator.xr.requestSession("immersive-ar", { requiredFeatures: ["hit-test"] })
  .then((session) => {
    console.log("AR Session started:", session);
  });
```

With Three.js, you can `anchor 3D objects` onto real-world surfaces detected by your camera.

## 7 Integrating WebXR with React

To make development faster, you can use `React Three Fiber` — a React renderer for Three.js.

#### Installation
```bash
npm install @react-three/fiber three @react-three/xr
```

#### Example: React VR Scene
```tsx
import { Canvas } from "@react-three/fiber";
import { VRButton, XR, Controllers } from "@react-three/xr";
import { Box } from "@react-three/drei";

export default function App() {
  return (
    <>
      <VRButton />
      <Canvas>
        <XR>
          <Controllers />
          <ambientLight />
          <Box args={[1, 1, 1]} position={[0, 1.5, -3]}>
            <meshStandardMaterial attach="material" color="orange" />
          </Box>
        </XR>
      </Canvas>
    </>
  );
}
```

#### Output:
A VR-ready React component with 3D interactions.

## 8 Advanced Features

| Feature	| Description |
|:--- |:--- |
| Controllers Input	| Detect hand gestures or VR controller buttons. |
| Teleportation	| Move around the 3D scene. |
| Physics Engine	| Use libraries like Cannon.js or Ammo.js for realism. |
| 360° Video	| Display immersive panoramic videos. |
| Haptics	| Provide tactile feedback on supported devices. |

## 9 Example Project — VR Data Visualization Room
### Goal

Build a `VR dashboard room` where users walk through and interact with floating data charts.

### Technologies

- Three.js for 3D rendering
- WebXR for VR headset support
- Chart.js for data charts as 3D textures
- React Three Fiber for React integration

### Features

- Enter a VR “data room”
- Grab and rotate charts
- Real-time data updates via WebSockets
- Themed lighting and textures

## 10 AR in E-commerce

AR is revolutionizing online shopping:

- Try furniture or fashion virtually.
- Use WebXR + 3D models (GLTF/GLB).
- Integrate with payment and analytics.

Example:

```js
<a-entity gltf-model="url(models/chair.glb)" scale="2 2 2" position="0 0 -3"></a-entity>
```

Preview a product in your real-world space before purchase.

## 11 Tools and Frameworks

| Tool	| Use Case |
|:--- |:--- |
| Three.js	| 3D engine for WebXR |
| A-Frame	| Declarative VR/AR |
| React Three Fiber	| React + 3D |
| Babylon.js	| Full 3D engine with XR tools |
| 8th Wall / ZapWorks	| Commercial AR SDKs |
| Blender	| For creating 3D models (export as GLTF/GLB) |

## 12 Performance Optimization

- Use low-poly models for mobile VR.
- Compress textures (WebP, Basis).
- Frustum culling — render only what’s visible.
- Use Web Workers for heavy logic.
- Lazy-load 3D assets via GLTFLoader.

## 13 Future of WebXR

- WebGPU integration for high-performance rendering.
- Hand tracking and body pose recognition.
- Spatial anchors for shared AR experiences.
- Metaverse interoperability via OpenXR.

## 14 Summary

| Concept	| Example |
|:--- |:--- |
| Basic VR Scene	| A-Frame `<a-scene>` |
| Custom 3D	| Three.js with WebXR |
| React Integration	| React Three Fiber + XR |
| AR Experience	| Real-world object placement |
| Applications	| Games, learning, product previews, data rooms |
