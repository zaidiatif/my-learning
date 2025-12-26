---

[<< Chapter 28](./29_webAssembly.md) | [Chapter 30 >>](./30_building_progressive_web_apps.md)

---

# Chapter 29: WebAssembly

WebAssembly (Wasm) is a binary instruction format that allows high-performance execution of code on web browsers. It enables developers to run languages like C, C++, and Rust on the web, opening up possibilities for computationally intensive tasks in a web environment.

---

## 1. Introduction to WebAssembly (Wasm)

WebAssembly is designed as a portable compilation target for programming languages, enabling efficient execution and compact binary code. It runs in a safe and sandboxed environment within web browsers.

Example:

```javascript
WebAssembly.instantiateStreaming(fetch("module.wasm")).then((result) => {
  const exports = result.instance.exports;
  console.log("Result from WebAssembly:", exports.add(2, 3));
});
```

---

## 2. Why Use WebAssembly?

WebAssembly provides several benefits:

- **High Performance**: Near-native execution speed for complex operations.
- **Language Interoperability**: Use languages other than JavaScript.
- **Compact Binary Format**: Smaller download sizes and faster load times.
- **Security**: Runs in a secure sandboxed environment.

---

## 3. The WebAssembly Execution Model

WebAssembly runs in a virtual machine with its own execution
model. Key components include:

- **Linear Memory**: A contiguous array of bytes accessible by WebAssembly code.
- **Modules**: Self-contained units of functionality.
- **Exports and Imports**: Interfaces for communication between WebAssembly and JavaScript.

---

## 3.1 Modules, Imports/Exports, Memory, and Tables

- Modules export functions, memories, tables (function refs), and globals; JS provides imports.

```javascript
const imports = {
  env: {
    jsLog: (x) => console.log("wasm says", x),
  },
};
const { instance, module } = await WebAssembly.instantiateStreaming(
  fetch("mod.wasm"),
  imports
);
instance.exports.main?.();
```

---

## 4. Writing WebAssembly Modules

To create WebAssembly, you typically write code in a language like C or Rust and compile it to Wasm using tools like Emscripten or Rust's wasm-bindgen.

### Example in C:

```c
#include <stdio.h>

int add(int a, int b) {
    return a + b;
}
```

Compile to Wasm using:

```bash
emcc add.c -s WASM=1 -o add.wasm
```

---

## 5. Interfacing JavaScript with WebAssembly for High-Performance Applications

JavaScript serves as the bridge to load and interact with WebAssembly modules. By offloading computationally expensive tasks to Wasm, developers can achieve significant performance gains.

Example:

```javascript
const wasmModule = await WebAssembly.instantiateStreaming(fetch("module.wasm"));
const add = wasmModule.instance.exports.add;
console.log("2 + 3 =", add(2, 3));
```

Use cases include:

- Complex mathematical computations
- Real-time data processing
- Rendering graphics or animations

---

## 6. Passing Data: Numbers, Arrays, and Strings

- Wasm uses a linear memory (ArrayBuffer). Pass arrays/strings by writing into memory and passing pointers/lengths.

```javascript
// JS writes bytes into wasm memory
const mem = instance.exports.memory; // WebAssembly.Memory
const view = new Uint8Array(mem.buffer);
const encoder = new TextEncoder();
const str = "hello";
const bytes = encoder.encode(str);
const ptr = instance.exports.malloc(bytes.length);
view.set(bytes, ptr);
instance.exports.process(ptr, bytes.length);
instance.exports.free(ptr);
```

- i64 values map to JS BigInt: import/export signatures use BigInt.

```javascript
// Exported i64 function must be called with BigInt
instance.exports.add64(1n, 2n);
```

---

## 7. Real-World Use Cases

- **Gaming**: Porting high-performance games to the web.
- **Image and Video Processing**: Running computationally expensive algorithms.
- **Cryptography**: Secure and fast encryption and decryption.
- **Machine Learning**: Accelerating model inference in browsers.

---

## 8. Performance Optimization with WebAssembly

WebAssembly is inherently optimized for speed, but further optimizations can enhance its performance:

### Tips for Optimization:

- Minimize the size of Wasm modules to reduce download times.
- Use smaller data types where possible to save memory.
- Optimize the source code before compilation (e.g., use `-O3` for GCC or Clang).
- Leverage browser caching for Wasm modules.

### Streaming Compilation and Caching

- Prefer `instantiateStreaming(fetch(url))` (with correct MIME `application/wasm`) for parse+compile during download.
- Fallback when server lacks MIME:

```javascript
const res = await fetch("mod.wasm");
const bytes = await res.arrayBuffer();
const mod = await WebAssembly.compile(bytes); // cache this Module
const inst = await WebAssembly.instantiate(mod, imports);
```

### Debugging

- Use browser developer tools to inspect and debug Wasm modules.
- Tools like wasm-pack and wasm-bindgen provide better debugging workflows.

Debugging tools like Chrome DevTools allow you to monitor Wasm performance metrics in real-time.

---

## 9. Compiling Other Languages to WebAssembly (C, C++, Rust) and Using it Within JavaScript

To create WebAssembly, you typically write code in a language like C, C++, or Rust and compile it to Wasm using tools like Emscripten or Rust's wasm-bindgen.

### Example in C:

```c
#include <stdio.h>

int add(int a, int b) {
    return a + b;
}
```

Compile to Wasm using:

```bash
emcc add.c -s WASM=1 -o add.wasm
```

### Using Rust:

```rust
#[no_mangle]
pub extern fn add(a: i32, b: i32) -> i32 {
    a + b
}
```

Compile to Wasm using:

```bash
cargo build --target wasm32-unknown-unknown --release
```

Integrate the compiled Wasm module into a JavaScript application to utilize its functionality efficiently.

---

## 10. Advanced: Threads, SIMD, Exceptions, and WASI (Brief)

- Threads: `--shared-memory` + `SharedArrayBuffer` enable wasm threads (cross-origin isolation required). Use atomics for synchronization.
- SIMD: 128-bit vector ops for data-parallel speedups (enable by default in modern engines).
- Exceptions: Exception Handling proposal adds try/catch to wasm; toolchains can target it progressively.
- WASI: System interface for non-web hosts (Node/Deno/servers) to access files, clocks, etc.

---

## 11. Security and Limitations

- No direct DOM access; interact through JS. Memory is sandboxed and bounds-checked.
- Watch for copy overhead JS↔Wasm; batch and use shared memory where possible.
- Ensure correct MIME type; enable COOP/COEP (cross-origin isolation) for threads and high-res timers.

---

WebAssembly is a transformative technology that bridges the gap between native application performance and the web's universal accessibility. Mastering Wasm empowers developers to build high-performance applications in the browser and beyond.

---

[<< Chapter 28](./28_webxr_webvr.md) | [Chapter 30 >>](./30_building_progressive_web_apps.md)

---
