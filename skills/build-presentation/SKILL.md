# build-presentation

**Create and restyle brand-aware HTML and editable PowerPoint presentations.** Use for "create a presentation", "HTML presentation", "PowerPoint deck", "HTML to PowerPoint", or when building branded decks with coherent palettes, icon systems, rounded diagrams, clear typography, and staged reveals.

## When to use this skill

- **Create a new presentation** with an explicit brand identity
- **Convert HTML to PowerPoint** while preserving identity and style
- **Restyle** an existing slide deck with brand colors, typography, and spacing
- **Build decks** for any topic, audience, or language with control over visual cohesion

## How it works

**This skill is brand-aware and agnostic.** It does not assume a default palette, icon family, or vendor identity. Instead, it requires you to resolve an **identity brief** first—a four-point decision that anchors the entire deck:

1. **Brand/client identity** — Explicitly name the brand, client, or request "brand-neutral" if no specific identity applies
2. **Authoritative source** — Provide a brand kit, design template, logo files, or exact guidelines (a link, a file, a color code, a typography rule)
3. **Dominant color and role** — Name the primary color and whether it serves as primary, accent, or support; include semantic intent (e.g., "navy for trust, teal for action")
4. **Icon policy** — Decide on one of:
   - Generic outline family (Lucide, Feather, etc.) for universal concepts
   - Official vendor icons (Microsoft, Apple, Google, etc.) for named products
   - Custom/brand asset set for branded marks
   - Intentional mix of families for specific reasons (described)

**Only missing questions are asked.** If your request already specifies brand, source, color, and icon rules, no additional clarification is needed. If some are missing, you will be asked precisely which ones.

**Fallback condition:** The skill uses a neutral "Build" palette (white, navy, teal) only if:
- You explicitly request "brand-neutral" AND no authoritative identity exists, OR
- You delegate style choices AND no brand kit or authoritative identity has been supplied

**If an authoritative brand or template has been supplied, delegated choices stay within that identity.** The skill never overrides a supplied brand kit with a default.

## Identity brief example

For a multilingual prompt, include these four decisions (or mark ones to be determined):

```
[Brand/Client]: PrevaIQ Assurances | Brand-neutral | TBD
[Authoritative Source]: Brand kit at company.com/branding | Logo SVG file | Color spec + typography doc
[Dominant Color & Role]: Navy #203859 (primary, trust); Teal #61C1B6 (accent, action)
[Icon Policy]: Lucide outline (generic concepts) + official vendor icons (Microsoft/Azure products)
```

## Implementation

The workflow is structured in eight steps:

1. **Establish identity brief** — Resolve brand, source, dominant color, and icon policy
2. **Gather evidence** — Collect source materials (brand kit, existing decks, logo files)
3. **Build narrative structure** — Outline slide sequences, transitions, speaker flow
4. **Preserve style fidelity** — Apply palette, typography, spacing, and motion consistently
5. **Craft HTML deck** — Author the semantic HTML with CSS design tokens and responsive layout
6. **Convert to PowerPoint** — Export approved HTML to native `.pptx` with object preservation
7. **Validate and iterate** — Review, test across platforms, adjust based on feedback
8. **Package assets** — Deliver the `.pptx`, `.html`, and brand documentation

See **references/workflow.md** for detailed implementation steps.

## Design system

The skill applies a semantic, authority-first design system:

- **Colors** are defined as roles (`brand-primary`, `brand-accent`, `surface`, `text`) not hex codes
- **Typography** uses a coherent stack (Bricolage Grotesque for headings, Instrument Sans for body, IBM Plex Mono for labels)
- **Icons** are selected by semantic meaning, not speculation:
  - Generic concepts use a single outline family
  - Named vendor products use official vendor icons (verified provenance)
  - Brand marks preserve their geometry and approved appearance
  - Families are not mixed within generic concepts; brand and vendor exceptions are deliberate
- **Layout** is responsive (CSS container queries at 1280px base canvas)
- **Motion** is optional and respectful (fade in, slide, stagger reveal with `prefers-reduced-motion` support)

The **Build neutral palette** (white, navy #203859, teal #61C1B6) is a fallback only—it is never imposed when an authoritative identity is supplied.

For full design rules, see **references/design-system.md**.

## HTML to PowerPoint

Once your HTML deck is approved, the skill converts it to native PowerPoint (`.pptx`):

- Slides become PowerPoint slides with preserved layout, colors, and typography
- Diagrams and charts remain as shapes or embedded objects
- Speaker notes transfer to notes pages
- Dark/light theme variants can generate separate decks if needed

See **references/html-to-pptx.md** for conversion rules and object mapping.

## Multilingual support

Decks can be authored in any language. When authoring, specify:

- **Target language(s)** — Single language, or multiple subtitles/versions
- **Localization scope** — UI labels only, or full content translation
- **Speaker notes** — Translated to match each language variant

Fonts and icon systems scale across scripts (Latin, CJK, Arabic, etc.) without re-authoring.

## Usage

```
Create a presentation for [client/topic] with:
[Brand/Client]: [name or "brand-neutral"]
[Authoritative Source]: [kit/template/file/link or "none specified"]
[Dominant Color & Role]: [color name, hex, and role—e.g., "navy #203859 (primary)"]
[Icon Policy]: [outline family | vendor icons | custom set | hybrid with reason]

Language: [en | fr | es | zh-HK | other]
Slide count: [N] slides
Audience: [role/context]
Topic: [brief description]
```

Then describe your narrative, key points, and any existing assets to repurpose.

---

**Full references:** See [workflow.md](references/workflow.md), [design-system.md](references/design-system.md), [html-to-pptx.md](references/html-to-pptx.md).
