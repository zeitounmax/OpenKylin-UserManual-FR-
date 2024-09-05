# <center>20 faits amusants sur les commandes Linux et le terminal Linux</center>
#### <center>Auteur : Petit K</center>
#### <center>2022-04-22 23:36:00</center>

Jouer avec Linux est infiniment amusant ! Ha ha ! Vous ne me croyez pas ? Rappelez-vous mes paroles, à la fin de l'article, vous croirez que Linux est vraiment amusant.
![](https://www.ubuntukylin.com/upload/images/tl0.png)

### Commande : sl (locomotive à vapeur)
Vous connaissez probablement la commande 'ls' et l'utilisez souvent pour voir le contenu des dossiers. Mais parfois, vous pourriez l'écrire comme 'sl'. Comment pouvons-nous nous amuser un peu au lieu de voir "commande non trouvée" ?

Installation de sl     
![](https://www.ubuntukylin.com/upload/images/tl1.png)
Sortie
![](https://www.ubuntukylin.com/upload/images/tl2.png) 
![](https://www.ubuntukylin.com/upload/images/t13.png)

Cette commande s'exécutera également lorsque vous tapez 'LS' au lieu de 'ls'.

### Commande : telnet

Non ! Non !! Ce n'est pas aussi compliqué que d'habitude. Vous connaissez peut-être telnet, Telnet est un protocole réseau bidirectionnel basé sur du texte. Ici, vous n'avez pas besoin d'installer quoi que ce soit, vous avez juste besoin d'un système Linux et d'une connexion réseau.
![](https://www.ubuntukylin.com/upload/images/t14.png)
![](https://www.ubuntukylin.com/upload/images/tl5.png)

### Commande : fortune

Testez votre chance inconnue, il y a parfois des choses amusantes et intéressantes dans le terminal.

Installation de fortune
![](https://www.ubuntukylin.com/upload/images/tl6.png)

### Commande : rev (inverser)

Elle inverse chaque chaîne qui lui est transmise, sortant la chaîne à l'envers, n'est-ce pas amusant ?
![](https://www.ubuntukylin.com/upload/images/tl7.png)

### Commande : factor

Il est temps de parler un peu de mathématiques, cette commande affiche tous les facteurs du nombre donné.

![](https://www.ubuntukylin.com/upload/images/tl8.png)

### Commande : script

Bien, ce n'est pas une commande, mais un script, un script très intéressant.

![](https://www.ubuntukylin.com/upload/images/tl9.png)

### Commande : cowsay

## cowsay

Une petite vache composée de caractères ASCII dans le terminal, cette petite vache dira ce que vous voulez qu'elle dise.

Installation de cowsay
![](https://www.ubuntukylin.com/upload/images/tl10.png)

Sortie

![](https://www.ubuntukylin.com/upload/images/tl11.png)

Que se passe-t-il si vous redirigez la commande 'fortune' vers cowsay en utilisant un tube ?

Entrée

> $ root@tecmint:~# fortune | cowsay

![](https://www.ubuntukylin.com/upload/images/tl12.png)

Astuce : '|' est le caractère de commande de tube, il prend généralement la sortie d'une commande comme entrée de la commande suivante. Dans l'exemple ci-dessus, la sortie de 'fortune' est l'entrée de la commande 'cowsay'. Les commandes de tube sont souvent utilisées dans l'écriture de scripts et de programmes.

## xcowsay

xcowsay est un programme d'interface graphique, similaire à cowsay, mais exprimé de manière graphique, on peut dire que c'est la version X de cowsay.

![](https://www.ubuntukylin.com/upload/images/tl13.png)

Sortie

![](https://www.ubuntukylin.com/upload/images/tl14.png)

![](https://www.ubuntukylin.com/upload/images/tl15.png)

## cowthink

cowthink est une autre commande, exécutez "cowthink Linux is sooo funny" et voyez la différence avec cowsay.
![](https://www.ubuntukylin.com/upload/images/tl16.png)

Sortie

![](https://www.ubuntukylin.com/upload/images/tl17.png)

### Commande : yes

yes est une commande très amusante et utile, surtout pour l'écriture de scripts et les administrateurs système, elle peut générer automatiquement des réponses prédéfinies ou les transmettre au terminal.

![](https://www.ubuntukylin.com/upload/images/tl18.png)

Astuce : (Ne s'arrête que lorsque vous appuyez sur Ctrl+C)

### Commande : toilet

Quoi, vous plaisantez ? Bien sûr que non ! Mais il est certain que le nom de cette commande est trop drôle, je ne sais pas non plus d'où vient le nom de cette commande.

Installation de toilet

![](https://www.ubuntukylin.com/upload/images/tl19.png)

Sortie

![](https://www.ubuntukylin.com/upload/images/tl20.png)

Cette commande offre même quelques couleurs et formats de police.

![](https://www.ubuntukylin.com/upload/images/tl21.png)

Astuce : Figlet est une autre commande qui produit des effets similaires à toilet.

### Commande : cmatrix

Vous avez peut-être vu le film hollywoodien "Matrix" et êtes fasciné par la capacité de Neo à voir tout ce qui se passe dans la matrice, ou vous pensez à une image vivante d'un bureau semblable à 'Hacker'.

Installation de cmatrix

![](https://www.ubuntukylin.com/upload/images/tl23.png)

Sortie

![](https://www.ubuntukylin.com/upload/images/tl24.png)

### Commande : oneko

Vous croyez peut-être que le pointeur de la souris Linux est toujours le même noir ou blanc, pas du tout vivant, mais vous avez tort. "oneko" est un package qui transformera "Jerry" en pointeur de souris et le fixera à votre souris.

Installation de oneko

![](https://www.ubuntukylin.com/upload/images/tl25(1).png)

Sortie

![](https://www.ubuntukylin.com/upload/images/tl26.png)

![](https://www.ubuntukylin.com/upload/images/tl27.png)

Astuce : Lorsque vous fermez le terminal exécutant oneko, Jerry disparaîtra également et ne réapparaîtra pas au redémarrage du terminal. Vous pouvez ajouter ce programme aux options de démarrage et continuer à l'utiliser.

### Bombe fork

C'est un code très malveillant, vous êtes responsable des conséquences de l'exécution de cette commande. Cette commande est en fait une bombe fork, elle se multipliera de manière exponentielle jusqu'à ce que toutes les ressources système soient utilisées ou que le système se bloque (si vous voulez voir la puissance de cette commande, vous pouvez l'essayer une fois, mais à vos risques et périls, n'oubliez pas de fermer et de sauvegarder tous les autres programmes et fichiers avant de l'exécuter).

![](https://www.ubuntukylin.com/upload/images/tl28.png)

### Commande : while

La commande "while" suivante est un script qui peut vous fournir des dates et des fichiers colorés jusqu'à ce que vous appuyiez sur la touche d'interruption (Ctrl+C).

Copiez et collez cette commande dans votre terminal.

![](https://www.ubuntukylin.com/upload/images/tl29.png)

![](https://www.ubuntukylin.com/upload/images/tl30.png)

Astuce : Le script ci-dessus produira une sortie similaire avec la modification suivante, mais c'est un peu différent, essayez-le dans votre terminal.

![](https://www.ubuntukylin.com/upload/images/tl31.png)

### Commande : espeak

Montez le volume de vos haut-parleurs multimédia au maximum, copiez cette commande dans votre terminal et voyez votre réaction lorsque vous entendrez la voix de Dieu.

Installation d'espeak

![](https://www.ubuntukylin.com/upload/images/tl32.png)

Sortie

![](https://www.ubuntukylin.com/upload/images/tl33.png)

### Commande : aafire

Que diriez-vous de mettre le feu à votre terminal. Tapez "aafire" dans votre terminal, sans guillemets, et voyez cette scène magique.
Installation d'aafire

![](https://www.ubuntukylin.com/upload/images/tl34.png)

Sortie

![](https://www.ubuntukylin.com/upload/images/tl35.png)


![](https://www.ubuntukylin.com/upload/images/tl36.png)

Appuyez sur n'importe quelle touche pour terminer le programme.

### Commande : bb

Tout d'abord, installez "apt-get install bb", puis tapez "bb" et voyez ce qui se passe.

![](https://www.ubuntukylin.com/upload/images/tl37.png)

![](https://www.ubuntukylin.com/upload/images/tl38.png)

### Commande : url

Ne serait-il pas cool de changer votre statut Twitter en ligne de commande devant vos amis ? Remplacez simplement username, password et "your status message" par votre nom d'utilisateur, mot de passe et le statut que vous souhaitez.

![](https://www.ubuntukylin.com/upload/images/tl39.png)

### ASCIIquarium

Vous voulez créer un aquarium dans le terminal, comment faire ?

![](https://www.ubuntukylin.com/upload/images/tl40.png)


Téléchargez et installez ASCIIquarium.

![](https://www.ubuntukylin.com/upload/images/tl41.png)

Enfin, exécutez "asciiquarium" ou "/usr/local/bin/asciiquarium" dans le terminal, sans guillemets, et une scène magique se déroulera sous vos yeux.

![](https://www.ubuntukylin.com/upload/images/tl42.png)

![](https://www.ubuntukylin.com/upload/images/tl43.png)

### Commande : funny manpages
Tout d'abord, installez "apt-get install funny-manpages" puis exécutez le manuel des commandes suivantes.

![](https://www.ubuntukylin.com/upload/images/tl44.png)

### Linux Tweaks
Il est temps de faire quelques optimisations.

![](https://www.ubuntukylin.com/upload/images/tl45.png)

Linux est toujours sexy : who | grep -i blonde | date; cd ~; unzip; touch; strip; finger; mount; gasp; yes; uptime; umount; sleep (si vous voyez ce que je veux dire, sueur !)

Il y a aussi d'autres commandes, mais elles ne peuvent pas s'exécuter sur tous les systèmes, donc cet article ne les a pas couvertes. Par exemple, dog, filter, banner.

Amusez-vous bien, vous pourrez me remercier plus tard :)