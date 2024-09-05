
# Gestionnaire de fichiers openKylin

Le gestionnaire de fichiers openKylin, en tant que logiciel offrant une interface utilisateur pour la gestion des fichiers, aide les utilisateurs à gérer les fichiers stockés localement et sur le réseau. Il permet également une série d'opérations de base telles que renommer des fichiers, créer des fichiers, ouvrir des fichiers, consulter des fichiers, modifier des fichiers, déplacer des fichiers et supprimer des fichiers. Voici la commande d'installation du gestionnaire de fichiers openKylin et des packages associés :

```shell
sudo apt install peony peony-extensions
```

Découvrons maintenant la structure et les fonctionnalités du gestionnaire de fichiers openKylin.

## 1. Présentation du gestionnaire de fichiers openKylin

### 1. Structure du gestionnaire de fichiers

La structure du système de fichiers openKylin est différente de celle de Windows, car il n'y a pas de concept de lettre de lecteur (la lettre de lecteur étant un identifiant pour les périphériques de stockage dans les systèmes DOS et Windows). Le système openKylin choisit de stocker tous les fichiers dans le répertoire racine.

![Image](https://www.openkylin.top/upload/202302/1675907521938004.png)

### 2. Commandes du gestionnaire de fichiers

Comme on le sait, les commandes Linux permettent de comprendre le système. Cependant, pour les utilisateurs ordinaires, si le système ne permet de parcourir les répertoires de fichiers ou d'accéder aux fichiers qu'à travers la ligne de commande, cela peut ne pas être très pratique. Voici quelques comparaisons entre la navigation avec un gestionnaire de fichiers et l'utilisation de la ligne de commande.

1. Entrer dans le répertoire personnel

![Image](https://www.openkylin.top/upload/202302/1675907534741650.png)

2. Afficher les dossiers et fichiers cachés

![Image](https://www.openkylin.top/upload/202302/1675907550991843.png)

3. Consulter les propriétés d'un fichier

![Image](https://www.openkylin.top/upload/202302/1675907569674163.png)

## 2. Fonctionnalités du gestionnaire de fichiers openKylin

### 1. Sélection multiple (inversée ou non)

Dans l'utilisation quotidienne, il est souvent nécessaire de sélectionner plusieurs fichiers en même temps, parfois de manière continue ou non. Voici comment procéder dans ces différentes situations :

1. **Sélectionner tout** : Comme la plupart des utilisateurs le savent, Windows propose le raccourci universel "Ctrl+A" pour sélectionner tous les fichiers. OpenKylin prend également en charge cette opération.

![Image](https://www.openkylin.top/upload/202302/1675907581574034.png)

2. **Sélection multiple continue** : Lorsque vous devez sélectionner plusieurs fichiers consécutifs dans un dossier, "Ctrl+A" ne suffit pas. Il suffit de cliquer sur le premier fichier, de maintenir la touche `Shift`, puis de cliquer sur le dernier fichier. Tous les fichiers entre ces deux fichiers seront alors sélectionnés.

![Image](https://www.openkylin.top/upload/202302/1675907593481598.png)

3. **Sélection multiple non continue** : Si les fichiers à sélectionner ne sont pas consécutifs, vous n'avez pas besoin de les sélectionner un par un. Il suffit de maintenir la touche `Ctrl` et de cliquer sur chaque fichier que vous ne voulez pas sélectionner, puis d'inverser la sélection avec la souris. Les fichiers restants seront ceux que vous souhaitez sélectionner.

![Image](https://www.openkylin.top/upload/202302/1675907634477814.png)

![Image](https://www.openkylin.top/upload/202302/1675907646763404.png)

### 2. Recherche rapide de fichiers

1. **Fichiers récemment utilisés** : Si vous devez ouvrir un document que vous avez récemment utilisé mais que son chemin est compliqué, vous pouvez utiliser la fonction "Fichiers récemment utilisés" pour localiser rapidement le fichier. Le système affiche une liste des documents récemment ouverts ainsi que leur chemin de stockage.

![Image](https://www.openkylin.top/upload/202302/1675907659317516.png)

2. **Recherche** : Si vous devez retrouver un fichier utilisé il y a longtemps, vous pouvez rechercher un mot-clé dans le nom du fichier et le retrouver dans les résultats de recherche.

![Image](https://www.openkylin.top/upload/202302/1675907676529748.png)

### 3. Copier le chemin d'un fichier

Si vous avez besoin d'enregistrer le chemin d'un fichier, ouvrez une fenêtre de terminal, cliquez sur le nom du dossier en haut de la fenêtre du gestionnaire de fichiers, cliquez avec le bouton droit pour copier, puis collez-le dans la fenêtre du terminal. Vous obtiendrez ainsi le chemin exact du fichier.

![Image](https://www.openkylin.top/upload/202302/1675907689750834.png)

Voilà pour cette présentation des fonctionnalités de base du gestionnaire de fichiers openKylin. Nous espérons que cela vous sera utile. Le gestionnaire de fichiers openKylin continue d'être amélioré et mis à jour. Si vous avez des suggestions ou des besoins, vous pouvez les soumettre sous forme d'Issue sur le dépôt Gitee openKylin.

