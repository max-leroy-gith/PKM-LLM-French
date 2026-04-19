---
description: Undo the last wiki command by reverting the last Git commit
argument-hint: <optional: number of steps back, default 1>
---

Run the BACK workflow. Steps to revert: $ARGUMENTS

## Step 0 — Déterminer le nombre de commits à annuler

**Si $ARGUMENTS est vide** : annule uniquement le dernier commit (1 étape en arrière).

**Si $ARGUMENTS est un nombre** (ex: `/back 2`) : annule les N derniers commits dans l'ordre, un par un.

**Si $ARGUMENTS contient autre chose qu'un nombre** : informe l'utilisateur que `/back` n'accepte qu'un nombre optionnel comme argument, et arrête là.

---

## Step 1 — Vérifier que Git est initialisé

Exécute dans le terminal :

```bash
git status
```

Si la commande retourne une erreur indiquant qu'il n'y a pas de dépôt Git :

```
Git n'est pas initialisé dans ce projet. Le /back n'est pas disponible.
Pour activer le versioning et le /back, lance ces commandes une seule fois :

git init
git add .
git commit -m "init: wiki initial"

Ensuite toutes tes prochaines commandes (/ingest, /save, etc.) créeront
automatiquement des commits que /back pourra annuler.
```

Arrête là si Git n'est pas initialisé.

---

## Step 2 — Afficher ce qui va être annulé

Avant d'annuler quoi que ce soit, montre à l'utilisateur exactement ce qui va être supprimé ou modifié.

Pour chaque commit qui sera annulé, exécute :

```bash
git log --oneline -[N]
```

Puis pour voir les fichiers affectés par le dernier commit :

```bash
git diff HEAD~1 HEAD --name-status
```

Affiche le résultat dans ce format :

```
Voici ce que /back va annuler :

Commit : [message du commit]
Date   : [date et heure]

Fichiers qui seront supprimés (créés par cette commande) :
  - wiki/sources/nom-du-fichier.md
  - wiki/concepts/nom-du-concept.md

Fichiers qui seront restaurés à leur état précédent (modifiés par cette commande) :
  - index.md
  - log.md

Es-tu sûr de vouloir annuler cette commande ? (oui / non)
```

Attends la confirmation de l'utilisateur avant de continuer.

---

## Step 3 — Annuler le ou les commits

Si l'utilisateur confirme, exécute pour chaque commit à annuler :

```bash
git revert HEAD --no-edit
```

Si $ARGUMENTS est un nombre N supérieur à 1, répète cette commande N fois dans l'ordre.

Note importante : `git revert` ne supprime pas l'historique — il crée un nouveau commit qui annule le précédent. Cela signifie que si tu fais `/back` par erreur, tu peux faire `/back` à nouveau pour annuler le `/back` lui-même.

---

## Step 4 — Confirmation finale

Après avoir exécuté le revert, affiche un résumé clair :

```
✅ /back effectué avec succès.

Ce qui a été annulé :
  - [liste des fichiers supprimés]
  - [liste des fichiers restaurés]

État actuel du wiki : identique à l'état avant la commande "[nom du commit annulé]".

Si tu as fait /back par erreur, tu peux l'annuler en relançant /back —
cela annulera le revert lui-même et restaurera les fichiers.
```

Exécute enfin :

```bash
git log --oneline -5
```

Et affiche les 5 derniers commits pour que l'utilisateur voie l'historique à jour.
