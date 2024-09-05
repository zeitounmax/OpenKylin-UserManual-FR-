Voici la traduction en français :

# <center>Easter egg — Utiliser sudo pour offenser les utilisateurs</center>
#### <center>Auteur : Petit K</center>
#### <center>22-04-2022 23:36:00</center>

On peut configurer sudo (utilisé pour autoriser les commandes) pour offenser les utilisateurs qui entrent un mauvais mot de passe. Voyons comment faire !

Tout d'abord, ouvrez un terminal et entrez la commande suivante :

**sudo visudo**

Il est fortement recommandé d'utiliser visudo pour éditer le fichier de configuration /etc/sudoers. Il y a deux raisons d'utiliser visudo : premièrement, il empêche deux utilisateurs de le modifier simultanément ; deuxièmement, il aide à vérifier si les modifications sont correctes. visudo ne sauvegarde pas automatiquement un fichier de configuration contenant des erreurs de syntaxe, il vous signalera les problèmes et vous demandera comment procéder. Par défaut, visudo ouvre le fichier de configuration dans vi, mais dans Ubuntu Kylin, visudo ouvre /etc/sudoers dans nano par défaut.

Ensuite, ajoutez cette ligne en haut du fichier :

**Defaults insults**

Comme montré dans l'image :　　

![](https://www.ubuntukylin.com/upload/201602/1456191668463393.jpg)

Ensuite, sauvegardez et fermez le fichier. Si vous utilisez nano, appuyez sur Ctrl+X pour quitter, il vous demandera si vous voulez sauvegarder les modifications. Appuyez sur Y pour sauvegarder.

Puis, dans le terminal, entrez sudo -k pour vider le cache des mots de passe. Enfin, entrez un mot de passe incorrect dans une commande sudo :

![](https://www.ubuntukylin.com/upload/201602/1456192456272768.jpg)

Regardez, il s'impatiente avec moi ! Maintenant, vous pouvez aussi vous plaindre à vos amis que votre sudo vous maltraite ^_^.