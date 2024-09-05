# <center>Commandes git pulcouramment utilisées</center

#### <center>2022-04-23 00:16:10</center>

Il n'y a pas encore beaucoup de contenu, s'il y a une collection de commandes utiles, elle sera mise à jour dès que possible.

1. installer les en-têtes

``bash
sudo apt install linux-headers-`uname -r`
``bash sudo apt install linux-headers-`uname -r``

2. mettre à jour le bootstrap

``bash
sudo update-grub
```

3) Vérifier si un paquet est installé

``bash
dpkg -l|grep le nom du paquet ou une partie de celui-ci
```

4. synchroniser l'heure du double système
``bash
sudo timedatectl set-local-rtc 1
sudo hwclock --localtime --systohc
```

5. copier des fichiers
# Copier le fichier source.cpp de /etc/ vers /home/ en tant que dest.cpp.
``bash
cp /etc/source.cpp /home/dest.cpp
```

# Copier le fichier source.cpp de /etc/ vers /home/.

``bash
cp /etc/source.cpp /home/
```

6. fusionner deux fichiers (fusionner file1.txt et file2.txt en file.txt)

``bash
cat file1.txt file2.txt > file.txt
``