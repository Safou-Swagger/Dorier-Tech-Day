# AGENTS.md — Règles de travail à donner à votre agent IA

> À copier à la racine de votre projet, ou à coller directement dans votre agent (Claude Code / Codex) en tout début de session. Ce fichier est la mémoire des règles du projet : on les écrit une seule fois ici, et on y renvoie plutôt que de les redonner à chaque conversation — c'est aussi une façon concrète d'économiser des tokens et de la fenêtre de contexte (voir la note en bas de page).

## A. Comment un vrai projet garde sa mémoire dans le temps

Voici le modèle complet, tel qu'utilisé sur des projets qui durent plusieurs semaines ou mois. Vous n'aurez pas besoin de tout ça pour l'exercice d'aujourd'hui (voir l'encadré plus bas) — mais c'est le patron vers lequel tendre si votre prototype devient un vrai projet.

**À la racine du repo**
- `AGENTS.md` (ce fichier) — les règles de travail, écrites une seule fois : « single source of truth ». Si votre outil utilise un fichier `CLAUDE.md` ou `GEMINI.md`, il doit simplement pointer vers celui-ci plutôt que dupliquer les règles.
- `CHANGELOG.md` — journal des versions livrées.
- `TEMPLATE.md` — le gabarit du projet, s'il y en a un.

**Dossier `docs/` — la base de connaissances du projet**
- `JOURNAL.md` — log chronologique daté : une entrée par session (ce qui a été fait + la prochaine étape). L'archive détaillée du *comment*.
- `ROADMAP.md` — le document « où on en est » : ✅ Fait / 🔜 À faire / 💬 À trancher. Le *quoi*.
- `DECISIONS.md` — index des décisions durables (ADR), chacune détaillée dans son propre fichier sous `docs/decisions/`. Une décision actée (« accepted ») est immuable — pour la changer, on écrit une nouvelle décision qui remplace l'ancienne.
- `HANDOVER_DEV.md` — passation technique : stack, modèle de données, API, auth, déploiement, dette.
- `CDC.md` — le cahier des charges.
- `open-questions.md` — les questions encore ouvertes.
- `docs/conventions/` — style d'écriture, stack et outillage.
- `docs/processes/` — les façons de faire répétables (ex. comment mettre à jour un tag, comment traiter un conflit).

**Pour l'exercice d'aujourd'hui**, l'essentiel à créer réellement est : `AGENTS.md` + `docs/JOURNAL.md` + `docs/ROADMAP.md` + `docs/DECISIONS.md`. Le reste (CHANGELOG, TEMPLATE, HANDOVER_DEV, CDC, open-questions, conventions, processes) est la suite logique si votre prototype devient un vrai projet suivi dans la durée.

*Note : si votre agent utilise par défaut un dossier `MEMOIRE/` plutôt que `docs/`, remplacez les chemins ci-dessus par `MEMOIRE/journal.md`, `MEMOIRE/roadmap.md`, `MEMOIRE/decisions.md` — le principe est identique.*

## B. Les règles à donner à votre agent (à coller telles quelles)

```
RÈGLES DE TRAVAIL (à respecter strictement)

## Communication
- Répondre en français, clair, pas-à-pas, sans jargon inutile. L'utilisateur n'est pas développeur ;
  guider étape par étape les manips externes (terminal, dashboard Supabase, etc.).

## Déploiement / Git
- Ne JAMAIS déployer ni pousser en production sans feu vert explicite de l'utilisateur (« deploy », « go »).
- Un commit par étape, message clair ; TOUJOURS « git pull --rebase origin main » avant de pousser ;
  ne jamais force-push un main partagé ; finir les messages de commit par « Co-Authored-By: Claude … ».
- Clés/API/mots de passe UNIQUEMENT dans .env — jamais dans le code ni sur GitHub.
- Ne jamais écraser/réinitialiser la base de données lors d'un déploiement ; sauvegarder avant tout déploiement.

## Mémoire de projet — tenir les .md À JOUR À CHAQUE DÉVELOPPEMENT (pas seulement en fin de session)
- docs/JOURNAL.md : une ligne/section DATÉE après chaque changement livré (fait + prochaine étape).
- docs/ROADMAP.md : cocher ce qui est fait, ajouter/retirer dans « À faire », mettre l'en-tête à jour.
- docs/DECISIONS.md (+ docs/decisions/NNNN-*.md) : un ADR pour toute décision durable ; une fois « accepted »,
  un ADR est immuable (pour changer, écrire un NOUVEL ADR et marquer l'ancien « superseded »).
- Chaque page de docs/ garde sa frontmatter (title/status/last-reviewed/sources/tags) et on BUMPE
  « last-reviewed » à la date du jour lors d'une modif substantielle.
- Les règles elles-mêmes vivent dans un seul fichier (AGENTS.md) ; pour changer une règle, éditer CE fichier.

## Rituel de fin de session (léger, ~3 min) — proposer et ATTENDRE la validation avant d'écrire
- A-t-on DÉCIDÉ quelque chose de durable ? → entrée dans DECISIONS (ADR).
- BLOQUÉ > 30 min sur un point ? → noter cause + solution dans JOURNAL.
- TOUJOURS (seul élément obligatoire) : une ligne datée dans JOURNAL (fait + prochaine étape).
  Une session de routine peut n'avoir qu'une ligne — ne pas forcer des entrées creuses.

## Façon de travailler
- Vérifier avant d'affirmer (tester, regarder le rendu, captures d'écran) ; rapporter fidèlement
  (si un test échoue, le dire ; si une étape a été sautée, le dire).
- Poser des questions quand quelque chose manque de précision, plutôt que deviner.
```

## Pourquoi ce fichier existe

Sans lui, chaque nouvelle session avec votre agent doit ré-expliquer depuis zéro où vous en êtes — ce qui consomme du contexte et des tokens pour rien, et augmente le risque que l'agent reparte dans une direction déjà écartée. Avec un `JOURNAL.md` à jour, une seule ligne suffit à un agent — le vôtre demain, ou celui d'un collègue qui reprend votre projet — pour savoir où reprendre, sans lui redonner tout l'historique de la conversation.

*Note technique : il existe aussi des règles globales (dans `~/.claude/CLAUDE.md`) et une mémoire de session propre à Claude Code — un autre Claude Code lancé sur le même ordinateur les retrouve automatiquement ; sur une autre machine, il faut les recopier.*
