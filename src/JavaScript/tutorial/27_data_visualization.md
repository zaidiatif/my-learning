---

[<< Chapter 26](./26_web_apis.md) | [Chapter 28 >>](./28_webxr_webvr.md)

---

# Chapter 27: Data Visualization in JavaScript

## 1 Introduction

Data visualization transforms raw information into interactive, visual insights — enabling faster decision-making and storytelling. In modern web applications, JavaScript provides a robust ecosystem of visualization libraries that work across browsers and frameworks.

In this chapter, you’ll learn:

- The core principles of data visualization
- How to create charts using Chart.js, D3.js, and Recharts
- Building React-based dashboards
- Styling and interactivity techniques
- Integrating visualization with real-time APIs

## 2 Core Concepts of Data Visualization

| Concept        | Description                                                                          |
| :------------- | :----------------------------------------------------------------------------------- |
| Data Mapping   | Linking data values to visual properties (color, size, position).                    |
| Scales         | Functions that translate data domain (e.g., 0–100) to display range (e.g., 0–500px). |
| Axes & Grids   | Provide context and scale interpretation.                                            |
| Interactivity  | Enables zoom, hover, and filtering.                                                  |
| Responsiveness | Charts that adapt to screen size and orientation.                                    |

## 3 Popular JavaScript Libraries

| Library   | Use Case                        | Key Features                                       |
| :-------- | :------------------------------ | :------------------------------------------------- |
| D3.js     | Custom, low-level visualization | Fine-grained control over SVGs, scales, animations |
| Chart.js  | Quick charts                    | Easy syntax, supports line, bar, pie, radar        |
| Recharts  | React-based dashboards          | Declarative components, animations                 |
| ECharts   | Enterprise-grade visualizations | Large dataset support, 3D charts                   |
| Plotly.js | Scientific & analytical plots   | Statistical visualizations, Python integration     |

## 4 Data Visualization with Chart.js

### Installation

```bash
npm install chart.js
```

### Example: Sales Bar Chart

```html
<canvas id="salesChart" width="400" height="200"></canvas>
```

```js
import { Chart } from "chart.js/auto";

const ctx = document.getElementById("salesChart");
const salesChart = new Chart(ctx, {
  type: "bar",
  data: {
    labels: ["Jan", "Feb", "Mar", "Apr", "May"],
    datasets: [
      {
        label: "Sales (in ₹K)",
        data: [15, 25, 18, 30, 22],
        backgroundColor: [
          "#4e79a7",
          "#f28e2b",
          "#e15759",
          "#76b7b2",
          "#59a14f",
        ],
      },
    ],
  },
});
```

**Output:** A simple, responsive bar chart of monthly sales.

## 5 Custom Visualizations with D3.js

### Installation

```bash
npm install d3
```

### Example: Line Chart with D3

```js
import * as d3 from "d3";

const data = [
  { month: "Jan", sales: 150 },
  { month: "Feb", sales: 200 },
  { month: "Mar", sales: 180 },
  { month: "Apr", sales: 250 },
];

const width = 400;
const height = 200;
const svg = d3
  .select("body")
  .append("svg")
  .attr("width", width)
  .attr("height", height);

const xScale = d3
  .scalePoint()
  .domain(data.map((d) => d.month))
  .range([40, width - 20]);
const yScale = d3
  .scaleLinear()
  .domain([0, d3.max(data, (d) => d.sales)])
  .range([height - 20, 20]);

const line = d3
  .line()
  .x((d) => xScale(d.month))
  .y((d) => yScale(d.sales))
  .curve(d3.curveMonotoneX);

svg
  .append("path")
  .datum(data)
  .attr("fill", "none")
  .attr("stroke", "#007bff")
  .attr("stroke-width", 2)
  .attr("d", line);
```

**Output:** Smooth animated line chart using SVG.

## 6 React + Recharts Example

### Installation

```bash
npm install recharts
```

### Example: Interactive Dashboard

#### `SalesDashboard.tsx`

```tsx
import {
  BarChart,
  Bar,
  XAxis,
  YAxis,
  CartesianGrid,
  Tooltip,
  ResponsiveContainer,
} from "recharts";

const data = [
  { name: "Jan", revenue: 4000, profit: 2400 },
  { name: "Feb", revenue: 3000, profit: 1398 },
  { name: "Mar", revenue: 2000, profit: 9800 },
  { name: "Apr", revenue: 2780, profit: 3908 },
];

export const SalesDashboard = () => {
  return (
    <div style={{ width: "100%", height: 300 }}>
      <ResponsiveContainer>
        <BarChart
          data={data}
          margin={{ top: 20, right: 30, left: 20, bottom: 5 }}
        >
          <CartesianGrid strokeDasharray="3 3" />
          <XAxis dataKey="name" />
          <YAxis />
          <Tooltip />
          <Bar dataKey="revenue" fill="#007bff" />
          <Bar dataKey="profit" fill="#82ca9d" />
        </BarChart>
      </ResponsiveContainer>
    </div>
  );
};
```

**Output:** A responsive bar chart showing revenue vs profit with tooltips.

## 7 Real-Time Visualization (with WebSockets)

#### Example: Live stock market prices

```js
const socket = new WebSocket("wss://realtime.stockserver.com");
socket.onmessage = (event) => {
  const stockData = JSON.parse(event.data);
  updateChart(stockData);
};
```

Integrate this in D3 or Recharts by dynamically updating the chart’s state.

## 8 Dashboard Composition

You can build dashboards by combining multiple chart types:

- Line Chart: Performance trend
- Pie Chart: Market share
- Bar Chart: Monthly comparison
- Map Visualization: Regional performance

#### Example:

```html
<Grid container spacing="{2}">
  <Grid item xs="{6}"><LineChartComponent /></Grid>
  <Grid item xs="{6}"><PieChartComponent /></Grid>
</Grid>
```

## 9 Enhancing Data Visualization UX

| Technique         | Description                                                    |
| :---------------- | :------------------------------------------------------------- |
| Animations        | Use D3 transitions or Recharts animations for smooth rendering |
| Tooltips          | Provide detailed insights on hover                             |
| Legends & Filters | Toggle datasets for better clarity                             |
| Responsive Design | Use ResponsiveContainer or CSS Grid                            |
| Accessibility     | Add ARIA labels and color-blind-friendly palettes              |

## 10 Combining with APIs

#### Example: Fetching Real Data

```js
useEffect(() => {
  fetch("https://api.coindesk.com/v1/bpi/currentprice.json")
    .then((res) => res.json())
    .then((data) => setPriceData(data.bpi));
}, []);
```

Then visualize `priceData` dynamically.

## 11 Exporting Visualizations

You can export charts as:

- Images: canvas.toDataURL()
- PDFs: Using html2canvas + jsPDF
- Interactive Embeds: via iframe or web components

## 12 Recommended Libraries

| Category         | Library                     | Use Case                |
| :--------------- | :-------------------------- | :---------------------- |
| Charts           | Chart.js, Recharts, ECharts | Business dashboards     |
| Custom Visuals   | D3.js                       | Data art, maps          |
| Scientific       | Plotly.js                   | Research analytics      |
| 3D Visualization | Three.js                    | Data in 3D environments |
| Maps             | Leaflet, Mapbox GL JS       | Geo data visualization  |

## 13 Conclusion

Data visualization is the `art of storytelling through data`.
JavaScript empowers developers to:

- Build real-time, interactive dashboards
- Integrate API-driven analytics
- Create cross-framework reusable charts

By mastering D3.js, Chart.js, and Recharts, you can bridge raw data and meaningful insights, whether for finance, e-commerce, or AI-driven analytics.

---

[<< Chapter 26](./26_web_apis.md) | [Chapter 28 >>](./28_webxr_webvr.md)

---
