# 【Cours pour débutants】Comment configurer les polices sur openKylin

Beaucoup de personnes trouvent la configuration des polices difficile lorsqu’elles utilisent un système Linux, par exemple, l’absence de polices chinoises, l’impossibilité de changer les polices ou de régler la taille des polices. Cependant, sur openKylin, ces problèmes peuvent être rapidement résolus. Suivez-moi pour découvrir comment configurer les polices sur openKylin.

## 1、Configurer les polices du système

Le système openKylin prend en charge l’environnement chinois sans nécessiter de téléchargement manuel et intègre plusieurs polices chinoises et anglaises. Ouvrez Paramètres -> Polices pour voir les polices actuellement utilisées par le système. Vous verrez deux interfaces de sélection de polices : “Sélection de polices” et “Polices à largeur fixe”.

![图片](https://www.openkylin.top/upload/202303/1677638058623502.png)

L’interface “Sélection de polices” est principalement responsable des polices utilisées par l’interface système et les logiciels. La plupart des applications utilisent cette police par défaut.

L’interface “Polices à largeur fixe” est principalement responsable de l’affichage des commandes et des sorties dans le terminal. Cette option n’est effective que pour les applications spécifiant l’utilisation de polices à largeur fixe. Les polices à largeur fixe sont celles où chaque lettre a la même largeur, et généralement, deux lettres ont une largeur égale à celle d’un caractère chinois. Ces polices permettent d’aligner facilement les indentations dans les textes multi-lignes, facilitant ainsi la mise en page et l’édition. Notez qu’openKylin n’affiche pas toutes les polices installées dans l’interface de paramètres ci-dessus, mais seulement quelques polices esthétiques et pratiques. Pour voir d’autres polices, vous pouvez entrer la commande ''sudo apt-get install kylin-font-viewer'' dans le terminal pour installer le gestionnaire de polices. Une fois installé, vous pouvez trouver l’application “Gestionnaire de polices” dans le menu système et l’ouvrir pour voir toutes les polices du système.

![图片](https://www.openkylin.top/upload/202303/1677638068353445.png)

## 2、Installer de nouvelles polices
Le système openKylin permet aux utilisateurs d’installer des polices. Cliquez sur le signe “+” en haut à gauche du “Gestionnaire de polices” mentionné ci-dessus, sélectionnez une police et confirmez. Vous pourrez alors voir les polices installées par l’utilisateur dans l’option “Mes polices”, comme illustré ci-dessous.

![图片](https://www.openkylin.top/upload/202303/1677638082434819.png)

## 3、Configurer les polices de l’éditeur de texte et du terminal

Le terminal openKylin et l’éditeur de texte permettent de configurer les polices individuellement. Ouvrez MATE Terminal -> cliquez sur le bouton “Éditer” -> sélectionnez “Préférences du profil”, vous verrez l’interface suivante. Décochez “Utiliser la police à largeur fixe du système” pour choisir la police et la taille affichées dans le terminal.

![图片](https://www.openkylin.top/upload/202303/1677638129164346.png)

Pour l’éditeur de texte, cliquez sur le bouton “Éditer”, sélectionnez “Préférences”, et dans l’onglet “Polices et couleurs”, changez la catégorie et la taille de la “Police de l’éditeur”.

![图片](https://www.openkylin.top/upload/202303/1677638141258517.png)

## 4、Configurer les polices pour des applications spécifiques

Les applications comme le terminal et l’éditeur de texte mentionnées ci-dessus offrent des fonctionnalités de sélection de polices. Pour les applications qui ne fournissent pas cette fonctionnalité, nous pouvons également configurer l’utilisation de polices spécifiques, mais cela est un peu plus complexe.

Pensez à cette question : si un développeur spécifie une police noire dans une application, mais que notre système a plusieurs bibliothèques de polices noires (ou n’a pas de police noire), quelle police sera affichée par l’application ?

C’est là que l’outil fonts.conf entre en jeu.

L’outil de configuration des polices fonts.conf d’openKylin permet de nombreuses opérations liées aux polices. Aujourd’hui, nous allons voir comment utiliser fonts.conf pour configurer la police noire de Firefox en utilisant la police HuaWen Hei du système.

La configuration de fonts.conf se trouve dans le répertoire /etc/fonts.

Tout d’abord, utilisez le gestionnaire de fichiers pour ouvrir le dossier /etc/fonts. Vous verrez le fichier fonts.conf. Ouvrez-le pour voir le texte de configuration au format XML. En lisant, vous découvrirez que notre fichier de configuration des polices se trouve dans le répertoire conf.d du répertoire actuel.

Ouvrez le répertoire conf.d, où sont stockés tous les fichiers de configuration des polices du système. Copiez-en un et renommez-le en 59-firefox-sans.conf. “sans” signifie sans empattement, avec des traits droits et lisses, et la police noire en est un type classique. Ce fichier est destiné à configurer la police noire de Firefox. Entrez le contenu suivant pour remplacer la police noire par défaut par la police HuaWen Hei.

![图片](https://www.openkylin.top/upload/202303/1677638196400944.png)

L’effet d’affichage est illustré ci-dessous :

![图片](https://www.openkylin.top/upload/202303/1677638216993160.png)

Vous pouvez voir que nous avons remplacé la police noire par défaut de l’interface du navigateur par la police HuaWen Hei. Avez-vous appris quelque chose ? Si vous avez d’autres questions, n’hésitez pas à nous le faire savoir dans les commentaires !