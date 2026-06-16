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
| -- | ---------- | ------------------------------------------------------------------ |
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

| #  | Commande       | Description                                                          |
| -- | -------------- | -------------------------------------------------------------------- |
| 13 | `/usage`       | Vérifier l'utilisation des jetons par rapport aux limites du forfait |
| 14 | `/cost`        | Afficher le coût de la session actuelle                              |
| 15 | `/help`        | Lister toutes les commandes disponibles                              |
| 16 | `/tasks`       | Vérifier les tâches en arrière-plan                                  |
| 17 | `/doctor`      | Exécuter des diagnostics d'environnement                             |
| 18 | `/stats`       | Générer des statistiques d'utilisation sous forme de rapport HTML    |
| 19 | `/debug`       | Afficher les informations de débogage                                |
| 20 | `/effort`      | Changer le niveau de réflexion : faible, moyen, élevé ou auto        |
| 21 | `/extra-usage` | Activer une capacité d'utilisation supplémentaire                    |

---

## Contrôle du Mode & du Modèle

| #  | Commande        | Description                                          |
| -- | --------------- | ---------------------------------------------------- |
| 22 | `/model`        | Basculer entre Opus, Sonnet et Haiku                 |
| 23 | `/fast`         | Activer ou désactiver le mode Rapide                 |
| 24 | `/plan`         | Activer ou désactiver le mode Plan, en lecture seule |
| 25 | `/vim`          | Activer ou désactiver l'édition de style Vim         |
| 26 | `/output-style` | Modifier le style de sortie                          |
| 27 | `/voice`        | Activer l'invite vocale                              |

---

## Gestion des Fonctionnalités

| #  | Commande       | Description                                                             |
| -- | -------------- | ----------------------------------------------------------------------- |
| 28 | `/hooks`       | Configurer les hooks de cycle de vie                                    |
| 29 | `/agents`      | Créer et gérer des sous-agents                                          |
| 30 | `/permissions` | Modifier les paramètres de permission                                   |
| 31 | `/sandbox`     | Activer le mode bac à sable                                             |
| 32 | `/config`      | Configurer l'interface des paramètres                                   |
| 33 | `/rewind`      | Revenir en arrière dans la conversation et/ou les modifications de code |
| 34 | `/login`       | Se réauthentifier                                                       |
| 35 | `/logout`      | Se déconnecter                                                          |

---

## Paramètres de l'Environnement

| #  | Commande          | Description                                    |
| -- | ----------------- | ---------------------------------------------- |
| 36 | `/terminal-setup` | Configurer le raccourci clavier `Shift+Entrée` |
| 37 | `/keybindings`    | Ouvrir la configuration des raccourcis clavier |
| 38 | `/status-line`    | Configurer la ligne de statut du terminal      |
| 39 | `/theme`          | Modifier le thème de coloration syntaxique     |
| 40 | `/upgrade`        | Mettre à niveau votre forfait Claude           |

---

## Intégrations & Extensions

| #  | Commande              | Description                                                        |
| -- | --------------------- | ------------------------------------------------------------------ |
| 41 | `/install-github-app` | Configurer l'auto-revue de pull requests GitHub                    |
| 42 | `/plugin`             | Gérer les plugins : ajout, suppression, marketplace                |
| 43 | `/mcp`                | Vérifier le statut et l'authentification MCP                       |
| 44 | `/rc`                 | Basculer vers la commande à distance depuis téléphone ou tablette  |
| 45 | `/review`             | Réaliser une revue de code pour une pull request spécifiée         |
| 46 | `/pr-comments`        | Afficher les commentaires de pull request pour la branche actuelle |
| 47 | `/security-review`    | Auditer la sécurité des modifications non validées                 |
| 48 | `/skills`             | Ouvrir le menu de gestion des compétences                          |
| 49 | `/find-skills`        | Parcourir et installer des compétences                             |
| 50 | `/chrome`             | Contrôler votre navigateur depuis Claude Code                      |
| 51 | `/ide`                | Gérer les intégrations IDE                                         |
| 52 | `/btw`                | Poser une question secondaire sans interrompre                     |
| 53 | `/loop`               | Exécuter une invite selon un calendrier récurrent                  |

---

## Commandes de Regroupement

| #  | Commande    | Description                                                                                   |
| -- | ----------- | --------------------------------------------------------------------------------------------- |
| 54 | `/simplify` | Analyser le projet avec 3 agents : architecture, doublons, performance                        |
| 55 | `/batch`    | Exécuter des modifications parallèles à grande échelle sur plusieurs arborescences de travail |

---

## Commandes CLI

| #  | Commande                                | Description                                                               |
| -- | --------------------------------------- | ------------------------------------------------------------------------- |
| 56 | `claude`                                | Démarrer une session interactive                                          |
| 57 | `claude "question"`                     | Démarrer avec une invite initiale                                         |
| 58 | `claude -p "question"`                  | Utiliser le mode non interactif, puis quitter                             |
| 59 | `claude -c`                             | Reprendre la session la plus récente                                      |
| 60 | `claude -r "ID"`                        | Reprendre une session par ID                                              |
| 61 | `claude --from-pr`                      | Reprendre une session liée à une pull request                             |
| 62 | `claude update`                         | Mettre à jour vers la dernière version                                    |
| 63 | `claude mcp list`                       | Lister les serveurs MCP configurés                                        |
| 64 | `claude mcp add`                        | Ajouter un serveur MCP                                                    |
| 65 | `claude mcp remove`                     | Supprimer un serveur MCP                                                  |
| 66 | `claude mcp serve`                      | Exécuter Claude Code en tant que serveur MCP                              |
| 67 | `claude auth login`                     | S'authentifier                                                            |
| 68 | `claude auth status`                    | Vérifier le statut d'authentification                                     |
| 69 | `claude auth logout`                    | Se déconnecter                                                            |
| 70 | `claude agents`                         | Lister les agents configurés                                              |
| 71 | `claude rc`                             | Découvrir une session de commande à distance                              |
| 72 | `claude plugin`                         | Gérer les plugins                                                         |
| 73 | `claude config list`                    | Afficher tous les paramètres                                              |
| 74 | `claude config set`                     | Mettre à jour un paramètre                                                |
| 75 | `claude --dangerously-skip-permissions` | Ignorer les demandes de permission                                        |
| 76 | `claude --worktree`                     | Utiliser une arborescence de travail Git isolée pour le travail parallèle |
| 77 | `claude --model opus`                   | Spécifier le modèle au lancement                                          |
| 78 | `claude --agents '{json}'`              | Définir des sous-agents au lancement                                      |
| 79 | `claude --append-system-prompt`         | Ajouter du contenu à l'invite système                                     |
| 80 | `claude --max-turns N`                  | Définir la limite de tours                                                |

---

## Raccourcis Clavier & Notation

| #  | Raccourci        | Description                                                     |
| -- | ---------------- | --------------------------------------------------------------- |
| 81 | `Esc`            | Arrêter le traitement                                           |
| 82 | `Esc x2`         | Ouvrir le menu de retour en arrière                             |
| 83 | `Shift+Tab`      | Faire défiler les modes : normal, acceptation automatique, plan |
| 84 | `Tab`            | Activer ou désactiver la réflexion étendue                      |
| 85 | `Ctrl+F`         | Arrêter tous les agents en arrière-plan                         |
| 86 | `Ctrl+V`         | Coller une image depuis le presse-papiers                       |
| 87 | `Ctrl+R`         | Lancer une recherche inversée dans l'historique des commandes   |
| 88 | `@filename`      | Faire référence à un fichier ou un répertoire                   |
| 89 | `#content`       | Ajouter du contenu à `CLAUDE.md`                                |
| 90 | `!command`       | Exécuter directement une commande shell                         |
| 91 | `& task`         | Exécuter une tâche en arrière-plan                              |
| 92 | `"Think harder"` | Forcer un effort élevé pour un tour                             |
| 93 | `"Ultra think"`  | Forcer une profondeur de raisonnement maximale                  |

---
