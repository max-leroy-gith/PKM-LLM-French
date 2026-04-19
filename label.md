---
description: Analyse the wiki, propose a tag system, apply tags, and generate the Obsidian graph legend
argument-hint: <optional: specific tag or category to apply>
---

Run the LABEL workflow. Focus: $ARGUMENTS

## Step 0 — Determine mode

**Si $ARGUMENTS est vide** (l'utilisateur a tapé `/label` sans argument) :
- Passe à l'étape 1 pour analyser le projet et proposer un système de tags complet
- Attends la validation de l'utilisateur avant d'appliquer quoi que ce soit

**Si $ARGUMENTS contient un tag ou une catégorie précise** (ex: `/label agents` ou `/label module-3`) :
- Applique directement les tags correspondants sur les pages concernées
- Passe directement à l'étape 3

---

## Step 1 — Analyser le projet pour construire le système de tags

Lis les fichiers suivants pour comprendre le projet dans sa globalité :
- `wiki/context/project-context.md` — objectifs, stack, modules, agents définis
- `index.md` — toutes les pages existantes et leur description
- `CLAUDE.md` — structure du wiki et conventions

À partir de cette lecture, détermine **toi-même** les catégories de tags qui ont du sens pour CE projet. Ne copie pas un système générique — construis un système qui reflète exactement ce que contient ce wiki et ce que l'utilisateur apprend.

Pour chaque catégorie de tags que tu identifies, définis :
- Le nom du tag en kebab-case (court, sans espaces, sans accents)
- Ce qu'il regroupe concrètement dans ce projet
- Combien de pages existantes il concernerait approximativement

Réfléchis aux grandes familles naturelles qui émergent du projet : les domaines de connaissance couverts, les types de pages, les niveaux de priorité, les modules de formation. Mais reste libre — si tu identifies d'autres regroupements pertinents qui n'entrent pas dans ces familles, propose-les.

---

## Step 2 — Proposer le système et attendre validation

Présente à l'utilisateur le système de tags que tu as construit, sous forme de tableau dans le terminal. Le tableau doit être généré dynamiquement depuis ton analyse — pas de valeurs fixes.

Format du tableau :

```
╔══════════════════════════════════════════════════════════════════════════╗
║                    SYSTÈME DE TAGS PROPOSÉ                              ║
║                    Basé sur l'analyse du projet                         ║
╠══════════════════════╦══════════════╦════════════════════════════════════╣
║ TAG                  ║ COULEUR      ║ CE QUE ÇA REGROUPE                 ║
╠══════════════════════╩══════════════╩════════════════════════════════════╣
║  [CATÉGORIE : nom de la catégorie]                                      ║
╠══════════════════════╦══════════════╦════════════════════════════════════╣
║ [tag-propose]        ║ [Couleur]    ║ [Ce que ce tag regroupe]           ║
║ [tag-propose]        ║ [Couleur]    ║ [Ce que ce tag regroupe]           ║
╠══════════════════════╩══════════════╩════════════════════════════════════╣
║  [CATÉGORIE SUIVANTE]                                                   ║
╠══════════════════════╦══════════════╦════════════════════════════════════╣
║ [tag-propose]        ║ [Couleur]    ║ [Ce que ce tag regroupe]           ║
╚══════════════════════╩══════════════╩════════════════════════════════════╝
  Tags proposés : X  |  Pages concernées estimées : X
```

Choisis des couleurs distinctes et visuellement logiques — les domaines proches peuvent avoir des teintes voisines, les types de pages des teintes neutres, les priorités des couleurs vives.

Après le tableau, affiche les instructions de configuration Obsidian :

```
─────────────────────────────────────────────────────────────
  COMMENT APPLIQUER CES COULEURS DANS OBSIDIAN
─────────────────────────────────────────────────────────────
  1. Ouvre Obsidian → Graph View (icône en forme de réseau)
  2. Clique sur l'icône ⚙ (paramètres du graph)
  3. Va dans la section "Groups"
  4. Pour chaque tag du tableau ci-dessus :
     → Clique sur "+ Add group"
     → Dans "Query" tape exactement : tag:[nom-du-tag]
     → Clique sur le cercle coloré et choisis la couleur
  5. Les nœuds se colorisent instantanément dans le graph
─────────────────────────────────────────────────────────────
  Les tags sont déjà écrits dans tes fichiers markdown.
  Obsidian les lit automatiquement — tu n'as rien à saisir
  dans les fichiers, uniquement dans les paramètres du graph.
─────────────────────────────────────────────────────────────
```

Demande ensuite à l'utilisateur :
"Est-ce que ce système de tags correspond à ce que tu veux ? Tu peux me demander d'ajouter, renommer ou fusionner des catégories. Une fois validé, j'applique les tags sur toutes les pages existantes automatiquement."

Attends sa réponse et intègre ses modifications avant de continuer.

---

## Step 3 — Appliquer les tags sur toutes les pages du wiki

Pour chaque fichier dans `wiki/sources/`, `wiki/concepts/`, `wiki/entities/`, et `wiki/analyses/` :

- Lis le contenu de la page
- Détermine quels tags du système validé s'appliquent à cette page
- Mets à jour le frontmatter YAML en ajoutant ou corrigeant le champ `tags`
- Ne modifie jamais le contenu de la page — uniquement le frontmatter

Règle importante : si une page a déjà des tags cohérents avec le système validé, conserve-les et complète si nécessaire. Ne réécris pas des tags corrects.

---

## Step 4 — Rapport final dans le terminal

Après avoir appliqué les tags sur toutes les pages, affiche un tableau de distribution dynamique :

```
╔══════════════════════════════════════════════════════════════╗
║           DISTRIBUTION DES TAGS — ÉTAT ACTUEL DU WIKI       ║
╠══════════════════════╦════════════╦══════════════════════════╣
║ TAG                  ║ NB PAGES   ║ EXEMPLE DE PAGES         ║
╠══════════════════════╬════════════╬══════════════════════════╣
║ [tag]                ║     X      ║ [page1], [page2]...      ║
║ [tag]                ║     X      ║ [page1], [page2]...      ║
╠══════════════════════╬════════════╬══════════════════════════╣
║ TOTAL PAGES TAGUÉES  ║   X / X    ║ Pages sans tags : X      ║
╚══════════════════════╩════════════╩══════════════════════════╝
```

---

## Step 5 — Log

Appende une entrée à `log.md` :

```
## [YYYY-MM-DD] label | [focus ou "système complet"]

- Tags créés : [liste]
- Pages mises à jour : N
- Pages sans tags résiduelles : N
- Notes : [observations sur les clusters identifiés]
```

## Étape finale — Commit Git

Une fois tous les fichiers créés et le log mis à jour, exécute cette commande dans le terminal :

```bash
git add wiki/ && git commit -m "[commande]: [slug-descriptif]"
```

Remplace `[commande]` par le nom de la commande exécutée et `[slug-descriptif]` par un nom court décrivant ce qui a été fait.

Exemples :
- `git commit -m "ingest: introduction-aux-agents-langchain"`
- `git commit -m "synthesize: systeme-de-scoring-knn"`
- `git commit -m "explain: tool-calling"`
- `git commit -m "checkpoint: module-1-fred-api-termine"`

Si la commande `git` retourne une erreur indiquant que le dépôt n'existe pas, informe l'utilisateur avec ce message :
"Git n'est pas initialisé dans ce projet. Lance ces commandes une seule fois dans ton terminal pour activer le versioning et le /back :
```bash
git init
git add .
git commit -m 'init: wiki initial'
```
Ensuite relance la commande."
