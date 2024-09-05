# Guide Installation Pour DW1000

## Préparation de la carte SD

L'image openKylin adaptée au DW1000 peut être téléchargée via le lien suivant :
> https://www.openkylin.top/downloads

Décompressez avec la commande suivante :
> tar xf openKylin-Embedded-V1.0-Release-DW1000-RV64G.tar.xf

Veuillez ajuster le chemin ci-dessus selon votre emplacement réel de décompression.

## Création d'un disque de démarrage SD

Tout d'abord, utilisez l'outil de disque pour formater la carte SD.
> sudo dd if=</path/to/image.img> of=/dev/sdb1 bs=1M status=progress

Cette commande suppose que vous avez inséré la carte SD dans le lecteur SD de votre PC Linux. Si vous utilisez un lecteur USB, il apparaîtra comme /dev/sdb ou similaire.

Note : Soyez très prudent avec le paramètre "of" dans la commande précédente. Si vous utilisez le mauvais disque, vous risquez de perdre des données. Vous pouvez également utiliser la fonction de restauration d'image disque de l'outil de disque pour flasher l'image sur la carte SD.

## Allocation de l'espace restant sur la carte SD après le flashage

Suivez ces étapes pour allouer l'espace restant de la carte SD à la partition racine.
Note : Cette commande suppose que le numéro de périphérique de votre carte SD est /dev/sdb. Lors du partitionnement, veuillez utiliser le numéro de périphérique réel.

> sudo apt install cloud-guest-utils
> sudo growpart /dev/sdb 1
> sudo resize2fs /dev/sdb1

## Premier démarrage

Insérez la carte SD flashée et partitionnée dans le slot du DW1000 et appuyez sur le bouton d'alimentation. Après le premier démarrage, il y aura un utilisateur par défaut dans le système. Lorsque l'environnement de bureau démarre, vous pouvez vous connecter pour la première fois au DW1000 avec l'utilisateur par défaut. Plus tard, vous pourrez modifier l'utilisateur ou le mot de passe selon vos besoins.

Le nom d'utilisateur/mot de passe par défaut est :
> username : openkylin
> password : openkylin