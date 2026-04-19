---
description: Lint the wiki for contradictions, orphans, gaps, and outdated content
argument-hint: <optional: focus on a specific section or module>
---

Run the LINT workflow from CLAUDE.md on the wiki. Focus area: $ARGUMENTS

## Step 0 — Read current state

Lis les fichiers suivants avant de commencer :
- `CLAUDE.md` pour les règles de maintenance du wiki
- `index.md` pour avoir une vue complète de toutes les pages existantes
- `log.md` (5 dernières entrées) pour comprendre ce qui a été fait récemment
- `wiki/context/project-context.md` pour évaluer la pertinence des gaps par rapport au module en cours

**Si $ARGUMENTS contient un focus** (ex: "Module 1", "agents", "base de données") : concentre l'analyse sur les pages liées à ce sujet en priorité, mais signale quand même les problèmes critiques trouvés ailleurs.

**Si $ARGUMENTS est vide** : analyse l'intégralité du wiki.

---

## Step 1 — Analyse complète du wiki

Lis toutes les pages listées dans `index.md`. Pour chaque page, vérifie les points suivants et note les problèmes trouvés sans les corriger encore — le rapport vient d'abord.

---

## Step 2 — Rapport de santé du wiki

Présente le rapport complet en français, structuré ainsi :

---

# Rapport de lint — [Date YYYY-MM-DD]

## Vue d'ensemble

Une ou deux phrases résumant l'état général du wiki : nombre de pages analysées, nombre de problèmes trouvés par catégorie, niveau de santé général (bon / à surveiller / critique).

## Contradictions entre pages

Pour chaque contradiction identifiée, décris précisément les deux pages en conflit, ce qu'elles disent de différent, et pourquoi c'est un problème. Ne te contente pas de signaler — propose quelle version est probablement correcte selon les sources disponibles.

Si aucune contradiction n'est trouvée : indique-le explicitement plutôt que d'omettre la section.

## Claims obsolètes

Pages dont le contenu a été contredit ou dépassé par des sources ingérées plus récemment. Pour chaque cas : quelle page, quelle affirmation, et quelle source plus récente la remet en question.

## Pages orphelines

Pages qui existent dans le wiki mais vers lesquelles aucune autre page ne pointe. Liste chaque page orpheline avec une proposition concrète : soit la supprimer si elle est inutile, soit identifier quelle page existante devrait y faire référence.

## Concepts mentionnés sans page dédiée

Termes ou concepts qui apparaissent dans plusieurs pages mais n'ont pas leur propre page `wiki/concepts/`. Pour chaque cas, indique dans combien de pages ce concept apparaît et si une page dédiée serait réellement utile ou si une mention suffit.

## Liens croisés manquants

Paires de pages qui devraient se référencer mutuellement mais ne le font pas. Indique quelles pages sont concernées et quelle relation justifie le lien.

## Gaps de contenu

Sujets importants pour le projet qui ne sont couverts par aucune page wiki actuelle. Évalue chaque gap selon deux critères : son importance pour le module en cours, et sa probabilité d'être comblé par une source facilement trouvable.

---

## Step 3 — Sources recommandées à ingérer

Sur la base des gaps identifiés et du module actuellement en cours dans `wiki/context/project-context.md`, propose 3 à 5 sources concrètes à ingérer en priorité.

Pour chaque source recommandée, écris un paragraphe court expliquant : ce qu'elle apporterait au wiki, quel gap spécifique elle comblerait, et où la trouver (URL ou description précise).

---

## Step 4 — Actions proposées

Liste ordonnée des corrections à effectuer, de la plus urgente à la moins urgente :

1. [Action concrète — ex: "Mettre à jour wiki/concepts/knn.md pour refléter la contradiction avec wiki/sources/..."]
2. ...

Demande à l'utilisateur : "Quels problèmes tu veux que je corrige maintenant ?" et attends sa réponse avant de modifier quoi que ce soit dans le wiki.

---

## Step 5 — Corrections demandées

Applique uniquement les corrections que l'utilisateur a validées à l'étape 4. Pour chaque correction :
- Mets à jour la page concernée
- Ajoute `> [!warning]` si une contradiction reste non résolue faute de source suffisante
- Met à jour `index.md` si des pages sont supprimées ou renommées

## Step 6 — Log

Appende une entrée à `log.md` :

```
## [YYYY-MM-DD] lint | [Focus ou "wiki complet"]

- Pages analysées : N
- Contradictions : N
- Orphelines : N
- Gaps identifiés : N
- Corrections appliquées : [liste ou "aucune"]
- Sources recommandées : [liste courte]
```

## Étape finale — Commit Git (si fichiers créés ou modifiés)

Si des fichiers ont été créés ou modifiés pendant cette commande, exécute :

```bash
git add wiki/ && git commit -m "[query/lint]: [description-courte]"
```

Si aucun fichier n'a été créé ou modifié, ne fais pas de commit.
