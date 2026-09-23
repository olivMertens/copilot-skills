# copilot-skills

Reusable, project-agnostic domain skills for **GitHub Copilot in VS Code**.
Each skill contains YAML frontmatter with discovery triggers, followed by
instructions and an exit checklist. All skills are written in English.

## Installation

Copy the desired directory from `skills/` into your repository's `.github/skills/`.
Copilot loads its `SKILL.md` when a request matches the description. Review the
instructions against your repository's policies before use.

To update an installed skill, import the latest upstream version, reconcile local
changes, and validate the resulting instructions and checklist. Do not overwrite
local customizations blindly.

## Available Skills

### Architecture Diagram Author

[`architecture-diagram-author`](skills/architecture-diagram-author/SKILL.md)
creates and maintains editable **drawio** diagrams with generated, self-contained
**SVG and PNG** outputs.

- Searches across five approved icon sources:
  [MsiconsCollections](https://aka.ms/MsiconsCollections),
  [Microsoft Learn Azure icons](https://learn.microsoft.com/en-us/azure/architecture/icons/),
  [az-icons.com](https://az-icons.com/),
  [Microsoft Learn Power Platform icons](https://learn.microsoft.com/en-us/power-platform/guidance/icons),
  and [DamoBird365/microsoft-cloud-icons](https://github.com/DamoBird365/microsoft-cloud-icons/tree/master).
  Prefers current Microsoft-published assets, verifies provenance and product identity,
  and distinguishes community catalogs from Microsoft publishers. Discovery is not
  limited to a single pack.
- Discovers the current project's paths, labels, identifiers, and renderer instead
  of inheriting assumptions from another repository.
- Requires logical columns, square-cornered shapes, dedicated orthogonal routing
  lanes, readable labels, and checks for crossings and overlaps.
- Uses drawio as the source of truth; requires geometry checks and visual inspection
  of the generated PNG. Reuses the existing export workflow where possible.
- Applies Foundry guidance only to implemented Foundry resources. Synchronizes
  existing documentation and presentation consumers without requiring a deck.

Use for architecture diagrams, technical schematics, drawio updates, routing fixes,
and Microsoft/Azure icon selection.

### Container Engine Compatibility

[`container-engine-compat`](skills/container-engine-compat/SKILL.md)
adapts local workflows **from Docker to Podman and from Podman to Docker**, with
explicit compatibility limits rather than assuming identical behavior.

- Checks engine reachability, versions, connections, platforms, and user preference.
- Preserves build context and checks ignore rules, BuildKit/Buildah features,
  cache syntax, image outputs, multi-platform manifests, and secrets.
- Covers Compose providers, API clients, rootless permissions, SELinux, networking,
  healthchecks, and conditional GPU support.
- Grounds performance recommendations in live official Docker **and** Podman
  documentation; requires measurements and discloses untested engines.
- Includes conditional Azure ACR/azd remote-build guidance, infrastructure quality
  gates, and what-if/saved-plan review. Successful checks do not authorize deployment.
- Preserves existing CI and remote release workflows unless changes are requested.

Use for engine migration, local container failures, build-context differences,
cache optimization, or Azure remote-build compatibility.

### App Demo & Marketing Video

[`app-demo-video`](skills/app-demo-video/SKILL.md) turns a live web app into a
narrated **demo** or **marketing** video with the **Remotion** (React) pipeline —
from just a URL and a short brief.

- Explores and screenshots the running app, writes why/how/value narration,
  generates **Azure TTS** voice-over, and renders with `npx remotion render`.
- Ships runnable assets: parameterized scene components, a transition wrapper, an
  assembly template, and a project-agnostic Azure TTS script (`generate_tts.py`).
- References cover the house visual language and the screenshot capture/crop
  workflow. Discovers the app's real content instead of inventing it.

Use for demo videos, product videos, marketing videos, or feature-tour videos.

### Exec Demo Video

[`exec-demo-video`](skills/exec-demo-video/SKILL.md) produces a short, punchy
**executive** demo/marketing video (~30–60s). A thin exec-focused wrapper that
runs an upfront questionnaire (voice/language, audience, tone, duration, persona)
and reuses the `app-demo-video` pipeline and assets.

- Bundles a questionnaire checklist and a multilingual Azure voice catalog
  (one multilingual voice can cover a FR+EN pair via SSML `<lang>`).

Use for exec/leadership demo videos and short product teasers.

## Structure

```text
skills/
  app-demo-video/
    SKILL.md
    assets/        # Remotion scene components + turnkey project skeleton (Root/index/package.json)
    scripts/       # generate_tts.py (+ clips.example.json)
    references/    # visual-language.md, screenshot-capture.md
  architecture-diagram-author/
    SKILL.md
  build-presentation/
    SKILL.md
  container-engine-compat/
    SKILL.md
  exec-demo-video/
    SKILL.md
    references/    # questionnaire.md, voice-catalog.md
  reveal-slide-builder/
    SKILL.md
```

## Contributing

Use one directory per skill. Keep frontmatter descriptions precise and rich in
triggers, and instructions concise and actionable. Discover project-specific
configuration rather than embedding private paths, labels, resource IDs, or names.

Check technical claims against authoritative documentation, align the exit checklist
with the instructions, and state validation gaps. Documentation review alone does
not prove that a diagram renders, a container builds, or infrastructure deploys.

## License

[MIT](LICENSE).
