---
description: Explain a concept or piece of code in French, then save it to the wiki
argument-hint: <concept, term, or code snippet to explain>
---

Run the EXPLAIN workflow on: $ARGUMENTS

## Step 0 — Identify what needs to be explained

Determine the nature of $ARGUMENTS :

**Si $ARGUMENTS est un terme ou concept** (ex: "tool calling", "KNN", "rate limiting", "JWT") :
- Vérifie d'abord si une page `wiki/concepts/[slug].md` existe déjà pour ce concept
- Si elle existe : lis-la, enrichis-la avec ce qui manque, et présente l'explication à l'utilisateur
- Si elle n'existe pas : continue avec les étapes suivantes pour créer une nouvelle explication

**Si $ARGUMENTS est un chemin vers un fichier de code** (ex: `src/agents/macro_agent.py`) :
- Lis le fichier
- L'explication portera sur ce que fait ce code, pourquoi il est structuré ainsi, et comment il s'intègre dans le projet
- La page concept créée décrira le pattern ou la logique implémentée, pas le code ligne par ligne

**Si $ARGUMENTS est un bloc de code collé directement** :
- Traite-le comme un fichier de code anonyme
- Demande à l'utilisateur quel nom donner à la page concept qui sera créée

---

## Step 1 — Lire le contexte projet

Lis `wiki/context/project-context.md` pour ancrer l'explication dans le contexte du projet. Une explication de "rate limiting" n'est pas la même selon qu'on construit une API REST ou un agent IA — adapte toujours l'explication au projet concret.

---

## Step 2 — Expliquer en français à l'utilisateur

Présente l'explication directement dans la conversation, en français, en suivant cette structure :

**L'idée centrale** — une ou deux phrases qui résument l'essentiel, comme si tu expliquais à quelqu'un qui n'a jamais entendu ce terme. Pas de jargon supplémentaire dans cette partie.

**Comment ça fonctionne** — explication développée en prose, en plusieurs paragraphes si nécessaire. Utilise une analogie concrète du monde réel si cela aide. Les termes techniques sans équivalent français naturel restent en anglais : tool calling, backtesting, rate limiting, middleware, payload, endpoint, etc.

**Dans le contexte du projet** — comment ce concept s'applique précisément à ce qu'on construit. Donne un exemple concret avec du pseudo-code ou un vrai exemple de code Python si c'est pertinent. L'exemple doit être directement lié au projet (agents IA, données macro, API FastAPI) et non générique.

**Ce qu'il ne faut pas confondre** — les erreurs classiques ou les concepts voisins qui prêtent à confusion. Une phrase ou deux par confusion potentielle.

---

## Step 3 — Demander confirmation avant de sauvegarder

Après avoir présenté l'explication, demande à l'utilisateur :

"Est-ce que cette explication est claire et complète ? Je peux la sauvegarder dans ton wiki sous `wiki/concepts/[slug].md`. Tu pourras la retrouver et la relire à tout moment. Je la sauvegarde ?"

Attends la confirmation avant de continuer.

---

## Step 4 — Créer ou mettre à jour la page concept

Si l'utilisateur confirme, crée ou mets à jour `wiki/concepts/[slug].md` avec ce format :

```markdown
---
type: concept
tags: []
source_count: 0
module: [numéro et nom du module concerné]
date_created: YYYY-MM-DD
language: french
---

## Définition

Ce que ce concept signifie dans le contexte précis de ce projet. Pas une définition de dictionnaire — une définition opérationnelle et ancrée dans ce qu'on construit.

## Explication détaillée

Plusieurs paragraphes de prose développant le concept. Inclut l'analogie utilisée pendant la session si elle était bonne. Les termes anglais sans traduction naturelle sont conservés tels quels et mis en gras à leur première occurrence.

## Dans le projet

Explication concrète de comment ce concept est ou sera utilisé dans le projet. Référence aux agents, modules, ou fichiers concernés. Inclut un exemple de code Python ou pseudo-code si pertinent.

## Ce qu'il ne faut pas confondre

Distinctions importantes avec des concepts voisins, erreurs classiques à éviter.

## Liens

- [[wiki/concepts/...]] — concepts directement liés
- [[wiki/sources/...]] — sources qui ont introduit ou approfondi ce concept
```

---

## Step 5 — Mettre à jour index et log

Ajoute la nouvelle page à `index.md` si elle vient d'être créée.

Appende une entrée courte à `log.md` :

```
## [YYYY-MM-DD] explain | [Nom du concept]

- Page créée/mise à jour : wiki/concepts/[slug].md
- Module concerné : [numéro et nom]
- Notes : [une phrase sur ce qui a motivé cette explication]
```

---

## Step 6 — Suggestions

Propose à l'utilisateur 1 ou 2 concepts directement liés qui mériteraient d'être expliqués lors d'une prochaine session, avec une phrase justifiant pourquoi ils sont pertinents pour la suite du projet.

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
