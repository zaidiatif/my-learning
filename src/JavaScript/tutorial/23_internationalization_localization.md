# Chapter 23: Internationalization and Localization in JavaScript
## 1. Introduction

Modern web applications serve users worldwide. Building an app that feels native to each user requires adapting:

- `Language` (text translations)
- `Locale-based formatting` (dates, times, numbers, currencies)
- `Cultural preferences` (measurement units, reading direction, pluralization rules)

This is the core of `Internationalization (i18n)` and `Localization (l10n)`.

- `Internationalization (i18n)` = Preparing your app for global use.
- `Localization (l10n)` = Customizing content for specific regions/languages.

## 2. Core Concepts

| Concept	| Description |
|:--- |:--- |
| Locale	| Defines regional preferences — e.g., en-US, fr-FR, hi-IN. |
| Translation keys	| Identifiers mapped to language-specific strings. |
| Pluralization	| Handling different word forms based on count. |
| Date/Number formatting	| Showing values based on locale rules. |
| Directionality	| Supporting LTR (left-to-right) and RTL (right-to-left) layouts. |

## 3. Using JavaScript’s Built-in Internationalization API (`Intl`)

The `Intl` API (ECMAScript Internationalization API) provides globalized formatting tools without needing external libraries.

### 3.1 Formatting Numbers
```js
const number = 1234567.89;

console.log(new Intl.NumberFormat('en-US').format(number)); // 1,234,567.89
console.log(new Intl.NumberFormat('de-DE').format(number)); // 1.234.567,89
console.log(new Intl.NumberFormat('hi-IN').format(number)); // 12,34,567.89
```

### 3.2 Formatting Currencies
```js
const price = 499.99;

console.log(new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD' }).format(price)); // $499.99
console.log(new Intl.NumberFormat('ja-JP', { style: 'currency', currency: 'JPY' }).format(price)); // ￥500
```

### 3.3 Formatting Dates and Times
```js
const date = new Date();

console.log(new Intl.DateTimeFormat('en-US').format(date)); // 10/11/2025
console.log(new Intl.DateTimeFormat('fr-FR', { dateStyle: 'full' }).format(date)); // samedi 11 octobre 2025
console.log(new Intl.DateTimeFormat('ar-EG', { timeStyle: 'short' }).format(date)); // ١٠:٣٢ م
```

## 4. Language Detection and Dynamic Locale Switching

You can automatically detect or allow users to switch their preferred language.

### 4.1 Detecting Browser Locale
```js
const userLocale = navigator.language || navigator.userLanguage;
console.log(userLocale); // e.g., "en-US"
```

### 4.2 Switching Locale Dynamically
```js
let currentLocale = 'en';

function setLocale(locale) {
  currentLocale = locale;
  document.documentElement.lang = locale;
  loadTranslations(locale);
}

function loadTranslations(locale) {
  fetch(`/locales/${locale}.json`)
    .then(res => res.json())
    .then(data => {
      document.querySelectorAll('[data-i18n]').forEach(el => {
        const key = el.getAttribute('data-i18n');
        el.textContent = data[key] || key;
      });
    });
}

setLocale('en'); // Default
```

## 5. Translation Management with JSON Files

Use per-language JSON files for translation mapping.

#### Example Directory:
```pgsql
/locales
├── en.json
├── fr.json
└── hi.json
```

#### Sample en.json
```json
{
  "app.title": "Welcome to Theme Builder",
  "button.save": "Save",
  "button.cancel": "Cancel"
}
```

#### Sample fr.json
```json
{
  "app.title": "Bienvenue dans le Créateur de Thème",
  "button.save": "Enregistrer",
  "button.cancel": "Annuler"
}
```

## 6. Using Intl.PluralRules for Grammar-Aware Translations
```js
const pluralRules = new Intl.PluralRules('en-US');

function pluralize(count, singular, plural) {
  return pluralRules.select(count) === 'one' ? singular : plural;
}

console.log(`You have ${5} ${pluralize(5, 'item', 'items')}.`); // You have 5 items.
```

For Hindi, Arabic, or Russian — rules differ, but Intl.PluralRules handles it automatically.

## 7. Right-to-Left (RTL) Language Support

#### CSS Adaptation
```css
:root[dir="rtl"] {
  direction: rtl;
  text-align: right;
}

:root[dir="ltr"] {
  direction: ltr;
  text-align: left;
}
```

#### Dynamic Direction Setting
```js
function setDirection(locale) {
  const rtlLangs = ['ar', 'fa', 'he', 'ur'];
  document.documentElement.dir = rtlLangs.includes(locale.split('-')[0]) ? 'rtl' : 'ltr';
}
```

## 8. Popular i18n Libraries

| Library	| Description	| Ecosystem |
|:--- |:--- |:--- |
| i18next	| Most popular JS i18n framework	| Works with React, Vue, Node |
| FormatJS / react-intl	| Message formatting for React	| React ecosystem |
| LinguiJS	| Lightweight message-based translation	| Framework-agnostic |
| Vue I18n	| Official Vue translation plugin	| Vue-only |

## 9. Example: React Integration with i18next
```bash
npm install i18next react-i18next
```

```js
// i18n.js
import i18n from "i18next";
import { initReactI18next } from "react-i18next";

i18n.use(initReactI18next).init({
  resources: {
    en: { translation: { "hello": "Hello World" } },
    fr: { translation: { "hello": "Bonjour le monde" } }
  },
  lng: "en",
  fallbackLng: "en",
});

export default i18n;
```

```jsx
// App.jsx
import { useTranslation } from 'react-i18next';

function App() {
  const { t, i18n } = useTranslation();
  return (
    <>
      <h1>{t('hello')}</h1>
      <button onClick={() => i18n.changeLanguage('fr')}>Français</button>
      <button onClick={() => i18n.changeLanguage('en')}>English</button>
    </>
  );
}
```

## 10. Localizing Design Systems

You can integrate i18n into Web Components or Theme Builders:

```ts
// In Lit Component
import { html, LitElement } from 'lit';

const translations = {
  en: { save: 'Save' },
  hi: { save: 'सहेजें' }
};

export class SaveButton extends LitElement {
  locale = 'en';

  render() {
    return html`<button>${translations[this.locale].save}</button>`;
  }
}
```

## 11. Advanced Techniques

| Feature	| Description |
|:--- |:--- |
| ICU Message Format	| Advanced plural, gender, and contextual expressions. |
| Dynamic module loading	| Load translations only for active locale using `import()`. |
| Locale-based routing	| e.g., `/en/dashboard` or `/fr/dashboard.` |
| Currency exchange APIs	| Convert amounts dynamically per region. |
| Date/time zones	| Adjust with `Intl.DateTimeFormat` + `timeZone` option. |

## 12. Best Practices

- Use locale codes properly — e.g., `en-GB` vs `en-US`.
- Avoid string concatenation — use placeholders or ICU syntax.
- Use fallback locales — always default to `en` or a neutral language.
- Load translations asynchronously — for performance.
- Test with real languages — especially RTL scripts.

## 13. Practical Project: Localized Theme Builder (React + TypeScript)

### 13.1 Project Overview

#### Goal

Build a React-based Theme Builder that supports:

- Dynamic language switching (English, French, Hindi, Arabic)
- RTL layout detection for Arabic
- Localized labels, buttons, and color tooltips
- Locale-aware date and currency formatting

### 13.2 Project Structure

```pgsql
localized-theme-builder/
├── src/
│   ├── i18n/
│   │   ├── index.ts
│   │   ├── en.json
│   │   ├── fr.json
│   │   ├── hi.json
│   │   └── ar.json
│   ├── components/
│   │   ├── ThemeEditor.tsx
│   │   ├── LanguageSelector.tsx
│   │   └── PreviewPanel.tsx
│   ├── App.tsx
│   └── main.tsx
├── package.json
└── vite.config.ts
```

### 13.3 Step 1 – Install Dependencies

```bash
npm install react-i18next i18next
npm install @types/react-i18next --save-dev
```

### 13.4 Step 2 – Setup i18n Configuration

#### `src/i18n/index.ts`

```ts
import i18n from "i18next";
import { initReactI18next } from "react-i18next";

import en from "./en.json";
import fr from "./fr.json";
import hi from "./hi.json";
import ar from "./ar.json";

i18n.use(initReactI18next).init({
  resources: {
    en: { translation: en },
    fr: { translation: fr },
    hi: { translation: hi },
    ar: { translation: ar },
  },
  lng: "en", // default language
  fallbackLng: "en",
  interpolation: {
    escapeValue: false,
  },
});

export default i18n;
```

### 13.5 Step 3 – Translation Files

#### `src/i18n/en.json`

```json
{
  "app.title": "Theme Builder",
  "app.subtitle": "Design your theme and preview instantly",
  "button.save": "Save",
  "button.export": "Export",
  "label.primaryColor": "Primary Color",
  "label.secondaryColor": "Secondary Color",
  "label.fontSize": "Font Size",
  "label.language": "Language",
  "preview.greeting": "Hello, welcome to your personalized theme!"
}
```

#### `src/i18n/fr.json`

```json
{
  "app.title": "Créateur de Thèmes",
  "app.subtitle": "Concevez votre thème et prévisualisez-le instantanément",
  "button.save": "Enregistrer",
  "button.export": "Exporter",
  "label.primaryColor": "Couleur principale",
  "label.secondaryColor": "Couleur secondaire",
  "label.fontSize": "Taille de la police",
  "label.language": "Langue",
  "preview.greeting": "Bonjour, bienvenue dans votre thème personnalisé !"
}
```

#### `src/i18n/hi.json`

```json
{
  "app.title": "थीम बिल्डर",
  "app.subtitle": "अपनी थीम डिज़ाइन करें और तुरंत पूर्वावलोकन करें",
  "button.save": "सहेजें",
  "button.export": "निर्यात करें",
  "label.primaryColor": "प्राथमिक रंग",
  "label.secondaryColor": "द्वितीयक रंग",
  "label.fontSize": "फ़ॉन्ट आकार",
  "label.language": "भाषा",
  "preview.greeting": "नमस्ते, आपके व्यक्तिगत थीम में स्वागत है!"
}
```

#### `src/i18n/ar.json`

```json
{
  "app.title": "منشئ السمات",
  "app.subtitle": "صمم سمةك وشاهد المعاينة فورًا",
  "button.save": "حفظ",
  "button.export": "تصدير",
  "label.primaryColor": "اللون الأساسي",
  "label.secondaryColor": "اللون الثانوي",
  "label.fontSize": "حجم الخط",
  "label.language": "اللغة",
  "preview.greeting": "مرحبًا بك في سمةك المخصصة!"
}
```

### 13.6 Step 4 – Language Selector Component

#### `src/components/LanguageSelector.tsx`

```tsx
import { useTranslation } from "react-i18next";
import { useEffect } from "react";

export const LanguageSelector = () => {
  const { i18n, t } = useTranslation();

  useEffect(() => {
    // Set RTL for Arabic
    const rtlLangs = ["ar"];
    document.documentElement.dir = rtlLangs.includes(i18n.language) ? "rtl" : "ltr";
  }, [i18n.language]);

  return (
    <div className="flex items-center gap-2">
      <label>{t("label.language")}:</label>
      <select
        value={i18n.language}
        onChange={(e) => i18n.changeLanguage(e.target.value)}
        className="p-1 border rounded"
      >
        <option value="en">English</option>
        <option value="fr">Français</option>
        <option value="hi">हिन्दी</option>
        <option value="ar">العربية</option>
      </select>
    </div>
  );
};
```

### 13.7 Step 5 – Theme Editor Component

#### `src/components/ThemeEditor.tsx`

```tsx
import { useState } from "react";
import { useTranslation } from "react-i18next";

export const ThemeEditor = ({ onChange }: { onChange: (theme: any) => void }) => {
  const { t } = useTranslation();
  const [theme, setTheme] = useState({
    primaryColor: "#007bff",
    secondaryColor: "#ff4081",
    fontSize: 16,
  });

  const handleChange = (key: string, value: string | number) => {
    const updated = { ...theme, [key]: value };
    setTheme(updated);
    onChange(updated);
  };

  return (
    <div className="space-y-3 border rounded p-4">
      <h2 className="font-bold">{t("app.title")}</h2>
      <p>{t("app.subtitle")}</p>

      <div>
        <label>{t("label.primaryColor")}</label>
        <input
          type="color"
          value={theme.primaryColor}
          onChange={(e) => handleChange("primaryColor", e.target.value)}
        />
      </div>

      <div>
        <label>{t("label.secondaryColor")}</label>
        <input
          type="color"
          value={theme.secondaryColor}
          onChange={(e) => handleChange("secondaryColor", e.target.value)}
        />
      </div>

      <div>
        <label>{t("label.fontSize")}</label>
        <input
          type="number"
          value={theme.fontSize}
          onChange={(e) => handleChange("fontSize", parseInt(e.target.value))}
        />
      </div>

      <button className="bg-blue-600 text-white px-3 py-1 rounded">{t("button.save")}</button>
      <button className="bg-green-600 text-white px-3 py-1 rounded ml-2">{t("button.export")}</button>
    </div>
  );
};
```

### 13.8 Step 6 – Preview Panel

#### `src/components/PreviewPanel.tsx`

```tsx
import { useTranslation } from "react-i18next";

export const PreviewPanel = ({ theme }: { theme: any }) => {
  const { t } = useTranslation();

  return (
    <div
      className="rounded shadow p-4 mt-4"
      style={{
        background: theme.secondaryColor,
        color: theme.primaryColor,
        fontSize: `${theme.fontSize}px`,
      }}
    >
      <p>{t("preview.greeting")}</p>
    </div>
  );
};
```

### 13.9 Step 7 – Main App

#### `src/App.tsx`

```tsx
import "./i18n";
import { useState } from "react";
import { ThemeEditor } from "./components/ThemeEditor";
import { PreviewPanel } from "./components/PreviewPanel";
import { LanguageSelector } from "./components/LanguageSelector";

function App() {
  const [theme, setTheme] = useState({
    primaryColor: "#007bff",
    secondaryColor: "#ff4081",
    fontSize: 16,
  });

  return (
    <div className="p-6 space-y-4">
      <LanguageSelector />
      <ThemeEditor onChange={setTheme} />
      <PreviewPanel theme={theme} />
    </div>
  );
}

export default App;
```

### 13.10 Step 8 – Add Locale-aware Data Formatting

Enhance preview to display date/currency localized to the selected language.

```tsx
const now = new Date();
const price = 2499.99;

const formattedDate = new Intl.DateTimeFormat(i18n.language, {
  dateStyle: "full",
}).format(now);

const formattedPrice = new Intl.NumberFormat(i18n.language, {
  style: "currency",
  currency: i18n.language === "fr" ? "EUR" : "USD",
}).format(price);

return (
  <div>
    <p>{t("preview.greeting")}</p>
    <p>{formattedDate}</p>
    <p>{formattedPrice}</p>
  </div>
);
```

### 13.11 Step 9 – RTL Support

We already included automatic RTL detection inside `LanguageSelector`.
The `document.documentElement.dir` will switch automatically between `ltr` and `rtl` based on language.

### 13.12 Step 10 – Optional Enhancements

- Add locale-based routing (/en, /fr, /hi, /ar)
- Lazy-load translation bundles per locale
- Use ICU message syntax for plural/gender-based text
- Integrate Design System tokens with translated labels

#### Result

Your Localized Theme Builder UI can now:
- Translate all interface text
- Switch between LTR and RTL layouts
- Format numbers, dates, and currencies per locale
- Maintain theme editing and preview functionality

## 14. Summary

- `i18n prepares`, `l10n customizes`.
- Use the `Intl` API for native formatting.
- Store translations in `JSON files`.
- Support `pluralization`, `RTL`, and `locale routing`.
- Adopt libraries like `i18next`, `LinguiJS`, or `FormatJS` for scalability.
- Combine with `design systems` to produce globally adaptable UI frameworks.