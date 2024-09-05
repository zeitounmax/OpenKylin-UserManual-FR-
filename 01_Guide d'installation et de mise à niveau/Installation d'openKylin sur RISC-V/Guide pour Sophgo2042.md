

# Guide Installation pour Sophgo sg2042 : 


## Préparation de la carte SD

L'image openKylin adaptée au Sophgo sg2042 peut être téléchargée via le lien suivant :
> https://www.openkylin.top/downloads

Décompressez avec la commande suivante :
> unxz openkylin-1.0-sophgo-sg2042-riscv64.img.xz

Veuillez ajuster le chemin ci-dessus selon votre emplacement réel de décompression.

## Création d'un disque de démarrage SD

Tout d'abord, utilisez l'outil de disque pour formater la carte SD.

Ensuite, flashez l'image sur la carte SD via la ligne de commande en exécutant :
> sudo dd if=</path/to/image.img> of=/dev/mmcblk0 bs=1M status=progress

Note : </path/to/image.img> représente le chemin de l'image téléchargée, n'écrivez pas les chevrons et le texte à l'intérieur, évitez les espaces dans le chemin si possible.

Cette commande suppose que vous avez inséré la carte SD dans le slot de carte SD de la carte de développement. Si vous utilisez un lecteur USB, il pourrait apparaître comme /dev/sdb ou similaire au lieu de /dev/mmcblk0.

Attention : Soyez très prudent avec le paramètre "of" dans la commande précédente. Si vous utilisez le mauvais disque, vous risquez de perdre des données. Vous pouvez également utiliser la fonction de restauration d'image disque de l'outil de disque pour flasher l'image sur la carte SD.

## Allocation de l'espace restant sur la carte SD après le flashage

Suivez ces étapes pour allouer l'espace restant de la carte SD à la partition racine.
Note : Ces commandes supposent que le numéro de périphérique de votre carte SD est /dev/sdb. Lors du partitionnement, veuillez utiliser le numéro de périphérique réel.

> sudo apt install cloud-guest-utils
> sudo growpart /dev/sdb 2
> sudo resize2fs /dev/sdb2

## Connexion à la console série

Vous pouvez utiliser un ordinateur openKylin pour surveiller la sortie série du processus de démarrage de Sophgo sg2042. Connectez l'ordinateur au troisième port Micro USB du Sophgo sg2042, puis exécutez la commande suivante pour ouvrir le port série :
sudo minicom -s

Dans les paramètres du port série, modifiez A - Périphérique série en : /dev/ttyUSB0, puis sauvegardez et ouvrez le port série.

## Premier démarrage

Insérez la carte SD flashée et partitionnée dans le slot du Sophgo sg2042 et appuyez sur le bouton d'alimentation. Après le premier démarrage, il y aura un utilisateur par défaut dans le système. Lorsque l'environnement de bureau démarre, vous pouvez vous connecter pour la première fois au Sophgo sg2042 avec l'utilisateur par défaut. Plus tard, vous pourrez modifier l'utilisateur ou le mot de passe selon vos besoins.

Le nom d'utilisateur/mot de passe par défaut est :
> username : openkylin
> password : openkylin
