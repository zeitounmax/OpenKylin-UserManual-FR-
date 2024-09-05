

# Guide Installation pour LicheePi4A 
## Flashage sur eMMC

### Préparation de l'environnement de flashage eMMC
Préparez un câble de données Type-C, généralement fourni dans la boîte de la carte de développement.

L'image openKylin adaptée au LicheePi4A peut être téléchargée via le lien suivant :
> https://www.openkylin.top/downloads

Décompressez le fichier avec la commande suivante :
> tar -xvf openKylin-Embedded-V2.0-Release-licheepi4a-riscv64.tar.xz

Prenez le câble Type-C préparé, maintenez le bouton BOOT sur la carte de développement pour entrer en mode fastboot, connectez le port Type-C de la carte au port USB de l'ordinateur. Vous pouvez relâcher le bouton BOOT une fois que le voyant rouge s'allume.

Un appareil d'architecture X86 est nécessaire.

Sous Linux, utilisez lsusb pour voir l'appareil, il affichera : ID 2345:7654 T-HEAD USB download gadget

Entrez dans le dossier décompressé, qui contient deux versions du fichier uboot. Choisissez l'uboot en fonction de la taille de la mémoire de votre carte de développement et modifiez l'uboot dans le script de flashage thead-image-linux.sh.

Exécutez le script pour flasher l'image :
sudo ./thead-image-linux.sh

Le terminal affichera le journal de flashage. Une fois le flashage terminé, débranchez le câble de données et mettez sous tension pour démarrer.

### Premier démarrage
Après le premier démarrage, il y aura un utilisateur par défaut dans le système. Lorsque l'environnement de bureau démarre, vous pouvez vous connecter pour la première fois au LicheePi4A avec l'utilisateur par défaut. Plus tard, vous pourrez modifier l'utilisateur ou le mot de passe selon vos besoins.

Le nom d'utilisateur/mot de passe par défaut est :
> username : openkylin
> password : openkylin

### Allocation de l'espace restant sur eMMC après le démarrage
> sudo btrfs filesystem resize max /

## Flashage sur carte SD

### Préparation de la carte SD
L'image openKylin adaptée au LicheePi4A peut être téléchargée via le lien suivant :
https://www.openkylin.top/downloads

Décompressez avec la commande suivante :
> tar -xvf openKylin-2.0-licheepi4a-riscv64.tar.xz

Entrez dans le dossier décompressé.

### Création d'un disque de démarrage SD
Tout d'abord, utilisez l'outil de disque pour formater la carte SD. Ensuite, flashez l'image sur la carte SD via la ligne de commande en exécutant :
> sudo dd if=</path/to/image.ext4> of=/dev/mmcblk0 bs=1M status=progress

Cette commande suppose que vous avez inséré la carte SD dans le slot de carte SD de la carte de développement. Si vous utilisez un adaptateur USB, il pourrait apparaître comme /dev/sdb ou similaire au lieu de /dev/mmcblk0.

Attention : Soyez très prudent avec le paramètre "of" dans la commande précédente. Si vous utilisez le mauvais disque, vous risquez de perdre des données. Vous pouvez également utiliser la fonction de restauration d'image disque de l'outil de disque pour flasher l'image sur la carte SD.

### Connexion à la console série
Vous pouvez utiliser un ordinateur openkylin pour surveiller la sortie série du processus de démarrage de LicheePi4A. Connectez les broches U0-TX, U0-RX, GND de la carte de développement à l'ordinateur via un câble USB vers série, puis exécutez la commande suivante pour ouvrir le port série :
> sudo minicom -s

Dans les paramètres du port série, modifiez A - Périphérique série en : /dev/ttyUSB0, puis sauvegardez et ouvrez le port série.

### Premier démarrage et modification des paramètres u-boot de la carte de développement
Démarrez le port série minicom de LicheePi4A sur l'ordinateur. Insérez la carte SD flashée dans le slot de LicheePi4A et connectez le câble d'alimentation. Lorsque la carte de développement démarre sur u-boot, appuyez rapidement sur Entrée pour arrêter la carte sur u-boot. Dans u-boot, entrez les commandes suivantes pour configurer la carte pour qu'elle démarre à partir de la carte SD :
> env set -f set_bootargs 'setenv bootargs console=ttyS0,115200 root=/dev/mmcblk1 rootfstype=ext4 rootwait rw earlycon clk_ignore_unused loglevel=7 eth=$ethaddr rootrwoptions=rw,noatime rootrwreset=${factory_reset} init=/lib/systemd/systemd'
> env save
> reset

Note : La première ligne est une seule commande, très longue, qui peut être affichée sur plusieurs lignes sur votre écran.

Ensuite, entrez reset dans u-boot pour redémarrer la carte. La carte pourra ainsi démarrer le système openKylin à partir de la carte SD.

Après le premier démarrage, il y aura un utilisateur par défaut dans le système. Lorsque l'environnement de bureau démarre, vous pouvez vous connecter pour la première fois au LicheePi4A avec l'utilisateur par défaut. Plus tard, vous pourrez modifier l'utilisateur ou le mot de passe selon vos besoins.

Le nom d'utilisateur/mot de passe par défaut est :
> username : openkylin
> password : openkylin

### Allocation de l'espace restant après le démarrage
> sudo btrfs filesystem resize max /

Note : Pour d'autres problèmes de flashage, comme l'installation de pilotes nécessaires pour le flashage sous Windows, l'affichage des journaux de flashage, etc., vous pouvez vous référer à la documentation pertinente de Sipeed Technology :
https://wiki.sipeed.com/hardware/zh/lichee/th1520/lpi4a/4_burn_image.html
