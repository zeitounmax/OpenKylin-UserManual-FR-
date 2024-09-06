

# Guide de contribution à la documentation openKylin

## 1. Présentation du dépôt de documentation
Branche FAQ de la documentation : https://gitee.com/openkylin/docs/tree/dev-faq/

## 2. Processus de contribution à la documentation
Pour des raisons de conformité open source, il est nécessaire de signer l'accord CLA pour contribuer à la documentation. Assurez-vous que votre Gitee ID est correct lors de la signature du CLA. Veuillez vous référer à :
[Accord CLA openKylin](https://cla.openkylin.top/cla)

### 2.1 Fork du dépôt
1. Assurez-vous que Git est installé sur votre ordinateur. Dans le terminal, entrez la commande suivante pour vérifier si l'installation a réussi :
```
git --version
```
2. Cliquez sur le bouton Fork en haut à droite du dépôt pour le forker dans votre espace de compte Gitee.
Si vous avez déjà forké, ignorez cette étape.

### 2.2 Clonage du dépôt
1. Dans votre compte Gitee, cliquez sur le dépôt forké pour accéder à la page du dépôt.
2. Cliquez sur le bouton Clone/Download en haut à droite de la page et copiez l'adresse HTTPS du dépôt.
3. Ouvrez un terminal sur votre ordinateur local et exécutez la commande suivante pour cloner le dépôt localement :
```
git clone https://gitee.com/your_gitee_username/docs.git
```
4. Entrez dans le répertoire du dépôt cloné :
```
cd docs
```

### 2.3 Création d'une branche
1. Exécutez la commande suivante pour créer une nouvelle branche pour modifier la documentation :
```
git checkout -b new_branch_name
```
2. Effectuez les modifications de la documentation sur la nouvelle branche.

### 2.4 Soumission des modifications
1. Exécutez les commandes suivantes pour soumettre les modifications au dépôt local. Le format du message de commit doit être :
```
[Nom du document] [Contenu de la modification]
Par exemple :
[FAQ] Modification de la question 1 dans le document FAQ
ou :
Modification de la question 1 dans le document FAQ
```
Commit :
```
git add .
git commit -m "Message de commit"
```
2. Exécutez la commande suivante pour pousser la branche locale vers le dépôt distant :
```
git push origin new_branch_name
```

### 2.5 Soumission d'une Pull Request
1. Sur Gitee, cliquez sur le bouton Pull Request en haut à droite de la page du dépôt pour créer une nouvelle Pull Request.
2. Sur la page Pull Request, remplissez le titre et la description, et sélectionnez la branche cible (master ou branche faq-dev).
3. Cliquez sur le bouton de soumission pour créer la Pull Request.
4. Attendez la revue et la fusion.

### 2.6 Fusion de la branche
1. Après approbation, l'administrateur fusionnera votre Pull Request dans la branche cible.
2. Une fois la fusion terminée, vos modifications prendront effet.
3. Vous pouvez supprimer la branche locale et tirer la dernière branche master ou faq-dev pour garder votre dépôt local synchronisé avec le dépôt distant.

## 3. Normes de documentation
### 3.1 Format de documentation
Veuillez utiliser le format Markdown pour rédiger la documentation et suivre les normes suivantes :
- Utilisez un interligne et un espacement entre paragraphes uniformes, évitez d'utiliser des interlignes et des espacements spéciaux.
- Utilisez une indentation et des espaces uniformes, évitez d'utiliser des indentations et des espaces spéciaux.
- Utilisez des formats de titre et de liste uniformes, évitez d'utiliser des formats de titre et de liste spéciaux.

## 4. Normes de documentation FAQ
La documentation FAQ est un document de réponses aux questions fréquemment posées par la communauté openKylin, utilisé pour répondre aux questions des utilisateurs lors de l'utilisation d'openKylin.
https://gitee.com/openkylin/docs/blob/dev-faq/03_%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98/FAQ.md
Veuillez suivre les normes suivantes :
- La documentation FAQ doit avoir des questions courantes comme titres, sous forme de questions et réponses, évitez d'utiliser des titres et des formats de liste spéciaux.
- Soyez simple et clair, allez droit au but.
- Vous pouvez fournir plusieurs solutions au problème si approprié, mais pas trop, pour éviter de confondre les utilisateurs.

Exemple :
## Comment consulter les informations du CPU
- Q : Comment consulter les informations CPU de l'ordinateur ?
- R : Généralement, vous pouvez voir les informations du modèle CPU dans l'interface Paramètres - À propos. Si vous avez besoin de voir des informations détaillées sur le CPU, vous pouvez utiliser la commande `lscpu` ou `cat /proc/cpuinfo` pour voir l'architecture du CPU (comme x86_64), le modèle, le nombre de cœurs, le nombre de threads, la fréquence CPU de chaque cœur, le cache, les jeux d'instructions supportés, etc.

## 4. Points d'attention
1. Avant de soumettre une Pull Request, assurez-vous que vos modifications ont passé la revue de code et qu'il n'y a pas de conflits. Les soumissions avec des conflits ne peuvent pas être fusionnées.
2. Veuillez suivre les normes de documentation pour vous assurer que le format et le contenu du document sont conformes aux exigences.
3. Lors de la soumission d'une Pull Request, veuillez remplir un titre et une description détaillés pour permettre à l'administrateur du dépôt de comprendre le contenu de vos modifications.

## 5. Retour d'information
Si vous rencontrez des problèmes ou avez de bonnes suggestions pendant le processus de contribution à la documentation, n'hésitez pas à soumettre un problème (issue). Nous répondrons et résoudrons votre problème dès que possible. Si vous trouvez des erreurs ou des omissions dans la documentation, n'hésitez pas non plus à soumettre un problème. Merci pour votre soutien et votre contribution à la communauté openKylin.