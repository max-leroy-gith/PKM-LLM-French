---
description: Ask a question answered from the wiki
argument-hint: <question>
---

Run the QUERY workflow from CLAUDE.md for: $ARGUMENTS

## Step 0 — Read current state

Lis les fichiers suivants avant de commencer :
- `index.md` pour identifier toutes les pages potentiellement pertinentes à la question
- `wiki/context/project-context.md` pour ancrer la réponse dans le contexte du projet

---

## Step 1 — Identifier les pages pertinentes

À partir de `index.md`, identifie les pages dont le contenu est susceptible de répondre à $ARGUMENTS, directement ou partiellement. Note les pages candidates sans les lire encore.

Si aucune page ne semble pertinente : informe l'utilisateur que le wiki ne contient pas encore de contenu sur ce sujet, suggère 1 ou 2 sources à ingérer pour combler ce gap, et arrête là.

---

## Step 2 — Lire et suivre les liens

Lis les pages candidates identifiées à l'étape précédente. Suis les `[[wikilinks]]` internes si des pages liées apportent un contexte complémentaire utile à la réponse. Limite la profondeur de lecture à ce qui est directement pertinent — ne lis pas le wiki entier.

---

## Step 3 — Construire la réponse en français

Rédige la réponse en français, en prose structurée. Pas de bullet points courts — des paragraphes développés qui répondent vraiment à la question posée.

La réponse doit suivre cette structure :

**Réponse directe** — commence par répondre à la question en une ou deux phrases claires, sans détour. L'utilisateur sait immédiatement si le wiki contient ce qu'il cherche.

**Développement** — approfondis la réponse avec le contenu des pages lues. Cite chaque page source avec sa référence wiki inline : `[[wiki/concepts/knn]]` ou `[[wiki/sources/2026-04-18-fred-api]]`. Les termes techniques sans équivalent français naturel restent en anglais.

**Limites de la réponse** — si le wiki ne couvre pas certains aspects de la question, dis-le explicitement. Une réponse honnête sur ce que le wiki ne sait pas encore vaut mieux qu'une réponse incomplète présentée comme complète.

---

## Step 4 — Évaluer si la réponse mérite d'être sauvegardée

Si la réponse est substantielle — c'est-à-dire qu'elle synthétise plusieurs pages, révèle une connexion non évidente entre des concepts, ou constitue une explication qui resservira — propose à l'utilisateur de la sauvegarder :

"Cette réponse croise plusieurs pages du wiki et pourrait être utile à relire. Je peux la sauvegarder dans `wiki/analyses/YYYY-MM-DD-[slug].md`. Tu veux que je le fasse ?"

Si la réponse est simple et factuelle (une définition, un chiffre, un rappel rapide), ne propose pas de sauvegarde — ça polluerait le wiki inutilement.

Attends la confirmation de l'utilisateur avant de créer le fichier.

---

## Step 5 — Sauvegarder si confirmé

Si l'utilisateur confirme, crée `wiki/analyses/YYYY-MM-DD-[slug].md` avec ce frontmatter :

```
---
type: analysis
subtype: query-answer
date: YYYY-MM-DD
query: "$ARGUMENTS"
sources_consulted: [liste des pages lues]
language: french
tags: []
---
```

Mets à jour `index.md` avec la nouvelle page.

---

## Step 6 — Log

Appende une entrée à `log.md` :

```
## [YYYY-MM-DD] query | $ARGUMENTS

- Pages consultées : [liste]
- Réponse sauvegardée : oui → wiki/analyses/[slug].md | non
- Gaps identifiés : [sujet manquant ou "aucun"]
```

## Étape finale — Commit Git (si fichiers créés ou modifiés)

Si des fichiers ont été créés ou modifiés pendant cette commande, exécute :

```bash
git add wiki/ && git commit -m "[query/lint]: [description-courte]"
```

Si aucun fichier n'a été créé ou modifié, ne fais pas de commit.
