---
description: Create a detailed French synthesis of a document or topic from the wiki
argument-hint: <document-path-or-topic>
---

Run the SYNTHESIZE workflow on: $ARGUMENTS

## Step 0 — Identify the synthesis type

Before doing anything, ask the user one single question:

"Est-ce que tu souhaites une synthèse de **l'article/document** `$ARGUMENTS`, ou une synthèse sur le **sujet** '$ARGUMENTS' en croisant plusieurs sources du wiki ?"

Wait for the answer before continuing. Then follow the appropriate branch below.

---

## Branch A — Document synthesis (single source)

The user wants a synthesis of one specific document or source file.

**Step A1 — Read the source**
Read the file at $ARGUMENTS. If it is a wiki source page, also read the raw source it summarizes if available.

**Step A2 — Extract structure**
Identify the main sections, arguments, and key data points of the document. Do not summarize yet — map the structure first.

**Step A3 — Write the synthesis in French**
Write a detailed, structured synthesis following this exact format:

---

# [Titre du document]

> **Type de source :** article | documentation | paper | tutoriel  
> **Pertinence projet :** Module [X] — [nom du module]  
> **Date d'ingestion :** YYYY-MM-DD

## Vue d'ensemble

2 à 3 paragraphes de prose continue (pas de bullet points) expliquant de quoi parle le document, pourquoi il est important, et ce qu'il apporte concrètement au projet. Le ton est direct et opinionné — pas une description neutre.

## Points clés

Pour chaque point clé, écrire un mini-paragraphe de 3 à 5 lignes minimum. Pas de bullet points courts. Chaque point doit être suffisamment développé pour être compris sans avoir lu le document original.

### [Titre du point clé 1]
Explication développée en prose. Les termes techniques sans équivalent français restent en anglais : tool calling, backtesting, confidence score, rate limiting, etc.

### [Titre du point clé 2]
Explication développée en prose.

### [Titre du point clé N]
...

## Ce que ça change pour le projet

1 à 2 paragraphes expliquant concrètement comment ce document impacte la façon de construire le projet. Pas de généralités — des implications précises.

## Termes techniques à retenir

Liste des termes importants introduits par ce document, avec une définition courte en français. Les termes anglais sans traduction naturelle sont conservés tels quels.

| Terme | Définition courte |
|---|---|
| Tool calling | Mécanisme par lequel un agent IA invoque un outil externe |
| ... | ... |

## Questions ouvertes

2 à 3 questions que ce document soulève et qui méritent d'être explorées dans de futures sessions ou sources.

---

**Step A4 — Save the synthesis**
Save the synthesis to `wiki/analyses/YYYY-MM-DD-synthesis-[slug].md` with this frontmatter:

```
---
type: analysis
subtype: synthesis-document
date: YYYY-MM-DD
source: "[path to source]"
query: "Synthesis of [document title]"
language: french
tags: []
---
```

Update `index.md` and append to `log.md`.

---

## Branch B — Topic synthesis (multiple sources)

The user wants a synthesis on a topic, pulling from multiple wiki sources.

**Step B1 — Read the index**
Read `index.md` to identify all pages related to the topic in $ARGUMENTS.

**Step B2 — Read relevant pages**
Read all relevant source, entity, and concept pages identified in the index. Note contradictions between sources explicitly.

**Step B3 — Write the synthesis in French**
Write a detailed, structured synthesis following this exact format:

---

# Synthèse : [Nom du sujet]

> **Sources consultées :** N pages wiki  
> **Pertinence projet :** Modules [X, Y] — [noms]  
> **Date :** YYYY-MM-DD

## Introduction

2 à 3 paragraphes de prose continue présentant le sujet, son importance dans le contexte du projet, et comment les différentes sources abordent ce sujet. Mentionner les angles divergents s'il y en a.

## [Thème principal 1]

Plusieurs paragraphes de prose développant ce thème à partir de ce que disent les sources. Citer les sources avec des références wiki inline : selon [[wiki/sources/slug]], ... Les contradictions entre sources sont explicitées clairement dans le texte, pas cachées.

## [Thème principal 2]

Idem.

## [Thème principal N]

...

## Synthèse et implications pour le projet

1 à 2 paragraphes conclusifs expliquant ce que l'ensemble de ces sources, prises ensemble, implique pour la construction du projet. Pas de généralités — des décisions concrètes ou des orientations précises.

## Contradictions identifiées

Si des sources se contredisent sur des points importants, les lister ici avec une explication claire du désaccord et, si possible, une position recommandée.

| Sujet | Source A dit | Source B dit | Position recommandée |
|---|---|---|---|
| ... | ... | ... | ... |

## Termes techniques à retenir

Même format que Branch A.

## Pour aller plus loin

2 à 3 sources manquantes qui permettraient d'enrichir cette synthèse, avec une justification courte.

---

**Step B4 — Save the synthesis**
Save to `wiki/analyses/YYYY-MM-DD-synthesis-[slug].md` with this frontmatter:

```
---
type: analysis
subtype: synthesis-topic
date: YYYY-MM-DD
sources_consulted: [list of wiki pages read]
query: "Topic synthesis: [topic]"
language: french
tags: []
---
```

Update `index.md` and append to `log.md`.

---

## Notion compatibility note

The synthesis is formatted in standard Markdown compatible with Notion's import feature. To import: open Notion → New page → Import → Markdown & CSV → select the file from `wiki/analyses/`. All headers, tables, and formatting will be preserved automatically.

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
