# CLI CheatSheet

## Table des matières

1. [Le Terminal Linux](#le-terminal-linux)
   - [Obtenir de l'aide sous Linux](#obtenir-de-laide-sous-linux)
   - [Pages MAN](#pages-man)
   - [Vérifier le type de commande](#vérifier-le-type-de-commande)
   - [Obtenir de l'aide pour les commandes intégrées au shell](#obtenir-de-laide-pour-les-commandes-intégrées-au-shell)
   - [Rechercher dans les pages man](#rechercher-dans-les-pages-man)
   - [Raccourcis clavier](#raccourcis-clavier)
   - [Historique Bash](#historique-bash)
   - [Obtenir l'accès root (sudo, su)](#obtenir-laccès-root-sudo-su)

2. [Chemins Linux](#chemins-linux)
   - [Chemins](#chemins)
   - [Changer de répertoire](#changer-de-répertoire)
   - [Installer des outils](#installer-des-outils)
   - [Utiliser Tree](#utiliser-tree)

3. [La commande ls](#la-commande-ls)
   - [Lister les répertoires](#lister-les-répertoires)
   - [Options](#options)
   - [Utilisation du disque](#utilisation-du-disque)
   - [Horodatage des fichiers et date](#horodatage-des-fichiers-et-date)
   - [Modifier les horodatages avec Touch](#modifier-les-horodatages-avec-touch)
   - [Date et calendrier](#date-et-calendrier)
   - [Tri avec ls](#tri-avec-ls)

4. [Visualiser les fichiers (cat, less, more, head, tail, watch)](#visualiser-les-fichiers-cat-less-more-head-tail-watch)
   - [Afficher le contenu des fichiers](#afficher-le-contenu-des-fichiers)
   - [Raccourcis Less](#raccourcis-less)
   - [Tail et Head](#tail-et-head)
   - [Commandes de surveillance](#commandes-de-surveillance)

5. [Travailler avec les fichiers et répertoires](#travailler-avec-les-fichiers-et-répertoires)
   - [Créer et mettre à jour des fichiers](#créer-et-mettre-à-jour-des-fichiers)
   - [Créer des répertoires](#créer-des-répertoires)
   - [La commande cp](#la-commande-cp)
   - [La commande mv](#la-commande-mv)
   - [La commande rm](#la-commande-rm)
   - [Suppression sécurisée de fichiers](#suppression-sécurisée-de-fichiers)

6. [Tubes et redirection de commandes](#tubes-et-redirection-de-commandes)
   - [Exemples de tubes](#exemples-de-tubes)
   - [Redirection de commandes](#redirection-de-commandes)

7. [Trouver des fichiers avec locate et find](#trouver-des-fichiers-avec-locate-et-find)
   - [locate](#locate)
   - [find](#find)

8. [Rechercher des motifs de texte avec grep](#rechercher-des-motifs-de-texte-avec-grep)
   - [Options](#options-1)
   - [Extraire les caractères ASCII des fichiers binaires](#extraire-les-caractères-ascii-des-fichiers-binaires)

9. [VIM - Éditeur de texte](#vim---éditeur-de-texte)
   - [Modes](#modes)
   - [Fichier de configuration](#fichier-de-configuration)
   - [Commandes](#commandes)
   - [Navigation](#navigation)

10. [Gestion des comptes](#gestion-des-comptes)
    - [Surveillance des utilisateurs](#surveillance-des-utilisateurs)

11. [Permissions de fichiers](#permissions-de-fichiers)
    - [Comprendre les permissions](#comprendre-les-permissions)
    - [Afficher les permissions](#afficher-les-permissions)
    - [Changer les permissions](#changer-les-permissions)
    - [Mode absolu](#mode-absolu)
    - [Permissions spéciales](#permissions-spéciales)
    - [UMASK](#umask)
    - [Propriété](#propriété)
    - [Attributs de fichier](#attributs-de-fichier)

12. [Processus](#processus)
    - [Visualisation des processus](#visualisation-des-processus)
    - [Vue dynamique en temps réel](#vue-dynamique-en-temps-réel)
    - [Tuer des processus](#tuer-des-processus)
    - [Gestion arrière-plan et premier plan](#gestion-arrière-plan-et-premier-plan)

13. [Réseau](#réseau)
    - [Obtenir les informations d'interface réseau](#obtenir-les-informations-dinterface-réseau)
    - [Configurer les interfaces réseau](#configurer-les-interfaces-réseau)
    - [Netplan pour la configuration réseau statique sur Ubuntu](#netplan-pour-la-configuration-réseau-statique-sur-ubuntu)

14. [Configuration et gestion OpenSSH](#configuration-et-gestion-openssh)
    - [Installation](#installation)
    - [Connexion serveur](#connexion-serveur)
    - [Configuration de sécurité](#configuration-de-sécurité)

15. [Techniques de transfert de fichiers avec SCP et RSYNC](#techniques-de-transfert-de-fichiers-avec-scp-et-rsync)
    - [Utilisation SCP](#utilisation-scp)
    - [Commandes RSYNC](#commandes-rsync)
    - [Exemple de motifs d'exclusion](#exemple-de-motifs-dexclusion)
    - [WGET pour le téléchargement de fichiers](#wget-pour-le-téléchargement-de-fichiers)

16. [Utilisation de NETSTAT et SS](#utilisation-de-netstat-et-ss)

17. [Commandes LSOF](#commandes-lsof)

18. [Guide de scan Nmap](#guide-de-scan-nmap)

19. [Gestion logicielle avec DPKG et APT](#gestion-logicielle-avec-dpkg-et-apt)
    - [DPKG](#dpkg)
    - [APT](#apt)

20. [Planification de tâches avec Cron](#planification-de-tâches-avec-cron)

21. [Obtenir les informations matérielles du système](#obtenir-les-informations-matérielles-du-système)
    - [Matériel général](#matériel-général)
    - [Informations CPU](#informations-cpu)
    - [Informations mémoire](#informations-mémoire)
    - [Périphériques PCI et USB](#périphériques-pci-et-usb)
    - [Périphériques de stockage](#périphériques-de-stockage)
    - [Périphériques réseau](#périphériques-réseau)
    - [Informations système via /proc](#informations-système-via-proc)
    - [Batterie](#batterie)
    - [Travailler avec les fichiers de périphérique (dd)](#travailler-avec-les-fichiers-de-périphérique-dd)

22. [Gestion des services](#gestion-des-services)
    - [Ubuntu](#ubuntu)
    - [CentOS](#centos)

23. [Configuration de sécurité](#configuration-de-sécurité-1)

24. [Programmation Bash](#programmation-bash)
    - [Alias Bash](#alias-bash)
    - [Alias utiles](#alias-utiles)
    - [Manipulation de fichiers interactive](#manipulation-de-fichiers-interactive)
    - [Variables Bash](#variables-bash)
    - [Variables spéciales](#variables-spéciales)
    - [Contrôle de flux du programme](#contrôle-de-flux-du-programme)
    - [Conditions de test](#conditions-de-test)
    - [Boucles et fonctions](#boucles-et-fonctions)
    - [Exemples de commandes](#exemples-de-commandes)

---

## Le Terminal Linux

### Obtenir de l'aide sous Linux

### Pages MAN

- Utilisez `man command` pour afficher le manuel d'une commande.
    - **Exemple** : `man ls`

Les pages man sont parcourues avec la commande `less` et ses raccourcis :

- `h` : Obtenir de l'aide dans `less`.
- `q` : Quitter `less`.
- `enter` : Afficher la ligne suivante.
- `space` : Afficher l'écran suivant.
- `/string` : Rechercher une chaîne vers l'avant.
- `?string` : Rechercher une chaîne vers l'arrière.
- `n` / `N` : Occurrence suivante/précédente du terme recherché.

### Vérifier le type de commande

- `type rm` : Vérifier si `rm` est une commande intégrée au shell ou un fichier exécutable.
    - **Exemple** : `rm is /usr/bin/rm`
- `type cd` : Vérifier si `cd` est une commande intégrée au shell.
    - **Exemple** : `cd is a shell builtin`

### Obtenir de l'aide pour les commandes intégrées au shell

- `help command` : Obtenir de l'aide pour les commandes intégrées au shell.
    - **Exemple** : `help cd`
- `command --help` : Obtenir de l'aide pour les commandes exécutables.
    - **Exemple** : `rm --help`

### Rechercher dans les pages man

- `man -k uname` : Rechercher `uname` dans toutes les pages man.
- `man -k "copy files"` : Rechercher "copy files" dans les pages man.
- `apropos passwd` : Rechercher les pages man liées à `passwd`.

### Raccourcis clavier

```
TAB TAB: Afficher toutes les commandes ou noms de fichiers commençant par les lettres saisies.
CTRL + L: Effacer la ligne courante.
CTRL + D: Fermer le shell.
CTRL + U: Couper la ligne courante.
CTRL + A: Déplacer le curseur au début de la ligne.
Ctrl + E: Déplacer le curseur à la fin de la ligne.
CTRL + C: Arrêter la commande courante.
CTRL + Z: Mettre en veille le programme en cours d'exécution.
CTRL + ALT + T: Ouvrir un terminal.
```

### Historique Bash

- `history` : Afficher l'historique.
- `history -d 100` : Supprimer une ligne spécifique de l'historique.
- `history -c` : Effacer tout l'historique.
- `echo $HISTFILESIZE` : Afficher le nombre de commandes sauvegardées dans le fichier d'historique.
- `echo $HISTSIZE` : Afficher le nombre de commandes d'historique sauvegardées en mémoire.
- `!!` : Réexécuter la dernière commande.
- `!20` : Exécuter une commande spécifique de l'historique.
- `!-10` : Exécuter la nième dernière commande.
- `!abc` : Exécuter la dernière commande commençant par `abc`.
- `!abc:p` : Afficher la dernière commande commençant par `abc`.
- `CTRL + R` : Recherche inversée dans l'historique.
- `HISTTIMEFORMAT="%d/%m/%y %T "` : Enregistrer date/heure de chaque commande (ajouter à `~/.bashrc` pour la persistance).

### Obtenir l'accès root (sudo, su)

- `sudo command` : Exécuter une commande en tant que root (pour les utilisateurs du groupe `sudo` ou `wheel`).
- `sudo su` : Devenir root temporairement.
- `sudo passwd root` : Définir le mot de passe root.
- `passwd username` : Changer le mot de passe d'un utilisateur.
- `su` : Devenir root (si root a un mot de passe).

## Chemins Linux

### Chemins

- **Absolu** : Commence par `/`
- **Relatif** : Relatif à l'emplacement actuel
- `.` : Répertoire de travail actuel
- `..` : Répertoire parent
- `~` : Répertoire personnel de l'utilisateur

### Changer de répertoire

- `cd` : Vers le répertoire personnel de l'utilisateur
- `cd ~` : Vers le répertoire personnel de l'utilisateur
- `cd -` : Vers le dernier répertoire
- `cd /path_to_dir` : Vers `path_to_dir`
- `pwd` : Afficher le répertoire de travail actuel

### Installer des outils

- `sudo apt install tree` : Installe la commande `tree`

### Utiliser Tree

- `tree directory/` : Exemple : `tree .`
- `tree -d .` : Afficher uniquement les répertoires
- `tree -f .` : Afficher les chemins absolus

## La commande ls

Utilisation : `ls [OPTIONS] [FICHIERS]`

### Lister les répertoires

- `ls`, `ls .` : Répertoire actuel
- `ls ~ /var /` : Plusieurs répertoires

### Options

- `-l` : Liste détaillée
- `-a` : Tous les fichiers (y compris cachés)
- `-1` : Colonne simple
- `-d` : Informations du répertoire
- `-h` : Tailles lisibles
- `-S` : Trier par taille
- `-X` : Trier par extension
- `--hide` : Masquer des fichiers spécifiques
- `-R` : Liste récursive
- `-i` : Numéro d'inode

### Utilisation du disque

- `du -sh ~` : Taille du répertoire personnel

### Horodatage des fichiers et date

- `ls -lu` : Heure d'accès (`atime`)
- `ls -l`, `ls -lt` : Heure de modification (`mtime`)
- `ls -lc` : Heure de changement (`ctime`)
- `stat file.txt` : Tous les horodatages
- `ls -l --full-time /etc/` : Horodatages complets

### Modifier les horodatages avec Touch

- `touch file.txt` : Créer ou mettre à jour les horodatages
- `touch -a file`, `touch -m file` : Modifier `atime` ou `mtime`
- `touch -m -t 201812301530.45 a.txt` : Date/heure spécifique
- `touch -d "2010-10-31 15:45:30" a.txt` : `atime` et `mtime`
- `touch a.txt -r b.txt` : Copier les horodatages

### Date et calendrier

- `date` : Date/heure actuelle
- `cal`, `cal 2021`, `cal 7 2021` : Calendriers
- `cal -3` : Mois précédent, actuel, suivant
- `date --set="2 OCT 2020 18:00:00"` : Définir date/heure

### Tri avec ls

- `ls -l` : Trié par nom
- `ls -lt` : Trié par `mtime`, le plus récent d'abord
- `ls -ltu` : Trié par `atime`
- `ls -ltu --reverse` : Ordre inverse

## Visualiser les fichiers (cat, less, more, head, tail, watch)

### Afficher le contenu des fichiers

- `cat filename` : Afficher le contenu
- `cat -n filename` : Numéros de ligne
- `cat filename1 filename2 > filename3` : Concaténer

### Raccourcis Less

- `h` : Aide
- `q` : Quitter
- `enter` : Ligne suivante
- `space` : Écran suivant
- `/string` : Rechercher vers l'avant
- `?string` : Rechercher vers l'arrière
- `n` / `N` : Résultat de recherche suivant/précédent

### Tail et Head

- `tail filename` : Dernières 10 lignes
- `tail -n 15 filename` : Dernières 15 lignes
- `tail -n +5 filename` : À partir de la ligne 5
- `tail -f filename` : Mises à jour en temps réel
- `head filename` : Premières 10 lignes
- `head -n 15 filename` : Premières 15 lignes

### Commandes de surveillance

- `watch -n 3 ls -l` : Actualiser toutes les 3 secondes

## Travailler avec les fichiers et répertoires

### Créer et mettre à jour des fichiers

- `touch filename` : Créer un nouveau fichier ou mettre à jour les horodatages.

### Créer des répertoires

- `mkdir dir1` : Créer un nouveau répertoire.
- `mkdir -p mydir1/mydir2/mydir3` : Créer des répertoires imbriqués.

### La commande cp

Copier fichiers et répertoires :

- `cp file1 file2` : Copier `file1` vers `file2`.
- `cp file1 dir1/file2` : Copier vers un autre répertoire avec un nom différent.
- `cp -i file1 file2` : Demander avant d'écraser.
- `cp -p file1 file2` : Préserver les permissions.
- `cp -v file1 file2` : Sortie verbeuse.
- `cp -r dir1 dir2/` : Copier récursivement les répertoires.
- `cp -r file1 file2 dir1 dir2 dest_dir/` : Copier plusieurs éléments vers une destination.

### La commande mv

Déplacer ou renommer fichiers et répertoires :

- `mv file1 file2` : Renommer un fichier.
- `mv file1 dir1/` : Déplacer vers un répertoire.
- `mv -i file1 dir1/` : Demander avant d'écraser.
- `mv -n file1 dir1/` : Empêcher l'écrasement.
- `mv -u file1 dir1/` : Mettre à jour selon l'heure de modification.
- `mv file1 dir1/file2` : Déplacer et renommer.
- `mv file1 file2 dir1/ dir2/ dest_dir/` : Déplacer plusieurs éléments.

### La commande rm

Supprimer fichiers et répertoires :

- `rm file1` : Supprimer un fichier.
- `rm -v file1` : Suppression verbeuse.
- `rm -r dir1/` : Supprimer un répertoire.
- `rm -rf dir1/` : Suppression forcée sans demande.
- `rm -ri file1 dir1/` : Demander pour chaque suppression.

### Suppression sécurisée de fichiers

- `shred -vu -n 100 file1` : Écraser et supprimer un fichier de manière sécurisée.

## Tubes et redirection de commandes

### Exemples de tubes

- `ls -lSh /etc/ | head` : Voir les 10 plus gros fichiers.
- `ps -ef | grep sshd` : Vérifier si `sshd` fonctionne.
- `ps aux --sort=-%mem | head -n 3` : Top 3 des processus par mémoire.

### Redirection de commandes

Rediriger sortie et erreurs :

- `ps aux > processes.txt` : Sortie vers un fichier.
- `id >> users.txt` : Ajouter la sortie.
- `tail -n 10 /var/log/*.log > output.txt 2> errors.txt` : Séparer sortie et erreurs.
- `tail -n 2 /etc/passwd /etc/shadow > all.txt 2>&1` : Rediriger tout vers un fichier.
- `cat /var/log/auth.log | grep "fail" | wc -l` : Compter les occurrences.

## Trouver des fichiers avec locate et find

### locate

- `sudo apt install plocate` : Installer `plocate`.
- `sudo updatedb` : Mettre à jour la base de données.
- `locate filename` : Trouver un fichier par nom.
- `locate -i filename` : Recherche insensible à la casse.
- `locate -b '\filename'` : Recherche de nom exact.
- `locate -r 'regex'` : Recherche par expression régulière.
- `locate -e filename` : Vérifier l'existence du fichier.
- `which command` : Afficher le chemin de la commande.

### find

Rechercher avec diverses options :

- `find ~ -type f -size +1M` : Fichiers de plus de 1MB.
- Les options incluent `-type`, `-name`, `-iname`, `-size`, `-perm`, `-links`, `-atime`, `-mtime`, `-ctime`, `-user`, et `-group`.

## Rechercher des motifs de texte avec grep

Utilisation : `grep [OPTIONS] MOTIF FICHIER`

### Options

- `-n` : Afficher le numéro de ligne.
- `-i` : Insensible à la casse.
- `-v` : Inverser la correspondance.
- `-w` : Correspondance de mots entiers.
- `-a` : Inclure les fichiers binaires.
- `-R` : Recherche récursive.
- `-c` : Compter les correspondances.
- `-C n` : Affichage contextuel (n lignes autour de la correspondance).

### Extraire les caractères ASCII des fichiers binaires

- `strings binary_file` : Exemple `strings /bin/ls`.

## VIM - Éditeur de texte

### Modes

- Mode Commande : Par défaut à l'entrée.
- Mode Insertion : Édition de texte.
- Mode Dernière ligne : Commandes de sauvegarde/sortie.

### Fichier de configuration

- Paramètres VIM : `~/.vimrc`.

### Commandes

- `i`, `I`, `a`, `A`, `o` : Entrer en mode Insertion.
- `:w!`, `:q!`, `:wq!`, `:e!` : Commandes de sauvegarde/sortie en mode Dernière ligne.
- `x`, `dd`, `ZZ`, `u`, `G`, `$`, `0`, `^` : Commandes d'édition en mode Commande.
- `/string`, `?string`, `n`, `N` : Commandes de recherche en mode Commande.
- `vim -o file1 file2` : Ouvrir les fichiers dans des fenêtres empilées.
- `vim -d file1 file2` : Mettre en évidence les différences.

### Navigation

- `Ctrl+w` : Basculer entre les fichiers.

## Gestion des comptes

```
## Gestion des comptes
/etc/passwd # utilisateurs et infos: 
/etc/shadow # mots de passe des utilisateurs
/etc/group # groupes
## Commandes utilisateur
useradd [OPTIONS] username # Créer utilisateur.
usermod [OPTIONS] username # Modifier utilisateur.
userdel -r username        # Supprimer utilisateur.
## Commandes de groupe
groupadd group_name # Créer groupe.
groupdel group_name # Supprimer groupe.
## Exemples
useradd -m -d /home/john -c "C++ Developer" -s /bin/bash -G sudo,adm,mail john # Exemple de création d'utilisateur.
usermod -aG developers,managers john # Exemple de modification d'utilisateur.
```

### Surveillance des utilisateurs

```
## Commandes
who -H    # Infos utilisateur.
id        # Infos utilisateur.
whoami    # Infos utilisateur.
w         # Utilisation système.
uptime    # Utilisation système.
last      # Historique de connexion.
last -u username # Historique de connexion pour un utilisateur spécifique.
```

## Permissions de fichiers

### Comprendre les permissions

- **Légende** : `u` (utilisateur), `g` (groupe), `o` (autres), `a` (tous), `r` (lecture), `w` (écriture), `x` (exécution), `-` (pas d'accès).

### Afficher les permissions

- `ls -l /etc/passwd` : Voir les permissions de fichier.
- `stat /etc/shadow` : Statistiques détaillées des permissions.

### Changer les permissions

- `chmod u+r filename` : Ajouter lecture à l'utilisateur.
- `chmod u+r,g-wx,o-rwx filename` : Ajuster plusieurs permissions.
- `chmod ug+rwx,o-wx filename` : Définir plusieurs permissions.
- `chmod ugo+x filename` : Ajouter exécution à tous.
- `chmod a+r,a-wx filename` : Modifier toutes les permissions.

### Mode absolu

- `chmod 777 filename` : Définir toutes les permissions pour tous.
- `chmod 755 filename` : Lecture & exécution pour groupe et autres.
- `chmod 644 filename` : Lecture seule pour groupe et autres.

### Permissions spéciales

- **SUID** : `chmod u+s executable_file`.
- **SGID** : `chmod g+s projects/`.
- **Bit collant** : `chmod o+t temp/`.

### UMASK

- Affichage : `umask`.
- Définir nouvelle valeur : `umask new_value`.

### Propriété

- Propriétaire : `chown new_owner file`.
- Groupe : `chgrp new_group file`.
- Les deux : `chown new_owner:new_group file`.
- Récursif : `chown -R new_owner file`.

### Attributs de fichier

- Affichage : `lsattr filename`.
- Changer : `chattr +-attribute filename`.

## Processus

### Visualisation des processus

- `type rm` : Vérifier si `rm` est intégré ou exécutable.
- `ps` : Processus dans le terminal actuel.
- `ps -ef`, `ps aux`, `ps aux | less` : Processus système.
- `ps aux --sort=%mem | less` : Trier par utilisation mémoire.
- `ps -ef --forest` : Arbre des processus ASCII.
- `ps -f -u username` : Processus par utilisateur.
- `pgrep -l sshd`, `pgrep -f sshd`, `ps -ef | grep sshd` : Vérifier `sshd`.
- `pstree`, `pstree -c` : Arbre des processus hiérarchique.

### Vue dynamique en temps réel

- `top` : Démarrer le moniteur système.
- Raccourcis `top` : `h` pour l'aide, `space` pour actualiser, `d` pour le délai, etc.
- `top -d 1 -n 3 -b > top_processes.txt` : Top en mode lot.
- Installer `htop` pour une vue interactive.

### Tuer des processus

- `kill -l` : Lister les signaux.
- `kill pid`, `kill -SIGNAL pid1 pid2 ...` : Envoyer des signaux.
- `kill -2 pid`, `kill -HUP pid` : Envoyer signal spécifique.
- `pkill process_name`, `killall process_name` : Tuer par nom.
- `kill $(pidof process_name)` : Tuer en utilisant `pidof`.

### Gestion arrière-plan et premier plan

- `command &` : Exécuter en arrière-plan.
- `jobs` : Lister les tâches.
- `Ctrl + Z` : Arrêter le processus.
- `fg %job_id` : Reprendre au premier plan.
- `bg %job_id` : Reprendre en arrière-plan.
- `nohup command &` : Immunisé contre les déconnexions.

## Réseau

### Obtenir les informations d'interface réseau

- `ifconfig` : Interfaces activées.
- `ifconfig -a`, `ip address show` : Toutes les interfaces.
- `ifconfig enp0s3`, `ip addr show dev enp0s3` : Interface spécifique.
- `ip -4 address` : Infos IPv4 uniquement.
- `ip -6 address` : Infos IPv6 uniquement.
- `ip link show`, `ip link show dev enp0s3` : Infos L2, y compris MAC.
- `route`, `route -n`, `ip route show` : Passerelle par défaut.
- `systemd-resolve --status` : Serveurs DNS.

### Configurer les interfaces réseau

- `ifconfig enp0s3 down`, `ip link set enp0s3 down` : Désactiver interface.
- `ifconfig enp0s3 up`, `ip link set enp0s3 up` : Activer interface.
- `ifconfig -a`, `ip link show dev enp0s3` : Vérifier le statut.
- `ifconfig enp0s3 192.168.0.222/24 up`, `ip address add 192.168.0.112/24 dev enp0s3` : Définir IP.
- `ifconfig enp0s3:1 10.0.0.1/24` : IP secondaire.
- `route del default gw`, `ip route del default`, `ip route add default via` : Passerelle par défaut.
- `ifconfig enp0s3 hw ether`, `ip link set dev enp0s3 address` : Changer MAC.

### Netplan pour la configuration réseau statique sur Ubuntu

1. Arrêter/Désactiver NetworkManager.
2. Créer/Modifier YAML dans `/etc/netplan`.
3. Appliquer config : `sudo netplan apply`.
4. Vérifier : `ifconfig`, `route -n`.

## Configuration et gestion OpenSSH

### Installation

**Ubuntu**

`sudo apt update && sudo apt install openssh-server openssh-client`

**CentOS**

`sudo dnf install openssh-server openssh-clients`

### Connexion serveur

```
ssh -p 22 username@server_ip  # Se connecter en utilisant le port SSH par défaut
ssh -p 22 -l username server_ip  # Se connecter avec un nom d'utilisateur spécifique
ssh -v -p 22 username@server_ip  # Se connecter en mode verbeux pour des informations détaillées
# Ubuntu
sudo systemctl status ssh  # Vérifier le statut SSH
sudo systemctl stop ssh    # Arrêter le service SSH
sudo systemctl restart ssh # Redémarrer le service SSH
sudo systemctl enable ssh  # Activer SSH au démarrage
sudo systemctl is-enabled ssh  # Vérifier si SSH est activé au démarrage
# CentOS
sudo systemctl status sshd  # Vérifier le statut SSH
sudo systemctl stop sshd    # Arrêter le service SSH
sudo systemctl restart sshd # Redémarrer le service SSH
sudo systemctl enable sshd  # Activer SSH au démarrage
sudo systemctl is-enabled sshd  # Vérifier si SSH est activé au démarrage
```

### Configuration de sécurité

Éditer /etc/ssh/sshd_config puis appliquer les modifications en redémarrant SSH :

- Changer le port : Port 2278
- Désactiver la connexion root : PermitRootLogin no
- Restreindre l'accès utilisateur : AllowUsers user1 user2
- Configurer le pare-feu pour filtrer l'accès SSH
- Activer l'authentification par clé publique, désactiver la connexion par mot de passe
- Utiliser uniquement le protocole SSH 2
- Définir les intervalles de session client et les tentatives max pour la sécurité

N'oubliez pas de consulter la page man (`man sshd_config`) pour les options de configuration détaillées.

## Techniques de transfert de fichiers avec SCP et RSYNC

### Utilisation SCP

```
# Copier un fichier local vers un hôte distant
scp a.txt john@80.0.0.1:~
scp -P 2288 a.txt john@80.0.0.1:~ # Port personnalisé
# Copier du distant vers le local
scp -P 2290 john@80.0.0.1:~/a.txt .
# Copier tout un répertoire vers le distant
scp -P 2290 -r projects/ john@80.0.0.1:~
```

### Commandes RSYNC

```
# Synchroniser répertoire local vers sauvegarde locale
sudo rsync -av /etc/ ~/etc-backup/
# Mettre en miroir le répertoire, supprimer les fichiers superflus de la destination
sudo rsync -av --delete /etc/ ~/etc-backup/
# Exclure des fichiers pendant la sync
rsync -av --exclude-from='~/exclude.txt' /source/ /dest/
# Synchroniser via SSH avec port personnalisé
sudo rsync -av -e 'ssh -p 2267' /etc/ student@192.168.0.108:~/etc-backup/
```

### Exemple de motifs d'exclusion

```
# exclude.txt pourrait inclure des motifs comme :
*.avi
music/
abc.mkv
# Exclure des types de fichiers spécifiques pendant le transfert
rsync -av --exclude='*.mkv' /source/ /dest/
```

### WGET pour le téléchargement de fichiers

```
# Installer wget
sudo apt install wget # Ubuntu
sudo dnf install wget # CentOS
# Téléchargement de fichier basique
wget https://example.com/file.iso
# Reprendre un téléchargement incomplet
wget -c https://example.com/file.iso
# Télécharger avec limitation de bande passante
wget --limit-rate=100k https://example.com/file.iso
# Télécharger plusieurs fichiers
wget -i urls.txt # urls.txt contient une liste d'URLs
# Téléchargement récursif pour visualisation hors ligne d'un site web
wget -mkEpnp http://example.org
```

Utilisez ces commandes pour copier efficacement fichiers et répertoires entre systèmes et pour télécharger du contenu depuis internet, assurant la synchronisation des données et maintenant l'accessibilité web.

## Utilisation de NETSTAT et SS

```
# Afficher tous les ports et connexions
sudo netstat -tupan
sudo ss -tupan
# Vérifier si le port 80 est ouvert
netstat -tupan | grep :80
```

## Commandes LSOF

```
# Lister les fichiers ouverts
lsof
# Fichiers ouverts par un utilisateur spécifique
lsof -u username
# Fichiers ouverts par une commande/processus spécifique
lsof -c sshd
# Fichiers ouverts pour les ports TCP en état LISTEN
lsof -iTCP -sTCP:LISTEN
lsof -iTCP -sTCP:LISTEN -nP
```

Utilisez ces commandes pour surveiller les connexions réseau, vérifier les ports ouverts, et voir les fichiers ouverts par utilisateurs ou processus, spécialement pour la sécurité et le dépannage.

## Guide de scan Nmap

```
# Scan SYN (root requis)
nmap -sS 192.168.0.1
# Scan TCP Connect
nmap -sT 192.168.0.1
# Scanner tous les ports
nmap -p- 192.168.0.1
# Scanner des ports spécifiques
nmap -p 20,22-100,443,1000-2000 192.168.0.1
# Détection de version de service
nmap -p 22,80 -sV 192.168.0.1
# Ping scan réseau
nmap -sP 192.168.0.0/24
# Ignorer la découverte d'hôte
nmap -Pn 192.168.0.0/24
# Exclure IP spécifique du scan
nmap -sS 192.168.0.0/24 --exclude 192.168.0.10
# Sortir le scan vers un fichier
nmap -oN output.txt 192.168.0.1
# Détection OS
nmap -O 192.168.0.1
# Scan agressif
nmap -A 192.168.0.1
# Lire les cibles depuis un fichier & sortir vers fichier sans résolution DNS
nmap -n -iL hosts.txt -p 80 -oN output.txt
```

Ne scannez que vos propres réseaux et systèmes, ou ceux pour lesquels vous avez une autorisation explicite. Le scan non autorisé peut être illégal.

## Gestion logicielle avec DPKG et APT

### DPKG

- Voir infos fichier .deb : `dpkg --info package.deb`
- Installer depuis .deb : `sudo dpkg -i package.deb`
- Lister programmes installés : `dpkg --get-selections` ou `dpkg-query -l`
- Trouver par nom : `dpkg-query -l | grep ssh`
- Lister fichiers du paquet : `dpkg -L openssh-server`
- Trouver paquet propriétaire : `dpkg -S /bin/ls`
- Supprimer paquet : `sudo dpkg -r package`
- Purger paquet : `sudo dpkg -P package`

### APT

- Mettre à jour l'index : `sudo apt update`
- Installer/mettre à jour : `sudo apt install apache2`
- Lister les mises à jour : `sudo apt list --upgradable`
- Mise à jour complète : `sudo apt full-upgrade`
- Supprimer : `sudo apt remove package`
- Purger : `sudo apt purge package`
- Suppression auto des dépendances : `sudo apt autoremove`
- Nettoyer cache : `sudo apt clean`
- Lister tous les paquets : `sudo apt list`
- Rechercher : `sudo apt list | grep nginx`
- Afficher infos paquet : `sudo apt show nginx`
- Lister installés : `sudo apt list --installed`

## Planification de tâches avec Cron

```
crontab -e # Éditer crontab
crontab -l # Lister tâches
crontab -r # Supprimer tâches
# Format de planification :
* * * * * command # Chaque minute
15 * * * * command # Chaque heure
30 18 * * * command # Quotidien
3 22 * * 1 command # Hebdomadaire
10 6 1 * * command # Mensuel
@yearly command # Annuel
@reboot command # Au redémarrage
```

## Obtenir les informations matérielles du système

### Matériel général

```
lshw               # Infos matériel complètes
lshw -short        # Format court
lshw -json         # Format JSON
lshw -html         # Format HTML
```

### Informations CPU

```
lscpu              # Détails CPU
lshw -C cpu        # Détails CPU spécifiques au matériel
lscpu -J           # Format JSON
```

### Informations mémoire

```
dmidecode -t memory           # Spécifications RAM
dmidecode -t memory | grep -i size
dmidecode -t memory | grep -i max
free -m                       # Utilisation mémoire
```

### Périphériques PCI et USB

```
lspci                         # Bus PCI et périphériques connectés
lspci | grep -i wireless
lspci | grep -i vga
lsusb                         # Contrôleurs USB et périphériques
lsusb -v                      # Sortie verbeuse
```

### Périphériques de stockage

```
lshw -short -C disk
fdisk -l                      # Lister disques
fdisk -l /dev/sda
lsblk                         # Liste des périphériques de bloc
```

### Périphériques réseau

```
lshw -C network
iw list                       # Cartes Wi-Fi
iwconfig                      # Configuration Wi-Fi
iwlist scan                   # Scan réseaux Wi-Fi
```

### Informations système via /proc

```
cat /proc/cpuinfo             # Info CPU
cat /proc/meminfo             # Info mémoire
cat /proc/version             # Version système
uname -r                      # Version noyau
uname -a                      # Toutes infos système
```

### Batterie

```
acpi -bi                      # Info batterie
acpi -V                       # Toutes infos ACPI
```

### Travailler avec les fichiers de périphérique (dd)

```
# Sauvegarder MBR
dd if=/dev/sda of=~/mbr.dat bs=512 count=1
# Restaurer MBR
dd if=~/mbr.dat of=/dev/sda bs=512 count=1
# Cloner partition
dd if=/dev/sda1 of=/dev/sdb2 bs=4M status=progress
```

Utilisez ces commandes pour vérifier les spécifications matérielles et effectuer des opérations avec les fichiers de périphérique en toute sécurité.

## Gestion des services

```
# Analyser le processus de démarrage
systemd-analyze
systemd-analyze blame
# Lister unités actives
systemctl list-units
systemctl list-units | grep ssh
# Statut service
sudo systemctl status nginx.service
# Arrêter service
sudo systemctl stop nginx
# Démarrer service
sudo systemctl start nginx
# Redémarrer service
sudo systemctl restart nginx
# Recharger config service
sudo systemctl reload nginx
sudo systemctl reload-or-restart nginx
# Activer service au démarrage
sudo systemctl enable nginx
# Désactiver service au démarrage
sudo systemctl disable nginx
# Vérifier si service est activé au démarrage
sudo systemctl is-enabled nginx
# Masquer service
sudo systemctl mask nginx
# Démasquer service
sudo systemctl unmask nginx
```

### Ubuntu

```
sudo systemctl status ssh       # Vérifier statut service SSH
sudo systemctl stop ssh         # Arrêter service SSH
sudo systemctl restart ssh      # Redémarrer service SSH
sudo systemctl enable ssh       # Activer SSH au démarrage
sudo systemctl is-enabled ssh   # Vérifier si SSH est activé au démarrage
```

### CentOS

```
sudo systemctl status sshd       # Vérifier statut service SSHD
sudo systemctl stop sshd         # Arrêter service SSHD
sudo systemctl restart sshd      # Redémarrer service SSHD
sudo systemctl enable sshd       # Activer SSHD au démarrage
sudo systemctl is-enabled sshd   # Vérifier si SSHD est activé au démarrage
```

## Configuration de sécurité

Pour configurer les paramètres de sécurité, éditez `/etc/ssh/sshd_config`. Appliquez les modifications en redémarrant SSH. Les configurations clés incluent :

- Changer le port SSH :
    - `Port 2278`
- Désactiver la connexion root :
    - `PermitRootLogin no`
- Restreindre l'accès utilisateur aux utilisateurs spécifiés uniquement :
    - `AllowUsers user1 user2`
- Configurer pare-feu pour filtrer l'accès SSH
- Activer l'authentification par clé publique et désactiver la connexion par mot de passe
- Utiliser uniquement le protocole SSH 2
- Définir les intervalles de session client et tentatives maximales pour une sécurité accrue

**Note** : Consultez la page man (`man sshd_config`) pour les options de configuration détaillées.

## Programmation Bash

### Alias Bash

```
alias                     # Lister tous les alias
alias name='command'      # Créer un alias
unalias name              # Supprimer un alias
```

### Alias utiles

```
alias c='clear'
alias cl='clear; ls; pwd'
alias root='sudo su'
alias ports='netstat -tupan'
alias sshconfig='sudo vim /etc/ssh/sshd_config'
alias update='sudo apt update && sudo apt dist-upgrade -y && sudo apt clean'
```

### Manipulation de fichiers interactive

```
alias cp='cp -i'
alias mv='mv -i'
alias rm='rm -i'
```

### Variables Bash

```
variable="value"          # Définir une variable
echo $variable            # Référencer une variable
declare -r const=100      # Définir une variable en lecture seule
unset variable            # Désaffecter une variable
env | grep PATH           # Trouver une variable d'environnement
export PATH=$PATH:~/bin   # Modifier la variable PATH
```

### Variables spéciales

```
$0, $1, $2, ..., ${10}    # Nom du script & arguments positionnels
$#                        # Nombre d'arguments positionnels
"$*"                      # Tous arguments positionnels comme chaîne unique
$?                        # Statut de sortie de la dernière commande
```

### Contrôle de flux du programme

```
if [ condition ]; then command; fi                          # Instruction if basique
if [ condition ]; then command; else other_command; fi       # Instruction if-else
if [ condition ]; then command; elif [ condition ]; then...  # Instruction if-elif-else
```

### Conditions de test

```
# Comparaisons numériques : -eq, -ne, -lt, -le, -gt, -ge
# Vérifications de fichier : -s, -f, -d, -x, -w, -r
# Comparaisons de chaînes : =, !=, -n (non zéro), -z (est zéro)
# Opérateurs logiques : && (et), || (ou)
```

### Boucles et fonctions

```
for i in {1..5}; do echo "Boucle $i"; done               # Boucle for
while [ condition ]; do command; done                  # Boucle while
case "$variable" in pattern) command;; esac            # Instruction case
function name() { command; }                           # Définition de fonction
name() { command; }                                    # Syntaxe alternative de fonction
name                                                   # Appeler une fonction
```

### Exemples de commandes

```
crontab -e    # Éditer fichier crontab
crontab -l    # Lister entrées crontab
crontab -r    # Supprimer entrées crontab
```
