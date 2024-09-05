
# FAQ
> Cette page est une liste de questions-réponses maintenue conjointement par les SIG Docs et FAQ, qui recueille largement les questions et solutions de tout le monde. Les contributions sont les bienvenues !

## Installation de paquets apk
- Q : Peut-on installer ses propres paquets apk téléchargés ?
- R : Oui, après avoir téléchargé l'apk localement, faites un clic droit et choisissez "Ouvrir avec" puis sélectionnez l'installateur.

## Comment mettre à jour openKylin
- Q : Comment mettre à jour openKylin ?
- R : Vous pouvez exécuter les commandes suivantes :
```
sudo apt update
sudo apt full-upgrade
```

## Vérification rapide de la présence d'USB 3.0 sur l'ordinateur
- Q : De plus en plus d'ordinateurs sont équipés de ports USB 3.0, mais comment savoir si votre ordinateur en a et lequel est un port 3.0 ?
- R : Ouvrez un terminal et utilisez la commande suivante :
```
lsusb
```
Cette commande affiche les informations relatives à l'USB sur le système. Vérifiez les résultats, si vous voyez "3.0 root hub", cela indique que le système dispose d'USB 3.0.
```
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
```
- Q : Comment identifier quel port est USB 3.0 ?
- R : Généralement, les ports USB 3.0 sont marqués SS (abréviation de Super Speed). Si le fabricant n'a pas marqué SS ou USB 3, vérifiez la couleur à l'intérieur du port, l'USB 3.0 est généralement bleu.

Dans les systèmes Linux, consulter les informations matérielles est une tâche courante qui peut vous aider à comprendre la configuration du système, à diagnostiquer les problèmes ou à optimiser les performances. Voici quelques commandes Linux courantes pour consulter diverses informations matérielles :

## Comment consulter les informations du CPU
- Q : Comment voir les informations du CPU de l'ordinateur ?
- R : Généralement, vous pouvez voir les informations du modèle de CPU dans Paramètres - À propos. Si vous avez besoin de voir des informations détaillées sur le CPU, vous pouvez utiliser la commande `lscpu` ou `cat /proc/cpuinfo` pour voir l'architecture du CPU (comme x86_64), le modèle, le nombre de cœurs, le nombre de threads, la fréquence CPU de chaque cœur, le cache, les jeux d'instructions pris en charge, etc.

## Comment consulter les informations de mémoire
- Q : Comment voir les informations de mémoire de l'ordinateur ?
- R : Vous pouvez utiliser la commande `cat /proc/meminfo` pour voir les informations de mémoire, y compris la mémoire totale, la mémoire disponible, l'utilisation du cache, etc. MemTotal est la capacité totale de la mémoire. Si vous avez besoin de plus d'informations, vous pouvez utiliser la commande `sudo lshw -class memory` pour voir des informations détaillées. Voici un exemple de sortie.
```bash
[Exemple de sortie omis pour la brièveté]
```

## Comment consulter les informations du disque dur Linux
- Q : Comment voir la taille, le type et les informations matérielles du disque dur Linux
- R : Vous pouvez utiliser la commande `sudo lshw -class disk` pour afficher des informations détaillées sur le disque dur, comme la description, le type de produit, le fournisseur, les informations de bus, la version et la taille.
 Vous pouvez utiliser `lsblk` pour voir le type de système de fichiers dans Linux.
 Vous pouvez utiliser `sudo fdisk -l` pour voir le type et la taille du disque, le modèle du disque, la taille des secteurs et d'autres informations supplémentaires.
 Vous pouvez utiliser `hwinfo --disk` pour voir les informations matérielles du disque dur dans le système Linux.