## 【Cours pour Débutants】 Guide d'utilisation d'openKylin : Apprenez à personnaliser !

Le guide utilisateur d'openKylin est un manuel détaillant les fonctionnalités et l'interface utilisateur du système d'exploitation openKylin, permettant aux utilisateurs de comprendre comment utiliser le logiciel. En lisant ce guide, vous pourrez rapidement et efficacement prendre en main openKylin. Aujourd'hui, nous allons vous expliquer brièvement le principe de fonctionnement du guide utilisateur openKylin et comment personnaliser son contenu.

![Image](https://www.openkylin.top/upload/202301/1673400351868967.png)

### 1. Présentation du principe de fonctionnement du guide utilisateur

#### 1. Introduction à QtWebkit

Le lancement, l'affichage et la navigation dans le guide utilisateur sur le système openKylin sont réalisés à l'aide de QtWebkit. Voici une brève introduction à QtWebkit. Le module QtWebkit fournit un moteur de navigateur web dans Qt, ce qui facilite l'utilisation de contenu du Web dans des applications Qt. Le contenu des pages web peut être contrôlé via des contrôles natifs. QtWebKit permet d'afficher des documents HTML, XHTML et SVG, stylisés avec CSS, et d'utiliser des scripts JavaScript.

#### 2. Interface de navigation du guide utilisateur

Afin de permettre aux utilisateurs de naviguer vers la documentation d'aide correspondante des différents composants, le guide utilisateur offre une interface permettant à d'autres composants d'ouvrir directement la page souhaitée. Les composants peuvent invoquer cette interface via les touches `F1` ou en accédant au menu "Aide". La méthode `DaemonIpcDbus::showGuide` fournit une interface DBus ; il suffit de transmettre les paramètres correspondants, et le guide utilisateur affichera la section adéquate en fonction de ces paramètres.

#### 3. Processus de fonctionnement du guide utilisateur

Après avoir introduit l'interface de navigation, nous allons maintenant détailler le processus global de fonctionnement du guide utilisateur.

Premièrement, il est nécessaire d'instancier `QWebView`, d'activer ou désactiver certains paramètres, et de charger les fichiers HTML du guide utilisateur.

![Image](https://www.openkylin.top/upload/202301/1673400403559673.png)
![Image](https://www.openkylin.top/upload/202301/1673400415449881.png)

Ensuite, lors du chargement des fichiers HTML, un signal est envoyé pour transmettre l'objet `QObject` à JavaScript, permettant à ce dernier d'invoquer les fonctions `public slots` de `QObject`.

![Image](https://www.openkylin.top/upload/202301/1673400425220458.png)
![Image](https://www.openkylin.top/upload/202301/1673400459129591.png)
![Image](https://www.openkylin.top/upload/202301/1673400473845933.png)

Puis, l'interface Qt est invoquée par la partie web pour obtenir les informations des documents et la structure du répertoire, générant ainsi dynamiquement la page d'accueil. JavaScript appelle les fonctions Qt pour récupérer ces informations :

![Image](https://www.openkylin.top/upload/202301/1673400482997460.png)

Enfin, Qt récupère la structure des fichiers du guide, ainsi que les noms des icônes et dossiers, ainsi que le chemin des documents associés :

![Image](https://www.openkylin.top/upload/202301/1673400491336760.png)

Les noms des icônes, les chemins des documents et les dossiers sont alors utilisés pour charger les informations d'icône et finaliser le chargement de la page d'accueil. Le processus de chargement de la page d'accueil du guide utilisateur openKylin est ainsi résumé, mais il implique également le rendu des fichiers Markdown, la navigation dans les contenus, la gestion des niveaux de titre, et la lecture automatique des dates de mise à jour des documents, ce qui ne sera pas détaillé ici.

### 2. Personnalisation du contenu du guide utilisateur

Lors de l'intégration de nouveaux composants dans le système, il est souvent souhaitable d'ajouter automatiquement les documents d'aide des nouveaux composants dans le guide utilisateur. Voici comment intégrer votre propre contenu de guide utilisateur !

#### 1. Structure des dossiers

![Image](https://www.openkylin.top/upload/202301/1673400502196692.png)

Il est nécessaire d'inclure un dossier de langue et une icône. Le nom du dossier et de l'icône doivent être identiques, car ils seront utilisés pour l'affichage de l'icône sur la page d'accueil et pour appeler l'interface DBus du guide utilisateur.

#### 2. Structure des fichiers Markdown

![Image](https://www.openkylin.top/upload/202301/1673400514730147.png)

Le titre de premier niveau des documents Markdown sera utilisé comme nom affiché sur la page d'accueil de l'application.

#### 3. Modification du fichier `install`

![Image](https://www.openkylin.top/upload/202301/1673400526886467.png)

Placez le dossier préparé dans les fichiers de ressources du guide utilisateur. Lors du chargement du guide utilisateur, ces fichiers seront automatiquement lus et affichés sur la page d'accueil. En suivant ces étapes, vous pouvez personnaliser le contenu du guide utilisateur, et ajouter ou supprimer des documents d'aide pour les composants du système en fonction des besoins ! Vous avez compris ?

---

Source : Xie Jiahua  
Vérification : openKylin
Traduction Fr : Zeima