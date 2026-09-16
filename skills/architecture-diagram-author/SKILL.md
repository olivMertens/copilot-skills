---
name: architecture-diagram-author
description: >
  Crée et maintient un diagramme d'architecture technique double format :
  (1) un .drawio ÉDITABLE avec les icônes officielles Microsoft, et
  (2) une SVG AUTO-CONTENUE (+ PNG) pour README et slides.
  Garantit des colonnes logiques, des couloirs de routage dédiés (aucune
  flèche ne traverse une boîte non concernée), des rectangles colorés à
  angles droits, et la synchro README ↔ deck. Icônes EXCLUSIVEMENT depuis
  la collection officielle https://aka.ms/MsiconsCollections.
  UTILISER QUAND : "diagramme d'architecture", "schéma technique",
  "drawio", "mets à jour l'archi", "flèches qui se croisent", "icônes Azure".
---

# Skill — Auteur de diagrammes d'architecture (drawio + SVG/PNG)

## Objectif
Produire un schéma d'architecture *lisible* et *maintenable*, livré en deux artefacts
qui restent cohérents :
- `docs/architecture.drawio` — source **éditable** (mxGraph XML), icônes officielles MS.
- `docs/images/architecture.svg` — **auto-contenue** (formes + texte uniquement, AUCUNE
  référence externe) → rendable par GitHub et par Remotion/slides.
- `docs/images/architecture.png` — rasterisée depuis la SVG.

## Règles de mise en page (NON négociables)
1. **Conteneurs = domaines** (ex. Fabric, Foundry, Azure, M365, Client), rectangles
   pointillés colorés, coins **droits** (`rounded=0` / `rx=0`).
2. **Colonnes logiques dans chaque conteneur** : ordonne les sous-boîtes selon le flux
   (ex. front → backend → pipeline ; data-agent → ontologie → lakehouse → SQL → BI).
   Place la boîte qui parle à un AUTRE conteneur sur le **bord le plus proche** de sa cible.
3. **Rectangles à fond coloré par composant** (une teinte cohérente par domaine, variée
   par nœud), texte lisible en clair ET sombre.
4. **Aucune flèche ne survole une boîte non concernée.** On y arrive avec des **couloirs**.

## Recette de routage (le cœur du skill)
Ne PAS se fier à l'auto-routage : dans mxGraph, `orthogonalEdgeStyle` évite seulement
source/cible, pas les autres boîtes. Il faut **épingler** chaque arête longue avec des
points d'ancrage + waypoints :
- Style d'arête : `exitX/exitY/exitDx/exitDy` (côté sortie), `entryX/entryY/...` (côté entrée),
  et un `<Array as="points"><mxPoint x=".." y=".."/></Array>` dans la `mxGeometry`.
- **Couloir haut** (au-dessus de tous les conteneurs) pour les longs sauts horizontaux.
- **Couloirs bas** (sous tous les conteneurs), un par arête, en parallèle.
- **Gouttière inter-conteneurs élargie** (≥ 80 px) découpée en **sous-couloirs** (ex. x=1132,
  1148, 1164, 1178) : une seule verticale longue par sous-couloir → jamais de chevauchement.
- **Couloirs internes inter-colonnes** (petites gouttières x/y libres entre deux colonnes)
  pour les arêtes qui « sautent » une boîte dans la même colonne.
- **ZÉRO croisement, y compris flèche/flèche et hors des boîtes.** Les croisements flèche/boîte
  sont interdits **et** les croisements flèche/flèche doivent être éliminés par le lane-routing
  ci-dessous (ne pas se contenter de « tolérer » un X).
- Décale les points d'entrée/sortie qui partagent un même bord (ex. `entryY=0.42` vs `0.52`).

### Discipline de lanes — zéro croisement, zéro chevauchement (OBLIGATOIRE)
Le routage propre n'est PAS une question de chance : chaque arête longue reçoit un **couloir
dédié** et on tient un **registre de lanes** mental (ou en commentaire) pour ne jamais réutiliser
un couloir occupé.
1. **Une lane = une arête.** Deux arêtes ne partagent JAMAIS le même `x` vertical ni le même `y`
   horizontal (même partiellement, même hors des boîtes) sauf bus explicitement commun. Espacer
   les lanes parallèles d'au moins **16 px** (`x=660` / `x=680` / `x=700` ; `y=656` / `y=675`).
2. **Route la plus courte et alignée.** Si source et cible sont alignables, sortir par le bord le
   **plus proche de la cible** et aller TOUT DROIT plutôt que de contourner par une gouttière déjà
   prise (ex. apigw→NAT : sortie **bas** + une seule verticale entre deux icônes libres, au lieu
   d'un détour par la gouttière droite déjà utilisée par egress/API tierces → supprime le X vécu).
3. **Côtés opposés = lanes distinctes.** Deux arêtes qui partent de la même zone vers des côtés
   opposés (une à gauche, une à droite) prennent des **lanes Y différentes ET des approches X
   différentes**, pour que leurs coudes ne coïncident pas.
4. **Contrôle mécanique AVANT rendu.** Lister tous les waypoints ; si deux arêtes ont un `x`
   (resp. `y`) identique à < 16 px près sur des plages qui se recouvrent, **re-laner** l'une des deux.
5. **Contrôle visuel APRÈS rendu.** Scanner le PNG : tout **X** (croisement) ou **double-trait
   parallèle rapproché** = échec → déplacer une des deux arêtes dans une lane libre et re-rendre.
   Ne jamais livrer un PNG avec un croisement, fût-il hors des boîtes.

En SVG, applique la même logique avec des `path` à segments orthogonaux (`M.. L.. L..`) et
des `marker` de flèche colorés ; regroupe des blocs déplaçables via `<g transform="translate()">`.

## Icônes officielles Microsoft — SOURCE OBLIGATOIRE : https://aka.ms/MsiconsCollections
**Toutes** les icônes du `.drawio` DOIVENT provenir de la collection officielle
**https://aka.ms/MsiconsCollections** (Azure / Fabric / Entra / Microsoft 365 /
Power Platform). N'utilise AUCUNE autre source d'icônes (pas de FontAwesome, pas
d'icônes génériques mxgraph, pas d'images tierces).

- La collection est hébergée (GitHub Pages) sous la base
  `https://tomkiljo.github.io/ms-icons/icons/` — c'est la cible de `aka.ms/MsiconsCollections`.
  Utilise cette base pour les URLs `image=` du drawio.
- **Sources officielles Microsoft Learn** (téléchargement des jeux d'icônes + templates ; à citer
  et utiliser pour récupérer/mettre à jour les SVG) :
  - **Azure** — https://learn.microsoft.com/en-us/azure/architecture/icons/
    (familles `Azure_Public_Service_Icons/…`, `Azure_UX_Patterns_icons/…`).
  - **Microsoft Fabric** — https://learn.microsoft.com/en-us/fabric/fundamentals/icons
    (famille `Microsoft_Fabric_icons/…`).
  - **Microsoft 365** (templates + icônes) — https://learn.microsoft.com/en-us/previous-versions/microsoft-365/solutions/architecture-icons-templates
    (famille `Microsoft_365_Content_Icons/…`).
  - **Microsoft Entra** — inclus dans le pack Azure ci-dessus (famille `Microsoft_Entra_architecture_icons/…`).
- drawio : `shape=image;image=<URL>` ou nœud `label` avec
  `image=<URL>;imageAlign=left;imageVerticalAlign=middle`.
- Familles : `Azure_Public_Service_Icons/Icons/<cat>/...`,
  `Microsoft_Fabric_icons/general/...`, `Microsoft_Entra_architecture_icons/...`,
  `Microsoft_365_Content_Icons/...`, `Azure_UX_Patterns_icons/...`.

### Chemins vérifiés (à réutiliser tels quels)
```
# base = https://tomkiljo.github.io/ms-icons/icons/   (= aka.ms/MsiconsCollections)
Function Apps        Azure_Public_Service_Icons/Icons/compute/10029-icon-service-Function-Apps.svg
Storage Accounts     Azure_Public_Service_Icons/Icons/storage/10086-icon-service-Storage-Accounts.svg
Managed Identities   Azure_Public_Service_Icons/Icons/identity/10227-icon-service-Managed-Identities.svg
Application Insights  Azure_Public_Service_Icons/Icons/management%20+%20governance/00012-icon-service-Application-Insights.svg
AI Studio / Foundry  Azure_Public_Service_Icons/Icons/ai%20+%20machine%20learning/03513-icon-service-AI-Studio.svg
Azure OpenAI         Azure_Public_Service_Icons/Icons/ai%20+%20machine%20learning/03438-icon-service-Azure-OpenAI.svg
Cognitive Search     Azure_Public_Service_Icons/Icons/ai%20+%20machine%20learning/10044-icon-service-Cognitive-Search.svg
Cognitive Services   Azure_Public_Service_Icons/Icons/ai%20+%20machine%20learning/10162-icon-service-Cognitive-Services.svg
Entra ID (couleur)   Microsoft_Entra_architecture_icons/Microsoft%20Entra%20color%20icons%20SVG/Microsoft%20Entra%20ID%20color%20icon.svg
Graph (logo MS)      Azure_UX_Patterns_icons/microsoft.svg
Microsoft 365 (logo) Azure_UX_Patterns_icons/microsoft-square.svg
M365 Copilot (logo)  Azure_UX_Patterns_icons/copilot.svg
Fabric (général)     Microsoft_Fabric_icons/general/<lakehouse_64_item|sql_database_64_item|data_warehouse_64_item|event_house_64_item|power_bi_32_color|notebook_64_item|one_lake_48_color|function_64_item|copilot_48_color|app_development_48_color|fabric_48_color|data_factory_48_color>.svg
Teams Bot            Microsoft_365_Content_Icons/Teams%20Purple/48x48%20Dark%20Purple%20Icon/Bot.svg
Teams Chat (client)  Microsoft_365_Content_Icons/Teams%20Purple/48x48%20Dark%20Purple%20Icon/Chat.svg
Person (analyste)    Microsoft_365_Content_Icons/Microsoft%20Blue/48x48%20Dark%20Blue%20Icon/Person.svg
Container Registry   Azure_Public_Service_Icons/Icons/containers/10105-icon-service-Container-Registries.svg
Container Apps Env   Azure_Public_Service_Icons/Icons/other/02989-icon-service-Container-Apps-Environments.svg
Worker Container App Azure_Public_Service_Icons/Icons/other/02884-icon-service-Worker-Container-App.svg
Service Bus          Azure_Public_Service_Icons/Icons/integration/10836-icon-service-Azure-Service-Bus.svg
Cosmos DB            Azure_Public_Service_Icons/Icons/databases/10121-icon-service-Azure-Cosmos-DB.svg
PostgreSQL Flexible  Azure_Public_Service_Icons/Icons/databases/10131-icon-service-Azure-Database-PostgreSQL-Server.svg
Azure Maps           Azure_Public_Service_Icons/Icons/iot/10185-icon-service-Azure-Maps-Accounts.svg
Log Analytics        Azure_Public_Service_Icons/Icons/analytics/00009-icon-service-Log-Analytics-Workspaces.svg
Virtual Networks     Azure_Public_Service_Icons/Icons/networking/10061-icon-service-Virtual-Networks.svg
Subnet               Azure_Public_Service_Icons/Icons/networking/02742-icon-service-Subnet.svg
NAT Gateway          Azure_Public_Service_Icons/Icons/networking/10310-icon-service-NAT.svg
Public IP            Azure_Public_Service_Icons/Icons/networking/10069-icon-service-Public-IP-Addresses.svg
Private Endpoints    Azure_Public_Service_Icons/Icons/other/02579-icon-service-Private-Endpoints.svg
DNS Zones (privé)    Azure_Public_Service_Icons/Icons/networking/10064-icon-service-DNS-Zones.svg
Browser (SPA/client) Azure_Public_Service_Icons/Icons/general/10783-icon-service-Browser.svg
Globe (API externe)  Azure_Public_Service_Icons/Icons/general/10808-icon-service-Globe-Success.svg
Keys (secret/clé)    Azure_Public_Service_Icons/Icons/menu/00787-icon-service-Keys.svg
Bot Service classique Azure_Public_Service_Icons/Icons/ai%20+%20machine%20learning/10165-icon-service-Bot-Services.svg
Foundry projet+agents Azure_Public_Service_Icons/Icons/ai%20+%20machine%20learning/03513-icon-service-AI-Studio.svg
Azure OpenAI (modèle) Azure_Public_Service_Icons/Icons/ai%20+%20machine%20learning/03438-icon-service-Azure-OpenAI.svg
AI Search (Foundry IQ) Azure_Public_Service_Icons/Icons/ai%20+%20machine%20learning/10044-icon-service-Cognitive-Search.svg
Copilot (orchestr.)  Microsoft_Fabric_icons/general/copilot_48_color.svg
Policy (contrôle)    Azure_Public_Service_Icons/Icons/management%20+%20governance/10316-icon-service-Policy.svg
```
Note : les espaces et le `+` dans les chemins doivent rester **URL-encodés** (`%20`, `%20+%20`).

- **Important** : la SVG du README NE DOIT PAS **pointer** ces URLs (sinon GitHub ne rend rien) —
  **inline** le contenu `<svg>` de chaque icône (nœud `<svg>` imbriqué avec `viewBox`), jamais un
  `image href=`. La SVG livrée ne doit contenir aucune URL externe.

## Représenter Foundry (Agent Service, Foundry IQ, tools & connexions)
Un agent Foundry n'est PAS une icône isolée à côté d'autres services : c'est un **projet
Foundry** qui **possède** des connexions (modèle, knowledge, tools). Le diagramme doit rendre
cette intégration **visible et connectée**, sinon on obtient des icônes éparses aux libellés
qui se chevauchent (bug vécu : label 2 lignes de l'agent recouvrant celui d'AI Search, arête
courte portant encore un libellé au milieu).

### Patron « Foundry platform » (agent = hub)
1. **Sous-cadre dédié** « Foundry (projet) » à l'INTÉRIEUR du domaine AI : rectangle pointillé
   coins droits, bord violet Foundry `#6D28D9`, fond `#F5F3FF` (variante plus claire que le
   `#EDE9FE` du domaine parent → se distingue sans casser l'harmonie). Il englobe l'agent **et**
   les ressources qu'il connecte, pour montrer qu'elles sont *intégrées au projet*, pas isolées.
2. **Agent Service au centre = hub** ; les ressources connectées **rayonnent autour** (une par
   bord libre). Ne jamais empiler l'agent et une cible sur la même rangée serrée.
3. **3 natures de connexion, visuellement distinctes** (toutes en violet `#6D28D9`) :
   - **connexion modèle** (`model deployment`) → Azure OpenAI : trait **plein**, libellé
     « connexion modèle · <deployment> ».
   - **Foundry IQ · knowledge** (`knowledge source`, RAG) → Azure AI Search : trait **plein**,
     libellé « Foundry IQ · knowledge (RAG) ».
   - **tool** (`agent tool` OpenAPI / MCP / function) → cible réelle (ex. API Gateway) : trait
     **tireté**, libellé « tool <type> · <cible> ».
4. **Icône Foundry = `03513-icon-service-AI-Studio.svg` (AI Foundry) pour la plateforme ET les
   agents.** La collection officielle n'a **AUCUNE** icône « Agent Service » dédiée ; le Foundry
   Agent Service se représente avec l'icône AI Foundry. **Ne PAS utiliser `10165-icon-service-Bot-Services`**
   (c'est l'Azure Bot Service *classique*, une autre ressource — même pour un agent publié dans Teams).
   **Garder le badge plateforme visible** : une petite icône AI Foundry (≈18px) dans l'en-tête du
   sous-cadre, à gauche du titre (`spacingLeft≈28` pour libérer la place).
5. **Plusieurs agents par projet** : un projet Foundry héberge souvent >1 agent (ex. FDR :
   `prevaiq-souscription-expert` pour la narration + `prevaiq-report-teams` publié Teams). Les
   représenter **tous**, chacun tagué de l'icône AI Foundry, **placé près de ses propres
   dépendances** (l'agent Teams près de la surface *M365 / Teams (publish)*, l'agent narration près
   de son modèle + Foundry IQ). Indiquer le **compte d'agents** sur le cadre projet (« … · 2 agents »).
   **Surface Microsoft 365 / Teams** : le pack officiel n'a **AUCUN** logo produit Teams ni M365 ;
   utiliser le logo **Microsoft 365 = `microsoft-square.svg`** (les 4 carrés) en badge d'en-tête du
   conteneur publish (ou `copilot.svg` = logo M365 Copilot). **Piège** : `Azure_UX_Patterns_icons/office01..14`
   ne sont PAS des logos Office/M365 mais des glyphes bâtiment/usine — ne pas les utiliser.
6. **Agent = rectangle titré, PAS une icône + caption**, dès que le nom est long (ex.
   `« prevaiq-souscription-expert »`). Un `rounded=0` rempli (`fillColor=#DDD6FE`,
   `strokeColor=#6D28D9`) avec le nom À L'INTÉRIEUR (align=left, spacingLeft≈30) et une petite
   icône AI Foundry (`03513`) 24×24 dans le coin (nœud image séparé déclaré APRÈS le rect pour
   passer au-dessus). Le nom ne déborde plus sous une icône voisine, et les arêtes s'ancrent proprement.
7. **Titre du sous-cadre court** (`Foundry (projet)`) : un titre long (aligné à gauche) court
   sous l'icône du coin haut-droit du cadre et la percute — vérifié sur le PNG.

### Anti-chevauchement des libellés d'icônes voisines (règle issue du bug)
Deux icônes 48px espacées de ~80px, chacune avec un **label sous l'icône sur 2 lignes**, se
recouvrent ; une arête courte entre elles avec un libellé au milieu aggrave tout.
- Entraxe **≥ 150 px** entre deux icônes d'une même rangée dès que les labels font 2 lignes ;
  sinon raccourcir les labels ou empiler les icônes verticalement.
- **Un libellé d'arête/connexion ne recouvre JAMAIS une icône** (ni ne se pose au-dessus/dessous
  d'une icône). Si la gouttière hub↔cible est trop étroite pour le libellé (bug vécu : « connexion
  modèle · gpt » posé sur l'icône OpenAI), DEUX stratégies propres :
  1. **Écarter la cible** pour ouvrir une gouttière ≥ 40 px et y centrer un **libellé court**
     (« modèle », « knowledge ») SANS offset — il tient entre les deux boîtes ;
  2. **Supprimer le libellé d'arête** (`value=""`) et porter la nature de connexion dans la
     **caption de l'icône cible** (ex. « Azure AI Search / Foundry IQ · knowledge ») — la caption
     est sous l'icône, jamais sur une autre boîte.
  Ne jamais « caser » un libellé long dans une gouttière de 20 px avec un offset qui le pousse
  sur l'icône voisine.
- Le renderer local (`logs/svg2png/drawio2svg.mjs`) **centre TOUJOURS le label d'une icône sous
  celle-ci** et **ignore `verticalLabelPosition`/`align`** pour `shape=image`. L'anti-chevauchement
  passe donc par l'**espacement** + labels courts + **offsets d'arête**, jamais par `verticalLabelPosition`.
- **Couloirs de routage = zones interdites** : des arêtes longues (ex. `e_nat`/`e_ext`) tracent une
  gouttière horizontale (ici y≈656) sous les conteneurs. Toute boîte/label d'un conteneur doit
  **finir au-dessus** de ce couloir (le domaine AI a été réduit pour rester ≥ 6 px au-dessus).
- Vérifier sur le PNG rendu : aucun libellé d'icône ne touche un libellé voisin, aucun libellé
  d'arête ne se pose sur un libellé d'icône, aucun label ne croise un couloir d'arête.


## Rendu & validation (environnement sans headless Chrome)
Le `.drawio` est la **seule source** : SVG et PNG en sont **générés**, jamais écrits à la main.
- Pas de CLI draw.io ici → script Node jetable dans `logs/svg2png/` (`drawio2svg.mjs`) qui parse
  le mxGraph XML et émet la SVG, puis `@resvg/resvg-js` pour le PNG
  (`new Resvg(svg,{fitTo:{mode:"width",value:1920}}).render().asPng()`).
- Les icônes officielles sont **téléchargées et inlinées** en `<svg>` imbriqué (ids préfixés par
  instance, sinon les dégradés se volent entre copies) → SVG 100 % auto-contenue, rendue par GitHub.
- draw.io réécrit le fichier à l'ouverture : il **supprime les attributs valant 0**
  (`<mxPoint x="0" y="-24">` devient `<mxPoint y="-24">`). Toujours lire avec un défaut à 0.
- Le routage orthogonal doit être **recalculé** : draw.io ne stocke que les points de passage, pas
  les coudes. Sans ça les flèches sortent en diagonale.
- Valider le XML avant commit (PowerShell) : `[xml](Get-Content <fichier> -Raw)`.

## Quand mettre le diagramme à jour (DÉCLENCHEURS OBLIGATOIRES)
Ne pas attendre qu'on te le demande : dès qu'une de ces conditions est vraie, la tâche
inclut la mise à jour du `.drawio` **et** la régénération des images **et** le re-rendu du deck.
- **Terraform** (`infra/*.tf`) : ressource ajoutée/supprimée/renommée, changement de réseau
  (subnet, NAT, Private Endpoint, DNS privé), de RBAC / identité, de SKU ou d'ingress.
- **Structure agentique** : agent `.github/agents/*.agent.md` ajouté/supprimé/renommé,
  garde-fou modifié, chaîne orchestrateur → contrôles → déploiement modifiée.
- **Code** : nouveau service Azure ou nouvelle API externe consommée par `Infrastructure`.
Un `.tf` ou un `.agent.md` modifié sans diagramme régénéré = livraison incomplète.

## Pointes de flèches (vérifier sur le PNG rendu)
- La pointe doit **arriver perpendiculairement sur le bord de la boîte cible** et la toucher :
  segment final vertical → pointe vers le haut/bas ; segment final horizontal → pointe
  vers la gauche/droite. Jamais une pointe couchée sur le côté, ni flottant à côté de la cible.
- Le **dernier segment ne doit jamais être quasi nul** : si le point de passage final est à
  quelques pixels du bord, l'orientation devient aléatoire — laisser ≥ 15 px d'approche.
- Ancrer explicitement `entryX/entryY` sur le bord visé (`0/1` = côté, `0.5` = milieu) plutôt
  que de laisser draw.io choisir.
- **Piège de rendu vécu** : `orient="auto-start-reverse"` est ignoré par resvg → **toutes** les
  pointes sortent tournées vers la droite (elles semblent « posées sur le côté » de la boîte).
  Utiliser `orient="auto"` sur le `<marker>`. Vérifier au moins une flèche montante, une
  descendante et une allant vers la gauche sur le PNG avant de conclure.

## Synchro des assets démo (OBLIGATOIRE)
Si un écran/feature change : régénère le(s) PNG, copie-les dans le deck
(`video/.../public/`), mets à jour l'ordre/les légendes du deck (`slides.ts`) ET la section
README correspondante. Ne jamais livrer un README ou un deck avec des captures périmées.

## Checklist de vérification
- [ ] **Toutes** les icônes proviennent de https://aka.ms/MsiconsCollections (aucune autre source).
- [ ] Foundry rendu comme un **projet-hub** : sous-cadre « Foundry (projet) », agent au centre,
      et les 3 connexions typées (modèle / Foundry IQ knowledge / tool) tracées et étiquetées.
- [ ] Aucun libellé d'icône ne chevauche un libellé voisin ; aucun libellé d'arête ne se pose
      sur un libellé d'icône (entraxe ≥ 150 px sur labels 2 lignes ; libellé de connexion au-dessus de l'arête).
- [ ] `.drawio` XML valide ; SVG XML valide.
- [ ] Aucune flèche ne traverse une boîte non concernée (vérifier le PNG rendu).
- [ ] **ZÉRO croisement flèche/flèche et zéro double-trait parallèle rapproché**, même hors des
      boîtes : chaque arête longue dans sa propre lane (x/y uniques, ≥ 16 px d'écart) — vérifié sur le PNG.
- [ ] Chaque pointe de flèche arrive **perpendiculairement sur le bord** de sa cible et la touche
      (contrôler une flèche montante, une descendante et une vers la gauche).
- [ ] Colonnes ordonnées selon le flux ; boîtes “sortantes” au bord de leur cible.
- [ ] SVG README 100 % auto-contenue (aucune URL externe).
- [ ] PNG re-rendu + copié dans le deck ; README pointe le bon PNG.
- [ ] Commit atomique (`docs(arch): …`) ; push sur la branche de la PR uniquement.