

# Guide Installation pour HiFive Unmatched : 



## Flashage sur carte SD

### Préparation de la carte SD
Même si vous prévoyez d'utiliser un SSD NVMe à long terme, une carte SD est nécessaire pour la première étape. Cela nous permettra de configurer le SSD NVMe sur l'Unmatched plus tard. L'image openKylin adaptée à l'Unmatched peut être téléchargée via le lien suivant :
> https://www.openkylin.top/downloads

Décompressez avec la commande suivante :
> unxz openkylin-1.0-hifive-unmatched-riscv64.img.xz

Veuillez ajuster le chemin ci-dessus selon votre emplacement réel de décompression.

### Création d'un disque de démarrage SD
Tout d'abord, utilisez l'outil de disque pour formater la carte SD.
Ensuite, flashez l'image sur la carte SD via la ligne de commande en exécutant :
> sudo dd if=</path/to/image.img> of=/dev/mmcblk0 bs=1M status=progress

Note : </path/to/image.img> représente le chemin de l'image téléchargée, n'écrivez pas les chevrons et le texte à l'intérieur, évitez les espaces dans le chemin si possible.

Cette commande suppose que vous avez inséré la carte SD dans le lecteur SD de votre ordinateur. Si vous utilisez un lecteur USB, il pourrait apparaître comme /dev/sdb ou similaire au lieu de /dev/mmcblk0.

Attention : Soyez très prudent avec le paramètre "of" dans la commande précédente. Si vous utilisez le mauvais disque, vous risquez de perdre des données. Vous pouvez également utiliser la fonction de restauration d'image disque de l'outil de disque pour flasher l'image sur la carte SD.

### Allocation de l'espace restant sur la carte SD après le flashage
Exécutez les commandes suivantes pour allouer l'espace restant de la carte SD à la partition racine.
Note : Ces commandes supposent que le numéro de périphérique de votre carte SD est /dev/sdb. Lors du partitionnement, veuillez utiliser le numéro de périphérique réel.

```
sudo apt install cloud-utils
sudo growpart /dev/sdb 4
sudo resize2fs /dev/sdb4
```

### Modification du fichier de configuration U-BOOT
```
sudo mount /dev/mmcblk0p4 /mnt
sudo mount /dev/mmcblk0p3 /mnt/boot
sudo chroot /mnt
```
Veuillez modifier les chemins commençant par /dev selon vos chemins réels.

Ouvrez "/etc/default/u-boot" avec un éditeur de texte (c'est le chemin après chroot, le chemin réel sur votre machine est "/mnt/etc/default/u-boot"), et ajoutez :
> U_BOOT_ROOT="root=/dev/mmcblk0p4"

Puis exécutez :
> u-boot-update

Ouvrez "/boot/extlinux/extlinux.conf" avec un éditeur de texte (c'est le chemin après chroot, le chemin réel sur votre machine est "/mnt/boot/extlinux/extlinux.conf"), et ajoutez dans les deux lignes vides :
> fdt /hifive-unmatched-a00.dtb

Puis quittez :
```
exit
sudo umount /mnt/boot
sudo umount /mnt
```

### Premier démarrage
Après le premier démarrage, il y aura un utilisateur par défaut dans le système. Lorsque l'environnement de bureau démarre, vous pouvez vous connecter pour la première fois à l'Unmatched avec l'utilisateur par défaut. Plus tard, vous pourrez modifier l'utilisateur ou le mot de passe selon vos besoins.

Le nom d'utilisateur/mot de passe par défaut est :
> username : openkylin
> password : openkylin

Deux autres méthodes de connexion sont également prises en charge.

### Connexion à la console série
Le guide de démarrage HiFive 14 explique comment se connecter à la console série à partir de différents systèmes d'exploitation. Si vous utilisez un ordinateur openkylin pour surveiller la sortie série, connectez cet ordinateur au port Micro USB à côté du slot de carte SD sur l'Unmatched et exécutez :
> sudo screen /dev/ttyUSB1 115200

Après avoir appuyé sur le bouton d'alimentation, la sortie de démarrage commencera à apparaître dans la session.

## Flashage sur NVMe

### Installation d'openKylin RISC-V sur un disque NVMe
L'utilisation d'un disque NVMe avec l'Unmatched fait une énorme différence en termes de performance et de convivialité. Cela demande un peu d'effort pour le faire fonctionner, mais croyez-moi, cela en vaut la peine. SiFive recommande le Samsung 970 EVO Plus. J'ai utilisé le Samsung 970 EVO (pas le plus) et il fonctionne bien. La façon la plus simple d'installer openKylin RISC-V sur un disque NVMe est de démarrer à partir de la carte SD et d'utiliser le connecteur M.2 sur l'Unmatched lui-même.

Après le démarrage, téléchargez l'image openKylin sur l'Unmatched :
> https://www.openkylin.top/downloads

Décompressez avec la commande suivante :
> unxz openkylin-1.0-hifive-unmatched-riscv64.img.xz

Veuillez ajuster le chemin ci-dessus selon votre emplacement réel de décompression.

Assurez-vous que le disque NVMe est présent en exécutant :
> ls -l /dev/nvme*

Sur ma carte mère, le disque NVMe apparaît comme /dev/nvme0n1. Tout d'abord, utilisez l'outil de disque pour formater le disque NVMe. Ensuite, flashez l'image sur le NVMe en exécutant :
> sudo dd if=</path/to/image.img> of=/dev/nvme0n1 bs=1M status=progress

Note : </path/to/image.img> représente le chemin de l'image téléchargée, n'écrivez pas les chevrons et le texte à l'intérieur, évitez les espaces dans le chemin si possible.

### Allocation de l'espace restant sur le disque NVMe après le flashage
Exécutez les commandes suivantes pour allouer l'espace restant du disque NVMe à la partition racine.
Note : Ces commandes supposent que le numéro de périphérique de votre carte SD est /dev/sdb. Lors du partitionnement, veuillez utiliser le numéro de périphérique réel.

```
sudo apt install cloud-utils
sudo growpart /dev/sdb 4
sudo resize2fs /dev/sdb4
```

Félicitations ! Vous avez maintenant installé openKylin RISC-V sur le disque NVMe de votre HiFive Unmatched. Cependant, il reste encore un problème. L'Unmatched nécessite toujours la présence de la carte SD pour démarrer, et il existe une condition de concurrence qui pourrait l'amener à monter le système de fichiers racine sur la carte SD plutôt que sur le disque NVMe. Pour éviter cela, montez le disque NVMe nouvellement flashé et faites un chroot dedans en exécutant :

```
sudo mount /dev/nvme0n1p4 /mnt
sudo mount /dev/nvme0n1p3 /mnt/boot
sudo chroot /mnt
```

Note : La commande chroot précédente ne fonctionnera que si elle est exécutée sur un ordinateur riscv64. C'est l'une des raisons pour lesquelles ce tutoriel suggère de configurer le disque NVMe en utilisant le lecteur M.2 sur l'Unmatched.

Utilisez votre éditeur de texte préféré pour éditer "/etc/default/u-boot", et ajoutez :
> U_BOOT_ROOT="root=/dev/nvme0n1p4"

Pour appliquer ces modifications, exécutez :
> u-boot-update

Ouvrez "/boot/extlinux/extlinux.conf" avec un éditeur de texte, et ajoutez dans les deux lignes vides :
> fdt /hifive-unmatched-a00.dtb

Quittez l'environnement chroot en exécutant exit :
```
exit
sudo umount /mnt/boot
sudo umount /mnt
```

Puis redémarrez le système. Il démarrera maintenant à partir de votre disque NVMe, et vous obtiendrez une amélioration significative des performances !
