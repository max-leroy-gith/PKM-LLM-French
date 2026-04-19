---
description: Reset the entire wiki to a blank state — irreversible without Git
argument-hint: <optional: "force" to skip confirmation>
---

Run the CLEAR workflow. Arguments: $ARGUMENTS

## Step 0 — Avertissement et première confirmation

Avant de faire quoi que ce soit, affiche ce message d'avertissement à l'utilisateur :

```
⚠️  ATTENTION — OPÉRATION IRRÉVERSIBLE

Tu es sur le point de supprimer l'intégralité du contenu du wiki :
  - Tous les fichiers dans wiki/sources/
  - Tous les fichiers dans wiki/concepts/
  - Tous les fichiers dans wiki/entities/
  - Tous les fichiers dans wiki/analyses/
  - La réinitialisation complète de index.md

Cette action ne peut pas être annulée avec /back si Git n'est pas initialisé.
Si Git est actif, un commit "clear: wiki reset" sera créé avant la suppression.

Es-tu absolument certain de vouloir continuer ? (oui / non)
```

**Si $ARGUMENTS contient "force"** : saute cette confirmation et passe directement à l'étape 1.

**Si l'utilisateur répond autre chose que "oui"** : arrête immédiatement sans modifier aucun fichier et dis : "Opération annulée. Aucun fichier n'a été modifié."

**Si l'utilisateur répond "oui"** : continue avec l'étape 1.

---

## Step 1 — Sauvegarder l'état actuel dans Git (si disponible)

Exécute dans le terminal :

```bash
git status
```

**Si Git est initialisé** : crée un commit de sauvegarde avant de supprimer quoi que ce soit :

```bash
git add .
git commit -m "clear: snapshot avant reset du wiki"
```

Informe l'utilisateur : "Un commit de sauvegarde a été créé. Si tu changes d'avis après le clear, tu pourras restaurer avec : git checkout HEAD~1"

**Si Git n'est pas initialisé** : informe l'utilisateur que la suppression sera définitive et irréversible, et demande une confirmation supplémentaire avant de continuer :

```
Git n'est pas initialisé — la suppression sera DÉFINITIVE et IRRÉVERSIBLE.
Il n'y aura aucun moyen de récupérer les fichiers supprimés.
Tu confirmes quand même ? (oui / non)
```

Si l'utilisateur dit non : arrête sans modifier aucun fichier.

---

## Step 2 — Supprimer tout le contenu du wiki

Supprime tous les fichiers dans ces dossiers, en conservant les dossiers vides eux-mêmes :

- `wiki/sources/` → supprimer tous les fichiers .md à l'intérieur
- `wiki/concepts/` → supprimer tous les fichiers .md à l'intérieur
- `wiki/entities/` → supprimer tous les fichiers .md à l'intérieur
- `wiki/analyses/` → supprimer tous les fichiers .md à l'intérieur

**Ne supprime pas les dossiers eux-mêmes** — conserve la structure vide pour que le wiki soit prêt à recevoir du nouveau contenu immédiatement.

**Ne touche pas à** : `CLAUDE.md`, `.claude/`, `.obsidian/`, `raw/`, `wiki/context/`.

---

## Step 3 — Réinitialiser index.md

Remplace le contenu de `index.md` par ce template vide :

```markdown
# Wiki Index

*Wiki réinitialisé le YYYY-MM-DD. Aucune page pour le moment.*

## Sources
*(aucune)*

## Concepts
*(aucun)*

## Entities
*(aucune)*

## Analyses
*(aucune)*
```

---

## Step 4 — Réinitialiser log.md

Ajoute une entrée dans `log.md` qui documente le clear, sans supprimer l'historique précédent :

```markdown
---

## [YYYY-MM-DD] clear | Réinitialisation complète du wiki

- Fichiers supprimés : [nombre total de fichiers supprimés]
- Dossiers conservés : wiki/sources/, wiki/concepts/, wiki/entities/, wiki/analyses/
- index.md : réinitialisé
- project-context.md : [conservé / supprimé — selon la réponse de l'étape 5]
- Notes : wiki remis à zéro, structure préservée
```

---

## Step 5 — Décision sur project-context.md

Une fois toutes les suppressions effectuées, pose cette question à l'utilisateur :

```
✅ Le wiki a été réinitialisé.

Une dernière décision : souhaites-tu également supprimer le fichier
wiki/context/project-context.md ?

  → "oui" : le fichier sera supprimé. Tu devras relancer /update-context
             pour recréer le contexte de ton prochain projet depuis zéro.

  → "non" : le project-context.md est conservé tel quel. C'est le seul
             fichier qui survivra au clear — utile si tu veux repartir
             sur le même projet avec un wiki vierge.

Supprimer également project-context.md ? (oui / non)
```

**Si l'utilisateur répond "oui"** :
- Supprime `wiki/context/project-context.md`
- Ajoute dans `log.md` : `- project-context.md : supprimé`
- Dis à l'utilisateur : "project-context.md supprimé. Lance /update-context pour démarrer un nouveau projet."

**Si l'utilisateur répond "non"** :
- Conserve `wiki/context/project-context.md` intact
- Ajoute dans `log.md` : `- project-context.md : conservé`
- Dis à l'utilisateur : "project-context.md conservé. Le wiki est vide mais le contexte du projet est intact."

---

## Step 6 — Commit Git final (si disponible)

Si Git est initialisé, crée un commit final qui documente le clear :

```bash
git add .
git commit -m "clear: wiki reset — [N] fichiers supprimés"
```

---

## Step 7 — Rapport final

Affiche un résumé complet :

```
✅ CLEAR TERMINÉ

Fichiers supprimés :
  wiki/sources/     : N fichiers
  wiki/concepts/    : N fichiers
  wiki/entities/    : N fichiers
  wiki/analyses/    : N fichiers
  project-context   : supprimé / conservé

Fichiers réinitialisés :
  index.md          : ✅ remis à zéro
  log.md            : ✅ entrée ajoutée

Structure conservée :
  wiki/sources/, wiki/concepts/, wiki/entities/, wiki/analyses/
  CLAUDE.md, .claude/commands/

Pour démarrer un nouveau projet :
  → Lance /update-context pour créer un nouveau project-context.md
  → Lance /ingest pour commencer à alimenter le wiki
```
