---

[<< Chapter 14](./14_accessibility_in_javascript.md) | [Chapter 16 >>](./16_javaScript_design_patterns.md)

---

# Chapter 15: Web Components and Cross-Framework Design Systems

This chapter represents a complete roadmap for mastering Web Components — from fundamental concepts to enterprise-level design system integration.
Let’s break down the main sections and subtopics so you fully understand what each covers and how they connect.

## 1. Understanding Web Components

### What It Is:

Web Components are native browser features that let you create custom HTML elements that work across frameworks without extra dependencies.

### Why It Matters:

They make it possible to build modular, reusable UI components that are framework-agnostic — meaning the same button can run in React, Angular, or even a plain HTML app.

### Key Concepts:

- Custom Elements — Define your own HTML tags.
- Shadow DOM — Encapsulates a component’s DOM and CSS.
- HTML Templates — Predefined markup fragments.
- ES Modules — Enable modern, modular code sharing.

## 2. Creating and Using a Web Component

### Goal:

Learn how to define and render your first custom element with JavaScript.

### Example:

```js
class HelloWorld extends HTMLElement {
  connectedCallback() {
    this.innerHTML = `<p>Hello Web Components!</p>`;
  }
}
customElements.define("hello-world", HelloWorld);
```

### Explanation:

- `connectedCallback()` runs when the element is added to the DOM.
- This example doesn’t use Shadow DOM, so styles can bleed in.
- Ideal for understanding lifecycle hooks.

## 3. Shadow DOM and Encapsulation

### Goal:

Encapsulate your component’s internal structure and styles from global CSS.

### Key Idea:

The Shadow DOM creates a private DOM tree. Outside CSS or JavaScript can’t affect it — ensuring components look and behave consistently anywhere.

### Example:

```js
this.attachShadow({ mode: "open" });
```

### Benefits:

- Prevents style leakage.
- Allows scoping and reusability.
- Enables styling via `::part` and `::theme`.

## 4. Shadow Parts and CSS Custom Properties for Advanced Styling

### Challenge:

How do you style inside the Shadow DOM from outside?
By default, you can’t — but Shadow Parts and CSS Custom Properties bridge that gap.

### Shadow Parts:

Mark parts of your component for external styling:

```html
<div part="header"></div>
```

Then from outside:

```css
user-card::part(header) {
  color: red;
}
```

### CSS Custom Properties:

Define theme colors that flow into shadowed components:

```css
:root {
  --color-primary: #007bff;
}
```

Components can reference:

```css
.card {
  color: var(--color-primary);
}
```

### Why Important:

Combining parts + CSS variables allows for global theming while maintaining encapsulation.

## 5. Building Modern Web Components with Lit and Stencil.js

### The Problem:

Writing plain Web Components can be verbose (manual DOM management, no reactivity).

#### The Solution:

Lit and Stencil.js abstract the complexity and offer modern development ergonomics.

### Lit (by Google)

- Minimalist library for building reactive components.
- Uses template literals for rendering.
- Integrates seamlessly with Web Component standards.

### Example:

```ts
@customElement("lit-button")
class LitButton extends LitElement {
  @property() label = "Click Me";
  render() {
    return html`<button>${this.label}</button>`;
  }
}
```

### Features:

- Reactive state updates
- Scoped CSS
- Tiny footprint (~5KB)

### Stencil.js (by Ionic)

- Compiler that generates standards-compliant Web Components using TypeScript + JSX.
- Used by big companies (Ionic, GitHub Primer).

### Example:

```tsx
@Component({ tag: "stencil-card", shadow: true })
export class StencilCard {
  @Prop() title: string;
  render() {
    return <h2>{this.title}</h2>;
  }
}
```

### Features:

- Auto-optimized output
- Reactive props
- Built-in testing + docs
- Can generate framework wrappers (React/Vue/Angular)

## 6. Web Component Theme Engine

### Purpose:

A Theme Engine enables dynamic styling of Web Components across an entire app using CSS Custom Properties.

### Concept:

Instead of hardcoding colors and fonts, store them in “design tokens.”

```ts
const ThemeTokens = {
  colorPrimary: "#007BFF",
  colorBackground: "#FFFFFF",
};
```

Apply them globally:

```ts
Object.entries(ThemeTokens).forEach(([key, val]) =>
  document.documentElement.style.setProperty(`--${key}`, val)
);
```

Switch themes (light/dark) on the fly using data-theme.

## 7. Building a Cross-Framework Design System using Lit + CSS Variables

### Vision:

One Design System, usable in all frameworks — without rebuilding for each.

### Why CSS Variables:

- Runtime changeable
- Native browser support
- Lightweight and declarative

### Example:

```ts
@customElement("ds-button")
class DSButton extends LitElement {
  static styles = css`
    button {
      background: var(--colorPrimary);
      color: var(--colorBackground);
    }
  `;
  render() {
    return html`<button><slot></slot></button>`;
  }
}
```

- Works in React, Angular, or Vue simply by importing <ds-button>.

## 8. Visual Theme Builder UI

### Purpose:

Let designers/developers visually tweak design tokens (colors, fonts, etc.)
and instantly preview the results.

### Features:

- Color pickers for theme values
- Live preview updates
- Export to JSON or CSS
- Works standalone in any app

### Implementation:

A Lit component that:

- Holds theme state `(@state() theme = {...})`
- Updates document.documentElement.style on input
- Exports theme JSON file

This becomes the core UI for design system theming.

## 9. Real-Time Preview System

The Preview component uses the same CSS variables to visually reflect theme changes instantly.

```html
<div
  class="preview"
  style="background: var(--color-background); color: var(--color-text)"
>
  <h3>Preview</h3>
  <button style="background: var(--color-primary)">Button</button>
</div>
```

Whenever you change a value in the Theme Builder, this updates dynamically — showing your design system in real time.

## 10. Extending and Exporting the Design System

### Advanced Features:

| Feature              | Description                              |
| :------------------- | :--------------------------------------- |
| Presets              | Save multiple named themes.              |
| Exports              | Generate CSS, SCSS, or JSON token files. |
| Accessibility Checks | Ensure sufficient contrast ratios.       |
| Live Collaboration   | Sync themes via APIs or Firebase.        |
| Framework Adapters   | Auto-generate React/Vue theme files.     |

### Example Exports:

#### React:

```js
export const theme = {
  colors: { primary: "var(--color-primary)" },
};
```

#### Angular SCSS:

```scss
:root {
  --color-primary: #007bff;
}
```

## 11. Mini Web Component Toolkit

A lightweight helper library that simplifies building new components fast.

### Core Modules:

- BaseComponent — Simplifies lifecycle handling.
- ThemeManager — Applies and switches themes.
- TokenRegistry — Manages design tokens.
- StyleMixer — Merges default + user themes.

### Example:

```js
ThemeManager.apply({
  colorPrimary: "#ff5722",
  colorBackground: "#121212",
});
```

## 12. Real-World Benefits

| Benefit | Description |
| Cross-Framework Reuse | One component library works in React, Vue, Angular. |
| Unified Design Language | Consistent branding and UX across platforms. |
| Performance | Native browser rendering, no runtime overhead. |
| Scalable Architecture | Easy to extend and integrate. |
| Low Maintenance | Update once — propagate everywhere. |

## 13. Summary

You’ve covered:

- Web Component fundamentals
- Shadow DOM, Parts, and CSS variables
- Building with Lit and Stencil.js
- Creating a theme engine
- Designing a cross-framework system
- Constructing a visual theme builder
- Exporting themes for multiple frameworks

This single architecture empowers teams to build, manage, and scale UI systems that look and behave consistently across every app — from websites to enterprise dashboards.

## 14 Theme Builder Web App

### 14.1 Updated Project Structure

```pgsql
theme-builder/
├── package.json
├── tsconfig.json
├── vite.config.ts
├── index.html
├── src/
│   ├── main.ts
│   ├── theme-model.ts
│   ├── theme-manager.ts
│   └── components/
│       ├── theme-builder-ui.ts
│       ├── preview-pane.ts
│       └── accessibility-checker.ts
└── styles/
    └── tokens.css
```

### 14.2 src/theme-manager.ts

```ts
import { Theme } from "./theme-model";

export class ThemeManager {
  static apply(theme: Theme) {
    Object.entries(theme.colors).forEach(([key, value]) => {
      document.documentElement.style.setProperty(`--color-${key}`, value);
    });
    Object.entries(theme.typography).forEach(([key, value]) => {
      document.documentElement.style.setProperty(`--${key}`, value);
    });
    localStorage.setItem("theme", JSON.stringify(theme));
  }

  static load(): Theme | null {
    const stored = localStorage.getItem("theme");
    return stored ? JSON.parse(stored) : null;
  }

  // Export theme for frameworks
  static exportFramework(theme: Theme, framework: "react" | "vue" | "angular") {
    let content = "";
    switch (framework) {
      case "react":
        content = `export const theme = ${JSON.stringify(theme, null, 2)};`;
        break;
      case "vue":
        content = `<style>\n:root {\n${Object.entries(theme.colors)
          .map(([k, v]) => `  --color-${k}: ${v};`)
          .join("\n")}\n${Object.entries(theme.typography)
          .map(([k, v]) => `  --${k}: ${v};`)
          .join("\n")}\n}\n</style>`;
        break;
      case "angular":
        content = `:root {\n${Object.entries(theme.colors)
          .map(([k, v]) => `  --color-${k}: ${v};`)
          .join("\n")}\n${Object.entries(theme.typography)
          .map(([k, v]) => `  --${k}: ${v};`)
          .join("\n")}\n}`;
        break;
    }
    const blob = new Blob([content], { type: "text/plain" });
    const url = URL.createObjectURL(blob);
    const a = document.createElement("a");
    a.href = url;
    a.download = `${theme.name.replace(/\s+/g, "-")}-${framework}.txt`;
    a.click();
    URL.revokeObjectURL(url);
  }
}
```

### 14.3 src/components/theme-builder-ui.ts (Updated)

#### Key Updates:

- Added theme presets (Light / Dark / Custom)
- Added Framework export buttons
- Added contrast accessibility checker integration

```ts
import { LitElement, html, css } from "lit";
import { customElement, state } from "lit/decorators.js";
import { Theme, defaultTheme } from "../theme-model";
import { ThemeManager } from "../theme-manager";
import "./accessibility-checker";

const lightTheme: Theme = { ...defaultTheme, name: "Light Theme" };
const darkTheme: Theme = {
  ...defaultTheme,
  name: "Dark Theme",
  colors: {
    primary: "#0d6efd",
    secondary: "#6c757d",
    background: "#121212",
    text: "#ffffff",
  },
};

@customElement("theme-builder-ui")
export class ThemeBuilderUI extends LitElement {
  @state() theme: Theme = ThemeManager.load() || lightTheme;

  static styles = css`
    :host {
      display: block;
      padding: 1rem;
      background: #f0f0f0;
      border-radius: 12px;
      margin-bottom: 1rem;
    }
    .section {
      margin-bottom: 1rem;
    }
    label {
      display: flex;
      justify-content: space-between;
      margin: 0.5rem 0;
    }
    input[type="color"],
    input[type="text"] {
      margin-left: 0.5rem;
    }
    button {
      padding: 0.5rem 1rem;
      margin-top: 0.5rem;
      cursor: pointer;
      border-radius: 6px;
      border: none;
      background: var(--color-primary);
      color: var(--color-background);
    }
    .preset-btn {
      background: #6c757d;
      color: #fff;
      margin-right: 0.5rem;
    }
  `;

  constructor() {
    super();
    ThemeManager.apply(this.theme);
  }

  selectPreset(preset: "light" | "dark") {
    this.theme = preset === "light" ? lightTheme : darkTheme;
    ThemeManager.apply(this.theme);
    this.requestUpdate();
  }

  updateColor(prop: keyof Theme["colors"], value: string) {
    this.theme = {
      ...this.theme,
      colors: { ...this.theme.colors, [prop]: value },
    };
    ThemeManager.apply(this.theme);
  }

  updateTypography(prop: keyof Theme["typography"], value: string) {
    this.theme = {
      ...this.theme,
      typography: { ...this.theme.typography, [prop]: value },
    };
    ThemeManager.apply(this.theme);
  }

  exportTheme() {
    const blob = new Blob([JSON.stringify(this.theme, null, 2)], {
      type: "application/json",
    });
    const url = URL.createObjectURL(blob);
    const a = document.createElement("a");
    a.href = url;
    a.download = `${this.theme.name.replace(/\s+/g, "-")}.json`;
    a.click();
    URL.revokeObjectURL(url);
  }

  exportFramework(framework: "react" | "vue" | "angular") {
    ThemeManager.exportFramework(this.theme, framework);
  }

  render() {
    return html`
      <div class="section">
        <h3>Theme Presets</h3>
        <button class="preset-btn" @click=${() => this.selectPreset("light")}>
          Light
        </button>
        <button class="preset-btn" @click=${() => this.selectPreset("dark")}>
          Dark
        </button>
      </div>

      <div class="section">
        <h3>Colors</h3>
        ${Object.entries(this.theme.colors).map(
          ([key, value]) => html`
            <label
              >${key}
              <input
                type="color"
                .value=${value}
                @input=${(e: Event) =>
                  this.updateColor(
                    key as keyof Theme["colors"],
                    (e.target as HTMLInputElement).value
                  )}
              />
            </label>
          `
        )}
      </div>

      <div class="section">
        <h3>Typography</h3>
        ${Object.entries(this.theme.typography).map(
          ([key, value]) => html`
            <label
              >${key}
              <input
                type="text"
                .value=${value}
                @input=${(e: Event) =>
                  this.updateTypography(
                    key as keyof Theme["typography"],
                    (e.target as HTMLInputElement).value
                  )}
              />
            </label>
          `
        )}
      </div>

      <div class="section">
        <button @click=${this.exportTheme}>Export JSON</button>
        <button @click=${() => this.exportFramework("react")}>
          Export React
        </button>
        <button @click=${() => this.exportFramework("vue")}>Export Vue</button>
        <button @click=${() => this.exportFramework("angular")}>
          Export Angular
        </button>
      </div>

      <accessibility-checker .theme=${this.theme}></accessibility-checker>
    `;
  }
}
```

### 14.4 src/components/accessibility-checker.ts

#### Contrast & Accessibility Checker

```ts
import { LitElement, html, css } from "lit";
import { customElement, property } from "lit/decorators.js";
import { Theme } from "../theme-model";

function luminance(r: number, g: number, b: number) {
  const a = [r, g, b].map((v) => {
    v /= 255;
    return v <= 0.03928 ? v / 12.92 : Math.pow((v + 0.055) / 1.055, 2.4);
  });
  return 0.2126 * a[0] + 0.7152 * a[1] + 0.0722 * a[2];
}

function contrast(rgb1: string, rgb2: string) {
  const toRGB = (hex: string) => {
    const c = hex.replace("#", "");
    return [
      parseInt(c.substr(0, 2), 16),
      parseInt(c.substr(2, 2), 16),
      parseInt(c.substr(4, 2), 16),
    ];
  };
  const L1 = luminance(...toRGB(rgb1));
  const L2 = luminance(...toRGB(rgb2));
  return (Math.max(L1, L2) + 0.05) / (Math.min(L1, L2) + 0.05);
}

@customElement("accessibility-checker")
export class AccessibilityChecker extends LitElement {
  @property({ type: Object }) theme!: Theme;

  static styles = css`
    :host {
      display: block;
      margin-top: 1rem;
      padding: 1rem;
      border: 1px dashed #ccc;
      border-radius: 8px;
    }
    .warning {
      color: red;
      font-weight: bold;
    }
  `;

  render() {
    const c = contrast(this.theme.colors.text, this.theme.colors.background);
    const warning =
      c < 4.5
        ? html`<p class="warning">
            ⚠️ Contrast ratio ${c.toFixed(2)} is too low!
          </p>`
        : html`<p>✅ Contrast ratio ${c.toFixed(2)} is sufficient.</p>`;
    return html`
      <h4>Accessibility Checker</h4>
      ${warning}
    `;
  }
}
```

### 14.5 Features in This Extended Version

| Feature          | Description                           |
| :--------------- | :------------------------------------ |
| Theme Presets    | Light / Dark / Custom                 |
| Framework Export | JSON / React / Vue / Angular files    |
| Live Preview     | Shows buttons updated in real-time    |
| Local Storage    | Themes persist across sessions        |
| Contrast Checker | Ensures WCAG-compliant color contrast |

### 14.6 Running the Extended Project

```bash
npm install
npm run dev
```

Open http://localhost:5173 and:

- Switch between Light/Dark presets
- Edit colors & typography live
- Export JSON or framework-specific theme files
- See contrast accessibility warnings

---

[<< Chapter 14](./14_accessibility_in_javascript.md) | [Chapter 16 >>](./16_javaScript_design_patterns.md)

---
