# copilot-skills

Collection de **skills** (compétences de domaine) pour **GitHub Copilot / VS Code** — des
instructions réutilisables qu'un agent charge à la demande pour réaliser une tâche spécialisée
correctement, du premier coup.

Chaque skill vit dans `skills/<nom>/SKILL.md` : un front‑matter YAML (`name`, `description`
avec les déclencheurs) suivi des instructions détaillées. Pour l'utiliser dans un dépôt, copiez
le dossier de la skill dans `.github/skills/` de ce dépôt ; l'agent lira le `SKILL.md`
lorsque la demande correspond à sa `description`.

## Skills disponibles

### 🏛️ `architecture-diagram-author` — Auteur de diagrammes d'architecture (drawio + SVG/PNG)

Crée et maintient un **diagramme d'architecture technique** en double format, cohérent et
maintenable :

- un **`.drawio` éditable** (mxGraph XML) avec **uniquement des icônes officielles Microsoft**
  (Azure / Fabric / Entra / Microsoft 365) ;
- une **SVG auto‑contenue** (formes + texte inlinés, aucune URL externe) rendue par GitHub et
  les slides, plus le **PNG** rasterisé — tous deux **générés** depuis le `.drawio`, jamais
  édités à la main.

**À utiliser quand** on parle de « diagramme d'architecture », « schéma technique », « drawio »,
« mets à jour l'archi », « flèches qui se croisent », « icônes Azure ».

Ce que la skill garantit :

- **Icônes officielles uniquement** — chemins vérifiés + liens de téléchargement Microsoft Learn
  (Azure Architecture Icons, Fabric icons, Microsoft 365 templates & icons). Pièges documentés
  (p. ex. `office01..14` sont des glyphes bâtiment, PAS des logos ; pas de logo produit Teams —
  utiliser le logo Microsoft 365 à 4 carrés).
- **Discipline de lanes — zéro croisement** : chaque arête longue reçoit un couloir dédié
  (lane `x`/`y` unique, ≥ 16 px d'écart) ; croisements flèche/flèche interdits, même hors des
  boîtes ; contrôle mécanique (waypoints) avant rendu et visuel (PNG) après.
- **Pas de chevauchement de libellés** : un libellé d'arête ne recouvre jamais une icône (écarter
  la cible + libellé court, ou porter la nature de connexion dans la caption de l'icône).
- **Patron « Foundry platform »** : sous‑cadre projet + badge plateforme, agent‑hub en rectangle
  titré, connexions typées (modèle / Foundry IQ knowledge / tool), plusieurs agents par projet.
- **Pointes de flèches** perpendiculaires au bord de la cible ; pièges de rendu `resvg`
  documentés (`orient="auto"`).
- **Rendu sans Chrome headless** : script Node jetable qui parse le mxGraph, inline les icônes
  officielles téléchargées, et produit SVG + PNG via `@resvg/resvg-js`.

📄 Détail complet et checklist : [`skills/architecture-diagram-author/SKILL.md`](skills/architecture-diagram-author/SKILL.md).

## Structure

```
skills/
  architecture-diagram-author/
    SKILL.md
```

## Contribuer

Ajoutez une skill par dossier sous `skills/`. Gardez la `description` du front‑matter précise
et riche en déclencheurs (c'est elle qui décide du chargement de la skill par l'agent), et les
instructions concises et actionnables.

## Licence

[MIT](LICENSE).
