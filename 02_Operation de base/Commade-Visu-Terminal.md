

# <center>Résumé des commandes pour visualiser les informations système Linux</center>
#### <center>Auteur : Hua Zai</center>
#### <center>10-03-2023 10:39:00</center>

Les commandes pour visualiser les informations système Linux sont des connaissances de base essentielles pour les débutants Linux. Il est généralement nécessaire de comprendre les informations système avant de commencer le travail de développement, il est donc important de connaître les commandes pertinentes dans Linux.

Voici une classification des commandes couramment utilisées pour interroger les informations système dans diverses distributions Linux, classées selon différentes fonctionnalités, pour votre référence.

## I. Commandes liées au noyau

### (1) Commandes pour voir la version du noyau Linux
1. cat /proc/version
2. uname -a

### (2) Lister les modules du noyau chargés
lsmod

## II. Commandes liées à la version du système

### (1) Commandes pour voir la version du système Linux
1. lsb_release -a
2. cat /etc/redhat-release (Note : uniquement pour les systèmes Linux basés sur Redhat)
3. cat /etc/issue

### (2) Voir la version du système d'exploitation
head -n 1 /etc/issue

### (3) Voir les informations CPU
cat /proc/cpuinfo

### (4) Voir le temps de fonctionnement du système, le nombre d'utilisateurs, la charge
uptime

## III. Commandes pour visualiser les processus et services

### (1) Voir tous les processus
ps -ef

### (2) Lister tous les services système
chkconfig –list

### (3) Lister tous les programmes de service système démarrés
chkconfig –list | grep on

### (4) Afficher en temps réel l'état des processus utilisateur
top

### (5) Rechercher des informations sur un processus par PID
ps -f -p <PID du processus>

### (6) Voir les processus occupant un port
lsof -i:<ID_DU_PORT>

## IV. Commandes liées aux utilisateurs système

### (1) Voir les informations d'un utilisateur spécifique
id <nom d'utilisateur>

### (2) Voir les journaux de connexion des utilisateurs
last

### (3) Voir tous les utilisateurs du système
cut -d: -f1 /etc/passwd

### (4) Voir tous les groupes du système
cut -d: -f1 /etc/group

## V. Commandes pour interroger les périphériques externes

### (1) Lister tous les périphériques PCI
lspci -tv

### (2) Lister tous les périphériques USB
lsusb -tv

### (3) Voir l'état de détection des périphériques IDE au démarrage
dmesg | grep IDE

### (4) Voir le nom de l'ordinateur
hostname

### (5) Voir les informations matérielles
1. lshw
2. lshw -short
3. dmidecode

## VI. Commandes liées aux disques système

### (1) Voir l'utilisation de la mémoire et de la zone d'échange
free -m

### (2) Voir l'utilisation de chaque partition
df -h

### (3) Voir la taille d'un répertoire spécifique
du -sh <nom du répertoire>

### (4) Voir la quantité totale de mémoire
grep MemTotal /proc/meminfo

### (5) Voir la quantité de mémoire libre
grep MemFree /proc/meminfo

### (6) Voir la charge du système, les disques et partitions
cat /proc/loadavg

### (7) Voir l'état des partitions montées
mount | column -t

### (8) Voir toutes les partitions
fdisk -l

### (9) Voir toutes les partitions d'échange
swapon -s

### (10) Voir les paramètres du disque (uniquement pour les périphériques IDE)
hdparm -i /dev/hda

## VII. Interroger les informations réseau

### (1) Voir les propriétés de toutes les interfaces réseau
ifconfig

### (2) Voir les paramètres du pare-feu
iptables -L

### (3) Voir la table de routage
route -n

### (4) Voir tous les ports en écoute
netstat -lntp

### (5) Voir toutes les connexions établies
netstat -antp

### (6) Voir les statistiques réseau
netstat -s

## VIII. Utiliser scp pour copier des fichiers entre différentes machines

### (1) Copier un fichier individuel du local vers une machine distante
scp fichier_local nom_utilisateur_distant@ip_distante:dossier_distant

### (2) Copier un dossier du local vers une machine distante
scp -r dossier_local nom_utilisateur_distant@ip_distante:dossier_distant

### (3) Copier un fichier individuel d'une machine distante vers le local
scp nom_utilisateur_distant@ip_distante:dossier_distant/fichier_distant dossier_local

### (4) Copier un dossier d'une machine distante vers le local
scp -r nom_utilisateur_distant@ip_distante:dossier_distant dossier_local