---

[<< Chapter 13](./13_dom_manipulation.md) | [Chapter 15 >>](./15_web_components.md)

---

# Chapter 14: Accessibility in JavaScript (React + TypeScript & NPM Toolkit)

## Introduction

Accessibility (a11y) ensures that digital experiences are inclusive for everyone — including users with visual, auditory, motor, or cognitive disabilities. Modern JavaScript, combined with React and TypeScript, provides powerful tools to create dynamic yet accessible web applications.

This chapter consolidates both theory and practice:

- Core accessibility principles
- A reusable Accessibility Toolkit
- A real-world audit workflow
- A production-ready React + TypeScript modal dialog project
- Packaging it all into an NPM module for reuse

Accessibility is not a feature — it’s a fundamental quality of professional software.

## 1. Core Accessibility Concepts

### 1.1 What Accessibility Means

Accessibility means designing apps that everyone can use — regardless of ability, device, or context. It follows standards like:

- WCAG 2.2
- ARIA 1.2
- Section 508 / EN 301 549

### 1.2 Key Principles (P.O.U.R.)

- Perceivable — content must be available to all senses.
- Operable — interface must work with keyboard and assistive tools.
- Understandable — UI must be predictable and readable.
- Robust — content must remain accessible across technologies and updates.

## 2. Accessibility in JavaScript

### 2.1 Common Challenges

- Dynamic content not announced to screen readers
- Focus loss after modal dialogs
- Keyboard traps
- Relying on color alone

### 2.2 JavaScript Solutions

| Concern             | Technique                                                 |
| :------------------ | :-------------------------------------------------------- |
| Keyboard navigation | Use `keydown` events and logical focus order              |
| Announce updates    | Use `aria-live="polite"` or `assertive`                   |
| Manage focus        | Call `element.focus()` after updates                      |
| Polyfills           | Use `focus-visible`, `aria-utils` for consistent behavior |

## 3. Accessibility in React + TypeScript

React components can encapsulate accessibility logic. TypeScript enforces type-safe props for ARIA attributes and event handling.

### 3.1 Accessible Button

```js
import React from "react";

interface AccessibleButtonProps {
  label: string;
  onClick: () => void;
}

export const AccessibleButton: React.FC<AccessibleButtonProps> = ({
  label,
  onClick,
}) => (
  <button
    onClick={onClick}
    aria-label={label}
    className="focus:outline-none focus:ring-2 focus:ring-blue-600 rounded px-3 py-2"
  >
    {label}
  </button>
);
```

### 3.2 Accessible Input

```js
interface AccessibleInputProps {
  id: string;
  label: string;
  value: string;
  onChange: (value: string) => void;
  type?: string;
}

export const AccessibleInput: React.FC<AccessibleInputProps> = ({
  id,
  label,
  value,
  onChange,
  type = "text",
}) => (
  <div className="flex flex-col gap-1">
    <label htmlFor={id}>{label}</label>
    <input
      id={id}
      type={type}
      value={value}
      onChange={(e) => onChange(e.target.value)}
      aria-label={label}
      className="border rounded px-2 py-1 focus:ring-2 focus:ring-blue-500"
    />
  </div>
);
```

### 3.3 Visually Hidden Text

```js
export const ScreenReaderText: React.FC<{ text: string }> = ({ text }) => (
  <span className="sr-only">{text}</span>
);
```

Add the following CSS in your toolkit:

```css
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  border: 0;
}
```

## 4. Mini Accessibility Toolkit for Developers

### 4.1 Hooks

#### useKeyboardNavigation

```js
import { useEffect } from "react";

export const useKeyboardNavigation = (handler: (key: string) => void) => {
  useEffect(() => {
    const fn = (e: KeyboardEvent) => handler(e.key);
    window.addEventListener("keydown", fn);
    return () => window.removeEventListener("keydown", fn);
  }, [handler]);
};
```

#### useFocusTrap

```js
import { useEffect } from "react";

export const useFocusTrap = (containerRef: React.RefObject<HTMLElement>) => {
  useEffect(() => {
    const focusable = containerRef.current?.querySelectorAll<HTMLElement>(
      "a[href], button, textarea, input, select, [tabindex]:not([tabindex='-1'])"
    );
    const first = focusable?.[0];
    const last = focusable?.[focusable.length - 1];

    const handleKeyDown = (e: KeyboardEvent) => {
      if (e.key !== "Tab") return;
      if (e.shiftKey && document.activeElement === first) {
        e.preventDefault(); (last as HTMLElement)?.focus();
      } else if (!e.shiftKey && document.activeElement === last) {
        e.preventDefault(); (first as HTMLElement)?.focus();
      }
    };
    document.addEventListener("keydown", handleKeyDown);
    return () => document.removeEventListener("keydown", handleKeyDown);
  }, [containerRef]);
};
```

## 5. Practical Project: Accessible Modal Dialog

### 5.1 Component

```js
import React, { useRef, useEffect } from "react";
import { useFocusTrap } from "./hooks/useFocusTrap";

interface ModalProps {
  isOpen: boolean;
  title: string;
  onClose: () => void;
  children: React.ReactNode;
}

export const AccessibleModal: React.FC<ModalProps> = ({
  isOpen,
  title,
  onClose,
  children,
}) => {
  const ref = useRef < HTMLDivElement > null;
  useFocusTrap(ref);

  useEffect(() => {
    const esc = (e: KeyboardEvent) => e.key === "Escape" && onClose();
    document.addEventListener("keydown", esc);
    return () => document.removeEventListener("keydown", esc);
  }, [onClose]);

  if (!isOpen) return null;

  return (
    <div
      role="dialog"
      aria-modal="true"
      aria-labelledby="modal-title"
      className="fixed inset-0 bg-black/50 flex items-center justify-center z-50"
    >
      <div ref={ref} className="bg-white rounded p-6 shadow-lg w-[400px]">
        <h2 id="modal-title" className="text-lg font-semibold mb-4">
          {title}
        </h2>
        {children}
        <button
          onClick={onClose}
          className="mt-4 bg-blue-600 text-white px-4 py-2 rounded"
        >
          Close
        </button>
      </div>
    </div>
  );
};
```

### 5.2 Usage

```tsx
<AccessibleModal
  isOpen={show}
  onClose={() => setShow(false)}
  title="Accessible Modal"
>
  <p>This modal is fully keyboard accessible and screen-reader friendly.</p>
</AccessibleModal>
```

## 6. Real-World Accessibility Audit Workflow

- Define scope — Identify pages and components to audit.
- Automated testing — Use axe-core, eslint-plugin-jsx-a11y, Lighthouse.
- Manual testing — Keyboard navigation, screen readers (NVDA/VoiceOver).
- Fix issues — Prioritize WCAG A/AA issues.
- Regression tests — Add unit and end-to-end tests (Cypress + axe).
- Document — Maintain accessibility test logs and reports.

## 7. Packaging as an NPM Library

### 7.1 File Structure

```pgsql
accessibility-toolkit/
├── src/
│   ├── components/
│   │   ├── AccessibleButton.tsx
│   │   ├── AccessibleInput.tsx
│   │   ├── AccessibleModal.tsx
│   ├── hooks/
│   │   ├── useKeyboardNavigation.ts
│   │   ├── useFocusTrap.ts
│   ├── index.ts
├── package.json
├── tsconfig.json
├── README.md
```

### 7.2 index.ts

```ts
export * from "./components/AccessibleButton";
export * from "./components/AccessibleInput";
export * from "./components/AccessibleModal";
export * from "./hooks/useKeyboardNavigation";
export * from "./hooks/useFocusTrap";
```

### 7.3 package.json (example)

```json
{
  "name": "@yourorg/accessibility-toolkit",
  "version": "1.0.0",
  "main": "dist/index.js",
  "module": "dist/index.esm.js",
  "types": "dist/index.d.ts",
  "scripts": {
    "build": "tsc && vite build",
    "lint": "eslint src --ext .ts,.tsx",
    "test": "vitest run"
  },
  "peerDependencies": {
    "react": "^18.0.0",
    "react-dom": "^18.0.0"
  },
  "license": "MIT"
}
```

### 7.4 Build & Publish

```bash
npm run build
npm publish --access public
```

Now developers can install and use:

```bash
npm install @yourorg/accessibility-toolkit
```

Usage:

```ts
import { AccessibleModal } from "@yourorg/accessibility-toolkit";
```

## 8. Testing Your Package

- Unit tests with Vitest or Jest.
- Run axe-core accessibility checks inside tests.
- Use Storybook with `@storybook/addon-a11y` for live validation.

## 9. Best Practices Recap

- Prefer semantic HTML over ARIA when possible.
- Always test with keyboard and screen readers.
- Maintain color contrast ≥ 4.5 : 1.
- Document accessible patterns in your design system.
- Include accessibility checks in CI/CD pipelines.

## 10 Conclusion

Accessibility is not an add-on — it is a measure of quality, inclusivity, and professionalism.
By integrating accessible practices into your React + TypeScript development workflow and distributing reusable tools through NPM, you multiply the impact of accessibility across projects.

**Accessible design is universal design.**
Making the web inclusive benefits every user — everywhere.

---

[<< Chapter 13](./13_dom_manipulation.md) | [Chapter 15 >>](./15_web_components.md)

---
