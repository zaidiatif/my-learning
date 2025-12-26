# Chapter 56: JavaScript for IoT (Internet of Things)

---

[<< Chapter 55](./55_legal_ethical_considerations.md)

---

This chapter explores how JavaScript can be used to build Internet of Things (IoT) applications, from controlling microcontrollers to handling communication protocols. It covers hardware programming frameworks, IoT protocols, and practical use cases.

## 1. JavaScript on Microcontrollers

Traditionally, IoT devices were programmed using C, C++, or Python. Modern frameworks allow JavaScript to control sensors, actuators, and embedded devices, making IoT development more accessible for web developers.

### a. Johnny-Five

- Overview: A JavaScript framework for robotics and IoT using Node.js.
- Supported Platforms: Arduino, Raspberry Pi, Intel Edison, BeagleBone, and more.
- Key Features:
- - Control LEDs, motors, sensors, buttons.
- - Event-driven API similar to Node.js.
- Example: Blink an LED

```js
const { Board, Led } = require("johnny-five");
const board = new Board();

board.on("ready", () => {
  const led = new Led(13);
  led.blink(500); // Blink every 500ms
});
```

### b. Espruino

- Overview: JavaScript interpreter for microcontrollers with extremely low memory usage.
- Supported Hardware: STM32 boards, Puck.js, ESP8266, ESP32.
- Key Features:
- - Runs JS directly on the device (no Node.js required).
- - Ideal for lightweight IoT devices.
- Example: Read a button input

```js
setWatch(
  function () {
    console.log("Button pressed!");
  },
  BTN,
  { repeat: true, edge: "rising", debounce: 50 }
);
```

### c. Other Notable Frameworks

| Framework        | Description                                                   |
| :--------------- | :------------------------------------------------------------ |
| Node-RED         | Visual programming for IoT flows, integrates JavaScript logic |
| Tessel 2         | Microcontroller with built-in Node.js support                 |
| Espruino Web IDE | Browser-based JS editor for Espruino devices                  |

## 2. IoT Protocols

IoT devices need efficient communication protocols because they often operate on low bandwidth and low power.

### a. MQTT (Message Queuing Telemetry Transport)

- Lightweight publish/subscribe messaging protocol.
- Ideal for sensor networks.
- Example using mqtt Node.js library

```js
const mqtt = require("mqtt");
const client = mqtt.connect("mqtt://broker.hivemq.com");

client.on("connect", () => {
  client.subscribe("iot/sensors/temp", () => {
    console.log("Subscribed to temperature topic");
  });
});

client.on("message", (topic, message) => {
  console.log(`${topic}: ${message.toString()}`);
});
```

### b. CoAP (Constrained Application Protocol)

- Lightweight REST-like protocol for resource-constrained devices.
- Supports GET, PUT, POST, DELETE requests.
- Works over UDP, low overhead.

### c. WebSockets

- Real-time bidirectional communication.
- Can be used when low latency updates are required.

### d. HTTP/HTTPS

- Standard protocol for cloud-connected IoT devices.
- Often combined with REST APIs for device management.

## 3. Common Use Cases of JavaScript in IoT

| Use Case                 | Example                                                                 |
| :----------------------- | :---------------------------------------------------------------------- |
| Home Automation          | Smart lights, thermostats, security cameras using Node.js + Johnny-Five |
| Industrial IoT           | Sensor monitoring in factories, predictive maintenance                  |
| Wearables                | Fitness trackers sending data to cloud services                         |
| Environmental Monitoring | Weather stations or pollution sensors reporting via MQTT                |
| Robotics                 | Controlling robots or drones with real-time JS logic                    |

## 4. Edge Computing with IoT

- Devices often process data locally (edge computing) before sending it to the cloud.
- Using JavaScript frameworks, you can:
- - Filter or aggregate sensor data.
- - Trigger local actuators without cloud dependency.
- - Reduce latency and network usage.

## 5. Best Practices

- Security First: Secure MQTT, CoAP, or WebSocket connections. Use TLS/SSL.
- Low Power Awareness: Optimize loops and sensor polling.
- Error Handling: Devices may disconnect; implement reconnection strategies.
- Modular Design: Separate sensor reading, processing, and communication code.
- Testing on Simulators: Use simulators before deploying to hardware.

## 6. Summary Table

| Concept        | Description                                                 |
| :------------- | :---------------------------------------------------------- |
| Johnny-Five    | Node.js framework for robotics and microcontrollers         |
| Espruino       | Lightweight JS interpreter for microcontrollers             |
| MQTT           | Lightweight pub/sub protocol for IoT communication          |
| CoAP           | UDP-based REST protocol for constrained devices             |
| Edge Computing | Processing data locally on devices to reduce latency        |
| Use Cases      | Home automation, industrial monitoring, robotics, wearables |

## 7. Conclusion

JavaScript has expanded from the browser to the physical world. By using frameworks like Johnny-Five and Espruino, and combining them with protocols like MQTT and WebSockets, developers can build interactive, real-time IoT systems with ease.

**Key takeaway:** With JavaScript, the barrier to entry for IoT development is much lower, enabling developers to prototype and deploy connected devices quickly and efficiently.

---

[<< Chapter 55](./55_legal_ethical_considerations.md)

---
