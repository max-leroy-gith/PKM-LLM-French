---
description: Ingest a source from raw/ into the wiki
argument-hint: <path-to-source-in-raw>
---

Run the INGEST workflow from CLAUDE.md on: $ARGUMENTS

## Step 0 — Relevance gate (do this before anything else)

Lis dans cet ordre :
1. `wiki/context/project-context.md` — pour comprendre les objectifs, le stack, et le plan d'apprentissage du projet
2. `index.md` — pour connaître ce qui est déjà dans le wiki
3. La source à $ARGUMENTS

À partir de la lecture du project-context, détermine **toi-même** ce qui est pertinent ou non pour CE projet spécifique. Ne te fie pas à des catégories prédéfinies — construis ton jugement depuis ce que le project-context dit sur les objectifs, le stack, et le plan d'apprentissage.

**Critères de jugement :**
- Est-ce que cette source apporte quelque chose que le projet nécessite, selon ce qu'on lit dans project-context ?
- Est-ce que cette source couvre un outil, concept, ou domaine présent dans le stack ou le plan de formation ?
- Est-ce que cette source est déjà couverte par une page existante dans index.md ?

**Décision :**
- Source **non pertinente** → explique clairement pourquoi en faisant référence au project-context, et arrête là. Ne crée aucun fichier.
- Source **partiellement pertinente** → indique quelles parties sont utiles et lesquelles ne le sont pas, demande confirmation avant de continuer.
- Source **pertinente** → continue avec les étapes suivantes.

---

## Step 1 — Discussion

Présente 3 à 5 takeaways clés à l'utilisateur. Demande s'il veut approfondir certains points avant d'écrire dans le wiki.
Présente les takeaways à l'utilisateur en français, même si la source est en anglais. 
Les pages wiki seront rédigées en anglais avec la terminologie technique standard.

---

## Règles de nommage — à appliquer sur TOUS les fichiers créés

Chaque fichier créé pendant cet ingest doit avoir un nom lisible, compréhensible et descriptif. Pas de slugs techniques cryptiques, pas de codes, pas d'abréviations obscures.

**Principe général :** le nom du fichier doit répondre à la question "de quoi parle cette page ?" sans avoir à l'ouvrir.

**Pour les pages sources** (`wiki/sources/`) : le nom reflète le sujet principal du document ingéré, pas son titre exact si celui-ci est vague ou technique. Exemples :
- ✅ `2026-04-18-comment-construire-un-agent-avec-langchain.md`
- ✅ `2026-04-18-introduction-aux-bases-de-donnees-vectorielles.md`
- ❌ `2026-04-18-doc-v2-final.md`
- ❌ `2026-04-18-article.md`

**Pour les pages concepts** (`wiki/concepts/`) : le nom est le concept lui-même, exprimé clairement. Si le concept est un terme sans traduction naturelle, garde la langue d'origine. Exemples :
- ✅ `tool-calling.md`
- ✅ `architecture-multi-agents.md`
- ✅ `knn-finance.md`
- ❌ `tc.md`
- ❌ `concept-3.md`

**Pour les pages entités** (`wiki/entities/`) : le nom est le nom propre de l'entité, en clair. Exemples :
- ✅ `langchain.md`
- ✅ `openai-api.md`
- ✅ `langchain.md`
- ❌ `entity-api-1.md`

Si tu hésites sur un nom, choisis le nom que tu utiliserais pour expliquer cette page à quelqu'un oralement.

---

## Règles de tagging — à appliquer sur TOUS les fichiers créés

Chaque fichier créé doit avoir des tags pertinents et cohérents dans son frontmatter YAML. Ces tags servent à coloriser le graph view d'Obsidian et à créer des clusters visuels.

**Comment construire les tags :** lis le `wiki/context/project-context.md` pour identifier les grandes catégories naturelles du projet — domaines de connaissance, composants techniques, modules de formation, priorités. Utilise ces catégories comme base pour tes tags. Ne crée pas de tags génériques qui n'ont pas de sens dans le contexte de ce projet.

**Tags de type** (chaque page en a exactement un, toujours) :
- `source` — page de résumé d'un document ingéré
- `concept` — page d'explication d'un concept ou terme
- `entity` — page sur une personne, organisation, outil, ou produit
- `analysis` — réponse à une question ou synthèse produite

**Tags de domaine et de module** : déduis-les depuis le project-context. Chaque page doit avoir au moins un tag de domaine et, si applicable, un tag correspondant à l'étape ou module du plan de formation concerné.

**Tags de priorité** (optionnel, ajoute si pertinent) :
- `fondamental` — concept ou source absolument essentiel à maîtriser
- `reference` — source à relire régulièrement
- `a-approfondir` — sujet qui mériterait plus de sources

Exemple de frontmatter complet :

```yaml
---
type: concept
tags: [tag-domaine, concept, tag-module, fondamental]
source_count: 1
date_created: YYYY-MM-DD
---
```

---

## Step 2 — Source summary

Crée `wiki/sources/YYYY-MM-DD-[nom-descriptif].md` en appliquant les règles de nommage et de tagging. Contenu :
- Résumé opinionné (pas neutre — qu'est-ce que ça apporte concrètement au projet ?)
- Key takeaways en bullets
- Connexions vers les pages entités et concepts existantes

## Step 3 — Entities et concepts

Pour chaque entité ou concept significatif mentionné, crée ou mets à jour la page correspondante dans `wiki/entities/` ou `wiki/concepts/` en appliquant les règles de nommage et de tagging :
- Si la source contredit une page existante, ajoute `> [!warning] Contradiction avec [[page]]`
- Incrémente `source_count` dans le frontmatter

## Step 4 — Index et log

- Ajoute chaque nouvelle page à `index.md`
- Appende une entrée à `log.md` avec la liste des pages créées/modifiées

## Step 5 — Rapport final

Dis à l'utilisateur :
- Combien de pages ont été créées et mises à jour
- Les noms choisis pour chaque fichier et pourquoi
- Les tags appliqués, regroupés par catégorie
- Les contradictions trouvées avec le wiki existant
- 2 à 3 questions que le wiki est maintenant en mesure de répondre grâce à cette source
- À quelle étape ou module du plan de formation (lu dans project-context) cette source correspond

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
