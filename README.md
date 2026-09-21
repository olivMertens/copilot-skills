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

- All icons must originate exclusively from
  [Microsoft's icon collection](https://aka.ms/MsiconsCollections), with verified
  provenance. No alternate icon sources are allowed.
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

## Structure

```text
skills/
  architecture-diagram-author/
    SKILL.md
  container-engine-compat/
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
