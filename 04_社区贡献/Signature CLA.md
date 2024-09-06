
## Signer le CLA
Selon les exigences de la communauté, les contributeurs doivent signer un CLA (Contributor License Agreement).

## Structure des répertoires 
Il est recommandé de placer les traductions des documents du dépôt principal dans le dossier [en](https://gitee.com/openkylin/docs/tree/master/en) correspondant. L'arborescence en anglais doit être identique à celle en chinois. Des membres du SIG seront chargés de la révision.

## Catégories de documents
Pour éviter toute confusion, voici une brève introduction aux différents types de documents :
- Les spécifications sont généralement des documents techniques (compilation de logiciels, traduction de documents, etc.)
- Les politiques sont plutôt des documents de gestion, orientés vers les statuts ou les programmes (philosophie de la communauté, structure organisationnelle, etc.) 
- Les règles sont aussi des documents de gestion, mais axés sur les codes de conduite ou les règlements (droits d'auteur du code, étiquette de discussion, etc.)

## Conventions de nommage
Utilisez des tirets "-" pour les espaces dans les noms de répertoires, et des underscores "_" pour les espaces dans les noms de fichiers.

## Vérification des doublons
Avant de traduire, vérifiez dans la bibliothèque de documents et les moteurs de recherche s'il existe déjà une traduction du document.

## Chemin des images
Les images dans les traductions doivent être placées dans un dossier "assets" au même niveau que la traduction. Utilisez des chemins relatifs "./" pour y faire référence.

## Traduction du texte dans les images
Si possible, il est recommandé de remplacer les interfaces en chinois par des interfaces en anglais dans les images et de les recapturer.

## Traduction automatique et vérification humaine  
La traduction automatique est acceptable si l'outil utilisé est fiable. Une vérification humaine minutieuse reste indispensable.

## Problèmes courants dans les traductions
1. Faites attention à la grammaire, aux parties du discours et aux temps en anglais.
   Mauvais : In order to better manage issues, we divide participants into the following roles:
   Bon : For better issues "management" , we "divided" the participants into the following roles:

2. Exprimez précisément le sens du texte original.
   "对各项目进行测试。"
   Mauvais : Test projects.
   Bon : Testing of various projects.

3. Veillez à l'uniformité du vocabulaire utilisé.
   - "分类标签" devrait être traduit par "Kind Tags".
   - "标签" devrait être traduit par "Tags", pas "label".
   - "参与者" apparaît comme "participant" et "contributor".
   - "提出" apparaît comme "proposed" et "raised".

## Points à noter
Sauf indication contraire, le dépôt principal fait référence au [dépôt docs](https://gitee.com/openkylin/docs), et le dépôt secondaire au [sig-documentation](https://gitee.com/openkylin/sig-documentation).
En général, les tutoriels écrits par des amateurs sont placés dans le dépôt secondaire, tandis que la documentation officielle est placée dans le dépôt principal. Cependant, ce n'est pas une règle absolue et le groupe SIG fera des recommandations en fonction du contenu.