---
description: End-of-session checkpoint — what was built, what is broken, what comes next
argument-hint: <optional: short description of the session>
---

Run the CHECKPOINT workflow. Session context: $ARGUMENTS

## Step 0 — Determine mode

**Si $ARGUMENTS est vide** (l'utilisateur a tapé `/checkpoint` sans rien après) :
- Lis le fichier checkpoint le plus récent dans `wiki/analyses/` dont le nom contient "checkpoint"
- Présente son contenu à l'utilisateur en français de façon lisible et structurée
- Rappelle explicitement la liste des prochaines actions issues de ce checkpoint
- Arrête là. Ne crée aucun nouveau fichier, ne mets rien à jour.

**Si $ARGUMENTS contient une description** (l'utilisateur a tapé `/checkpoint "description de la session"`) :
- Continue avec les étapes suivantes pour enregistrer un nouveau checkpoint de fin de session.

---

## Step 1 — Read current state

Read the following files before doing anything:
- `wiki/context/project-context.md` to understand the current module and objectives
- `log.md` (last 3 entries) to understand what was done recently
- Any files that were created or modified during this session

---

## Step 2 — Bilan de session

Écris un bilan structuré de la session en français. Sois précis et factuel — pas de généralités, pas de flatterie. L'objectif est qu'à la prochaine session, en lisant ce bilan, l'utilisateur sache exactement où il en est sans avoir à relire tout l'historique.

Utilise ce format exact :

---

# Checkpoint — [Date YYYY-MM-DD] [Description courte de la session]

## Ce qui a été accompli

Pour chaque chose concrète réalisée pendant la session, écris un paragraphe court mais précis. Pas de bullet points vagues comme "travail sur le module 1" — des descriptions exactes comme "le script `get_fed_rate()` récupère correctement les données FEDFUNDS depuis l'API FRED sur les 12 derniers mois et les stocke dans `data/fed_rate.json`".

## État actuel du code

Décris l'état exact du projet à cet instant :
- Quels fichiers existent et ce qu'ils font
- Quels fichiers sont incomplets ou en cours
- Ce qui fonctionne et peut être testé maintenant
- Ce qui ne fonctionne pas encore et pourquoi

## Problèmes rencontrés

Pour chaque problème rencontré pendant la session, qu'il soit résolu ou non :

**[Nom du problème]**
Description précise du problème, comment il s'est manifesté, et ce qui a été tenté pour le résoudre. Si résolu : expliquer la solution retenue. Si non résolu : expliquer où ça bloque exactement.

## Prochaine session — actions exactes

Liste ordonnée des actions à faire au prochain démarrage, dans l'ordre exact à suivre. Chaque action doit être suffisamment précise pour être exécutée sans réfléchir :

1. [Action concrète avec fichier et commande si applicable]
2. [Action suivante]
3. ...

La première action doit toujours être : relire ce checkpoint et vérifier que l'état du code correspond à ce qui est décrit ici.

## Contexte à ne pas oublier

Décisions prises pendant la session qui ne sont pas évidentes dans le code — choix d'architecture, raisons d'avoir écarté une approche, dépendances à installer, variables d'environnement à configurer, etc.

---

## Step 2 — Mise à jour du context projet

Mets à jour `wiki/context/project-context.md` :
- Change le statut du module en cours si applicable (⬜ → 🔄 → ✅)
- Mets à jour la section "Prochaine étape immédiate" avec ce qui sort du checkpoint
- Ne touche pas aux autres sections sauf si quelque chose a fondamentalement changé

## Step 3 — Log

Appende une entrée à `log.md` :

```
## [YYYY-MM-DD] checkpoint | [Description courte]

- Accompli : [liste courte]
- Problèmes ouverts : [liste ou "aucun"]
- Prochaine étape : [une phrase]
```

## Step 5 — Confirmation

Dis à l'utilisateur :
- Le fichier checkpoint créé et son chemin
- Le statut mis à jour dans project-context.md
- Un rappel : "À la prochaine session, commence par `/checkpoint` pour relire ce bilan avant de coder."

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
