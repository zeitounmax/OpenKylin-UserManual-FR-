

# <center>Utilisation de base de SSH</center>
#### <center>Auteur : Petit K</center>
#### <center>2022-04-22 23:36:00</center>

1. SSH est l'abréviation de Secure Shell, établi par le Network Working Group de l'IETF. SSH est un protocole de sécurité basé sur la couche application, généralement utilisé pour se connecter à des hôtes distants. Il intègre un module sftp pour le transfert de fichiers chiffrés, utilisant également le port 22, offrant une meilleure sécurité que le ftp non chiffré. La commande la plus simple est :
    ```sh
    ssh nom_utilisateur@adresse_hôte
    ```
    Bien sûr, cela suppose que le port est par défaut. Si le port du serveur SSH n'est pas le port par défaut, la commande devient :
    ```sh
    ssh nom_utilisateur@adresse_hôte -p numéro_port
    ```
    Ensuite, il vous sera demandé d'entrer un mot de passe. Ce mot de passe est celui de l'utilisateur que vous essayez de connecter. Cet utilisateur doit exister et être autorisé à se connecter. Généralement, les utilisateurs créés lors de l'installation du système fonctionnent bien, et le mot de passe n'est pas affiché par défaut.

    [Le reste de la traduction suit...]

2. Un petit truc pour SSH : si SSH ne répond pas pendant un certain temps, vous pouvez modifier le temps de battement de cœur. [...]

3. Si c'est la première fois que vous vous connectez à un hôte, un message d'avertissement apparaîtra, comme ceci : [...]

4. Les utilisateurs qui n'ont jamais essayé pourraient découvrir qu'ils sont refusés lorsqu'ils tentent de se connecter à distance à leur propre machine. [...]

5. Utilisation de la clé publique pour se connecter à SSH

   Avantages de l'utilisation de la clé publique pour se connecter :
   * Pas besoin de mémoriser un mot de passe, pas besoin de saisir un mot de passe à chaque connexion
   * Meilleure sécurité, car les mots de passe peuvent être divulgués ou forcés par force brute, ce qui n'est pas le cas des clés publiques
   * Les outils de développement à distance comme VS Code nécessitent une connexion par clé publique

   Méthode pour utiliser la connexion par clé publique :
   1. Si vous n'avez jamais généré de clé publique auparavant, générez-en une d'abord. [...]
    
   2. Configurer le serveur SSH pour autoriser la connexion par clé publique [...]

   3. Écrire la clé publique du client dans le serveur SSH [...]

   4. Vérification [...]

Cet article a été rédigé en référence aux articles suivants :

[https://forum.openkylin.top/forum.php?mod=viewthread&tid=193097](https://forum.openkylin.top/forum.php?mod=viewthread&tid=193097)

[https://forum.openkylin.top/forum.php?mod=viewthread&tid=193211](https://forum.openkylin.top/forum.php?mod=viewthread&tid=193211)