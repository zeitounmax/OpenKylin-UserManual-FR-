
# <center>Opérations de base sur les fichiers Linux</center>
#### Auteur : winifred
#### Date : 15.03.2023

## I. Répertoires de fichiers

### 1.1 Commande ls
La commande `ls` est utilisée pour afficher le contenu du répertoire courant. Elle permet de voir les permissions des fichiers (y compris les répertoires, dossiers et fichiers), ainsi que les informations sur les répertoires.
Format de la commande : `ls [options] [nom_du_répertoire]`
Options courantes :
- -a : Liste tous les fichiers, y compris les fichiers cachés commençant par .
- -l : Liste les permissions, le propriétaire, la taille des fichiers et autres informations
- -t : Trie les fichiers par date de modification

### 1.2 Commande cd
La commande `cd` est utilisée pour changer le répertoire courant vers un répertoire spécifié.
Format de la commande : `cd [nom_du_répertoire]`
Exemples courants :
- `cd /` : Change vers le répertoire racine
- `cd ..` : Remonte d'un niveau dans l'arborescence
- `cd ~` : Change vers le répertoire personnel de l'utilisateur

### 1.3 Commande pwd
La commande `pwd` est utilisée pour déterminer l'emplacement exact du répertoire courant dans le système de fichiers.
Format de la commande : `pwd [options]`
Options courantes :
- -P : Affiche le chemin physique réel
- -L : Affiche le chemin symbolique lorsque le répertoire est un lien symbolique

## II. Opérations de base sur les fichiers

### 2.1 Créer un fichier
Utilisez les commandes `touch` ou `vi` pour créer un fichier vide.
Exemples courants :
```linux
touch test.md
vi test.md
```

### 2.2 Créer un répertoire
Utilisez la commande `mkdir` pour créer un répertoire vide. Vous pouvez également spécifier les permissions du répertoire lors de sa création.
Exemples courants :
- Créer un répertoire vide nommé mydir : `mkdir mydir`
- Créer une arborescence de répertoires : `mkdir -p A/B/C`

### 2.3 Supprimer des fichiers/répertoires
Utilisez la commande `rm` pour supprimer un fichier. Utilisez l'option -f pour forcer la suppression, et l'option -r pour supprimer un répertoire.
Exemples courants :
- Supprimer le fichier test.md : `rm test.md`
- Forcer la suppression du fichier test.md : `rm -f test.md`
- Supprimer le dossier A : `rm -r A`

### 2.4 Copier des fichiers/répertoires
Utilisez la commande `cp` pour copier un fichier vers un répertoire spécifié. Utilisez l'option -r pour copier un répertoire.
Format de la commande :
- Copier un fichier : `cp [nom_du_fichier] [répertoire]`
- Copier un répertoire : `cp -r [nom_du_fichier] [répertoire]`
Exemples courants :
- Copier le fichier test.md dans le dossier C : `cp test.md /home/user/A/B/C`
> Il faut être dans le répertoire contenant test.md
- Copier A dans le dossier alphabet : `cp -r A alphabet`

### 2.5 Déplacer et renommer des fichiers
Utilisez la commande `mv` pour déplacer (couper) ou renommer des fichiers
Format de la commande :
- Déplacer un fichier : `mv [fichier_source] [répertoire_destination]`
- Renommer un fichier : `mv [ancien_nom] [nouveau_nom]`
Exemples courants :
- Déplacer le fichier file vers le répertoire filedir : `mv file filedir`
- Renommer le fichier file1 en file2 : `mv file1 file2`

### 2.6 Afficher le contenu d'un fichier

#### 2.6.1 Commandes cat, tac
Les commandes `cat` et `tac` sont utilisées pour afficher le contenu d'un fichier sur la sortie standard (terminal). cat affiche dans l'ordre des lignes, tac dans l'ordre inverse.
Exemples courants :
- Afficher le contenu du fichier /etc/passwd : `cat /etc/passwd`
- Afficher avec numéros de ligne : `cat -n /etc/passwd`

#### 2.6.2 Commande more
`more` est utilisé pour "lire" le contenu d'un fichier. Vous pouvez utiliser Enter pour faire défiler, Space pour tout afficher, q pour quitter.

#### 2.6.3 Commandes head, tail
Affichent les premières ou dernières lignes d'un fichier. L'option -n spécifie le nombre de lignes à afficher.

### 2.7 Commande grep
La commande `grep` est utilisée pour trouver du texte correspondant dans un fichier. Elle accepte les expressions régulières et les jokers, et peut utiliser diverses options pour générer différents formats de sortie.
Format de la commande : `grep [options] motif [fichier]`
Exemples courants :
- Extraire les lignes contenant "root" dans /etc/passwd : `grep "root" /etc/passwd`
- Extraire les lignes ne contenant pas "root" dans /etc/passwd : `grep -v "root" /etc/passwd`
