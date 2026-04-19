---
description: Update the project context with new decisions, progress or learnings
argument-hint: <brief description of what changed>
---

Run the CONTEXT UPDATE workflow on: $ARGUMENTS

## Step 0 — Vérifier l'existence du project-context

Vérifie si `wiki/context/project-context.md` existe déjà.

**Si le fichier n'existe pas encore** : demande à l'utilisateur de décrire son projet en répondant aux questions suivantes, une par une. Attends chaque réponse avant de poser la suivante :

1. "Quel est l'objectif principal de ce projet ? En une ou deux phrases, qu'est-ce que tu construis ?"
2. "Quel est ton niveau actuel en développement ? Et quelles sont tes connaissances dans le domaine du projet ?"
3. "Quel est le contexte d'apprentissage — est-ce un projet pour apprendre, pour livrer un produit réel, ou les deux ?"
4. "Quelles sont les grandes technologies ou outils que tu penses utiliser ou que tu dois apprendre ?"
5. "Y a-t-il un plan de formation ou des étapes définies ? Si oui, lesquelles ?"

Avec ces réponses, crée `wiki/context/project-context.md` en suivant la structure définie à l'étape 2. Puis continue avec l'étape 3 pour intégrer les informations de $ARGUMENTS si présentes.

**Si le fichier existe déjà** : lis-le intégralement, puis continue avec l'étape 1.

---

## Step 1 — Lire l'état actuel

Lis les fichiers suivants pour comprendre où en est le projet :
- `wiki/context/project-context.md` — contexte complet du projet
- `log.md` (5 dernières entrées) — ce qui a été fait récemment
- `index.md` — état actuel du wiki

---

## Step 2 — Structure du project-context.md

Le fichier `wiki/context/project-context.md` doit toujours respecter cette structure. Si des sections manquent, les ajouter. Ne jamais supprimer une section existante — la mettre à jour ou la laisser vide avec une note.

```markdown
# PROJECT CONTEXT

## Résumé du projet
Description courte et précise de ce qu'on construit. Mis à jour à chaque pivot ou changement d'objectif.

## Objectif d'apprentissage
Ce que l'utilisateur cherche à apprendre ou maîtriser à travers ce projet. Distinct du livrable — c'est la motivation pédagogique.

## Profil utilisateur
- Niveau technique actuel
- Connaissances dans le domaine du projet
- Style d'apprentissage (par projet, par cours, par expérimentation...)

## Stack technique
Liste des technologies, langages, frameworks, outils utilisés ou à apprendre. Mise à jour à chaque décision technique.

## Architecture & composants
Description des composants principaux du projet, leurs rôles et leurs relations. Peut inclure des agents, des modules, des services, des APIs — selon ce que le projet implique.

## Décisions importantes
Décisions d'architecture ou de conception prises et leurs raisons. Chaque décision annule ou remplace la précédente si elle est contradictoire.

## Plan de formation ou d'exécution
Étapes ordonnées avec leur statut :
- ⬜ À faire
- 🔄 En cours
- ✅ Terminé

## Prochaine étape immédiate
Une seule chose — la prochaine action concrète à réaliser. Mise à jour après chaque session.

## Sources de données ou ressources clés
APIs, datasets, outils externes, documentations importantes pour le projet.

## Questions ouvertes
Ce qui n'est pas encore décidé ou résolu. Supprimé quand résolu, pas archivé.
```

---

## Step 3 — Intégrer les nouvelles informations

Si $ARGUMENTS contient une description de ce qui a changé, intègre ces informations dans le fichier en respectant ces règles :

- Mets à jour la section concernée **en place** — ne duplique jamais du contenu
- Si une décision contredit une décision précédente, ajoute `> [!warning] Décision modifiée : [ancienne décision] → [nouvelle décision]` et mets à jour le contenu
- Si $ARGUMENTS est vide, demande à l'utilisateur ce qui a changé depuis la dernière session avant de continuer
- La section `## Prochaine étape immédiate` doit toujours refléter l'état réel après la mise à jour

**Niveau de détail attendu dans le project-context :** chaque section doit être suffisamment précise pour qu'un LLM lisant uniquement ce fichier comprenne exactement le projet, ses contraintes, ses décisions et son état d'avancement — sans avoir besoin d'autres fichiers. C'est la source de vérité unique. Quand on doute si un détail est important, on l'inclut.

---

## Step 4 — Créer les pages wiki associées si nécessaire

Si les nouvelles informations introduisent un concept, un outil, ou une décision technique suffisamment importante pour mériter sa propre page :
- Crée `wiki/concepts/[slug].md` ou `wiki/entities/[slug].md`
- Applique les règles de nommage et de tagging du projet
- Lie la nouvelle page depuis le project-context avec un `[[wikilink]]`

---

## Step 5 — Mettre à jour index et log

- Mets à jour `index.md` si de nouvelles pages ont été créées
- Appende une entrée à `log.md` :

```
## [YYYY-MM-DD] context-update | $ARGUMENTS

- Sections modifiées : [liste]
- Pages créées : [liste ou "aucune"]
- Décisions modifiées : [liste ou "aucune"]
- Prochaine étape : [une phrase]
```

---

## Step 6 — Rapport final

Présente à l'utilisateur un résumé en français :
- Ce qui a été modifié dans le project-context et pourquoi
- Les éventuelles contradictions détectées et comment elles ont été résolues
- La prochaine étape immédiate telle qu'elle apparaît maintenant dans le fichier
- Un rappel si des questions ouvertes restent sans réponse depuis plus de 2 sessions

## Étape finale — Commit Git

```bash
git add wiki/context/ && git commit -m "update-context: [description-courte]"
```

Si git n'est pas initialisé, informe l'utilisateur avec les instructions d'initialisation ci-dessus.
