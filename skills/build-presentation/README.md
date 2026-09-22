# build-presentation

**Create and restyle brand-aware HTML and editable PowerPoint presentations.**

## Quick Start

Use this skill when you need to:

- **Create a new presentation** — "build me a presentation for [topic/client]"
- **Convert HTML to PowerPoint** — "convert this HTML deck to PPTX"
- **Restyle an existing deck** — "restyle my slides with [brand] colors and [font]"
- **Build multilingual decks** — Supports any language; localize content and typography automatically
- **Export branded decks** — Generate `.pptx` and `.html` with matching brand identity

## How It Works

This skill is **brand-aware and agnostic**. It requires a four-point identity decision before authoring begins:

1. **Brand/Client Identity** — Name or "brand-neutral"
2. **Authoritative Source** — Brand kit URL, logo, color spec, or "none supplied"
3. **Dominant Color & Role** — Color name/hex and semantic role (primary, accent, etc.)
4. **Icon Policy** — Generic outline family, vendor icons, custom assets, or justified mix

**Only missing questions are asked.** If your request supplies all four, no additional clarification is needed.

**Fallback:** If no identity is supplied and you delegate style choices, the skill uses a neutral "Build" palette (navy, teal) only after explicit approval.

## Resources

- **[SKILL.md](SKILL.md)** — Skill overview and usage instructions
- **[references/workflow.md](references/workflow.md)** — Eight-step implementation workflow
- **[references/design-system.md](references/design-system.md)** — Color, typography, layout, icon, and component rules
- **[references/html-to-pptx.md](references/html-to-pptx.md)** — HTML to PowerPoint conversion guide

## Examples

### Example 1: Brand-Specific Presentation

```
Brand/Client: Acme Corp
Authoritative Source: Brand kit at brand.acme.com/guidelines.pdf
Dominant Color & Role: Orange #FF6B35 (primary, action)
Icon Policy: Lucide outline (all generic concepts)

Language: English
Slide count: 6 slides
Topic: Q4 Roadmap

[Your narrative and slide content...]
```

**Output:** `acme-q4-roadmap.pptx` + `acme-q4-roadmap.html` with Acme's orange and Lucide icons.

### Example 2: Brand-Neutral Presentation

```
Brand/Client: brand-neutral
Authoritative Source: none supplied
Dominant Color & Role: [delegated]
Icon Policy: [delegated]

I'd like a simple, professional pitch deck in English.
[Your narrative...]
```

**Result:** Skill asks before proceeding:
- Confirm that brand-neutral + delegated style → Build neutral palette (navy, teal) is acceptable
- Confirm Lucide outline for all generic icons

**Output:** Deck with neutral palette and standard icon family.

### Example 3: Multilingual Presentation

```
Brand/Client: PrevaIQ Assurances
Authoritative Source: Logo at assets/logos/prevaiq-logo.svg + navy #203859, teal #61C1B6
Dominant Color & Role: Navy #203859 (primary, trust); Teal #61C1B6 (accent, action)
Icon Policy: Lucide outline + Microsoft/Azure vendor icons

Languages: English, French, Spanish, Chinese (Hong Kong)
Slide count: 7 slides
Topic: Mainframe integration and agentic hub trajectory

[Your narrative with translation markers...]
```

**Output:**
- `deck-en.pptx`, `deck-fr.pptx`, `deck-es.pptx`, `deck-zh-HK.pptx` (four PPTX variants)
- `sources/deck-en.html`, `sources/deck-fr.html`, etc. (source HTML for each)
- `BRANDING.md` (shared brand documentation)

## Key Principles

**Authority-First Design** — Supplied brand identities take precedence over all defaults.

**Semantic Color Roles** — Colors are named by role (`brand-primary`, `brand-accent`) not by hex code; consistency is enforced through design tokens.

**Icon Semantics** — Icons are selected by meaning:
- Generic concepts use one outline family (e.g., Lucide) consistently
- Named products use official vendor icons (Microsoft, Apple, Google, etc.)
- Brand marks are preserved as-is
- Families are not mixed within generic concepts; exceptions are deliberate and justified

**Responsive & Accessible** — Decks scale to any viewport using container-query units; text contrast and heading hierarchy are verified.

**Multilingual by Design** — Target any language; fonts, icons, and layout adapt automatically.

## Workflow Overview

| Phase | Input | Output |
|---|---|---|
| **Identity** | Request + brand kit (if available) | Confirmed 4-point brief |
| **Evidence** | Brief + assets | Collected sources |
| **Narrative** | Sources + content outline | Slide sequence + speaker notes |
| **Design Tokens** | Brief → CSS tokens | Color/typography/spacing rules |
| **Authoring** | Tokens + narrative | Semantic HTML deck |
| **Export** | Approved HTML | `.pptx` + `.html` + `BRANDING.md` |
| **Validation** | Cross-platform test | Polished, stakeholder-approved deck |
| **Delivery** | Sign-off | Final package (PDF, PPTX, source) |

See **[references/workflow.md](references/workflow.md)** for detailed steps.

## Design System

The skill applies a cohesive design system:

- **Colors** are defined as roles (`--brand-primary`, `--brand-accent`, etc.) and mapped to CSS tokens
- **Typography** uses a standard stack: Bricolage Grotesque (headings), Instrument Sans (body), IBM Plex Mono (code)
- **Layout** is responsive with CSS Container Query units (`cqw`) for proportional scaling
- **Icons** follow strict semantic rules (one generic family, official vendor icons, brand marks as-is)
- **Components** (cards, badges, callouts) have standard definitions
- **Motion** is optional and respectful (fade-in, stagger with `prefers-reduced-motion` support)
- **Dark theme** is built-in (all tokens have light and dark variants)

See **[references/design-system.md](references/design-system.md)** for full specification.

## HTML to PowerPoint

Once your deck is approved as HTML, convert it to native PowerPoint format:

1. **Export HTML → PDF** — Browser print or Puppeteer script
2. **Convert PDF → PPTX** — CloudConvert, LibreOffice, or PptxGenJS
3. **Verify and polish** — Test in PowerPoint, Google Slides, Keynote
4. **Deliver** — Final `.pptx` + source `.html` + `BRANDING.md`

See **[references/html-to-pptx.md](references/html-to-pptx.md)** for tools and step-by-step instructions.

## Supported Languages

The skill supports decks in:

- **Western Latin** — English, French, Spanish, German, Portuguese, Italian, etc.
- **Cyrillic** — Russian, Ukrainian, Bulgarian, etc.
- **Greek** — Greek
- **Arabic** — Arabic, Persian, Urdu (right-to-left)
- **CJK** — Chinese (Simplified & Traditional), Japanese, Korean
- **Others** — Any language with Unicode support; fonts may need fallback definitions

## Typical Use Cases

- **Executive briefings** — Polished, branded decks for leadership
- **Sales pitches** — Multi-variant (light/dark, multilingual) sales decks
- **Training and education** — Accessible, translatable courseware
- **Technical presentations** — Architecture diagrams, timelines, decision trees
- **Annual reports** — Branded PDF and interactive PPTX for stakeholders
- **Conference talks** — Speaker decks with notes and easy navigation
- **Internal comms** — Newsletter-style decks with company branding

## Getting Started

Start with the **[SKILL.md](SKILL.md)** file for a complete overview. Then:

1. Read **[references/workflow.md](references/workflow.md)** to understand the eight-step process
2. Review **[references/design-system.md](references/design-system.md)** for component and styling rules
3. Use **[references/html-to-pptx.md](references/html-to-pptx.md)** when ready to export to PowerPoint

Or jump straight in:

```
I'd like to create a presentation for [client/topic] with:
[Brand/Client]: [name or "brand-neutral"]
[Authoritative Source]: [kit/file/link or "none supplied"]
[Dominant Color & Role]: [color and role]
[Icon Policy]: [family/policy]

Language: [en/fr/es/zh-HK/...]
Topic: [brief description]
Slides: [approximate count]

[Your narrative and talking points...]
```

---

**Built for clarity, authority, and coherence.** Questions? See the resource links above.
