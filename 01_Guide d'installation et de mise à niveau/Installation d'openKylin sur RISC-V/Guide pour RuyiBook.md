
# Guide Installation pour RuyiBook : 


## Préparation de l'environnement de flashage eMMC

Préparez un câble de données Type-C, généralement fourni dans la boîte du RuyiBook.

L'image openKylin adaptée au RuyiBook peut être téléchargée via le lien suivant :
> https://www.openkylin.top/downloads

Décompressez le fichier avec la commande suivante :
> tar -xvf openKylin-Embedded-V2.0-Release-RuyiBook-riscv64.tar.xz

Un appareil d'architecture X86 est nécessaire.

Entrez dans le dossier décompressé et exécutez le script pour flasher l'image :
sudo ./burn_techvision.sh

Prenez le câble Type-C préparé, utilisez un trombone ou une épingle pour maintenir enfoncé le bouton de flashage situé à l'avant droit de l'ordinateur portable afin d'entrer en mode fastboot. Connectez le port Type-C situé à l'avant gauche de l'ordinateur portable au port USB de l'ordinateur.

Le terminal affichera le journal de flashage. Une fois le flashage terminé, débranchez le câble de données, mettez sous tension pour démarrer.

## Premier démarrage

Après le flashage, il faut attendre un moment, puis charger l'ordinateur portable jusqu'à ce que le voyant vert sur le côté gauche s'allume. À ce moment-là, vous pouvez appuyer sur le bouton d'alimentation pour démarrer. Après le premier démarrage, il y aura un utilisateur par défaut dans le système. Lorsque l'environnement de bureau démarre, vous pouvez vous connecter pour la première fois au RuyiBook avec l'utilisateur par défaut. Plus tard, vous pourrez modifier l'utilisateur ou le mot de passe selon vos besoins.

Le nom d'utilisateur/mot de passe par défaut est :
> username : openkylin
> password : openkylin
