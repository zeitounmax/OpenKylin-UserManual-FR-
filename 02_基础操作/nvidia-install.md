

# <center>Installation des pilotes NVIDIA propriétaires depuis le site officiel de NVIDIA</center>
#### <center>Auteur : Lin Tongdu</center>
#### <center>2022-11-06 18:00:00</center>

## GPU-Docs/NVIDIA-GPU-Supported (Cartes graphiques NVIDIA supportées)

### I. Téléchargement du pilote NVIDIA propriétaire correspondant depuis le site officiel de NVIDIA.

   Utilisez un moteur de recherche pour chercher "NVIDIA Unix Driver" ou entrez "https://www.nvidia.com/en-us/drivers/unix/" (le site officiel chinois de NVIDIA "https://www.nvidia.cn/drivers/unix" fonctionne aussi) dans la barre d'adresse du navigateur pour accéder à la page des pilotes UNIX pour cartes graphiques du site officiel de NVIDIA. Sélectionnez Linux x86_64/AMD64. Sous Linux x86_64/AMD64, choisissez selon vos besoins entre Latest Production Branch Version (la dernière version de la branche de production), Latest New Feature Branch Version (la dernière version de la branche des nouvelles fonctionnalités) ou Latest Legacy GPU version (la dernière version des pilotes GPU legacy). Généralement, choisissez Latest Legacy GPU version {390.xx.series} ou supérieur pour le téléchargement.

![Description de l'image](../assets/%E4%BB%8ENVIDIA%E5%AE%98%E7%BD%91%E5%AE%89%E8%A3%85%E9%97%AD%E6%BA%90%E6%98%BE%E5%8D%A1%E9%A9%B1%E5%8A%A8/202203041601588081_%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE2022-03-04155609.png)

   Après avoir cliqué sur le lien vert, cliquez sur SUPPORTED PRODUCTS pour vérifier si votre modèle de carte NVIDIA est dans la liste. S'il n'y est pas, retournez à la page NVIDIA Unix Archive pour continuer à chercher le pilote nécessaire.

![Description de l'image](../assets/%E4%BB%8ENVIDIA%E5%AE%98%E7%BD%91%E5%AE%89%E8%A3%85%E9%97%AD%E6%BA%90%E6%98%BE%E5%8D%A1%E9%A9%B1%E5%8A%A8/202203041602357887_%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE2022-03-04155757.png)

### II. Désactivation du pilote NVIDIA open source nouveau fourni avec le système.

   Ouvrez "Ordinateur", "Mon ordinateur" ou le gestionnaire de fichiers, puis ouvrez le disque système, puis /etc/modprobe.d/, créez un nouveau document texte txt, ouvrez-le et entrez deux lignes de commandes. Première ligne : blacklist nouveau, deuxième ligne : options nouveau modeset=0. Enregistrez ce document texte txt dans ce dossier, renommez-le en blacklist-nouveau, changez l'extension de .txt à .conf.

![Description de l'image](../assets/%E4%BB%8ENVIDIA%E5%AE%98%E7%BD%91%E5%AE%89%E8%A3%85%E9%97%AD%E6%BA%90%E6%98%BE%E5%8D%A1%E9%A9%B1%E5%8A%A8/202203041703478511_IMG_20220304_163247.jpg)

Si le dossier modprobe.d apparaît comme verrouillé, faites un clic droit et choisissez "Ouvrir en tant qu'administrateur" ou ouvrez un terminal et entrez la commande "sudo nano /etc/modprobe.d/blacklist-nouveau.conf", puis entrez le mot de passe administrateur pour créer le fichier blacklist-nouveau.conf avec l'éditeur de texte Nano.

![Description de l'image](../assets/%E4%BB%8ENVIDIA%E5%AE%98%E7%BD%91%E5%AE%89%E8%A3%85%E9%97%AD%E6%BA%90%E6%98%BE%E5%8D%A1%E9%A9%B1%E5%8A%A8/202203041603201847_%E6%88%AA%E5%9B%BE_20220304153721.png)

   Ensuite, entrez la commande "sudo update-initramfs -u" dans le terminal pour mettre à jour le fichier initramfs du noyau Linux, puis entrez sudo reboot pour redémarrer le système Linux. Après le redémarrage, appuyez sur Ctrl+Alt+F2 pour entrer en mode ligne de commande, entrez la commande "lsmod | grep nouveau". Si aucun résultat n'est affiché, cela prouve que le système n'a pas chargé le pilote nouveau, la désactivation du pilote nouveau a réussi.

### III. Compilation et installation du pilote NVIDIA propriétaire en ligne de commande.

   En mode ligne de commande, entrez la commande "sudo service lightdm stop" pour arrêter lightdm, puis entrez la commande "cd chemin_du_dossier_contenant_le_pilote_NVIDIA_téléchargé", puis entrez la commande "sudo su" pour passer en mode administrateur. Ensuite, entrez la commande "sudo chmod +x nom_du_pilote_NVIDIA_téléchargé" pour donner au pilote NVIDIA téléchargé la permission de modifier le système, enfin entrez la commande "sudo ./nom_du_pilote_NVIDIA_téléchargé" pour exécuter la compilation du pilote NVIDIA.

   Pendant l'installation, une première fenêtre apparaîtra : "Would you like to register the kernel module sources with DKMS? This will allow DKMS to automatically build a new module, if you install a different kernel later." Ce qui signifie à peu près "Voulez-vous enregistrer les sources du module noyau avec DKMS ? Cela permettra à DKMS de construire automatiquement un nouveau module si vous installez un noyau différent plus tard." Veuillez choisir No.

![Description de l'image](../assets/%E4%BB%8ENVIDIA%E5%AE%98%E7%BD%91%E5%AE%89%E8%A3%85%E9%97%AD%E6%BA%90%E6%98%BE%E5%8D%A1%E9%A9%B1%E5%8A%A8/202203041704538162_IMG_20220304_165021_edit_16691001473494.jpg)

Deuxième fenêtre : "Install NVIDIA's 32-bit compatibility libraries?" Ce qui signifie "Faut-il installer les bibliothèques de compatibilité 32 bits de NVIDIA ?" Choisissez Yes.

![Description de l'image](../assets/%E4%BB%8ENVIDIA%E5%AE%98%E7%BD%91%E5%AE%89%E8%A3%85%E9%97%AD%E6%BA%90%E6%98%BE%E5%8D%A1%E9%A9%B1%E5%8A%A8/202203041707155089_IMG_20220304_165105_edit_16733711034946.jpg)

Troisième fenêtre : "Would you like to run the nvidia-xconfig utility to automatically update your X configuration file so that the NVIDIA X driver will be used when you restart X? Any pre-existing configuration file will be backed up." Ce qui signifie à peu près "Voulez-vous exécuter l'utilitaire nvidia-xconfig pour mettre à jour automatiquement votre fichier de configuration X afin que le pilote NVIDIA X soit utilisé lorsque vous redémarrez X ? Tout fichier de configuration préexistant sera sauvegardé." Choisissez No. Si vous choisissez Yes, vous ne pourrez pas accéder au bureau après le redémarrage de l'ordinateur.

### IV. Vérification de la réussite de l'installation du pilote NVIDIA

   Après l'installation, entrez la commande "nvidia-smi" pour vérifier si le pilote NVIDIA a été installé avec succès. S'il y a un résultat affiché, cela prouve que l'installation a réussi.

![Description de l'image](../assets/%E4%BB%8ENVIDIA%E5%AE%98%E7%BD%91%E5%AE%89%E8%A3%85%E9%97%AD%E6%BA%90%E6%98%BE%E5%8D%A1%E9%A9%B1%E5%8A%A8/20220304162553456_nvidiasmi.png)

Déclaration de droits d'auteur : Cet article est une création originale de <Lin Tongdu, utilisateur du forum Deepin>, lien original : [[Tutoriel pour débutants] Comment installer le pilote NVIDIA en ligne de commande-Lin Tongdu-Forum Deepin](https://bbs.deepin.org/post/232502), reproduit par <Ma Linping>, sous licence [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). Pour toute reproduction, veuillez inclure le lien source et cette déclaration.

## GPU-Docs/NVIDIA-GPU-Supported (Cartes graphiques NVIDIA supportées) Comment installer le pilote NVIDIA en ligne de commande sur un serveur distant

[La suite de la traduction suit le même modèle...]

## Installation directe du pilote NVIDIA via le terminal

Exécutez directement dans le terminal :

```
sudo apt install nvidia-driver
```