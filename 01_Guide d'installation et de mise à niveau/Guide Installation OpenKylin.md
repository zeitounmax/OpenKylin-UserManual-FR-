# **Guide d'installation du système openKylin**

Ce document explique principalement les trois scénarios d'installation les plus courants pour l'architecture X86 utilisée par les utilisateurs de la communauté. Les étapes nécessaires à l'installation dans ces trois scénarios diffèrent légèrement, veuillez choisir le chapitre correspondant en fonction de votre scénario d'installation réel.

De plus, les utilisateurs (joueurs de haut niveau) qui ont besoin d'installer le système d'exploitation openKylin sur des cartes de développement RISC-V et ARM peuvent se référer aux annexes à la fin du document pour accéder aux documents correspondants.

# **Préparation avant l'installation**

 1. Un ordinateur x86_64 bits ;

 2. Une clé USB formatable d'une capacité d'au moins 8 Go ;

*Astuce : 1 Go = 1024 Mo, pour faciliter le calcul, on peut estimer que 1 Go = 1000 Mo*

## **Exigences de configuration**

### **Configuration recommandée pour une expérience fluide**

● CPU : Produits grand public d'après 2018 (6 cœurs et plus)

● Mémoire : 8 Go et plus (*Astuce : Il est recommandé d'allouer 4 Go et plus pour les machines virtuelles*)

● Disque dur : 128 Go et plus d'espace de stockage libre

### **Configuration minimale pour l'installation du système**

● CPU : Produits d'après 2015 (double cœur)

● Mémoire : Pas moins de 4 Go (*Astuce : Allouer au moins 2 Go pour les machines virtuelles*)

● Disque dur : Au moins 50 Go d'espace de stockage libre

# **Installation d'un système unique sur une machine physique**

## **Étape 1 : Télécharger l'image openKylin**

Allez sur le site officiel pour télécharger l'image x86_64 (https://www.openkylin.top/downloads/628-cn.html)

*Astuce : Après avoir téléchargé le fichier image, veuillez d'abord vérifier si la valeur MD5 du fichier correspond à celle du site officiel. Si elle ne correspond pas, veuillez le télécharger à nouveau*

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/1-%E5%AE%98%E7%BD%91%E4%B8%8B%E8%BD%BD%E9%95%9C%E5%83%8F.png)

Le site officiel propose également plusieurs sites miroirs pour le téléchargement (https://www.openkylin.top/downloads/mirrors-cn.html)

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/2-%E9%95%9C%E5%83%8F%E7%AB%99.png)

## **Étape 2 : Choisir un outil de création de disque de démarrage pour créer le disque de démarrage**

Il est recommandé d'utiliser rufus ou ventoy pour créer le disque de démarrage du système openKylin.

1. rufus

Allez sur le site officiel (http://rufus.ie/) pour télécharger et installer l'outil rufus, puis configurez-le selon l'image ci-dessous pour terminer la création du disque de démarrage

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/3-rufus.png)

2. ventoy

Allez sur le site officiel (https://www.ventoy.net/) pour télécharger l'outil ventoy, suivez les étapes ci-dessous pour créer le disque de démarrage, puis copiez l'image à installer sur la clé USB

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/4-ventoy.png)

## **Étape 3 : Démarrer l'ordinateur à partir du disque de démarrage (Ventoy comme exemple)**

Éteignez l'ordinateur, insérez le disque de démarrage créé dans le port USB de l'ordinateur, puis démarrez l'ordinateur tout en appuyant continuellement sur la touche delete, F2, F10 ou F12 pour entrer dans l'interface BIOS.

*Astuce : Les différentes marques et modèles d'ordinateurs varient, vous pouvez rechercher sur Baidu quelle touche utiliser pour entrer dans le BIOS de votre ordinateur spécifique*

Désactivez le démarrage sécurisé (secure boot) et définissez la clé USB comme premier périphérique de démarrage, puis sauvegardez et redémarrez

*Astuce : Généralement, la touche F10 est pour "Sauvegarder et redémarrer", l'interface BIOS a généralement des indications*

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/5-%E8%AE%BE%E7%BD%AEbios.jpeg)

Vous entrerez ensuite dans l'interface du disque de démarrage Ventoy, sélectionnez l'image openKylin 1.0. Comme illustré ci-dessous :

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/6-ventoy%E9%80%89%E6%8B%A9%E9%95%9C%E5%83%8F.jpeg)

Entrez dans l'interface de démarrage Grub, sélectionnez "Try without installing" pour entrer dans le bureau d'essai openKylin

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/39-%E5%AE%89%E8%A3%85grub%E5%BC%95%E5%AF%BC%E9%A1%B9.jpeg)

Astuce : Les deux premières options démarrent avec le noyau 6.1 ; les deux dernières options démarrent avec le noyau 5.15

## **Étape 4 : Commencer l'installation du système**

Sur le bureau d'essai openKylin, cliquez sur l'icône "Installer openKylin" sur le bureau pour commencer la configuration avant l'installation

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/7-%E8%AF%95%E7%94%A8%E6%A1%8C%E9%9D%A2.png)

Sélectionnez la langue

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/8-%E9%80%89%E6%8B%A9%E8%AF%AD%E8%A8%80.png)

Sélectionnez le fuseau horaire

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/40-%E9%80%89%E6%8B%A9%E6%97%B6%E5%8C%BA.png)

Créez un utilisateur

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/9_%E5%88%9B%E5%BB%BA%E7%94%A8%E6%88%B7.png)

Choisissez la méthode d'installation (partitionnement)

1. Installation complète (recommandée pour les débutants)

*Astuce : L'installation complète formatera l'intégralité du disque dur, assurez-vous qu'il n'y a plus de fichiers à conserver sur le disque dur*

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/10_%E9%80%89%E6%8B%A9%E5%AE%89%E8%A3%85%E6%96%B9%E5%BC%8F.png)

2. Installation personnalisée (recommandée pour les utilisateurs avancés)

Cliquez sur "Créer une table de partition" pour effacer les partitions actuelles (*Astuce : À ce stade, l'effacement n'est pas réellement effectué*)

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/11-%E6%B8%85%E7%A9%BA%E5%88%86%E5%8C%BA%E8%A1%A8.png)

Ensuite, créez des partitions selon vos besoins. En général, il faut au moins créer : une partition racine (utilisée pour "ext4", point de montage "/", allocation d'espace supérieure à 15 Go) :

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/21-%E5%88%9B%E5%BB%BA%E6%A0%B9%E5%88%86%E5%8C%BA.png)

Une partition efi (utilisée pour "efi", allocation d'espace de 256 Mo à 2 Go)

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/13-%E5%88%9B%E5%BB%BAefi%E5%88%86%E5%8C%BA.png)

Cliquez sur OK et confirmez la stratégie de partitionnement que vous avez choisie

*Astuce : À ce stade, il est encore temps de changer d'avis**. Après avoir cliqué sur "Commencer l'installation", le ****formatage**** sera d'abord effectué et vous perdrez le contenu original de ce disque dur*

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/14-%E7%A1%AE%E8%AE%A4%E5%88%86%E5%8C%BA%E7%AD%96%E7%95%A5.png)

Commencez le processus d'installation

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/15-%E5%BC%80%E5%A7%8B%E5%AE%89%E8%A3%85%E8%BF%9B%E7%A8%8B.png)

## **Étape 5 : Terminer l'installation**

Une fois la progression de l'installation terminée, vous serez informé que l'installation est terminée.

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/16-%E5%AE%89%E8%A3%85%E5%AE%8C%E6%88%90.jpeg)

Cliquez sur "Redémarrer maintenant", puis suivez les instructions pour retirer la clé USB et appuyez sur la touche "entrée", le système redémarrera automatiquement pour entrer dans le système. À ce moment, votre ordinateur a réussi à installer le système openKylin !!

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/17-%E6%8B%94%E6%8E%89%E4%BC%98%E7%9B%98%E9%87%8D%E5%90%AF.jpeg)

# **Installation d'un double système sur une machine physique**

## **Étape 1 : Télécharger l'image openKylin**

Allez sur le site officiel pour télécharger l'image x86_64 (https://www.openkylin.top/downloads/628-cn.html)

*Astuce : Après avoir téléchargé le fichier image, veuillez d'abord vérifier si la valeur MD5 du fichier correspond à celle du site officiel. Si elle ne correspond pas, veuillez le télécharger à nouveau*

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/1-%E5%AE%98%E7%BD%91%E4%B8%8B%E8%BD%BD%E9%95%9C%E5%83%8F.png)

Le site officiel propose également plusieurs sites miroirs pour le téléchargement (https://www.openkylin.top/downloads/mirrors-cn.html)

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/2-%E9%95%9C%E5%83%8F%E7%AB%99.png)

## **Étape 2 : Choisir un outil de création de disque de démarrage pour créer le disque de démarrage**

Il est recommandé d'utiliser rufus ou ventoy pour créer le disque de démarrage du système openKylin.

1. rufus

Allez sur le site officiel (http://rufus.ie/) pour télécharger et installer l'outil rufus, puis configurez-le selon l'image ci-dessous pour terminer la création du disque de démarrage

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/3-rufus.png)

2. ventoy

Allez sur le site officiel (https://www.ventoy.net/) pour télécharger l'outil ventoy, suivez les étapes ci-dessous pour créer le disque de démarrage, puis copiez l'image à installer sur la clé USB

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/4-ventoy.png)

## **Étape 3 : Partitionner le disque dur de l'ordinateur**

Par défaut, la machine a déjà installé le système Windows. Ouvrez l'outil de gestion des disques, sélectionnez l'espace disque à partitionner, faites un clic droit sur ce disque et sélectionnez "Réduire le volume".

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/18-%E7%A3%81%E7%9B%98%E5%88%86%E5%8C%BA-1.png)

Une fenêtre de réduction apparaîtra, saisissez la taille de l'espace à réduire. Dans cet exemple, environ 135 Go sont alloués (astuce : l'espace alloué ne doit pas être inférieur à 50 Go). Après avoir confirmé la taille de l'espace à réduire, cliquez sur "Réduire".

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/19-%E7%A3%81%E7%9B%98%E5%88%86%E5%8C%BA-2.png)

Une fois la réduction terminée, un espace libre "non alloué" apparaîtra, qui sera utilisé pour installer le système d'exploitation openKylin.

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/20-%E7%A3%81%E7%9B%98%E5%88%86%E5%8C%BA-3.png)

## **Étape 4 : Démarrer l'ordinateur à partir du disque de démarrage (Ventoy comme exemple)**

Éteignez l'ordinateur, insérez le disque de démarrage créé dans le port USB de l'ordinateur, puis démarrez l'ordinateur tout en appuyant continuellement sur la touche delete, F2, F10 ou F12 pour entrer dans l'interface BIOS.

*Astuce : Les différentes marques et modèles d'ordinateurs varient, vous pouvez rechercher sur Baidu quelle touche utiliser pour entrer dans le BIOS de votre ordinateur spécifique*

Désactivez le démarrage sécurisé (secure boot) et définissez la clé USB comme premier périphérique de démarrage, puis sauvegardez et redémarrez

*Astuce : Généralement, la touche F10 est pour "Sauvegarder et redémarrer", l'interface BIOS a généralement des indications*

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/5-%E8%AE%BE%E7%BD%AEbios.jpeg)

Vous entrerez ensuite dans l'interface du disque de démarrage Ventoy, sélectionnez l'image openKylin 1.0. Comme illustré ci-dessous :

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/6-ventoy%E9%80%89%E6%8B%A9%E9%95%9C%E5%83%8F.jpeg)

Entrez dans l'interface de démarrage Grub, sélectionnez "Try without installing" pour entrer dans le bureau d'essai openKylin

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/39-%E5%AE%89%E8%A3%85grub%E5%BC%95%E5%AF%BC%E9%A1%B9.jpeg)

Astuce : Les deux premières options démarrent avec le noyau 6.1 ; les deux dernières options démarrent avec le noyau 5.15

## **Étape 5 : Commencer l'installation du système**

Sur le bureau d'essai openKylin, cliquez sur l'icône "Installer openKylin" sur le bureau pour commencer la configuration avant l'installation

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/7-%E8%AF%95%E7%94%A8%E6%A1%8C%E9%9D%A2.png)

Sélectionnez la langue

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/8-%E9%80%89%E6%8B%A9%E8%AF%AD%E8%A8%80.png)

Sélectionnez le fuseau horaire

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/40-%E9%80%89%E6%8B%A9%E6%97%B6%E5%8C%BA.png)

Créez un utilisateur

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/9_%E5%88%9B%E5%BB%BA%E7%94%A8%E6%88%B7.png)

Sélectionnez "Installation personnalisée" et définissez la partition libre créée à l'étape 3 (comme indiqué dans l'image, la partition libre tout en bas) comme partition racine ("/") du système openKylin

*Astuce : Pour l'installation en double système, vous ne pouvez choisir que l'installation personnalisée. L'installation complète détruirait le système original. Il n'est pas nécessaire de configurer une partition efi*

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/21-%E5%88%9B%E5%BB%BA%E6%A0%B9%E5%88%86%E5%8C%BA.png)

Après avoir confirmé la stratégie de partitionnement que vous avez choisie, cliquez sur Commencer l'installation

*Astuce : À ce stade, il est encore temps de changer d'avis**. Après avoir cliqué sur "Commencer l'installation", le ****formatage**** sera d'abord effectué et vous perdrez le contenu original de ce disque dur*

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/22-%E7%A1%AE%E8%AE%A4%E5%88%86%E5%8C%BA%E7%AD%96%E7%95%A5%E5%8F%8C%E7%B3%BB%E7%BB%9F.png)

Commencez le processus d'installation

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/23-%E5%AE%89%E8%A3%85%E8%BF%9B%E7%A8%8B.png)

## **Étape 6 : Terminer l'installation**

Une fois la progression de l'installation terminée, vous serez informé que l'installation est terminée.

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/16-%E5%AE%89%E8%A3%85%E5%AE%8C%E6%88%90.jpeg)

Cliquez sur "Redémarrer maintenant", puis suivez les instructions pour retirer la clé USB et appuyez sur la touche "entrée", le système redémarrera automatiquement

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/17-%E6%8B%94%E6%8E%89%E4%BC%98%E7%9B%98%E9%87%8D%E5%90%AF.jpeg)

Sélectionnez l'option de démarrage openKylin dans l'interface grub

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/41-%E5%8F%8C%E7%B3%BB%E7%BB%9F%E5%BC%95%E5%AF%BC%E9%80%89%E6%8B%A9.jpg)

# **Installation sur machine virtuelle**

## **Étape 1 : Télécharger l'image openKylin**

Allez sur le site officiel pour télécharger l'image x86_64 (https://www.openkylin.top/downloads/628-cn.html)

*Astuce : Après avoir téléchargé le fichier image, veuillez d'abord vérifier si la valeur MD5 du fichier correspond à celle du site officiel. Si elle ne correspond pas, veuillez le télécharger à nouveau*

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/1-%E5%AE%98%E7%BD%91%E4%B8%8B%E8%BD%BD%E9%95%9C%E5%83%8F.png)

Le site officiel propose également plusieurs sites miroirs pour le téléchargement (https://www.openkylin.top/downloads/mirrors-cn.html)

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/2-%E9%95%9C%E5%83%8F%E7%AB%99.png)

## **Étape 2 : Créer une machine virtuelle**

Installez un logiciel de machine virtuelle sur le système existant. Dans ce document, nous utiliserons VMware Workstation comme exemple. Cliquez sur "Créer une nouvelle machine virtuelle"

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/24-%E6%89%93%E5%BC%80%E8%99%9A%E6%8B%9F%E6%9C%BA%E8%BD%AF%E4%BB%B6.png)

La configuration par défaut est suffisante, cliquez sur Suivant

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/25-%E8%99%9A%E6%8B%9F%E6%9C%BA%E9%85%8D%E7%BD%AE.png)

Cochez "Installer à partir d'un fichier image ISO", cliquez sur "Parcourir", sélectionnez le fichier image du système d'exploitation openKylin que vous avez téléchargé, puis cliquez sur Suivant

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/26-%E9%80%89%E6%8B%A9%E9%95%9C%E5%83%8F.png)

La configuration par défaut est suffisante, cliquez sur Suivant

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/27-%E9%80%89%E6%8B%A9%E7%B3%BB%E7%BB%9F%E7%B1%BB%E5%9E%8B.png)

Modifiez le nom de la machine virtuelle, choisissez l'emplacement d'installation, puis cliquez sur Suivant

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/28-%E7%BC%96%E8%BE%91%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%90%8D%E7%A7%B0.png)

Ajustez la taille du disque selon les indications, la configuration par défaut du disque virtuel est suffisante, cliquez sur Suivant

*Astuce : L'espace disque alloué ne doit pas être inférieur à 50 Go*

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/29-%E8%AE%BE%E7%BD%AE%E8%99%9A%E6%8B%9F%E6%9C%BA%E7%A3%81%E7%9B%98%E5%A4%A7%E5%B0%8F.png)

Confirmez la configuration de la machine virtuelle. Si vous souhaitez effectuer des ajustements personnalisés, cliquez sur "Personnaliser le matériel". Après avoir confirmé la configuration matérielle, cliquez sur "Terminer"

*Astuce : **Pour garantir une expérience fluide de la machine virtuelle autant que possible,** il est recommandé d'avoir au moins 4 Go de mémoire et au moins 4 cœurs CPU*

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/30-%E7%A1%AE%E8%AE%A4%E7%A1%AC%E4%BB%B6%E9%85%8D%E7%BD%AE.png)

La création de la machine virtuelle est terminée. Cliquez sur : "Démarrer cette machine virtuelle"

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/31-%E5%AE%8C%E6%88%90%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%88%9B%E5%BB%BA.png)

Sélectionnez "Essayer openKylin sans l'installer (T)" pour Sélectionnez "Essayer openKylin sans l'installer (T)" pour entrer dans le bureau d'essai openKylin

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/32-%E5%BC%80%E5%90%AF%E8%99%9A%E6%8B%9F%E6%9C%BA.png)

## **Étape 3 : Commencer l'installation du système**

Sur le bureau d'essai openKylin, cliquez sur l'icône "Installer openKylin" sur le bureau pour commencer la configuration avant l'installation

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/7-%E8%AF%95%E7%94%A8%E6%A1%8C%E9%9D%A2.png)

Sélectionnez la langue

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/8-%E9%80%89%E6%8B%A9%E8%AF%AD%E8%A8%80.png)

Sélectionnez le fuseau horaire

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/40-%E9%80%89%E6%8B%A9%E6%97%B6%E5%8C%BA.png)

Créez un utilisateur

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/9_%E5%88%9B%E5%BB%BA%E7%94%A8%E6%88%B7.png)

Choisissez la méthode d'installation (partitionnement)

1. Installation complète (recommandée pour les débutants)

*Astuce : Vous n'avez pas à vous soucier de perdre des fichiers lors du formatage pour une installation sur machine virtuelle*

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/10_%E9%80%89%E6%8B%A9%E5%AE%89%E8%A3%85%E6%96%B9%E5%BC%8F.png)

2. Installation personnalisée (recommandée pour les utilisateurs avancés)

Cliquez sur "Créer une table de partition" pour effacer les partitions actuelles (*Astuce : À ce stade, l'effacement n'est pas réellement effectué*)

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/11-%E6%B8%85%E7%A9%BA%E5%88%86%E5%8C%BA%E8%A1%A8.png)

Ensuite, créez des partitions selon vos besoins. En général, il faut au moins créer : une partition racine (utilisée pour "ext4", point de montage "/", allocation d'espace supérieure à 15 Go) :

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/21-%E5%88%9B%E5%BB%BA%E6%A0%B9%E5%88%86%E5%8C%BA.png)

Une partition efi (utilisée pour "efi", allocation d'espace de 256 Mo à 2 Go)

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/13-%E5%88%9B%E5%BB%BAefi%E5%88%86%E5%8C%BA.png)

Cliquez sur OK et confirmez la stratégie de partitionnement que vous avez choisie

*Astuce : À ce stade, il est encore temps de changer d'avis**. Après avoir cliqué sur "Commencer l'installation", le ****formatage**** sera d'abord effectué et vous perdrez le contenu original de ce disque dur*

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/14-%E7%A1%AE%E8%AE%A4%E5%88%86%E5%8C%BA%E7%AD%96%E7%95%A5.png)

Commencez le processus d'installation

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/15-%E5%BC%80%E5%A7%8B%E5%AE%89%E8%A3%85%E8%BF%9B%E7%A8%8B.png)

## **Étape 4 : Terminer l'installation**

Une fois la progression de l'installation terminée, vous serez informé que l'installation est terminée.

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/33-%E5%AE%89%E8%A3%85%E5%AE%8C%E6%88%90.png)

Cliquez sur "Redémarrer maintenant", puis suivez les instructions pour retirer la clé USB et appuyez sur la touche "entrée", le système redémarrera automatiquement pour entrer dans le système. À ce moment, vous avez réussi à installer le système openKylin sur votre machine virtuelle !!

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/34-logo%E7%95%8C%E9%9D%A2.png)

## **Vidéo de référence**

*Démonstration vidéo*

# **FAQ sur l'installation du système**

Q : Que faire si l'installation se bloque à 96% ?  

R : Lorsque la progression de l'installation atteint 96%, il faut généralement attendre un peu plus longtemps (le temps exact dépend des performances du matériel). Si le temps d'attente est trop long (plus d'une demi-heure), veuillez vérifier si l'espace alloué à la partition efi est insuffisant, réallouez l'espace et réessayez.

Q : Lors de l'installation à partir d'une clé USB de démarrage, l'option "try openKylin without installation" ou "install openKylin" apparaît, mais après avoir sélectionné "installation" avec Entrée, l'écran devient noir sans aucun affichage. Que faire ?

R : Méthode 1 : L'écran noir peut être dû à un problème de prise en charge de l'affichage de la carte graphique, essayez de corriger manuellement.

Déplacez le curseur sur "install openKylin", appuyez sur "e" pour entrer en mode édition et passer en mode ligne de commande,

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/35-%E5%AE%89%E8%A3%85grub%E7%95%8C%E9%9D%A2.jpeg)

Trouvez "quite splash" et ajoutez "nomodeset" après, puis appuyez sur la touche F10 pour installer.

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/36-%E5%AE%89%E8%A3%85grub%E7%95%8C%E9%9D%A2-2.jpeg)

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/37-%E5%AE%89%E8%A3%85grub%E7%95%8C%E9%9D%A2-3.png)

Astuce : Ajoutez différentes options de pilote graphique en fonction de la carte graphique utilisée, par exemple, si vous utilisez une carte graphique Nvidia, ajoutez nomodeset.

Q : Après l'installation du système, un message indique qu'aucune carte réseau sans fil n'a été détectée. Que faire ?

R : Redémarrez, puis dans l'interface grub, sélectionnez les options avancées et choisissez le noyau version 5.15 pour redémarrer le système. Vérifiez si le réseau est revenu à la normale.

Q : Que faire si l'installation échoue à la dernière étape ?

R : Généralement, cela est dû à un fichier image endommagé lors du téléchargement ou de la copie. En cas d'erreur d'installation, veuillez d'abord vérifier si la valeur MD5 du fichier image correspond à celle du site officiel. Méthode pour calculer la valeur MD5 :

Sur un système Linux, appuyez sur Win+T pour ouvrir le terminal et exécutez la commande :

`md5sum openkylin-1.0-x86_64.iso`

Sur un système Windows, tapez cmd dans la barre de recherche pour ouvrir le terminal et exécutez la commande :

`certUtil -hashfile openkylin-1.0-x86_64.iso MD5`

Q : Impossible d'entrer dans l'interface grub après avoir sélectionné le disque de démarrage

R : Veuillez vérifier si le démarrage sécurisé (secure boot) a été désactivé dans les paramètres du BIOS

Q : Que faire s'il n'y a pas d'interface de sélection du système de démarrage après l'installation du double système ?

R : Il est possible qu'il y ait un problème avec l'élément de démarrage. Vous pouvez télécharger et installer le logiciel EasyBCD pour réparer l'élément de démarrage.

Q : Lors de l'installation personnalisée, après avoir configuré les partitions, un message indique qu'il n'y a pas de partition racine ou de partition efi. Que faire ?

R : La partition racine correspond à "/", la partition efi doit être définie sur le type "efi" lors du partitionnement. Ces deux partitions doivent être créées. De plus, la partition de données (correspondant à "/data"), la partition de sauvegarde (correspondant à "/backup") et la partition d'échange (linux-swap) doivent être créées selon vos besoins.

Q : Que faire si un message indique "Il ne peut y avoir qu'une seule partition efi" après avoir configuré les partitions ?

R : Il est probable que le système Windows existant ait déjà une partition efi. Pour une installation en double système, nous devons supprimer la partition efi que nous avons ajoutée. Pour une installation en système unique (le système Windows sera écrasé), supprimez la partition efi existante et ajoutez une nouvelle partition efi.

Q : Comment savoir si le BIOS de mon ordinateur est UEFI ou Legacy ?

R : En prenant l'exemple du système Windows, appuyez sur les touches de raccourci "Win+R" et appuyez sur Entrée pour confirmer, saisissez "msinfo32", appuyez sur Entrée, l'interface d'information système apparaîtra, vous pourrez voir le mode BIOS, comme illustré ci-dessous :

![Saisir la description de l'image](assets/%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85/38-biso%E6%9F%A5%E7%9C%8B.png)

# **Annexe**

Installation du système d'exploitation openKylin sur une carte de développement RISC-V

[Installation d'openKylin sur riscv](./riscv上安装openKylin.md)

Installation du système d'exploitation openKylin sur une carte de développement ARM

[Installation d'openKylin sur arm](./arm上安装openKylin.md)