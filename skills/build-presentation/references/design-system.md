# Design System: Brand-Aware Presentation Authoring

## Overview

This design system provides authoritative visual rules for brand-aware decks. **It is not a template or default style.** Instead, it serves as a reference for consistency, accessibility, and responsive behavior.

**Key principle:** The resolved identity brief (brand, dominant color, icon policy) takes precedence. The system enables you to apply that identity coherently across all slides.

---

## Color & Typography

### Color Tokens (CSS Variables)

Define these in the `<style>` section of your HTML deck. Use **semantic role names**, not color names:

```css
:root {
  /* Brand identity colors */
  --brand-primary: #203859;      /* Primary color (trust, hierarchy, text) */
  --brand-accent: #61C1B6;       /* Accent color (action, highlights) */
  --brand-secondary: #E67E22;    /* Secondary/tertiary (sparse, warnings) */
  
  /* Surface and text */
  --bg: #F7F9FB;                 /* Page background */
  --paper: #FFFFFF;              /* Card/slide background */
  --text: #1A1A1A;               /* Primary text (high contrast) */
  --text-muted: #5A6472;         /* Secondary/muted text */
  
  /* Semantic states */
  --success: #1E7D4B;            /* Success, completion, confirmation */
  --warning: #B5790F;            /* Caution, pending, review */
  --error: #C41C1C;              /* Error, blocked, failure */
}

/* Dark theme variant (optional) */
[data-theme="dark"] {
  --bg: #0A0E12;
  --paper: #1A2332;
  --text: #E8EAED;
  --text-muted: #9AA0A8;
  --brand-primary: #B0D4F1;      /* Inverted/lightened version */
  --brand-accent: #4ED9CC;       /* Inverted/lightened version */
}
```

### Typography Stack

Use a coherent three-font stack:

| Font | Weight | Use |
|---|---|---|
| **Bricolage Grotesque** | 800 | H1 (hero title) |
| **Bricolage Grotesque** | 600 | H2, H3 (section headers) |
| **Instrument Sans** | 400 | Body text, paragraphs |
| **Instrument Sans** | 600 | Emphasis, strong text |
| **IBM Plex Mono** | 400 | Code, labels, metadata |
| **IBM Plex Mono** | 600 | Emphasized code/labels |

#### Font Declaration

```css
@font-face { font-family: "Bricolage Grotesque"; font-weight: 800; src: local("Bricolage Grotesque ExtraBold"); }
@font-face { font-family: "Bricolage Grotesque"; font-weight: 600; src: local("Bricolage Grotesque SemiBold"); }
@font-face { font-family: "Instrument Sans"; font-weight: 400; src: local("Instrument Sans Regular"); }
@font-face { font-family: "Instrument Sans"; font-weight: 600; src: local("Instrument Sans SemiBold"); }
@font-face { font-family: "IBM Plex Mono"; font-weight: 400; src: local("IBM Plex Mono Regular"); }
@font-face { font-family: "IBM Plex Mono"; font-weight: 600; src: local("IBM Plex Mono SemiBold"); }

:root {
  --heading: "Bricolage Grotesque", "Trebuchet MS", "Segoe UI", sans-serif;
  --body: "Instrument Sans", "Segoe UI", Arial, sans-serif;
  --mono: "IBM Plex Mono", Consolas, monospace;
}
```

#### Sizing Scale

```css
/* Heading sizes (cqw units for responsive scaling) */
h1 { font: 800 3.35cqw / 1.08 var(--heading); }  /* Hero title */
h2 { font: 600 1.9cqw / 1.2 var(--heading); }   /* Section header */
h3 { font: 600 1.9cqw / 1.2 var(--heading); }   /* Subsection header */

/* Body text sizes */
.body-lg { font: 400 1.8cqw / 1.45 var(--body); }  /* Large body */
.body { font: 400 1.7cqw / 1.4 var(--body); }      /* Standard body */
.body-sm { font: 400 1.5cqw / 1.35 var(--body); }  /* Small body */

/* Labels and metadata */
.label { font: 600 1.25cqw / 1.2 var(--mono); }  /* Card labels */
.caption { font: 400 1.1cqw / 1.35 var(--mono); } /* Captions */
```

### Contrast and Accessibility

- **Text on background:** Minimum 4.5:1 contrast (WCAG AA)
- **Text on colored areas:** Test with color contrast tools; adjust `--text` or background if needed
- **Links:** Use `--brand-accent` with underline (`text-decoration: underline`); underline-offset: `0.15em`

---

## Layout & Spacing

### Container-Based Sizing (cqw)

All responsive measurements use CSS Container Query Width units (`cqw`). The slide container is the reference:

```css
.slide {
  position: relative;
  width: min(1280px, 100%);           /* Max 1280px, scale down on smaller screens */
  aspect-ratio: 16 / 9;
  container-type: inline-size;        /* Enable cqw units */
  overflow: hidden;
  border-radius: 12px;
  box-shadow: 0 24px 60px rgba(16, 40, 63, 0.14);
}

.slide-inner {
  position: absolute;
  inset: 0;
  padding: 3cqw 4cqw 2cqw;           /* Proportional padding */
  display: flex;
  flex-direction: column;
  gap: 1.25cqw;                       /* Proportional gap */
}
```

### Spacing Scale

```css
:root {
  --space-xs: 0.55cqw;   /* 7px at 1280px base */
  --space-sm: 0.7cqw;    /* 9px */
  --space-md: 1cqw;      /* 12.8px */
  --space-lg: 1.25cqw;   /* 16px */
  --space-xl: 1.55cqw;   /* 20px */
  --space-2xl: 2cqw;     /* 25.6px */
}
```

### Radius Scale

```css
:root {
  --radius-sm: 4px;
  --radius-md: 7px;
  --radius-lg: 10px;
  --radius-xl: 12px;
  --radius-pill: 999px;
}
```

### Grid Layout Example

```css
.two-column {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2cqw;
}

.three-column {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 1cqw;
}
```

---

## Icon Systems

### Generic Concepts (One Family)

For universal concepts (document, gear, heart, checkmark, etc.), choose **one outline icon family** and use it consistently:

**Recommended families:**
- [Lucide Icons](https://lucide.dev) — Feather-weight, 24×24 base, excellent coverage (ISC license)
- [Feather Icons](https://feathericons.com) — Minimal, clean, 24×24 base
- [Heroicons](https://heroicons.com) — Professional, 24×24 base (MIT license)

**Do not mix outline families within generic concepts.** All generic icons must come from the same family.

### Official Vendor Icons

For named vendor products (Microsoft, Apple, Google, Amazon, etc.), use **official vendor icons only:**

- **Microsoft/Azure:** [Microsoft Icons](https://aka.ms/MsiconsCollections) and [Azure Icon Gallery](https://learn.microsoft.com/en-us/azure/architecture/icons/)
- **Apple:** [SF Symbols](https://developer.apple.com/sf-symbols/)
- **Google:** [Material Design Icons](https://fonts.google.com/icons)
- **Amazon:** [AWS Architecture Icons](https://aws.amazon.com/architecture/icons/)

**Verify provenance.** Confirm icons are sourced from official registries; do not use third-party recreations.

**Preserve geometry.** Vendor icons must not be resized, recolored, or distorted; use them as-is.

### Brand Marks and Custom Assets

If your brand has custom icons or marks:

- **Preserve geometry and color** — Do not override vendor brand colors with semantic tokens
- **Provide in multiple formats** — SVG preferred for scales up to 2000×2000 px; PNG at 2× base size as fallback
- **Include in `BRANDING.md`** — Document provenance, license, and usage rules

### Mixing Policy

- **Do not mix within generic concepts:** All document icons are Lucide, not Lucide + Feather.
- **Vendor exceptions are deliberate:** If a slide shows "Microsoft Teams" and "Slack", using their official icons is correct and encouraged.
- **Brand marks are standalone:** A brand logo is never mixed with generic icon families; it appears separate or as a full-width header.

### Icon Sizing and Styling

```css
.icon-sm { width: 1.2cqw; height: 1.2cqw; }     /* Small badges */
.icon { width: 2.4cqw; height: 2.4cqw; }        /* Standard */
.icon-lg { width: 3.5cqw; height: 3.5cqw; }     /* Large / hero */

.icon { fill: none; stroke: currentColor; stroke-width: 1.5px; stroke-linecap: round; stroke-linejoin: round; }
```

---

## Component Library

### Card / Contained Element

```css
.card {
  padding: 1.1cqw;
  border: 1px solid var(--text-muted);
  border-radius: var(--radius-lg);
  background: var(--paper);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.06);
}

.card.brand-tinted {
  border-color: var(--brand-accent);
  background: color-mix(in srgb, var(--brand-accent) 8%, var(--paper));
}
```

### Badge / Pill

```css
.badge {
  display: inline-flex;
  align-items: center;
  gap: var(--space-sm);
  padding: var(--space-xs) var(--space-md);
  border: 1px solid var(--text-muted);
  border-radius: var(--radius-pill);
  font: 400 1.25cqw / 1 var(--mono);
  color: var(--text-muted);
  background: var(--paper);
}

.badge.success {
  border-color: var(--success);
  color: var(--success);
  background: color-mix(in srgb, var(--success) 8%, var(--paper));
}
```

### Callout / Outcome Box

```css
.outcome {
  display: flex;
  align-items: center;
  gap: 1cqw;
  padding: 0.8cqw 1.1cqw;
  border: 1px solid var(--brand-accent);
  border-radius: var(--radius-lg);
  background: color-mix(in srgb, var(--brand-accent) 8%, var(--paper));
  font: 400 1.7cqw / 1.35 var(--body);
}

.outcome strong {
  flex: none;
  font-weight: 600;
  color: var(--brand-primary);
}
```

### Flow/Process Step

```css
.flow-step {
  display: flex;
  flex-direction: column;
  gap: var(--space-lg);
  padding: 1cqw;
  border: 1px solid var(--text-muted);
  border-radius: var(--radius-lg);
  background: var(--paper);
}

.step-number {
  width: 3cqw;
  height: 3cqw;
  border-radius: 50%;
  display: grid;
  place-items: center;
  background: var(--brand-primary);
  color: var(--paper);
  font: 600 1.55cqw var(--heading);
}
```

---

## Motion & Animation

### Entrance Animations

Optional reveals for staged presentation:

```css
@keyframes enter {
  from { opacity: 0; transform: translateY(8px); }
  to { opacity: 1; transform: none; }
}

.reveal {
  animation: enter 0.55s ease both;
  animation-delay: var(--delay, 0s);
}

/* Example: stagger 5 reveals */
.slide .reveal:nth-child(1) { --delay: 0s; }
.slide .reveal:nth-child(2) { --delay: 0.2s; }
.slide .reveal:nth-child(3) { --delay: 0.4s; }
.slide .reveal:nth-child(4) { --delay: 0.6s; }
.slide .reveal:nth-child(5) { --delay: 0.8s; }
```

### Motion Preference

```css
@media (prefers-reduced-motion: reduce) {
  .reveal {
    animation: none !important;
    opacity: 1 !important;
    transform: none !important;
  }
}
```

---

## Responsive Behavior

### Base Sizes

```css
/* At 1280px slide width */
h1: 3.35cqw = 43px
h2: 1.9cqw = 24px
body: 1.7cqw = 22px
label: 1.25cqw = 16px

/* At 800px slide width */
h1: 3.35cqw = 27px
h2: 1.9cqw = 15px
body: 1.7cqw = 14px
```

### Media Queries

```css
/* Large screens (1400px+) */
@media (min-width: 1400px) {
  .slide { width: 1400px; }  /* Explicit max, or max-width: 100%; */
}

/* Tablets and small screens */
@media (max-width: 768px) {
  .slide { padding: 2.5cqw 3cqw; gap: 1cqw; }
  h1 { font-size: 2.8cqw; }  /* Scale down if needed */
}

/* Mobile */
@media (max-width: 480px) {
  .slide { padding: 2cqw; gap: 0.8cqw; }
  h1 { font-size: 2.2cqw; }
  .multi-col { grid-template-columns: 1fr; }  /* Stack columns */
}
```

---

## Dark Theme Implementation

If dark mode is needed, define theme-aware tokens:

```css
:root {
  color-scheme: light;
  --bg: #F7F9FB;
  --paper: #FFFFFF;
  --text: #1A1A1A;
  /* ... other tokens ... */
}

[data-theme="dark"] {
  color-scheme: dark;
  --bg: #0A0E12;
  --paper: #1A2332;
  --text: #E8EAED;
  /* ... ensure all tokens have dark variants ... */
}

/* Automatic: respect user system preference */
@media (prefers-color-scheme: dark) {
  :root {
    color-scheme: dark;
    /* Apply dark theme by default */
    --bg: #0A0E12;
    --paper: #1A2332;
    --text: #E8EAED;
  }
}
```

---

## Multilingual Typography

### Font Support

The default stack (Bricolage Grotesque, Instrument Sans, IBM Plex Mono) supports:

- **Latin** — English, French, Spanish, German, Portuguese, etc.
- **Cyrillic** — Russian, Ukrainian, etc.
- **Greek** — Greek
- **Arabic** — Arabic, Farsi, Urdu (right-to-left; requires `lang` and `dir` attributes)
- **CJK** — Limited; add fallback fonts for Chinese, Japanese, Korean

### Extended Font Stack (CJK)

For Chinese (Simplified/Traditional), Japanese, or Korean:

```css
@font-face { font-family: "Noto Sans SC"; font-weight: 400; src: local("Noto Sans SC Regular"); }
@font-face { font-family: "Noto Sans SC"; font-weight: 600; src: local("Noto Sans SC SemiBold"); }

:root {
  --heading-cjk: "Bricolage Grotesque", "Noto Sans SC", "Segoe UI", sans-serif;
  --body-cjk: "Instrument Sans", "Noto Sans SC", "Segoe UI", sans-serif;
}

html[lang="zh-HK"] h1, html[lang="zh-CN"] h1 { font-family: var(--heading-cjk); }
html[lang="zh-HK"] body, html[lang="zh-CN"] body { font-family: var(--body-cjk); }
```

### Language Direction (Right-to-Left)

```html
<html lang="ar" dir="rtl">
```

In CSS:

```css
html[dir="rtl"] .slide-inner { direction: rtl; text-align: right; }
html[dir="rtl"] .badge { margin-left: auto; }  /* Flip layout */
```

---

## Printing and PDF Export

### Print Styles

```css
@page {
  size: 13.333333in 7.5in;  /* 16:9 at standard resolution */
  margin: 0;
}

@media print {
  html, body { margin: 0; background: white; print-color-adjust: exact; -webkit-print-color-adjust: exact; }
  .slide { display: block; width: 13.333333in; height: 7.5in; margin: 0; break-after: page; border: none; box-shadow: none; }
  .controls, .speaker-notes { display: none; }
  .reveal { animation: none !important; opacity: 1 !important; }
}
```

---

## Checklist: Design System Application

- [ ] **Color tokens defined** — `--brand-primary`, `--brand-accent`, `--bg`, `--text`, `--text-muted`
- [ ] **Typography stack** — Bricolage Grotesque (headings), Instrument Sans (body), IBM Plex Mono (labels)
- [ ] **Font sizes** — `h1` 3.35cqw, `h2` 1.9cqw, body 1.7cqw
- [ ] **Spacing** — All gaps and padding use cqw units; radii use defined scale
- [ ] **Icons** — One generic family (Lucide, Feather, etc.); vendor icons from official sources only
- [ ] **Cards and components** — Use standard classes (`.card`, `.badge`, `.outcome`)
- [ ] **Animations** — Optional staggered reveals with `@keyframes enter` and `--delay` variable
- [ ] **Accessibility** — Text contrast ≥ 4.5:1; heading hierarchy preserved; `lang` and `dir` attributes set
- [ ] **Responsive** — Slide scales with container-query units; media queries for small screens if needed
- [ ] **Dark theme** — All tokens have `[data-theme="dark"]` variants (if dark mode needed)
- [ ] **Print styles** — `@page` and `@media print` rules ensure correct sizing and hiding of UI

---

## References

- [CSS Container Queries](https://developer.mozilla.org/en-US/docs/Web/CSS/Container_queries)
- [WCAG Accessibility Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)
- [Lucide Icons](https://lucide.dev)
- [Microsoft Icons](https://aka.ms/MsiconsCollections)
