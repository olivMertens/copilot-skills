# Workflow: Brand-Aware Presentation Authoring

## Overview

This eight-step workflow ensures decks are built with an explicit, coherent identity rather than implicit defaults. The key principle is **authority-first**: if an authoritative brand kit or template is supplied, it takes precedence over all defaults.

---

## Step 1: Establish Identity Brief

**Goal:** Resolve the four identity decisions before authoring begins.

### Decisions

| Decision | Clarification | Examples |
|---|---|---|
| **Brand/Client Identity** | Name the brand, client, or organization. Or specify "brand-neutral" if no identity applies. | "Acme Corp", "TechStart Inc", "Government Agency", "brand-neutral" |
| **Authoritative Source** | Provide the definitive reference: brand kit URL, design template, logo file, color specification, typography doc, or exact guidelines. If nothing exists, state "none supplied". | "Brand kit: brand.company.com/guidelines.pdf", "Logo: assets/acme-logo.svg + color spec", "Typography: Figma link", "none supplied" |
| **Dominant Color & Role** | Name the primary color, hex code (if available), and its semantic role: primary (trust, hierarchy), accent (action, highlight), or support (muted, secondary). | "Navy #203859 (primary, trust)", "Teal #61C1B6 (accent, action)", "Orange (warning, sparse)" |
| **Icon Policy** | Choose how icons will be selected. Specify one or more: generic outline family (Lucide, Feather, etc.), official vendor icons (Microsoft, Apple, Google, etc.), custom/brand assets, or a justified hybrid. | "Lucide outline (all generic concepts)", "Lucide + Microsoft/Azure vendor icons", "Custom brand asset set", "Outline family + vendor exceptions" |

### Ask Pattern

**If all four are provided:** Proceed with Step 2. No questions needed.

**If any are missing:** Ask only for the missing ones, grouped in a single concise question.

**Example (multilingual prompt with gaps):**

```
[Brand/Client]: PrevaIQ (provided)
[Authoritative Source]: TBD (missing)
[Dominant Color & Role]: TBD (missing)
[Icon Policy]: TBD (missing)

Clarification needed:
- Where is the brand kit, template, or color specification?
- What is the dominant color (name or hex)?
- Which icon family: Lucide outline, vendor icons, custom set, or hybrid?
```

### Delegation Rule

If a user delegates style choices ("decide for me", "use your judgment", "make it look good"):

- **If an authoritative identity was supplied:** Delegate stays within that identity. Apply the supplied brand kit, colors, and typography.
- **If no authoritative identity exists:** Use the Build neutral fallback (white, navy #203859, teal #61C1B6) only after explicit user approval.

---

## Step 2: Gather Evidence

**Goal:** Collect all source materials that inform the deck.

### Assets to Identify

- **Brand kit or design system** — Color palette, typography, spacing rules, icon families
- **Logo and mark files** — SVG preferred; ensure file is accessible or embedded
- **Existing decks or templates** — If repurposing or matching a house style
- **Authoritative guidelines** — Brand standards, tone, visual hierarchy
- **Content outline** — Slide titles, key points, speaker notes (if available)

### Evidence Checklist

- [ ] Brand identity is named
- [ ] Authoritative source is accessible (URL, file path, or inline specification)
- [ ] Dominant color is confirmed (name + hex if available)
- [ ] Icon family/policy is decided
- [ ] Logo and any required assets are provided or linked
- [ ] Content outline or narrative points are documented

---

## Step 3: Build Narrative Structure

**Goal:** Outline the slide sequence, flow, and speaker intent.

### Narrative Planning

- **Slide titles and sequence** — Logical flow from setup to resolution
- **Pacing and transitions** — Which slides reveal all at once? Which staged?
- **Speaker notes** — Key talking points, tone, emphasis
- **Call-to-action or conclusion** — How does the deck end?

### Multilingual Considerations

If authoring for multiple languages:

- **Identify target languages** — e.g., en, fr, es, zh-HK
- **Plan for length variation** — Some languages expand text; slides may need height or column adjustment
- **Speaker notes strategy** — Translated to match language variant, or single master?
- **Image/diagram captions** — Will be localized or shared across variants?

---

## Step 4: Preserve Style Fidelity

**Goal:** Define the visual rules that will apply across all slides.

### Design Token Application

Map identity brief decisions to CSS tokens:

| Identity Decision | CSS Token | Example |
|---|---|---|
| Dominant primary color | `--brand-primary` | `#203859` (navy) |
| Dominant accent color | `--brand-accent` | `#61C1B6` (teal) |
| Heading font | `--heading` | Bricolage Grotesque 800 |
| Body font | `--body` | Instrument Sans 400/600 |
| Label/mono font | `--mono` | IBM Plex Mono 400/600 |
| Background | `--bg` | `#F7F9FB` |
| Text color | `--text` | `#1A1A1A` |
| Muted/secondary text | `--text-muted` | `#5A6472` |

### Consistency Rules

- **Colors:** All accent colors come from `--brand-accent` or approved derivatives; no arbitrary hex
- **Typography:** Headings use `--heading`, body uses `--body`, code/labels use `--mono`
- **Spacing:** Container-query widths (`cqw`) for responsive scaling; consistent gaps between elements
- **Borders and radius:** Standard values (1–1.6px for borders, 10–12px for card radius, `999px` for pills)
- **Icons:** Selected by semantic meaning, constrained to one family for generic concepts
- **Double-theme:** If dark mode is needed, define `[data-theme="dark"]` variants for all color tokens

### Layout Grid

- **Canvas:** 16:9 aspect ratio, 1280×720 CSS px base
- **Content area:** 3–4 column width unit (cqw) padding
- **Component gaps:** 1–1.3 cqw between sections

---

## Step 5: Craft HTML Deck

**Goal:** Author semantic HTML with CSS design tokens and responsive behavior.

### HTML Structure

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Presentation Title</title>
  <style>
    :root {
      --brand-primary: #203859;
      --brand-accent: #61C1B6;
      --bg: #F7F9FB;
      --text: #1A1A1A;
      --text-muted: #5A6472;
      --heading: "Bricolage Grotesque", "Trebuchet MS", sans-serif;
      --body: "Instrument Sans", "Segoe UI", sans-serif;
      --mono: "IBM Plex Mono", Consolas, monospace;
    }
    * { box-sizing: border-box; }
    html, body { margin: 0; background: var(--bg); color: var(--text); font-family: var(--body); }
    .slide { position: relative; width: min(1280px, 100%); aspect-ratio: 16 / 9;
             container-type: inline-size; overflow: hidden; border-radius: 12px; }
    h1 { font: 800 3.35cqw / 1.08 var(--heading); }
    h2 { font: 600 1.9cqw / 1.2 var(--heading); }
    .brand { color: var(--brand-primary); }
    .accent { color: var(--brand-accent); }
  </style>
</head>
<body>
  <section class="slide" data-title="Slide 1">
    <h1>Welcome</h1>
    <p class="accent">Your story starts here.</p>
  </section>
  <!-- More slides... -->
</body>
</html>
```

### Key Authoring Rules

- **Use semantic tokens for all colors** — No hex codes in slide markup
- **Responsive sizing with `cqw`** — Font sizes, gaps, and widths scale with container
- **Accessibility:** `lang` attribute, heading hierarchy, color contrast (WCAG AA minimum)
- **Speaker notes:** Include in `<aside class="speaker-notes">` within each slide
- **Data attributes:** `data-title` on each slide for navigation labels
- **Animations:** Optional; use `@keyframes enter` with `--delay` CSS var for stagger; respect `prefers-reduced-motion`

### Icon Integration

- **Lucide icons:** Embed as inline `<svg>` with `currentColor` fill and `width: 2.4cqw` sizing
- **Brand logos:** Use `<img src="...">` with `width: 12cqw` in footers; preserve aspect ratio
- **Vendor icons:** Source from official registries (Microsoft, Azure Icon Gallery, etc.); verify provenance and retain as-is (no color override)

---

## Step 6: Convert to PowerPoint

**Goal:** Export the approved HTML deck to native `.pptx` format while preserving layout, colors, and typography.

### Conversion Process

1. **Validate HTML** — Ensure all slides render correctly in a modern browser
2. **Extract styles** — CSS tokens and layout are mapped to PowerPoint theme colors and master slide geometry
3. **Build slide deck** — Each HTML section becomes a PowerPoint slide
4. **Preserve objects** — Diagrams, charts, and embedded images are converted to PowerPoint shapes or linked images
5. **Transfer notes** — Speaker notes become PowerPoint notes pages
6. **Test platforms** — Verify `.pptx` opens correctly in PowerPoint, Google Slides, and Keynote

### Mapping Rules

| HTML Element | PowerPoint Mapping |
|---|---|
| `<h1>` with `--heading` font | Title placeholder with theme color |
| `<p>` with `--body` font | Text placeholder |
| Inline `<svg>` | Shape or picture (rasterized at 150 dpi) |
| `<img src="...">` | Picture object (linked or embedded) |
| `<aside class="speaker-notes">` | Notes page |
| CSS background color | Slide fill (theme color if applicable) |
| CSS borders and radius | Shape properties (border style, border radius) |

### Dark/Light Theme Variants

If the HTML deck defines both `[data-theme="light"]` and `[data-theme="dark"]`:

- Generate two `.pptx` files: `deck-light.pptx` and `deck-dark.pptx`
- Variant naming: Include `(Light)` or `(Dark)` in the title slide

For multilingual decks:

- Language variant naming: Include language code, e.g., `deck-en.pptx`, `deck-fr.pptx`

---

## Step 7: Validate and Iterate

**Goal:** Review the deck for fidelity to identity, correctness, and polish.

### Validation Checklist

- [ ] **Identity fidelity:** Brand primary and accent colors are applied consistently; logo is present and properly sized
- [ ] **Typography:** All headings are `--heading`, all body text is `--body`, all monospace is `--mono`
- [ ] **Spacing and alignment:** Elements are evenly spaced (using cqw units); no orphaned text or cramped layouts
- [ ] **Icons:** Correct family (all Lucide, or justified mix of Lucide + vendor icons); no color overrides on vendor marks
- [ ] **Accessibility:** Text contrast ≥ 4.5:1 (WCAG AA); headings in logical order; alt text on images
- [ ] **Animations:** Optional reveals are smooth and respect `prefers-reduced-motion` setting
- [ ] **Cross-platform:** `.pptx` opens and renders correctly in PowerPoint, Google Slides, and Keynote
- [ ] **Speaker notes:** Present and meaningful on each slide
- [ ] **Multilingual (if applicable):** All target languages are complete; fonts support target scripts (CJK, Arabic, etc.)

### Feedback Loop

- **Collect feedback** — Share a draft with stakeholders, project managers, or decision-makers
- **Document requested changes** — Note specific slides, elements, or messaging to adjust
- **Iterate** — Update HTML deck, re-export, and validate
- **Approve** — Obtain stakeholder sign-off before finalizing

---

## Step 8: Package Assets

**Goal:** Deliver the presentation and supporting files in a clean, documented package.

### Deliverables

```
presentation-package/
├── deck.pptx                    # Final PowerPoint (or deck-en.pptx, deck-light.pptx, etc. for variants)
├── deck.html                    # Source HTML (for future edits or reference)
├── BRANDING.md                  # Identity brief and design decisions (for handoff/governance)
├── assets/
│   ├── logo.svg                 # Brand logo (if extracted or original)
│   ├── palette.json             # CSS token definitions
│   └── [other brand/icon assets]
└── README.md                    # How to edit, translate, or regenerate variants
```

### BRANDING.md Template

```markdown
# Branding — [Presentation Title]

## Identity Brief

- **Brand/Client:** [Name or "brand-neutral"]
- **Authoritative Source:** [Kit URL, file, or specification]
- **Dominant Color & Role:** [Name, hex, and role]
- **Icon Policy:** [Family/policy and justification]

## Palette (CSS Tokens)

\`\`\`css
--brand-primary: [hex];
--brand-accent: [hex];
--bg: [hex];
--text: [hex];
--text-muted: [hex];
\`\`\`

## Typography

- **Headings:** [Font name, weight]
- **Body:** [Font name, weight]
- **Labels:** [Font name, weight]

## Logo and Assets

- **Logo:** [File/path or license]
- **Icons:** [Family and provenance]

## Variants

- [ ] Light theme
- [ ] Dark theme
- [ ] Language variants: [list]

## Maintenance

To edit or regenerate:
1. Open `deck.html` in a text editor
2. Update CSS tokens in `<style>` section if palette changes
3. Modify slide content as needed
4. Convert to PowerPoint using [conversion tool]
5. Update this document

---

*Last updated: [date]*
*Authored by: [creator/team]*
```

### Multilingual Package

If authoring for multiple languages, organize as:

```
presentation-package/
├── deck-en.pptx
├── deck-fr.pptx
├── deck-es.pptx
├── deck-zh-HK.pptx
├── sources/
│   ├── deck-en.html
│   ├── deck-fr.html
│   ├── ...
├── BRANDING.md (single, shared)
└── README.md (translation notes)
```

---

## Workflow Summary

| Step | Input | Decision/Action | Output |
|---|---|---|---|
| 1 | Request | Resolve 4-point identity brief | Identity brief (brand, source, color, icons) |
| 2 | Identity brief | Gather brand kit, assets, content outline | Evidence (files, links, specs) |
| 3 | Evidence | Plan slide sequence and narrative | Narrative outline + speaker notes |
| 4 | Outline | Map identity to CSS tokens, layout rules | Design token specification |
| 5 | Tokens + narrative | Author semantic HTML with CSS | HTML deck (approved) |
| 6 | HTML | Export to PowerPoint | `.pptx` file (validated) |
| 7 | `.pptx` | Review and iterate with feedback | Polished `.pptx` + sign-off |
| 8 | Approved deck | Package with branding docs | Deliverable set (`.pptx`, `.html`, `BRANDING.md`) |

---

## Notes

- **Authority-first principle:** Supplied brand kits and authoritative sources take precedence over all defaults.
- **Multilingual at any step:** Any step can introduce a new language variant; the workflow repeats only for divergent content (slides that differ by language).
- **Delegation rule:** Delegated choices apply only the neutral fallback (Build style) if no authority exists and user explicitly approves; otherwise, delegated choices stay within supplied identity.
- **Progressive refinement:** Each iteration refines the deck closer to the desired outcome; stakeholder feedback at Step 7 is expected and encouraged.
