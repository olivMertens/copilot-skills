---
name: exec-demo-video
description: >-
  Produce a short, punchy executive demo/marketing video (~30–60s) of a web app
  from a GitHub repo and a live demo link, using the Remotion + Azure TTS
  pipeline. Runs a quick upfront questionnaire (voice/language, audience, tone,
  duration, persona), then writes the script, captures/zooms the real screens,
  generates the voice-over and assembles the final video. A thin exec-focused
  wrapper over the app-demo-video skill (load it for the full mechanical
  pipeline). Use for "exec demo video", "executive video", "leadership demo",
  "punchy marketing video", "short product teaser", "vidéo exec", "vidéo de démo
  exec", "vidéo marketing courte". Not for long feature-by-feature walkthroughs.
---

# Skill: exec-demo-video

Produce a short, high-impact "exec presentation" video (~30–60s) from (a) a live
web-app demo link and/or (b) a GitHub repo. This skill adds a **targeted upfront
questionnaire** and exec-cut conventions on top of the general pipeline.

## Step 0 — Always load `app-demo-video` too
This skill does not replace `app-demo-video` — it layers exec framing on top.
Load `app-demo-video` for the full mechanical detail (bundled Remotion scene
components, transition wrapper, assembly template, the `generate_tts.py` script,
and the visual-language / screenshot-capture references). Everything below reuses
those bundled assets.

## Step 1 — Questionnaire (ALWAYS ask before generating anything)
Work through `references/questionnaire.md` (voice/language, audience, tone,
duration, demo link, repo, optional persona, priority "wow" element). Never
assume language/voice defaults — ask first. Recommend the **standard exec
profile** (6–7 scenes / 45–55s, dynamic corporate) when the user is unsure.
Pick a voice from `references/voice-catalog.md` and offer short samples first.

## Step 2 — Script
- One scene = one new idea. Never repeat the same information on screen and in
  narration — no narration sentence should paraphrase text visible in the shot
  at that moment.
- Each sentence transitions logically to the next scene.
- Use native vocabulary, never word-for-word translations.
- If a persona is used, keep the on-screen data consistent with it (don't invent
  a persona that contradicts the repo/demo content).

## Step 3 — Captures & animation
- Prefer real screenshots of the app (via browser automation or user-provided)
  over recreations. Use real documents (PDF, report) as-is: full page first, then
  zoom to the relevant area.
- Zooms: `transform-origin` centered on the point of interest (never a pan that
  pushes content out of frame). Fast ramp to a moderate zoom, then a slow drift
  for a "living video" feel. Vary the animation pattern between scenes.
- If real screen-recordings exist in a different language than the narration,
  flag it and let the user choose (translated stills vs. real video in the other
  language's UI).
- Add "processing" elements (progress bars, spinners) synced with narration when
  the product has real compute time to stage, instead of cutting to the result.

## Step 4 — Music & sound
Keep background music quiet (base ~0.05–0.06, small flares at scene transitions)
so it never covers the voice. The bundled `MainVideo.template.jsx` already
implements this ducked envelope.

## Step 5 — Iterative validation
- Render stills (`npx remotion still`) at key frames BEFORE the full render, for
  fast user review.
- Only run the full render (`npx remotion render`) after stills are approved or
  on explicit request.
- Verify real clip durations (`ffprobe`) and set each scene's `durationInFrames`
  = `ceil(seconds*fps) + ~20` accordingly.

## Guardrails
- Never use a real third-party/competitor brand as a persona without explicit
  user confirmation.
- Never show real confidential data — if the demo already uses fictitious/demo
  data, note it in the video's legal mention if the source product does.
- If the Azure/AAD token for TTS expires (401/tenant mismatch), regenerate it and
  `.strip()`/`.Trim()` the token before using it in an Authorization header (a
  trailing newline silently breaks the request). See `generate_tts.py` header.

## Expected output
An MP4 in the Remotion project folder (e.g. `out_<name>.mp4`), plus a short
summary of the choices made (voice, duration, scenes) so the user can iterate
scene by scene. Produce one language/variant end-to-end before starting another.
