# HTML to PowerPoint Conversion

## Overview

Once your HTML presentation deck is approved, convert it to native PowerPoint format (`.pptx`) for compatibility with Microsoft Office, Google Slides, and Keynote.

This guide covers:
- **Conversion tools and methods**
- **Object and style mapping**
- **Theme and variant handling**
- **Troubleshooting**

---

## Conversion Methods

### Option 1: Browser-Based Export (Recommended for Most Decks)

**Tools:**
- [Marp](https://marp.app) — Markdown to HTML/PPTX (if source is Markdown)
- [Reveal.js Export](https://revealjs.com/pdf-export/) — HTML to PDF, then PDF to PPTX
- Native browser "Save as PDF" → PDF-to-PPTX converter

**Workflow:**

1. **Open HTML deck in Chrome or Edge**
   ```bash
   open file:///path/to/deck.html
   ```

2. **Trigger print dialog** — `Ctrl+P` (Windows) or `Cmd+P` (Mac)

3. **Export to PDF**
   - Destination: "Save as PDF"
   - Margins: None
   - Paper size: 13.33 × 7.5 inches (16:9)
   - Background graphics: Enabled

4. **Convert PDF to PPTX**
   - Use online tool: [CloudConvert](https://cloudconvert.com/pdf-to-pptx), [iLovePDF](https://www.ilovepdf.com/pdf-to-pptx)
   - Or desktop tool: [PDF2Go](https://www.pdf2go.com/), [Aspose.PDF](https://products.aspose.app/)

5. **Polish in PowerPoint**
   - Verify layout and colors
   - Adjust text if needed
   - Correct any rasterization artifacts

---

### Option 2: JavaScript Library (Advanced)

**Tools:**
- [Puppeteer](https://pptr.dev/) — Headless browser automation (Node.js)
- [PptxGenJS](https://gitbrent.github.io/PptxGenJS/) — Generate PPTX from JavaScript
- [Decktape](https://github.com/astefanutti/decktape) — Specialized HTML deck to PDF exporter

**Workflow (Puppeteer → PDF):**

```javascript
// convert-to-pdf.js
const puppeteer = require('puppeteer');
const fs = require('fs');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  
  await page.goto('file:///path/to/deck.html', { waitUntil: 'networkidle0' });
  
  await page.pdf({
    path: 'deck.pdf',
    width: '1280px',
    height: '720px',
    margin: { top: 0, right: 0, bottom: 0, left: 0 },
    printBackground: true
  });
  
  await browser.close();
})();
```

```bash
node convert-to-pdf.js
```

Then convert PDF to PPTX using a tool like [libreoffice](https://www.libreoffice.org/):

```bash
libreoffice --headless --convert-to pdf:writer_pdf_Export deck.pdf
```

---

### Option 3: Direct PPTX Generation (For Maximum Control)

**Tools:**
- [PptxGenJS](https://gitbrent.github.io/PptxGenJS/) — JavaScript library
- [python-pptx](https://python-pptx.readthedocs.io/) — Python library
- [OpenXML/Ooxml](https://docs.microsoft.com/en-us/openspecs/office_standards/ms-pptx/) — Direct XML editing

**Workflow (PptxGenJS Example):**

```javascript
// generate-pptx.js
const PptxGenJS = require("pptxgenjs");

const pres = new PptxGenJS();

// Set theme colors from identity brief
pres.defineLayout({ name: 'LAYOUT16x9', width: 10, height: 5.625 });
pres.theme = {
  name: 'Office Theme',
  colorScheme: {
    accent1: '203859',  // Navy
    accent2: '61C1B6',  // Teal
  }
};

// Slide 1: Title
const slide1 = pres.addSlide('LAYOUT16x9');
slide1.background = { color: 'F7F9FB' };
slide1.addText('Welcome', {
  x: 0.5, y: 2.0, w: 9, h: 1.5,
  fontSize: 54,
  fontFace: 'Bricolage Grotesque',
  fontWeight: 800,
  color: '203859'
});

// Add more slides...

pres.writeFile({ fileName: 'deck.pptx' });
```

```bash
npm install pptxgenjs
node generate-pptx.js
```

---

## Object Mapping: HTML → PowerPoint

### Text Elements

| HTML | PowerPoint | Notes |
|---|---|---|
| `<h1>` | Title placeholder | Font: `--heading`, Color: `--brand-primary` |
| `<h2>` | Subtitle or body text | Font: `--heading`, smaller size |
| `<p>` | Body text placeholder | Font: `--body` |
| `<em>`, `<strong>` | Italic / bold formatting | Preserved in PowerPoint text |
| `<code>` | Monospace text | Font: `--mono`, background highlight |

### Visual Elements

| HTML | PowerPoint | Mapping |
|---|---|---|
| `<div class="card">` | Grouped shape or rectangle | Fill color from token, border from CSS |
| Inline `<svg>` (icon) | Picture object | Rasterized at 150 dpi; size preserved |
| `<img>` | Picture object | Linked if remote; embedded if local |
| SVG diagram | Picture or editable shape | Rasterized; consider exporting as PDF vector for editability |
| `<table>` | PowerPoint table | Columns/rows mapped; formatting preserved |
| `<canvas>` | Rasterized picture | Export canvas as PNG, embed as picture |

### Colors and Styles

| CSS Property | PowerPoint Equivalent |
|---|---|
| `background-color: var(--brand-primary)` | Slide or shape fill (use theme color if possible) |
| `color: var(--text)` | Font color |
| `border: 1px solid var(--line)` | Shape border (line style, weight, color) |
| `border-radius: 12px` | Shape adjust (if supported) |
| `box-shadow: 0 2px 8px rgba(...)` | Shadow effect (if available) |
| `opacity: 0.8` | Transparency/alpha |
| `transform: translate(...) scale(...)` | Not directly supported; flatten before export |

### Speaker Notes

| HTML | PowerPoint |
|---|---|
| `<aside class="speaker-notes">` | Notes page (below slide) |

---

## Conversion Workflow: Step-by-Step

### 1. Validate HTML

Before exporting, ensure:
- All slides render correctly in a browser (Chrome, Edge, Firefox)
- Colors and fonts display as expected
- Images and SVG icons load without errors
- Responsive layout works at 1280×720 (or your base size)

```bash
# Quick validation in browser
open file:///path/to/deck.html

# Or automated (Puppeteer)
node validate-deck.js
```

### 2. Prepare for Export

Add print-specific styles if not already present:

```html
<style>
  @page {
    size: 13.333333in 7.5in;
    margin: 0;
  }
  @media print {
    html, body { margin: 0; background: white; print-color-adjust: exact; }
    .slide { display: block; width: 13.333333in; height: 7.5in; page-break-after: always; }
    .controls, .speaker-notes { display: none; }
  }
</style>
```

### 3. Export to PDF

**Option A: Browser Print**

```
1. Open deck.html in Chrome
2. Ctrl+P (Windows) or Cmd+P (Mac)
3. Destination: "Save as PDF"
4. Paper size: 13.333333 × 7.5 inches (16:9)
5. Margins: None
6. Background graphics: Checked
7. Click "Save"
```

**Option B: Puppeteer Script**

```javascript
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  
  await page.goto('file:///path/to/deck.html', { waitUntil: 'networkidle0' });
  await page.pdf({
    path: 'deck.pdf',
    width: '1280px',
    height: '720px',
    printBackground: true
  });
  
  await browser.close();
})();
```

### 4. Convert PDF to PPTX

**Option A: Online Tool**

Visit [CloudConvert](https://cloudconvert.com/pdf-to-pptx):
1. Upload PDF
2. Select "PPTX" output format
3. Download converted file

**Option B: LibreOffice (Command Line)**

```bash
libreoffice --headless --convert-to pptx deck.pdf
```

This generates `deck.pptx` in the same directory.

**Option C: Python Script (Advanced)**

```python
# convert.py
from pdf2image import convert_from_path
from pptx import Presentation
from pptx.util import Inches, Pt

# Convert PDF pages to images
images = convert_from_path('deck.pdf', dpi=150)

# Create PPTX
prs = Presentation()
prs.slide_width = Inches(13.333)
prs.slide_height = Inches(7.5)

for img in images:
    slide = prs.slides.add_slide(prs.slide_layouts[6])  # Blank layout
    left = Inches(0)
    top = Inches(0)
    pic = slide.shapes.add_picture(
        img,
        left, top,
        width=prs.slide_width,
        height=prs.slide_height
    )

prs.save('deck.pptx')
print("✓ Conversion complete: deck.pptx")
```

```bash
python convert.py
```

### 5. Polish in PowerPoint

Open `deck.pptx` and review:

- [ ] Slide count matches original HTML
- [ ] Colors render correctly (compare to brand kit)
- [ ] Typography is readable (no small fonts crushing during rasterization)
- [ ] Images and icons are sharp and correctly sized
- [ ] Speaker notes populated on slide notes pages
- [ ] No text overflow or layout breaks

### 6. Adjust and Re-export

If adjustments are needed:

**Minor fixes (text, colors):** Edit directly in PowerPoint
- Update title text
- Adjust shape colors
- Resize images

**Major changes (layout, structure):** Edit HTML source and re-export
1. Modify `deck.html`
2. Re-export to PDF
3. Re-convert to PPTX

---

## Theme and Variant Handling

### Single Deck

Export as `deck.pptx`:

```
deck/
├── deck.pptx
├── deck.html
└── BRANDING.md
```

### Dark Theme Variant

If HTML defines `[data-theme="dark"]`, export two versions:

1. **Light theme:** Default (or triggered by `data-theme="light"`)
   ```bash
   node export-pdf.js --theme=light
   # Output: deck-light.pdf → deck-light.pptx
   ```

2. **Dark theme:** Triggered by `data-theme="dark"`
   ```bash
   node export-pdf.js --theme=dark
   # Output: deck-dark.pdf → deck-dark.pptx
   ```

**Naming:**
```
deck/
├── deck-light.pptx
├── deck-dark.pptx
├── deck.html
└── BRANDING.md
```

### Multilingual Variants

Export one `.pptx` per language:

```
deck/
├── deck-en.pptx
├── deck-fr.pptx
├── deck-es.pptx
├── deck-zh-HK.pptx
├── sources/
│   ├── deck-en.html
│   ├── deck-fr.html
│   ├── deck-es.html
│   └── deck-zh-HK.html
└── BRANDING.md (single, shared)
```

---

## Troubleshooting

### Issue: Colors appear washed out or incorrect in PPTX

**Cause:** PDF export lost color precision; HTML-to-PDF color management issue.

**Fix:**
1. Validate CSS tokens are using exact hex codes: `--brand-primary: #203859` (not `navy`)
2. Test print preview in browser; compare to original
3. Re-export with `print-color-adjust: exact` CSS rule
4. Use option 3 (PptxGenJS) to generate PPTX directly from parsed HTML

### Issue: Text size too small or too large in PPTX

**Cause:** Rasterization or font scaling mismatch during PDF export.

**Fix:**
1. Check `@page` size matches slide dimensions (13.333 × 7.5 inches for 16:9)
2. Verify base `cqw` calculations: at 1280px canvas, 1.7cqw = 22px
3. Increase base slide width in HTML if exporting a smaller canvas
4. Use direct PPTX generation (option 3) for more control

### Issue: Images/SVGs blurry or pixelated in PPTX

**Cause:** Rasterization at low DPI; or SVG rendered at low resolution.

**Fix:**
1. Export PDF at higher DPI: `--dpi=300` (Puppeteer) or use browser zoom before printing
2. Export SVG icons as vector (PDF) instead of rasterized (PNG)
3. For critical diagrams, export as PDF and embed vector in PPTX

### Issue: Speaker notes missing from PPTX

**Cause:** HTML `<aside class="speaker-notes">` not mapped during conversion.

**Fix:**
1. If using browser PDF export: speaker notes are lost; use direct PPTX generation (option 3)
2. If using PptxGenJS: explicitly map notes:
   ```javascript
   slide.addNotes('Speaker note text here');
   ```
3. Manually add notes in PowerPoint after export

### Issue: Animations / transitions not preserved

**Cause:** PDF and static PPTX don't support CSS animations; they are flattened.

**Fix:**
1. Export all animation states as individual slides (if important to preserve)
2. Or: Accept that transitions are lost in PDF/PPTX; these formats are static
3. Use PowerPoint native transitions and animations in the final deck if needed

### Issue: Dark theme variant doesn't generate

**Cause:** `[data-theme="dark"]` CSS not applied during export.

**Fix:**
1. Export script must set `data-theme="dark"` attribute on root element before printing:
   ```javascript
   await page.evaluate(() => {
     document.documentElement.setAttribute('data-theme', 'dark');
   });
   ```
2. Wait for CSS reflow: `await page.waitForTimeout(500)`
3. Then export to PDF

---

## Verification Checklist

After converting to PPTX, verify:

- [ ] **Slide count** — Matches original HTML
- [ ] **Layout** — No text overflow; elements in correct positions
- [ ] **Colors** — Brand colors rendered accurately; compare to brand kit
- [ ] **Typography** — Headings and body text readable; fonts applied correctly
- [ ] **Images/icons** — Sharp, correctly sized, no distortion
- [ ] **Speaker notes** — Present on each slide (if applicable)
- [ ] **Animations** — Noted as lost (if expected)
- [ ] **Cross-platform test** — Opens and renders correctly in:
  - [ ] PowerPoint (Windows)
  - [ ] PowerPoint (Mac)
  - [ ] Google Slides
  - [ ] Keynote (Mac)

---

## Tools and Resources

| Task | Tool | Notes |
|---|---|---|
| **Export HTML → PDF** | Chrome/Edge browser | Built-in; free |
| | Puppeteer | Node.js automation; programmatic |
| | Decktape | Specialized for HTML decks |
| **Convert PDF → PPTX** | CloudConvert | Online; free tier |
| | LibreOffice | Open-source; command-line |
| | python-pptx + pdf2image | Python; scriptable |
| **Generate PPTX directly** | PptxGenJS | JavaScript; full control |
| | python-pptx | Python; object-level control |
| **Validate PPTX** | Office Open XML SDK | Microsoft; validation |
| | LibreOffice | Edit and re-save to validate |

---

## Best Practices

1. **Export from HTML, not from PowerPoint source** — Keeps design system in version control
2. **Automate the workflow** — Script export and conversion to catch changes early
3. **Test early and often** — Export a few slides first; verify output before full deck
4. **Document variant strategy** — README explaining light/dark/multilingual exports
5. **Keep source HTML clean** — Remove debug classes, unused styles, console logs before export
6. **Use theme colors in PPTX** — When exporting directly with PptxGenJS, map CSS tokens to PowerPoint theme colors
7. **Verify on target platforms** — Test final PPTX in PowerPoint, Google Slides, and Keynote

---

## Example: Full Automation Script

```javascript
// export-deck.js
const puppeteer = require('puppeteer');
const fs = require('fs');
const { execSync } = require('child_process');

async function exportDeck(theme = 'light', lang = 'en') {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();
  
  // Navigate to HTML and set theme
  await page.goto(`file://${process.cwd()}/deck.html`, { waitUntil: 'networkidle0' });
  await page.evaluate((t, l) => {
    document.documentElement.setAttribute('data-theme', t);
    document.documentElement.lang = l;
  }, theme, lang);
  
  // Wait for theme CSS to reflow
  await page.waitForTimeout(500);
  
  // Export to PDF
  const pdfName = `deck-${lang}-${theme}.pdf`;
  await page.pdf({
    path: pdfName,
    width: '1280px',
    height: '720px',
    printBackground: true,
    margin: { top: 0, right: 0, bottom: 0, left: 0 }
  });
  
  await browser.close();
  
  // Convert PDF to PPTX
  const pptxName = pdfName.replace('.pdf', '.pptx');
  try {
    execSync(`libreoffice --headless --convert-to pptx ${pdfName}`);
    console.log(`✓ Generated: ${pptxName}`);
    fs.unlinkSync(pdfName);  // Clean up PDF
  } catch (e) {
    console.error(`✗ Conversion failed: ${e.message}`);
  }
}

// Export all variants
(async () => {
  const themes = ['light', 'dark'];
  const langs = ['en', 'fr', 'es', 'zh-HK'];
  
  for (const lang of langs) {
    for (const theme of themes) {
      await exportDeck(theme, lang);
    }
  }
  
  console.log('✓ All exports complete');
})();
```

Run with:

```bash
npm install puppeteer
node export-deck.js
```

Output:
```
deck-en-light.pptx
deck-en-dark.pptx
deck-fr-light.pptx
deck-fr-dark.pptx
deck-es-light.pptx
deck-es-dark.pptx
deck-zh-HK-light.pptx
deck-zh-HK-dark.pptx
```
