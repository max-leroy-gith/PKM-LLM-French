---
description: File the last assistant response as a permanent wiki page
argument-hint: <optional-slug>
---

File the previous assistant response into the wiki. Slug: $ARGUMENTS

## Step 0 — Identify what to save

Lis la dernière réponse de la conversation. Détermine son type pour choisir le bon emplacement et format :

- **Réponse à une question** (`/query`) → `wiki/analyses/YYYY-MM-DD-[slug].md`
- **Synthèse d'un sujet ou document** (`/synthesize`) → `wiki/analyses/YYYY-MM-DD-synthesis-[slug].md`
- **Explication d'un concept** (`/explain`) → `wiki/concepts/[slug].md`
- **Autre réponse substantielle** → `wiki/analyses/YYYY-MM-DD-[slug].md`

**Pour le slug :**
- Si $ARGUMENTS contient un slug : utilise-le tel quel en kebab-case (ex: `knn-news-agent`)
- Si $ARGUMENTS est vide : dérive un slug concis et descriptif depuis le contenu (ex: une réponse sur le rate limiting en FastAPI devient `rate-limiting-fastapi`)

**Si la dernière réponse est trop courte ou trop factuelle pour mériter une page wiki** (moins de 5-6 phrases substantielles, simple rappel d'une information déjà dans le wiki) : informe l'utilisateur que cette réponse ne justifie pas une page dédiée, et propose à la place de la rattacher comme note dans une page existante. Attends sa confirmation.

---

## Step 1 — Lire le contexte

Lis `index.md` pour vérifier qu'une page similaire n'existe pas déjà. Si une page proche existe, demande à l'utilisateur s'il préfère créer une nouvelle page ou enrichir la page existante. Attends sa réponse avant de continuer.

---

## Step 2 — Préparer le contenu

Reprends la dernière réponse de la conversation et prépare-la pour le wiki :

- Conserve tous les `[[wikilinks]]` existants tels quels
- Ajoute les wikilinks manquants vers les pages d'entités et concepts déjà dans le wiki qui sont mentionnés dans la réponse
- Vérifie que les termes techniques en anglais sont bien préservés (tool calling, backtesting, rate limiting, etc.)
- Si la réponse était en français dans la conversation, elle reste en français dans le wiki
- Restructure légèrement si nécessaire pour que la page soit lisible indépendamment de la conversation — quelqu'un qui ouvre cette page sans avoir lu la conversation doit comprendre de quoi il s'agit

---

## Step 3 — Écrire la page wiki

Crée le fichier à l'emplacement déterminé à l'étape 0, avec ce frontmatter :

```
---
type: analysis
subtype: [query-answer | synthesis-topic | synthesis-document | explanation | note]
date_created: YYYY-MM-DD
date_updated: YYYY-MM-DD
slug: [slug]
query: "[question ou prompt qui a généré cette réponse]"
language: french
sources_consulted: [liste des pages wiki lues pour générer la réponse, si applicable]
tags: []
---
```

Suivi du contenu de la réponse, restructuré si nécessaire, avec en fin de page :

```markdown
## Références

- [[wiki/sources/...]] — note courte sur ce que cette source apporte
- [[wiki/concepts/...]] — note courte sur le lien avec ce concept
- [[wiki/entities/...]] — si applicable
```

---

## Step 4 — Mettre à jour index et log

Ajoute la nouvelle page à `index.md` avec une description d'une ligne claire et précise.

Appende une entrée à `log.md` :

```
## [YYYY-MM-DD] save | [slug]

- Page créée : [chemin complet]
- Type : [type de contenu]
- Issu de : [query | synthesize | explain | conversation libre]
- Wikilinks ajoutés : [liste ou "aucun"]
```

---

## Step 5 — Confirmation

Informe l'utilisateur :
- Le chemin exact de la page créée
- Le nombre de wikilinks présents dans la page
- Si des concepts ou entités mentionnés dans la page n'ont pas encore leur propre page wiki — propose de les créer avec `/explain` lors d'une prochaine session

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
