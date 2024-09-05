# <center>Comment ajouter des paramètres de démarrage à grub</center>
#### <center>Auteur : Little K</center>
#### <center>2022-04-22 23:36:00</center>
#### <center>Édition : f4prime</center>


## Comment ajouter des paramètres de démarrage à grub
Pour grub2, ubuntu fournit un fichier de configuration officiel `/etc/default/grub`. Dans la plupart des cas, les paramètres de grub2 peuvent être gérés dans ce fichier, et la structure de ce fichier est relativement simple et facile à modifier. Il n'est pas nécessaire de modifier `/boot/grub/grub.cfg` ou `/etc/grub.d/`.

Modifier `/etc/default/grub` est une commande simple :
```sh
sudo vim /etc/default/grub
```
Vous trouverez ci-dessous les valeurs par défaut du système et les moyens les plus courants de modifier l'heure d'affichage du menu et le système d'exploitation par défaut :
```sh
 #If you change this file, run 'update-grub' afterwards to update
 #/boot/grub/grub.cfg.

GRUB_DEFAULT=0 # Changer 0 en saved permet à grub de se souvenir du système sélectionné lors du dernier démarrage.
GRUB_HIDDEN_TIMEOUT=0
GRUB_HIDDEN_TIMEOUT_QUIET=true
GRUB_TIMEOUT="5" # Temps d'affichage du menu de sélection du démarrage
GRUB_DISTRIBUTOR=`lsb_release -i -s 2> /dev/null || echo Debian`
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
GRUB_CMDLINE_LINUX=""
```


Exécutez la commande suivante pour mettre à jour automatiquement `/boot/grub/grub.cfg` une fois la modification terminée.

```sh
$ sudo update-grub  // Générer le fichier de configuration de grub
```
