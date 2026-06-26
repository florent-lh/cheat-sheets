# Claude Code — Commandes, CLI et raccourcis

## Table des matières

1. [Gestion de Projet](#gestion-de-projet)
2. [Informations & Statut](#informations--statut)
3. [Contrôle du Mode & du Modèle](#contrôle-du-mode--du-modèle)
4. [Gestion des Fonctionnalités](#gestion-des-fonctionnalités)
5. [Paramètres de l'Environnement](#paramètres-de-lenvironnement)
6. [Intégrations & Extensions](#intégrations--extensions)
7. [Commandes de Regroupement](#commandes-de-regroupement)
8. [Commandes CLI](#commandes-cli)
9. [Raccourcis Clavier & Notation](#raccourcis-clavier--notation)

---

## Gestion de Projet

| #  | Commande   | Description                                                        |
| --- | ---------- | ------------------------------------------------------------------ |
| 1  | `/init`    | Auto-générer un fichier `CLAUDE.md` pour votre projet              |
| 2  | `/memory`  | Modifier le fichier de mémoire `CLAUDE.md`                         |
| 3  | `/context` | Voir ce qui consomme votre fenêtre de contexte                     |
| 4  | `/compact` | Compresser le contexte pour libérer de l'espace                    |
| 5  | `/clear`   | Réinitialiser l'historique de la conversation                      |
| 6  | `/resume`  | Reprendre une session précédente                                   |
| 7  | `/fork`    | Brancher la conversation vers une nouvelle session                 |
| 8  | `/rename`  | Renommer la session actuelle                                       |
| 9  | `/add-dir` | Ajouter un répertoire supplémentaire au contexte                   |
| 10 | `/copy`    | Sélectionner et copier des blocs de code                           |
| 11 | `/diff`    | Visualiser toutes les modifications dans un visualiseur interactif |
| 12 | `/export`  | Exporter la conversation vers un fichier ou le presse-papiers      |

---

## Informations & Statut

| #  | Commande       | Description                                                                                  |
| --- | -------------- | ---------------------------------------------------------------------------------------------- |
| 13 | `/usage`       | Vérifier l'utilisation des jetons par rapport aux limites du forfait                         |
| 14 | `/cost`        | Afficher le coût de la session actuelle                                                       |
| 15 | `/help`        | Lister toutes les commandes disponibles                                                       |
| 16 | `/tasks`       | Vérifier les tâches en arrière-plan                                                           |
| 17 | `/doctor`      | Exécuter des diagnostics d'environnement                                                      |
| 18 | `/stats`       | Visualiser l'usage quotidien, l'historique de sessions, les streaks et préférences de modèle  |
| 19 | `/debug`       | Afficher les informations de débogage                                                         |
| 20 | `/effort`      | Changer le niveau de réflexion : faible, moyen, élevé ou auto                                 |
| 21 | `/extra-usage` | Activer une capacité d'utilisation supplémentaire                                              |
| 22 | `/insights`    | Générer un rapport d'analyse de vos sessions : zones de projet, patterns d'interaction, points de friction |

---

## Contrôle du Mode & du Modèle

| #  | Commande        | Description                                          |
| --- | --------------- | ---------------------------------------------------- |
| 23 | `/model`        | Basculer entre Opus, Sonnet et Haiku                 |
| 24 | `/fast`         | Activer ou désactiver le mode Rapide                 |
| 25 | `/plan`         | Activer ou désactiver le mode Plan, en lecture seule |
| 26 | `/vim`          | Activer ou désactiver l'édition de style Vim         |
| 27 | `/output-style` | Modifier le style de sortie                          |
| 28 | `/voice`        | Activer l'invite vocale                              |

---

## Gestion des Fonctionnalités

| #  | Commande       | Description                                                             |
| --- | -------------- | ------------------------------------------------------------------------ |
| 29 | `/hooks`       | Configurer les hooks de cycle de vie                                    |
| 30 | `/agents`      | Créer et gérer des sous-agents                                          |
| 31 | `/permissions` | Modifier les paramètres de permission                                   |
| 32 | `/sandbox`     | Activer le mode bac à sable                                             |
| 33 | `/config`      | Configurer l'interface des paramètres                                   |
| 34 | `/rewind`      | Revenir en arrière dans la conversation et/ou les modifications de code |
| 35 | `/login`       | Se réauthentifier                                                       |
| 36 | `/logout`      | Se déconnecter                                                          |

---

## Paramètres de l'Environnement

| #  | Commande          | Description                                    |
| --- | ----------------- | ----------------------------------------------- |
| 37 | `/terminal-setup` | Configurer le raccourci clavier `Shift+Entrée`  |
| 38 | `/keybindings`    | Ouvrir la configuration des raccourcis clavier  |
| 39 | `/status-line`    | Configurer la ligne de statut du terminal       |
| 40 | `/theme`          | Modifier le thème de coloration syntaxique      |
| 41 | `/upgrade`        | Mettre à niveau votre forfait Claude            |

---

## Intégrations & Extensions

| #  | Commande              | Description                                                                                            |
| --- | --------------------- | -------------------------------------------------------------------------------------------------------- |
| 42 | `/install-github-app` | Configurer l'auto-revue de pull requests GitHub                                                          |
| 43 | `/plugin`             | Gérer les plugins : ajout, suppression, marketplace                                                      |
| 44 | `/mcp`                | Vérifier le statut et l'authentification MCP                                                             |
| 45 | `/rc`                 | Basculer vers la commande à distance depuis téléphone ou tablette                                        |
| 46 | `/review`             | **Déprécié** — installer le plugin `code-review` à la place : `claude plugin install code-review@claude-plugins-official` |
| 47 | `/pr-comments`        | Afficher les commentaires de pull request pour la branche actuelle                                       |
| 48 | `/security-review`    | Auditer la sécurité des modifications non validées                                                       |
| 49 | `/skills`             | Ouvrir le menu de gestion des compétences                                                                |
| 50 | `/find-skills`        | Parcourir et installer des compétences                                                                   |
| 51 | `/chrome`             | Configurer l'intégration de Claude dans Chrome (permissions par site, navigation web)                    |
| 52 | `/ide`                | Gérer les intégrations IDE                                                                                |
| 53 | `/btw`                | Poser une question secondaire sans interrompre                                                            |
| 54 | `/loop`               | Exécuter une invite selon un calendrier récurrent                                                         |

---

## Commandes de Regroupement

| #  | Commande    | Description                                                                                   |
| --- | ----------- | ----------------------------------------------------------------------------------------------- |
| 55 | `/simplify` | Analyser le projet avec 3 agents : architecture, doublons, performance                         |
| 56 | `/batch`    | Exécuter des modifications parallèles à grande échelle sur plusieurs arborescences de travail  |

---

## Commandes CLI

| #  | Commande                                       | Description                                                                          |
| --- | ----------------------------------------------- | --------------------------------------------------------------------------------------- |
| 57 | `claude`                                        | Démarrer une session interactive                                                       |
| 58 | `claude "question"`                            | Démarrer avec une invite initiale                                                       |
| 59 | `claude -p "question"`                         | Utiliser le mode non interactif, puis quitter                                          |
| 60 | `claude -c`                                     | Reprendre la session la plus récente                                                   |
| 61 | `claude -r "ID"`                                | Reprendre une session par ID                                                            |
| 62 | `claude --from-pr`                              | Reprendre une session liée à une pull request                                          |
| 63 | `claude update`                                 | Mettre à jour vers la dernière version                                                 |
| 64 | `claude mcp list`                               | Lister les serveurs MCP configurés                                                     |
| 65 | `claude mcp add`                                | Ajouter un serveur MCP                                                                  |
| 66 | `claude mcp remove`                             | Supprimer un serveur MCP                                                                |
| 67 | `claude mcp serve`                              | Exécuter Claude Code en tant que serveur MCP                                            |
| 68 | `claude auth login`                             | S'authentifier                                                                          |
| 69 | `claude auth status`                            | Vérifier le statut d'authentification                                                  |
| 70 | `claude auth logout`                            | Se déconnecter                                                                          |
| 71 | `claude agents`                                 | Ouvrir l'agent view pour piloter des sessions en arrière-plan (`--json` pour scripting) |
| 72 | `claude attach <id>`                            | Se rattacher à une session en arrière-plan dans ce terminal                            |
| 73 | `claude logs <id>`                              | Afficher les logs récents d'une session en arrière-plan                               |
| 74 | `claude stop <id>` / `claude rm <id>`           | Arrêter / supprimer une session en arrière-plan                                        |
| 75 | `claude respawn <id>`                           | Relancer une session en arrière-plan en conservant la conversation                      |
| 76 | `claude rc`                                     | Découvrir une session de commande à distance                                           |
| 77 | `claude plugin`                                 | Gérer les plugins                                                                       |
| 78 | `claude config list`                            | Afficher tous les paramètres                                                           |
| 79 | `claude config get <key>`                       | Vérifier la valeur d'un paramètre précis                                               |
| 80 | `claude config set`                             | Mettre à jour un paramètre                                                              |
| 81 | `claude config add <key> <value>`               | Ajouter une valeur à un paramètre de type tableau                                       |
| 82 | `claude project purge [path]`                   | Supprimer l'état local Claude Code d'un projet (transcripts, logs, historique)         |
| 83 | `claude setup-token`                            | Générer un token OAuth longue durée pour CI/scripts                                     |
| 84 | `claude --dangerously-skip-permissions`         | Ignorer les demandes de permission                                                     |
| 85 | `claude --worktree`, `-w`                       | Utiliser une arborescence de travail Git isolée pour le travail parallèle              |
| 86 | `claude --model opus`                           | Spécifier le modèle au lancement                                                        |
| 87 | `claude --agent <name>`                         | Spécifier un sous-agent pour la session courante                                       |
| 88 | `claude --agents '{json}'`                      | Définir des sous-agents au lancement                                                    |
| 89 | `claude --permission-mode <mode>`               | Démarrer dans un mode précis : `default`, `acceptEdits`, `plan`, `auto`, `dontAsk`, `bypassPermissions` |
| 90 | `claude --append-system-prompt`                 | Ajouter du contenu à l'invite système                                                  |
| 91 | `claude --max-turns N`                          | Définir la limite de tours                                                              |
| 92 | `claude --max-budget-usd N`                     | Plafonner la dépense API en dollars (mode `-p` uniquement)                              |
| 93 | `claude --chrome`                               | Activer l'intégration du navigateur Chrome pour cette session                          |
| 94 | `claude --verbose`                              | Logs détaillés, affiche la sortie complète tour par tour                               |
| 95 | `claude --allowedTools "..."`                   | Outils exécutés sans demande de permission                                             |
| 96 | `claude --disallowedTools "..."`                | Bloquer des outils spécifiques (ex : `"mcp__*"` pour tout le MCP)                       |
| 97 | `claude --tools "Bash,Edit,Read"`               | Restreindre Claude à cette liste d'outils intégrés                                     |
| 98 | `claude -p --output-format json`               | Définir le format de sortie en mode `-p` : `text`, `json` ou `stream-json`             |
| 99 | `claude --teleport`                             | Reprendre en local une session démarrée sur le web                                     |
| 100 | `claude --remote "tâche"`                      | Lancer une nouvelle session web sur claude.ai                                          |

---

## Raccourcis Clavier & Notation

| #   | Raccourci                  | Description                                                                |
| --- | --------------------------- | ------------------------------------------------------------------------------ |
| 101 | `Esc`                       | Arrêter le traitement                                                       |
| 102 | `Esc x2`                    | Ouvrir le menu de retour en arrière                                          |
| 103 | `Shift+Tab`                 | Faire défiler les modes : normal, acceptation automatique, plan              |
| 104 | `Option+T` / `Alt+T`        | Activer ou désactiver la réflexion étendue                                  |
| 105 | `Ctrl+X Ctrl+K`             | Arrêter tous les sous-agents en arrière-plan (2 fois en 3s pour confirmer)   |
| 106 | `Ctrl+T`                    | Afficher/masquer la liste des tâches en cours                               |
| 107 | `Ctrl+O`                    | Afficher/masquer le visualiseur de transcript (détail des appels d'outils)  |
| 108 | `Ctrl+G` / `Ctrl+X Ctrl+E`  | Ouvrir le prompt dans l'éditeur de texte par défaut (`$EDITOR`)              |
| 109 | `Ctrl+V`                    | Coller une image depuis le presse-papiers                                   |
| 110 | `Ctrl+R`                    | Lancer une recherche inversée dans l'historique des commandes               |
| 111 | `Option+P` / `Alt+P`        | Changer de modèle sans effacer le prompt en cours                           |
| 112 | `Option+O` / `Alt+O`        | Activer/désactiver le mode rapide (fast mode)                               |
| 113 | `Ctrl+L`                    | Redessiner l'écran (en cas d'affichage corrompu)                            |
| 114 | `@filename`                 | Faire référence à un fichier ou un répertoire                               |
| 115 | `#content`                  | Ajouter du contenu à `CLAUDE.md`                                             |
| 116 | `!command`                  | Exécuter directement une commande shell                                     |
| 117 | `Ctrl+B`                    | Basculer la tâche bash en cours en arrière-plan (2x sous tmux)               |
| 118 | `"Think harder"`            | Forcer un effort élevé pour un tour                                          |
| 119 | `"Ultra think"`             | Forcer une profondeur de raisonnement maximale                              |
