<properties 
	pageTitle="Azure Machine Learning Studio - De quoi s'agit-il ? | Microsoft Azure"
	description="Vue d'ensemble d'Azure ML Studio, un outil de glisser-déplacer pour créer rapidement des modèles à partir d'une bibliothèque prête à l'emploi d'algorithmes et de modules."
	keywords="azure machine learning,azure ml, ml studio"
	services="machine-learning"
	documentationCenter=""
	authors="garyericson"
	manager="paulettm"
	editor="cgronlun"/>

<tags
	ms.service="machine-learning"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="06/10/2016"
	ms.author="garye"/>

# Azure Machine Learning Studio - De quoi s'agit-il ?

Microsoft Azure Machine Learning Studio est un outil collaboratif fonctionnant par glisser-déplacer qui vous permet de générer, tester et déployer des solutions d'analyse prédictive à partir de vos données. Machine Learning Studio publie des modèles en tant que services web pouvant facilement être consommés par des applications personnalisées ou des outils décisionnels tels qu'Excel.

Machine Learning Studio : là où convergent votre connaissance des données, l'analyse prédictive, les ressources cloud et vos données.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## Espace de travail interactif de Machine Learning Studio

Pour développer un modèle d'analyse prédictive, vous utilisez généralement des données d'une ou plusieurs sources que vous transformez et analysez par diverses manipulations et fonctions statistiques. Vous créez ensuite un ensemble de résultats. Le développement d'un modèle de ce type est un processus itératif. Quand vous modifiez les diverses fonctions et leurs paramètres, vos résultats convergent jusqu'à ce que l'efficacité du modèle formé vous donne satisfaction.

**Azure Machine Learning Studio** offre un espace de travail visuel et interactif qui vous permet de générer, tester et répéter facilement un modèle d'analyse prédictive. Vous faites glisser des ***jeux de données*** et des ***modules*** d'analyse sur un ***canevas*** interactif, en les connectant ensemble pour former une ***expérience***, que vous ***exécutez*** sous Machine Learning Studio. Pour affiner votre modèle, vous ***modifiez*** l'expérience, ***enregistrez*** une copie si vous le souhaitez et l'exécutez de nouveau. Quand vous êtes prêt, vous pouvez convertir votre ***expérience de formation*** en une ***expérience prédictive***, puis la ***publier*** en tant que ***service web*** afin que votre modèle soit accessible à d’autres.

>[AZURE.TIP] Pour télécharger et imprimer un diagramme offrant une vue d’ensemble des fonctionnalités de Machine Learning Studio, consultez [Diagramme de vue d’ensemble des fonctionnalités d’Azure Machine Learning Studio](machine-learning-studio-overview-diagram.md).

Aucune programmation n'est nécessaire : il suffit de visualiser la connexion des jeux de données et des modules pour construire votre modèle d'analyse prédictive.

![Diagramme Azure ML Studio : créer des expériences, lire les données de nombreuses sources, écrire des données évaluées, écrire des modèles.][ml-studio-overview]

## Prise en main de Machine Learning Studio

Quand vous ouvrez [Machine Learning Studio](https://studio.azureml.net) pour la première fois, la page **Accueil** apparaît. À partir de là, vous pouvez afficher la documentation, des vidéos, des webinaires et rechercher d’autres ressources précieuses.

Trois onglets apparaissent en haut de la page : **Accueil** (votre point de départ), **Studio** et **Galerie**.

### Studio

Cliquez sur l’onglet **Studio** et vous serez invité à vous connecter à l’aide de votre compte Microsoft, professionnel ou scolaire. Une fois que vous êtes connecté, les onglets suivants apparaissent sur la gauche :

- **PROJETS** - Collections d’expériences, de DataSets, de notebooks et d’autres ressources représentant un projet spécifique
- **EXPÉRIENCES** : expériences qui ont été créées, exécutées et enregistrées comme brouillons
- **SERVICES WEB** : services que vous avez déployés à partir de vos expériences web
- **NOTEBOOKS** : notebooks Jupyter que vous avez créés
- **JEUX DE DONNÉES** : jeux de données que vous avez téléchargés dans Studio
- **MODÈLES FORMÉS** : modèles que vous avez formés dans les expériences, puis enregistrés dans Studio
- **PARAMÈTRES** : ensemble des paramètres que vous pouvez utiliser pour configurer votre compte et vos ressources.

### Galerie

Cliquez sur l’onglet **Galerie** pour accéder à la galerie Cortana Intelligence. La galerie est l’endroit où la communauté des chercheurs et développeurs spécialisés dans les données peut partager des solutions créées à l’aide des composants de la suite Cortana Intelligence.

Pour plus d’informations sur la galerie, voir [Partager et découvrir des solutions dans la galerie Cortana Intelligence](machine-learning-gallery-how-to-use-contribute-publish.md).

## Composants d'une expérience

Une expérience se compose de jeux de données qui fournissent des données aux modules d'analyse que vous connectez ensemble pour construire un modèle d'analyse prédictive. En particulier, une expérience valide a les caractéristiques suivantes :

- L'expérience comporte au moins un jeu de données et un module
- Il est possible de connecter des jeux de données uniquement à des modules
- Les modules peuvent être connectés à des jeux de données ou à d'autres modules
- Tous les ports d'entrée des modules doivent comporter une connexion au flux de données
- Tous les paramètres obligatoires de chaque module doivent être configurés

Vous pouvez créer une expérience à partir de zéro, ou utiliser un exemple d’expérience existante comme modèle. Pour plus d’informations, consultez [Utilisez des exemples d’expériences pour créer de nouvelles expériences](machine-learning-sample-experiments.md).

Pour obtenir un exemple de création d'une expérience simple, consultez la rubrique [Création d'une expérience simple dans Azure Machine Learning Studio](machine-learning-create-experiment.md).

Pour une description plus complète de la création d'une solution d'analyse prédictive, consultez la rubrique [Développement d'une solution prédictive avec Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md).

### Jeux de données

Un jeu de données représente des données téléchargées dans Machine Learning Studio de façon à les utiliser dans la procédure de modélisation. Machine Learning Studio fournit divers exemples de jeux de données utilisables pour vos expériences ; vous pouvez télécharger vers le serveur d'autres jeux de données si vous en avez besoin. Voici quelques exemples de jeux de données fournis :

- **Données sur la quantité de litres au 100 pour différents véhicules automobiles** : valeurs de quantité de litres au 100 pour des automobiles identifiées par leur nombre de cylindres, leur puissance, etc.
- **Données sur le cancer du sein** : données de diagnostics sur le cancer du sein.
- **Données sur les feux de forêts** : tailles des incendies de forêts au nord-est du Portugal.

Lorsque vous créez une expérience, vous pouvez utiliser la liste des jeux de données à gauche du canevas.

Pour obtenir une liste des exemples de jeux de données inclus dans Machine Learning Studio, consultez [Utilisation des exemples de jeux de données dans Azure Machine Learning Studio](machine-learning-use-sample-datasets.md).

### Modules

Un module est un algorithme que vous appliquez à vos données. Machine Learning Studio comporte divers modules, allant de fonctions de saisie des données à des procédures de formation, de notation et de validation. Voici quelques exemples de modules fournis :

- [Conversion au format ARFF][convert-to-arff] : convertit un jeu de données sérialisé .NET au format ARFF (Attribute-Relation File Format)
- [Statistiques de calcul élémentaires][elementary-statistics] \: calcule des statistiques élémentaires (par exemple, moyenne, écart-type, etc.).
- [Régression linéaire][linear-regression] : crée en ligne un modèle de régression linéaire à gradient décroissant.
- [Noter le modèle][score-model] : note une classification formée ou un modèle de régression.

Lorsque vous créez une expérience, vous pouvez utiliser la liste des modules à gauche du canevas.

Un module peut comporter un ensemble de paramètres utilisables pour configurer les algorithmes internes du module. Quand vous sélectionnez un module dans le canevas, ses paramètres sont affichés dans le volet **Propriétés** à droite du canevas. Vous pouvez modifier les paramètres figurant dans ce volet pour affiner votre modèle.

Pour obtenir de l’aide sur la navigation dans la vaste bibliothèque d’algorithmes disponibles dans Machine Learning, consultez [Comment choisir les algorithmes dans Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md).

## Déploiement d'un service web d'analyse prédictive

Une fois votre modèle d'analyse prédictive prêt, vous pouvez le déployer comme un service web directement à partir de Machine Learning Studio. Pour plus de détails sur ce processus, consultez [Déploiement d’un service web Azure Machine Learning](machine-learning-publish-a-machine-learning-web-service.md).

[ml-studio-overview]: ./media/machine-learning-what-is-ml-studio/azure-ml-studio-diagram.jpg

<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/

<!---HONumber=AcomDC_0615_2016-->