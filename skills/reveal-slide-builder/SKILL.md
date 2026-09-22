# Reveal.js Slide Builder

Create engaging, accessible presentations with reveal.js through interactive guidance on animations, plugins, layouts, and design patterns.

## When to Use This Skill

Use this skill whenever you need to:
- **Design a new presentation deck** — "create a presentation", "build slides", "new reveal.js deck"
- **Choose animations and transitions** — "what animations work for", "animate my slides", "add movement to"
- **Select plugins** — "what plugins should I use", "add interactivity", "enable annotations"
- **Plan slide layouts** — "how should I structure this", "layout for comparison", "best format for"
- **Build accessible, audience-focused presentations** — "tailor to executives", "tech audience", "make it visual"
- **Choose colors and typography** — "color scheme for", "font choices", "design system"

## Workflow Overview

The skill follows this workflow:

1. **Audience & Intent** — Understand who will see the deck and what you want them to do
2. **Content Map** — Clarify the key message, narrative arc, and interaction points
3. **Animation & Plugin Fit** — Recommend animations and plugins matched to audience and intent
4. **Layout & Pacing** — Suggest slide structures and reveal sequencing
5. **Design & Accessibility** — Propose color, typography, spacing, and contrast choices
6. **Implementation** — Provide reveal.js syntax, HTML structure, and code examples

---

## Phase 1: Audience & Intent Discovery

Start by asking:

### Questions to Ask

1. **Primary Audience**
   - Executives / C-level (focus: outcome, ROI, risk mitigation)
   - Technical specialists (focus: architecture, internals, tradeoffs)
   - End-users / customers (focus: value, simplicity, benefits)
   - Mixed (multi-layered content, branch logic by role)

2. **Presentation Intent**
   - Inform/educate (knowledge transfer, teaching)
   - Persuade/sell (conviction, urgency, call-to-action)
   - Report/review (facts, metrics, accountability)
   - Demonstrate/explore (live interaction, discovery, workshop)

3. **Delivery Context**
   - Live presentation with speaker (room interaction, pausing, questions)
   - Self-guided viewer (kiosk, online, watch alone)
   - Hybrid (share online, present live at different times)

4. **Duration & Pacing**
   - Timeboxed (e.g., 20-minute pitch, 5-minute teaser)
   - Self-paced (viewer controls speed)
   - Live with Q&A (speaker controls flow)

5. **Key Outcomes** (Rank top 3)
   - Understand concept X
   - Decide on option Y
   - Take action Z
   - Remember insight A
   - Feel confidence/emotion B

---

## Phase 2: Content Map

### Questions to Ask

1. **Main Message** — One sentence that captures the core idea

2. **Narrative Arc**
   - Problem → Solution → Proof → Action
   - Current → Desired → Path → Commitment
   - Why → How → What → When
   - Data → Insight → Decision → Next Step

3. **Interaction Needs**
   - Read-only (no interaction needed)
   - Click-to-reveal (progressive disclosure)
   - Annotate/draw (speaker marks up live)
   - Poll/Q&A (audience input during presentation)
   - Embed live content (dashboard, video, live feed)

4. **Content Density** (Average per slide)
   - Minimal (headline + 1 visual)
   - Light (3-5 bullet points + visual)
   - Moderate (comparison table, diagram, 6-8 items)
   - Dense (detailed analysis, code, data table)

5. **Media Mix**
   - Text-dominant (words, quotations, data)
   - Visual-dominant (photos, diagrams, illustrations)
   - Hybrid (balanced text + visuals)
   - Rich media (video, animation, embedded widgets)

---

## Phase 3: Animation & Plugin Recommendations

### Animation Patterns by Audience

#### Executive / Business Audience
- **Goal**: Clarity, pace, emphasis on key numbers
- **Recommended animations**:
  - Fade in (subtle, professional)
  - Slide in from left/right (directional flow)
  - Highlight / scale pulse (draw attention to metrics)
  - Staggered list reveal (one point at a time, controlled pacing)
  - Grow / shrink (show/hide alternatives)
- **Avoid**: Spinning, bouncing, strobe, or playful effects (distract from message)
- **Best plugins**: Appearance (polished), Highlight (code if needed)

#### Technical / Developer Audience
- **Goal**: Detail, precision, traceability, live interaction
- **Recommended animations**:
  - Fade (neutral, lets content speak)
  - Grow (reveal complexity incrementally)
  - Code syntax highlight (show errors/solutions step-by-step)
  - Diagram animation (trace architecture, flow)
  - Speaker annotations (draw on live code/diagram)
- **Avoid**: Cartoon-like effects, oversimplification
- **Best plugins**: Highlight (syntax coloring), Chalkboard (live markup), Appearance
- **Consider**: Embed live demos, code playgrounds, vs. slides

#### Creative / Design Audience
- **Goal**: Inspiration, emotion, narrative flow
- **Recommended animations**:
  - Fade, slide, scale (smooth transitions)
  - Custom CSS animations (personality, brand)
  - Video backdrop (cinematic)
  - Animated SVG diagrams (show relationships, process)
  - Interactive elements (hover, click to explore)
- **Avoid**: Overuse; keep hierarchy clear
- **Best plugins**: Animate (SVG motion), Appearance (themes), custom CSS

#### End-User / Customer Audience
- **Goal**: Engagement, clarity, comfort
- **Recommended animations**:
  - Gentle fade / zoom (familiar, welcoming)
  - Progressive reveal of benefits (build confidence)
  - Interactive polls / click-through (participation)
  - Whitespace + breathing room (not cluttered)
  - Consistent timing (predictable, not jarring)
- **Avoid**: Rapid flashing, overwhelming motion
- **Best plugins**: Appearance (accessible themes), TouchControls (mobile-friendly)

---

### Plugin Recommendations by Use Case

| Plugin | Best For | Audience | Notes |
|--------|----------|----------|-------|
| **Appearance** | General styling, themes | All | Built-in, no config. Provides dark/light/serif/sans-serif toggles. Always include. |
| **Highlight** | Code syntax coloring | Tech, Dev | Integrates highlight.js. Show errors, diffs, traces. Use `data-line` to step through code. |
| **Chalkboard** | Live speaker markup | Tech, Sales, Design | Draw on slides in real-time (C=draw, B=board, X=color, D=download). Keyboard only; turn off UI buttons for offline. |
| **Animate** | SVG & CSS animation | Design, Creative | Animate SVG paths, transitions. Requires custom SVG + CSS. Heavyweight if used lightly. |
| **TouchControls** | Mobile, smartboard | Any | On-screen control bar (pen, present, timer, overview). Exclude from `?export` (PDF export mode). |
| **Search** | Find text across slides | Any | Ctrl+F to search. Lightweight. Useful for long decks. |
| **Zoom** | Detail inspection | Tech, Data | Alt+click to zoom into a region. Helpful for dense diagrams, small text. |
| **Math** | Equations, formulas | Academic, Research, Finance | KaTeX or MathJax rendering. Lightweight. Use `$$...$$` or `` `\(...\)` ``. |
| **Speaker Notes** | Speaker view | Presenter | Built-in via Notes plugin. Press S to open speaker view in popup. Offline; no external server. |

### Plugin Patterns for Offline Delivery

- **Always inline**: Appearance, Highlight, Speaker Notes (no external fetch)
- **Conditionally load**: TouchControls (exclude from `?export` mode to prevent PDF clutter)
- **Niche plugins**: Wrap in `...(window.RevealX ? [RevealX()] : [])` so missing globals don't break boot
- **Avoid**: Reveal Remote, PubNub, notell (require relay server → breaks offline guarantee)

---

## Phase 4: Layout & Pacing Recommendations

### Slide Layouts by Content Type

#### Title Slide
```html
<section>
  <h1>Main Message</h1>
  <p class="subtitle">Context or tagline</p>
  <small>Speaker name | Date | Venue</small>
</section>
```
- **Animation**: Fade in for title, then subtitle (2-step reveal)
- **Spacing**: Vertical center, ample whitespace
- **Design**: Large type (60-72px), strong contrast

#### Content + Image
```html
<section>
  <div style="display: flex; gap: 2rem;">
    <div style="flex: 1;">
      <h2>Title</h2>
      <ul>
        <li class="fragment fade-in-then-semi-out">Point 1</li>
        <li class="fragment fade-in-then-semi-out">Point 2</li>
      </ul>
    </div>
    <div style="flex: 1;">
      <img src="image.png" alt="Description" />
    </div>
  </div>
</section>
```
- **Animation**: Staggered list + image (fade-in-then-semi-out grays previous points)
- **Spacing**: 2rem gap, 1:1 ratio or 60/40 split
- **Design**: Left text, right visual (left-to-right reading)

#### Comparison / Two-Column
```html
<section>
  <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 2rem;">
    <div>
      <h3>Option A</h3>
      <ul>
        <li>✓ Benefit 1</li>
        <li>✗ Cost 1</li>
      </ul>
    </div>
    <div>
      <h3>Option B</h3>
      <ul>
        <li>✓ Benefit 2</li>
        <li>✗ Cost 2</li>
      </ul>
    </div>
  </div>
</section>
```
- **Animation**: Reveal each column separately (build decision process)
- **Spacing**: Equal columns, 2rem gap
- **Design**: Consistent styling per column, clear labels

#### Code / Technical Detail
```html
<section>
  <h2>How It Works</h2>
  <pre><code data-line="5-7|9-11" data-trim>
    function example() {
      // Step 1
      const data = fetch(url);
      
      // Highlighted first
      const result = process(data);
      return result;
      
      // Then this part
      console.log(result);
    }
  </code></pre>
</section>
```
- **Animation**: `data-line="5-7|9-11"` steps through code sections
- **Spacing**: Monospace font, syntax highlight, visible line numbers
- **Design**: Dark background for readability, increase font size (16-18px minimum)

#### Quote / Callout
```html
<section style="background: linear-gradient(135deg, #f5f5f5, #e0e0e0);">
  <blockquote style="font-size: 2.5rem; text-align: center;">
    <p>"The hard part isn't writing code—it's reviewing and releasing it safely."</p>
    <small>— Your speaker</small>
  </blockquote>
</section>
```
- **Animation**: Fade in, pause for emphasis
- **Spacing**: Large type, centered, ample padding
- **Design**: High contrast background, serif font for formality (optional)

#### Data / Dashboard
```html
<section>
  <h2>Q3 Metrics</h2>
  <div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 1.5rem;">
    <div class="fragment" style="text-align: center; padding: 1rem; border: 2px solid #0066cc;">
      <div style="font-size: 3rem; font-weight: bold;">42%</div>
      <small>Growth YoY</small>
    </div>
    <!-- repeat for other metrics -->
  </div>
</section>
```
- **Animation**: Staggered reveal of metric cards
- **Spacing**: Grid layout, 1.5rem gaps, consistent card size
- **Design**: Numerical hierarchy (large numbers, small labels), border or background for emphasis

### Pacing Strategy

| Audience | Slides Per Minute | Notes |
|----------|-------------------|-------|
| Executive | 0.5–1 | Plenty of time to digest, pause for Q&A |
| Technical | 1–1.5 | Code, diagrams need time; skip verbose bullet points |
| End-user | 0.75–1.25 | Balanced: not too fast, not too slow |
| Self-paced | N/A | Viewer controls (no time pressure) |

---

## Phase 5: Design & Accessibility

### Color Palette Strategy

#### Brand-Agnostic, Accessible Foundation

**Neutral Base** (background + text):
- Light: Off-white (#f8f8f8, #fafafa) or white (#ffffff)
- Dark: Near-black (#1a1a1a, #222222) or true dark gray (#2c2c2c)
- Text on light: #1a1a1a or #2c2c2c (contrast ratio ≥ 7:1)
- Text on dark: #f8f8f8 or #ffffff (contrast ratio ≥ 7:1)

**Accent Colors** (call-to-action, emphasis, hierarchy):
- Primary accent: #0066cc (mid-blue, high contrast on light/dark)
- Success: #0d8659 (teal-green, WCAG AA)
- Warning: #d97706 (amber, not pure yellow)
- Critical: #cc0000 (red, reserved for errors only)

**Usage**:
- Headings: Primary accent or text color
- Links: Primary accent + underline
- Highlights: Accent color with semi-transparent background `rgba(0, 102, 204, 0.1)`
- Data viz: Use colorblind-safe palette (blue, orange, gray)

### Typography

| Use | Font | Size (px) | Weight | Notes |
|-----|------|-----------|--------|-------|
| Headings (h1, h2) | Sans-serif (Barlow, Source Sans Pro, System) | 48–72 | 600–700 (bold) | Hierarchy, strong contrast |
| Body text | Sans-serif (Barlow, Source Sans Pro, System) | 24–32 | 400 (regular) | Readable at distance (~8ft for 1080p) |
| Small text (labels, captions) | Sans-serif | 14–18 | 400 | Only for secondary info; avoid tiny text |
| Code / monospace | Courier New, Monaco, Fira Code | 16–20 | 400 | Syntax highlight for readability |
| Quotes / emphasis | Serif (Garamond, Georgia) or sans-serif italic | 28–36 | 400 italic | Visually distinct from body |

### Spacing & Whitespace

- **Slide padding**: 3–5% of slide width on all sides (build breathing room)
- **Vertical rhythm**: 1.5–2x line height for body text (24–32px for 16px base)
- **Between elements**: Use consistent gaps (0.5rem, 1rem, 1.5rem, 2rem)
- **List indentation**: 1.5rem per level
- **Content width**: Max 90% of slide width; center content

### Contrast Checklist

- [ ] Headings vs. background: contrast ratio ≥ 7:1 (WCAG AAA)
- [ ] Body text vs. background: contrast ratio ≥ 4.5:1 (WCAG AA)
- [ ] Data labels vs. chart background: ratio ≥ 4.5:1
- [ ] Interactive elements (buttons, links): visible focus indicator (outline, underline, or color change)
- [ ] Color-only coding: Never use color alone to convey info; add icons, text, or patterns

### Accessibility Best Practices

1. **Alt text for images**: `<img src="..." alt="Specific, descriptive text" />`
2. **Semantic HTML**: Use `<h2>`, `<h3>` (not `<div>` + CSS) for structure
3. **Keyboard navigation**: Ensure all interactive elements (buttons, links) are keyboard-accessible
4. **Focus indicators**: Default blue outline or custom style visible at ≥ 3:1 contrast
5. **Screen reader support**: Use `aria-label`, `aria-describedby` for complex diagrams
6. **Captions & transcripts**: For video and audio content
7. **Readable fonts**: Avoid decorative fonts for body text; use sans-serif for screen readability

---

## Phase 6: Implementation & Code Examples

### Basic Reveal.js Setup

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Presentation Title</title>
  <link rel="stylesheet" href="reveal.js/dist/reveal.css" />
  <link rel="stylesheet" href="assets/deck-theme.css" />
</head>
<body>
  <div class="reveal">
    <div class="slides">
      <!-- Slides go here -->
    </div>
  </div>
  <script src="reveal.js/dist/reveal.js"></script>
  <script src="assets/deck-behavior.js"></script>
</body>
</html>
```

### Fragment Animations (Progressive Reveal)

```html
<!-- Fade in -->
<li class="fragment fade-in">Point 1</li>

<!-- Fade in, then semi-transparent (previous points gray out) -->
<li class="fragment fade-in-then-semi-out">Point 2</li>

<!-- Grow (scale up with fade) -->
<li class="fragment grow">Point 3</li>

<!-- Highlight (blink color or background) -->
<span class="fragment highlight-red">Important</span>

<!-- Stagger: reveal order (step 1, step 2, ...) -->
<li class="fragment" data-fragment-index="1">First</li>
<li class="fragment" data-fragment-index="2">Second</li>
<li class="fragment" data-fragment-index="3">Third</li>
```

### Speaker Notes

```html
<section>
  <h2>Slide Title</h2>
  <p>Slide content</p>
  <aside class="notes">
    - Emphasize key insight here
    - Pause for questions
    - Transition to next topic with: "Now, let's explore..."
  </aside>
</section>
```

Press **S** to open speaker view (popup with notes + timer).

### Lightbox for Images / Videos

```html
<!-- Image preview -->
<img src="thumbnail.png" data-preview-image="full.png" alt="Description" />

<!-- Video preview (click to play in fullscreen) -->
<img src="poster.png" data-preview-video="https://example.com/video.mp4" alt="Video" />
```

### Code Highlighting with Line Steps

```html
<pre><code data-line="2,4|6-8|10">
  function calculate(x) {
    const step1 = x * 2;      // Line 2 highlighted first
    
    const step2 = step1 + 1;  // Line 4 highlighted next
    
    // Then lines 6-8
    if (step2 > 10) {
      return step2;
    }
    console.log(step2);       // Finally line 10
  }
</code></pre>
```

Press spacebar to advance through `data-line` groups.

---

## Design System Recommendations by Audience

### Executive Deck
```css
/* Professional, minimal, high-contrast */
.reveal {
  --color-bg: #ffffff;
  --color-text: #1a1a1a;
  --color-accent: #0066cc;
  --font-main: "Source Sans Pro", sans-serif;
  font-size: 28px;
}
.reveal h2 { color: var(--color-accent); font-size: 56px; }
.reveal li { margin-bottom: 1rem; }
```

### Technical Deck
```css
/* Dark mode, syntax-friendly, code-forward */
.reveal {
  --color-bg: #1a1a1a;
  --color-text: #f8f8f8;
  --color-accent: #00d4ff;
  --font-main: "Source Sans Pro", sans-serif;
  font-size: 24px;
}
.reveal pre { background: #0d1117; padding: 1.5rem; }
.reveal code { color: #58a6ff; }
```

### Creative Deck
```css
/* Bold, dynamic, visual-first */
.reveal {
  --color-bg: linear-gradient(135deg, #f5f5f5, #e8e8e8);
  --color-text: #1a1a1a;
  --color-accent: #ff6b6b;
  --font-main: "Barlow", sans-serif;
  font-size: 32px;
}
.reveal section { text-align: center; }
.reveal img { max-width: 90%; border-radius: 8px; box-shadow: 0 10px 30px rgba(0,0,0,0.3); }
```

---

## Quick Decision Tree

Use this flowchart to quickly recommend animations and plugins:

```
START: "What's your primary audience?"

├─ EXECUTIVE
│  ├─ Animation: Fade, Slide (directional), Scale (for emphasis)
│  ├─ Plugins: Appearance, Highlight (if code)
│  └─ Layout: Minimal text, large numbers, one idea per slide
│
├─ TECHNICAL
│  ├─ Animation: Fade, Code line-step (data-line), Diagram trace
│  ├─ Plugins: Highlight (syntax), Chalkboard (live markup), Zoom
│  └─ Layout: Code blocks, architecture diagrams, detailed captions
│
├─ CREATIVE / DESIGN
│  ├─ Animation: Fade, Scale, SVG motion (Animate plugin), CSS custom
│  ├─ Plugins: Animate, Appearance, custom CSS
│  └─ Layout: Visual-dominant, asymmetric, whitespace emphasis
│
└─ END-USER / CUSTOMER
   ├─ Animation: Gentle fade, Zoom, Progressive reveal
   ├─ Plugins: TouchControls, Appearance (accessible theme)
   └─ Layout: Centered, large type, ample breathing room
```

---

## File Structure for a Reveal Deck

```
my-presentation/
├── index.html               # Main deck
├── assets/
│   ├── deck-theme.css       # Custom styles
│   ├── deck-behavior.js     # Boot + plugin config
│   ├── fonts/               # Local fonts (Barlow, Source Sans Pro)
│   ├── icons/               # SVG icons (Lucide or custom)
│   ├── images/              # Presentation images
│   ├── diagrams/            # Mermaid or SVG diagrams
│   ├── video/               # Video files + subtitles (*.srt)
│   └── vendor/
│       ├── appearance/      # appearance.js, appearance.css
│       ├── plugins/         # chalkboard, highlight, animate, etc.
│       └── reveal.js/       # reveal.js library (dist/)
├── qa/                      # Quality assurance scripts
│   ├── verify-decks.cjs     # Check offline, remoteRequests, structure
│   ├── export-presentations.cjs  # Generate PPTX
│   └── package-offline.cjs  # Bundle for offline ZIP
└── README.md                # Deck documentation
```

---

## Validation Checklist

Before finalizing your deck, confirm:

- [ ] **Offline ready**: No remote CDN fetches; all assets local (`remoteRequests === 0`)
- [ ] **Animations purposeful**: Each animation supports the message (not gratuitous)
- [ ] **Plugin load**: All plugins in `deck-behavior.js` `plugins:[]` array; optional plugins wrapped in `...(window.X ? [X()] : [])`)
- [ ] **Accessibility**: Contrast ≥ 4.5:1, alt text for images, semantic HTML, keyboard navigation
- [ ] **Pacing**: Slide count / speaker duration matches intended pace (e.g., 20 slides ÷ 20 min = 1 slide/min)
- [ ] **Speaker notes**: Key talking points in `<aside class="notes">` for each slide
- [ ] **Export readiness**: TouchControls excluded from `?export` mode (PDF export)
- [ ] **Tested on target device**: Preview on projector, mobile, or intended viewing context

---

---

## Presenter Slide Setup

If your deck includes a **title slide with presenter credentials** (speaker intro slide), gather this information upfront:

### Presenter Slide Questions

1. **Presenter Photo**
   - Do you have a headshot or profile photo? (JPG, PNG recommended)
   - Preferred size/format (square, circular crop, full-body?)
   - Permission/rights confirmed?

2. **Branding Elements**
   - Include Microsoft logo? (If yes, which variant: full logo, icon-only, or monochrome?)
   - Company/organization logo? (If yes, provide file)
   - Logo placement (top-left, top-right, bottom, centered?)

3. **Interactive Elements**
   - Generate LinkedIn QR code / flashcode linking to your profile?
   - If yes, provide LinkedIn URL
   - Position on slide (bottom-right, bottom-left, or separate QR slide?)
   - Display URL text or QR only?

4. **Presenter Notes Generation**
   - Do you want speaker notes auto-generated? (Yes/No)
   - If yes, what **language** for speaker notes?
     - English
     - French (Français)
     - Spanish (Español)
     - German (Deutsch)
     - Other (specify)
   - What **scope** should notes cover?
     - Key talking points only (bullets, 1–2 lines per slide)
     - Full narration (complete sentences, natural speech rhythm)
     - Timing cues (pause points, how long to dwell on each slide)
     - Q&A prompts (suggested audience questions + answers)
     - Transitions (bridge language between slides)

### Presenter Slide HTML Template

```html
<section id="presenter-slide">
  <div class="presenter-grid">
    <!-- Photo -->
    <img src="assets/images/presenter-photo.jpg" alt="Your Name" class="presenter-photo" />
    
    <!-- Credentials -->
    <div class="presenter-info">
      <h2>Your Name</h2>
      <p>Title / Role</p>
      <p>Company / Organization</p>
      
      <!-- LinkedIn QR -->
      <img src="assets/images/linkedin-qr.png" alt="LinkedIn QR Code" class="qr-code" />
      <p class="qr-label">linkedin.com/in/yourprofile</p>
    </div>
    
    <!-- Logos -->
    <div class="logos">
      <img src="assets/images/microsoft-logo.svg" alt="Microsoft" class="logo" />
      <img src="assets/images/company-logo.svg" alt="Company" class="logo" />
    </div>
  </div>
  
  <aside class="notes">
    Welcome everyone. I'm [Name], [title]. Today we'll explore [topic].
    Pause here to let people settle in.
  </aside>
</section>
```

### Presenter Slide CSS Starter

```css
#presenter-slide {
  background: linear-gradient(135deg, #ffffff, #f5f5f5);
  display: flex;
  align-items: center;
  justify-content: center;
}

.presenter-grid {
  display: grid;
  grid-template-columns: 1fr 1.5fr 1fr;
  gap: 2rem;
  align-items: center;
  max-width: 90%;
}

.presenter-photo {
  width: 200px;
  height: 200px;
  border-radius: 50%;
  object-fit: cover;
  border: 3px solid #0066cc;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.15);
}

.presenter-info h2 {
  font-size: 48px;
  margin: 0 0 0.5rem 0;
  color: #1a1a1a;
}

.presenter-info p {
  font-size: 24px;
  margin: 0.5rem 0;
  color: #555555;
}

.qr-code {
  width: 120px;
  height: 120px;
  margin-top: 1rem;
  border: 1px solid #ccc;
}

.qr-label {
  font-size: 14px;
  color: #999;
  margin-top: 0.5rem;
}

.logos {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  justify-self: center;
}

.logos .logo {
  height: 60px;
  width: auto;
  object-fit: contain;
}
```

---

## Resources

- **Reveal.js Docs**: https://revealjs.com/
- **Reveal.js Plugins & Tools**: https://github.com/hakimel/reveal.js/wiki/Plugins,-Tools-and-Hardware
- **Rajgoel Reveal.js Plugins** (Chalkboard, Animate, Appearance): https://github.com/rajgoel/reveal.js-plugins
- **Accessibility (WCAG 2.1)**: https://www.w3.org/WAI/WCAG21/quickref/
- **Colorblind-Safe Palettes**: https://colorblindok.com/
- **Typography for Presentations**: https://www.typewolf.com/
- **Icon Libraries**: Lucide, Feather, Material Design Icons (all available as SVG)
- **QR Code Generators**: https://www.qr-code-generator.com/ (SVG export for integration)

---

## When to Hand Off

Delegate specific implementation to other skills:

- **Slide creation**: Use `new-topic` or `add-node` for Copilot Studio; adapt for reveal.js HTML
- **Animation fine-tuning**: Reference reveal.js docs for fragment types and timing
- **Plugin debugging**: Check plugin repo (GitHub) for known issues and configuration
- **PPTX export**: Use your build script (e.g., `export-presentations.cjs`) or reveal.js `?print-pdf`
- **Offline verification**: Run `qa/verify-decks.cjs` to confirm zero remote requests

