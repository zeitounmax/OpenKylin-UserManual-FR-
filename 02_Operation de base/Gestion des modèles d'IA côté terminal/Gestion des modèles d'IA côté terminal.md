# Gestion des modèles d'IA côté terminal

Le système openKylin prend en charge non seulement les modèles cloud, mais aussi les grands modèles locaux. Le système intègre un outil de gestion des modèles d'IA de Kylin, capable de télécharger automatiquement les grands modèles locaux. Les machines ayant de bonnes performances peuvent utiliser ces grands modèles locaux pour expérimenter un assistant IA.

## Obtention de l'outil

Le nom du package de l'outil de gestion des modèles de Kylin est `kylin-ai-model-manager`. Ce package est déjà intégré par défaut dans le nouveau système openKylin. Si votre système ne l'a pas, vous pouvez l'installer en exécutant :

## Utilisation de l'outil

### Lancement de l'outil

Ouvrez l'**outil de gestion des modèles de Kylin** dans le menu Démarrer.

### Téléchargement des modèles

Actuellement, trois types de modèles sont pris en charge, respectivement pour le traitement du langage naturel, la reconnaissance vocale et la recherche.

#### Téléchargement en un clic

L'outil de gestion des modèles prend en charge le téléchargement en un clic. Il vous suffit de cliquer sur le bouton **Téléchargement en un clic**, puis de saisir le mot de passe administrateur.

> (Le mot de passe administrateur n'est valide que pendant un certain temps. En raison de la vitesse du réseau, la durée de téléchargement peut varier, et il est possible que plusieurs demandes de permission administrateur soient nécessaires. Il est recommandé de cliquer à nouveau sur le bouton **Téléchargement en un clic** une fois le téléchargement terminé pour s'assurer qu'il n'y a pas eu d'échec à cause de l'expiration de la permission administrateur. Après le téléchargement, vous pouvez cliquer sur le bouton **Vérifier les fichiers de modèles locaux**. Si certains modèles n'ont pas été téléchargés, vous pouvez cliquer à nouveau sur **Téléchargement en un clic** pour les télécharger à nouveau.)

#### Téléchargement avancé

L'utilisateur peut également choisir manuellement les modèles à télécharger.

- Pour le modèle de **traitement du langage naturel**, il n'y a qu'un seul modèle :
  - Sélectionnez `nlp` dans la section **Type de modèle** ;
  - Cliquez sur le bouton **Télécharger le modèle**.

- Pour les modèles de **reconnaissance vocale**, il y en a 5 :
  - Sélectionnez `speech` dans la section **Type de modèle** ;
  - Sélectionnez un modèle dans la section **Nom du modèle** ;
  - Cliquez sur le bouton **Télécharger le modèle** ;
  - Répétez les étapes 2 et 3 jusqu'à ce que tous les modèles de `speech` soient téléchargés.

- Pour les modèles de **recherche**, il y en a 2 :
  - Sélectionnez `recherche` dans la section **Type de modèle** ;
  - Sélectionnez un modèle dans la section **Nom du modèle** ;
  - Cliquez sur le bouton **Télécharger le modèle** ;
  - Répétez les étapes 2 et 3 jusqu'à ce que tous les modèles de `recherche` soient téléchargés.

### Vérification des modèles

L'outil comprend un bouton **Vérifier les fichiers de modèles locaux** pour vérifier l'intégrité des modèles téléchargés. Si tous les modèles sont présents et corrects, la sortie sera "succès". En cas d'échec, vous devrez télécharger à nouveau le modèle correspondant.

### Quitter l'outil

Une fois le téléchargement des modèles terminé avec succès, vous pouvez quitter l'outil.

## Remarques

1. Le téléchargement des modèles nécessite une **connexion à un réseau capable d'accéder à `https://www.modelscope.cn/`**.
2. Lors du premier téléchargement des modèles, certaines dépendances doivent être installées, ce qui peut prendre du temps. Veuillez patienter.

## Mise à jour

Cet outil est principalement conçu pour télécharger et tester de grands modèles pour l'instant, et il est encore très rudimentaire. De nouvelles fonctionnalités et conceptions sont prévues dans le futur. Merci pour votre compréhension et votre patience ! :) 
