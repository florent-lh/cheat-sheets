# Git CheatSheet 

## Configuration Initiale

### Configuration de l'utilisateur
```bash
git config --global user.name "Votre Nom"
git config --global user.email "votre@email.com"
git config --global init.defaultBranch main
```

### Configuration des alias
```bash
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'
```

### Voir la configuration
```bash
git config --list
git config user.name
git config --show-origin user.name
```

## Création et Clonage

### Initialiser un dépôt
```bash
git init
git init --initial-branch=main
```

### Cloner un dépôt
```bash
git clone <url>
git clone <url> <nom-dossier>
git clone --depth 1 <url>  # Clone superficiel (dernier commit)
git clone --branch <branche> <url>
```

## Branches

### Gestion des branches
```bash
git branch                    # Liste les branches locales
git branch -a                 # Liste toutes les branches
git branch -r                 # Liste les branches distantes
git branch <nom>              # Crée une nouvelle branche
git branch -d <nom>           # Supprime une branche
git branch -D <nom>           # Force la suppression
git branch -m <ancien> <nouveau>  # Renomme une branche
git branch --show-current     # Affiche la branche courante
```

### Changer de branche
```bash
git checkout <branche>
git checkout -b <nouvelle-branche>  # Crée et bascule
git switch <branche>                # Nouvelle commande (Git 2.23+)
git switch -c <nouvelle-branche>    # Crée et bascule
```

### Restaurer des fichiers
```bash
git restore <fichier>               # Restaure le fichier (Git 2.23+)
git restore --staged <fichier>      # Désindexe le fichier
git restore --source=HEAD~2 <fichier>  # Restaure depuis un commit
```

## Staging et Commits

### Ajouter des fichiers
```bash
git add <fichier>
git add .                     # Ajoute tous les fichiers
git add *.js                  # Ajoute par pattern
git add -p                    # Ajout interactif par chunk
git add -A                    # Ajoute tout (modifs, suppressions, nouveaux)
```

### Commits
```bash
git commit -m "Message"
git commit -am "Message"      # Add + commit des fichiers trackés
git commit --amend            # Modifie le dernier commit
git commit --amend --no-edit  # Amende sans changer le message
git commit --allow-empty -m "Message"  # Commit vide
```

### Vérifier l'état
```bash
git status
git status -s                 # Format court
git status --short
```

## Historique et Différences

### Consulter l'historique
```bash
git log
git log --oneline
git log --graph --oneline --all
git log --author="Nom"
git log --since="2 weeks ago"
git log --until="2023-12-31"
git log -p                    # Avec les diffs
git log --stat                # Avec les stats
git log -n 5                  # 5 derniers commits
git log --follow <fichier>    # Historique d'un fichier (même renommé)
```

### Différences
```bash
git diff                      # Différences non indexées
git diff --staged             # Différences indexées
git diff HEAD                 # Toutes les différences
git diff <branche1>..<branche2>
git diff <commit1> <commit2>
git diff --name-only          # Uniquement les noms de fichiers
```

### Recherche dans l'historique
```bash
git log --grep="motif"        # Recherche dans les messages
git log -S"code"              # Recherche dans le contenu (pickaxe)
git log --all -- <fichier>    # Historique incluant les fichiers supprimés
```

## Annulation et Modification

### Annuler des modifications
```bash
git reset <fichier>           # Désindexe un fichier
git reset --soft HEAD~1       # Annule le dernier commit (garde les modifs)
git reset --mixed HEAD~1      # Annule le commit et désindexe
git reset --hard HEAD~1       # Annule tout (attention: destructif)
git reset --hard <commit>     # Reset au commit spécifié
```

### Nettoyer le répertoire
```bash
git clean -n                  # Dry-run (aperçu)
git clean -f                  # Supprime les fichiers non trackés
git clean -fd                 # Supprime fichiers et dossiers
git clean -fx                 # Inclut les fichiers ignorés
```

### Revert
```bash
git revert <commit>           # Crée un nouveau commit qui annule
git revert HEAD               # Annule le dernier commit
git revert --no-commit <commit>  # Sans créer de commit
```

## Stash (Remisage)

### Utilisation du stash
```bash
git stash                     # Remise les modifications
git stash save "Message"
git stash -u                  # Inclut les fichiers non trackés
git stash list                # Liste les stashs
git stash show                # Affiche le dernier stash
git stash show -p stash@{0}   # Affiche les détails
git stash apply               # Applique le dernier stash
git stash apply stash@{2}     # Applique un stash spécifique
git stash pop                 # Applique et supprime
git stash drop stash@{0}      # Supprime un stash
git stash clear               # Supprime tous les stashs
git stash branch <branche>    # Crée une branche depuis un stash
```

## Remote (Dépôts Distants)

### Gestion des remotes
```bash
git remote                    # Liste les remotes
git remote -v                 # Avec les URLs
git remote add <nom> <url>
git remote remove <nom>
git remote rename <ancien> <nouveau>
git remote show <nom>
git remote set-url <nom> <nouvelle-url>
```

### Synchronisation
```bash
git fetch                     # Récupère les modifications
git fetch <remote>
git fetch --all               # Tous les remotes
git fetch --prune             # Nettoie les branches supprimées
```

### Pull et Push
```bash
git pull                      # Fetch + merge
git pull --rebase             # Fetch + rebase
git pull <remote> <branche>
git push
git push <remote> <branche>
git push -u origin main       # Push et configure le tracking
git push --force              # Force le push (attention: destructif)
git push --force-with-lease   # Force safe (vérifie les changements distants)
git push --all                # Push toutes les branches
git push --tags               # Push tous les tags
git push <remote> --delete <branche>  # Supprime une branche distante
```

## Merge et Rebase

### Merge
```bash
git merge <branche>
git merge --no-ff <branche>   # Force un commit de merge
git merge --squash <branche>  # Combine tous les commits en un
git merge --abort             # Annule le merge en cours
```

### Rebase
```bash
git rebase <branche>
git rebase -i HEAD~3          # Rebase interactif (3 derniers commits)
git rebase --continue         # Continue après résolution
git rebase --skip             # Saute le commit en conflit
git rebase --abort            # Annule le rebase
git rebase --onto <nouvelle-base> <ancienne-base> <branche>
```

### Résolution de conflits
```bash
git status                    # Voir les conflits
git diff                      # Voir les différences
git mergetool                 # Outil de résolution
git add <fichier>             # Marque comme résolu
git merge --continue          # Continue le merge
```

## Tags

### Gestion des tags
```bash
git tag                       # Liste les tags
git tag <nom>                 # Tag léger
git tag -a <nom> -m "Message" # Tag annoté
git tag -a <nom> <commit>     # Tag un commit spécifique
git show <tag>                # Affiche les infos du tag
git tag -d <tag>              # Supprime un tag local
git push origin <tag>         # Push un tag
git push origin --tags        # Push tous les tags
git push origin --delete <tag>  # Supprime un tag distant
git checkout <tag>            # Checkout un tag
```

## Cherry-pick

### Appliquer des commits spécifiques
```bash
git cherry-pick <commit>
git cherry-pick <commit1> <commit2>
git cherry-pick <commit1>..<commit2>
git cherry-pick --continue
git cherry-pick --abort
git cherry-pick --no-commit <commit>  # Sans créer de commit
```

## Bisect (Recherche de Bug)

### Recherche binaire
```bash
git bisect start
git bisect bad                # Marque le commit actuel comme mauvais
git bisect good <commit>      # Marque un commit comme bon
git bisect reset              # Termine la recherche
git bisect run <script>       # Automatise avec un script
```

## Submodules

### Gestion des sous-modules
```bash
git submodule add <url> <chemin>
git submodule init
git submodule update
git submodule update --init --recursive
git submodule foreach git pull origin main
git clone --recurse-submodules <url>
git submodule deinit <chemin>
git submodule status
```

## Worktree (Git 2.5+)

### Travailler sur plusieurs branches simultanément
```bash
git worktree add <chemin> <branche>
git worktree add ../hotfix hotfix-branch
git worktree list
git worktree remove <chemin>
git worktree prune
```

## Reflog

### Historique des références
```bash
git reflog                    # Historique complet
git reflog show <branche>
git reset --hard HEAD@{2}     # Restaure depuis reflog
```

## Maintenance et Optimisation

### Maintenance du dépôt
```bash
git gc                        # Garbage collection
git gc --aggressive
git prune                     # Supprime les objets inaccessibles
git fsck                      # Vérifie l'intégrité
git count-objects -v          # Statistiques du dépôt
```

### Optimisation
```bash
git repack -a -d              # Réempaquetage
git verify-pack -v <pack>     # Vérifie un pack
```

## Blame et Show

### Traçabilité du code
```bash
git blame <fichier>           # Qui a modifié chaque ligne
git blame -L 10,20 <fichier>  # Lignes spécifiques
git blame -C <fichier>        # Détecte les copies
git show <commit>             # Affiche un commit
git show <commit>:<fichier>   # Contenu d'un fichier à un commit
git show HEAD~3:README.md
```

## Nouveautés Récentes

### Git 2.42+ (2023)
```bash
git rebase --update-refs      # Met à jour automatiquement les refs pendant rebase
git commit --trailer "Key: value"  # Ajoute des trailers structurés
```

### Git 2.43+ (2023)
```bash
git rev-list --disk-usage     # Affiche l'utilisation disque
git maintenance start         # Active la maintenance automatique en arrière-plan
git maintenance stop
```

### Git 2.44+ (2024)
```bash
git diff --merge-base         # Compare avec la base de merge commune
git push --force-if-includes  # Sécurité supplémentaire pour force push
```

### Git 2.45+ (2024)
```bash
git switch --orphan <branche> # Crée une branche orpheline (sans historique)
git commit --pathspec-from-file=<fichier>  # Commit depuis une liste de fichiers
```

## .gitignore

### Patterns courants
```bash
# Fichiers
*.log
*.tmp
.DS_Store

# Dossiers
node_modules/
dist/
build/

# Exceptions
!important.log

# Patterns avancés
**/logs/*.log
docs/**/*.pdf
```

### Commandes utiles
```bash
git check-ignore -v <fichier>  # Vérifie quelle règle ignore un fichier
git rm --cached <fichier>      # Arrête de tracker un fichier
```

## .gitattributes

### Configuration des attributs
```bash
# Gestion des fins de ligne
* text=auto
*.txt text
*.jpg binary

# Diff personnalisés
*.json diff=json
*.md diff=markdown

# Merge strategies
package-lock.json merge=ours
```

## Astuces et Bonnes Pratiques

### Alias utiles
```bash
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
git config --global alias.undo "reset --soft HEAD^"
git config --global alias.amend "commit --amend --no-edit"
```

### Commandes combinées
```bash
git fetch && git rebase origin/main
git add -A && git commit -m "Message" && git push
git stash && git pull && git stash pop
```

### Recherche avancée
```bash
git log --all --full-history -- <fichier>  # Trouve un fichier supprimé
git grep "motif"              # Recherche dans les fichiers trackés
git grep -n "motif"           # Avec numéros de ligne
git log --follow -p -- <fichier>  # Historique complet avec diffs
```

## Hooks

### Hooks courants
```bash
# Emplacements: .git/hooks/
pre-commit          # Avant le commit
prepare-commit-msg  # Avant le message de commit
commit-msg          # Validation du message
post-commit         # Après le commit
pre-push            # Avant le push
post-merge          # Après un merge
```

## Dépannage

### Problèmes courants
```bash
# Annuler un push
git revert <commit>
git push

# Récupérer un commit perdu
git reflog
git cherry-pick <commit>

# Synchroniser avec le remote après force push
git fetch origin
git reset --hard origin/main

# Changer l'auteur du dernier commit
git commit --amend --author="Nom <email>"

# Supprimer des fichiers sensibles de l'historique
git filter-branch --tree-filter 'rm -f <fichier>' HEAD
# Ou utiliser git-filter-repo (recommandé)
```

### Performance
```bash
# Accélérer les opérations
git config --global core.preloadindex true
git config --global core.fscache true
git config --global gc.auto 256

# Activer la parallélisation
git config --global submodule.fetchJobs 4
```

## Sécurité

### Signature des commits
```bash
git config --global user.signingkey <key-id>
git config --global commit.gpgsign true
git commit -S -m "Message"    # Commit signé
git log --show-signature      # Vérifie les signatures
git verify-commit <commit>
```

### Protection
```bash
# Interdire force push
git config --global receive.denyNonFastForwards true

# Vérification des URLs
git config --global url."https://github.com/".insteadOf git://github.com/
```

---
