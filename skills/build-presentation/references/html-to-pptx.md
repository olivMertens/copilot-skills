# HTML to Editable PowerPoint

## Purpose and boundaries

Rebuild an approved HTML deck as native PowerPoint text, shapes and connectors.
A screenshot on each slide is not an editable conversion.

This guide is a generic implementation recipe, not a runnable exporter.
Choose an available `pptx` skill, native PowerPoint canvas, PptxGenJS,
python-pptx or equivalent, and follow that tool's current documentation.
Do not assume that a particular library supports animations or font embedding.
Report missing capabilities explicitly.

The approved HTML and its resolved identity own the visual style. Generic tool
suggestions must not replace its semantic palette, typography or icon policy.
White/blue/amber is only the packaged neutral fallback, not a mandatory identity.

## 1. Establish the approved input

- Confirm the HTML file, approved revision, slide count and output directory.
- Preserve the approved content, reading order, source links and speaker notes.
- Use only authorized local assets. Do not upload source decks or captures to
  online converters without explicit approval for that data destination.
- Separate HTML controls and browser chrome from actual slide content.
- List exact fonts, required native objects and animations before rebuilding.

## 2. Measure the HTML

Use a browser inspector or Playwright when available.

1. Render the slide at its unscaled reference size: **1280 x 720 CSS pixels**.
   A 1280 x 720 browser viewport is not sufficient if the viewer shrinks the
   stage to leave room for controls.
2. If reproducing entrances, read their timing **before** disabling animations.
   Include ancestor delays and distinguish entrance effects from continuous
   pulses. Inspect the actual animation model; a simple delay formula does
   not generalize to nested, repeated or differently filled animations.
3. Freeze the final visible state: remove animations and transitions, restore
   final opacity and transforms, and exclude viewer controls.
4. Measure each element relative to the slide, at the unscaled stage size.
   Collect bounds, z-order, fill, stroke, radius, text runs, alignment, padding
   and link targets. Resolve responsive units at this fixed size.
5. Keep icons as small vector or high-resolution assets where needed; retain
   larger diagrams as shapes. Record authorized asset sources and licenses.
6. Save a final-state reference render for comparison.

Never read entrance timing only after `animation: none`; that loses the
information needed for mapping the builds.

## 3. Rebuild native slides

Use a wide slide: **13.333333 x 7.5 inches**.

| Quantity | Conversion |
|---|---|
| Position or size in inches | CSS pixels / 96 |
| Position or size in EMU | CSS pixels x 9525 |
| Font size in points | CSS pixels x 0.75 |
| 1cqw at 1280 pixels | 12.8 pixels |

Convert exactly once. A canvas may already report points or inches.

- Text becomes editable text boxes/runs with deliberate margins and wrapping.
- Cards become rounded rectangles; pills are not stretched ovals.
- Connectors attach to the intended native objects and avoid labels.
- Tables and charts remain editable where the chosen tool supports them.
- Preserve component geometry between concept and implementation slides.
- Keep diagram grouping logical and preserve accessibility/reading order.
- Put speaker notes and sources in the appropriate native fields.
- Check theme defaults: unintended shadows, line styles and text padding can
  alter the result even when all measured coordinates are correct.

Verify the actual font family and weight exposed by the installed font files.
Some distributions use separate family names for weights; others do not.
Avoid synthetic bold on an already-bold face.

## 4. Decide animation and font fidelity explicitly

Prefer simple native Appear/Fade builds when supported. CSS springs, border
draws, pseudo-elements and continuous effects may require approximation.

If the selected tool cannot author animations, deliver a readable static
version only with a clear limitation, not a claim of animation parity.
Test requested animations in a capable presentation viewer; a static thumbnail
cannot prove playback behavior.

Fonts are not bundled. Use installed licensed fonts or obtain approval for
specific substitutes. Embed fonts only when the license, font format and
chosen tooling permit it. Never promise identical rendering on another machine
solely because the font name is recorded.

Avoid manual OOXML editing unless necessary and supported by a validator.
If timing or font relationships are changed directly, validate the package
after each change and open it in PowerPoint to check for repair prompts.

## 5. Validate and deliver

Check every slide, not just the title page:

1. Render the PPTX and compare it with the approved HTML reference.
2. Inspect clipping, overlap, line wrapping, font substitution, connector
   endpoints, image quality, contrast and reading order.
3. Extract text and compare slide order, headings, numerical claims, links
   and notes with the approved content.
4. Confirm editability: inspect native text/shape objects and edit a
   representative text box and diagram component in a capable editor.
5. Check animations in slideshow mode when requested. Validate OOXML after
   any direct package modification.
6. Correct defects and re-render affected slides.

Deliver the actual `.pptx` and preserve the approved HTML. State any font,
animation or validation limitation. If no rendering/viewer tool is available,
report visual verification as blocked, not passed.

This procedure does not authorize sending, uploading or publishing either file.
