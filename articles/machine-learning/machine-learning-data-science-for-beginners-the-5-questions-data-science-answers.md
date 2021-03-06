<properties
   pageTitle="Les 5 questions de la science des données - Science des données pour les débutants | Microsoft Azure"
   description="Découvrez une présentation rapide de la science des données avec la série Science des données pour les débutants, cinq courtes vidéos qui commencent par Les 5 questions auxquelles la science des données répond."
   keywords="utiliser la science des données,introduction à la science des données,science des données pour les débutants,types de questions,questions de science des données,algorithmes de science des données"
   services="machine-learning"
   documentationCenter="na"
   authors="brohrer-ms"
   manager="paulettm"
   editor="cjgronlund"/>

<tags
   ms.service="machine-learning"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/24/2016"
   ms.author="cgronlun;brohrer;garye"/>

# Science des données pour les débutants, vidéo 1 : Les 5 questions auxquelles la science des données répond

Découvrez la science des données dans la série de cinq courtes vidéos de présentation *Science des données pour les débutants*. Cette série de vidéos est utile si vous vous intéressez à la science des données (ou travaillez avec des personnes utilisant la science des données) et souhaitez commencer à apprendre certains concepts de base.

Cette première vidéo concerne les types de questions auxquelles la science des données est capable de répondre. La science des données prévoit des réponses aux questions à l’aide d’un nombre ou d’une catégorie. Pour tirer le meilleur parti de la série, regardez-les dans l’ordre. [Accéder à la liste des vidéos](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-series-the-5-questions-data-science-answers]

## Autres vidéos de cette série

La série *Science des données pour les débutants* offre une introduction rapide à la science des données en 25 minutes au total. Découvrez les quatre autres vidéos :

  * Vidéo 1 : Les 5 questions auxquelles la science des données répond
  * Vidéo 2 : [Vos données sont-elles prêtes pour la science des données ?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 min 56 sec)*
  * Vidéo 3 : [Poser une question à laquelle les données permettent de répondre](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sec)*
  * Vidéo 4 : [Prédire une réponse à l’aide d’un modèle simple](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min 42 sec)*
  * Vidéo 5 : [Copier le travail d’autres personnes pour des projets de science des données](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 min 18 sec)*

## Transcription : Les 5 questions auxquelles la science des données répond

Bonjour ! Bienvenue dans la série de vidéos *Science des données pour les débutants*.

La science des données peut être intimidante. Mais je vais vous présenter ici les principes de base sans équations ni jargon de programmation informatique.

Dans cette première vidéo, nous allons parler des « 5 questions auxquelles la science des données répond ».

La science des données utilise des nombres et des noms (également connus sous les noms de catégories ou d’étiquettes) pour prédire des réponses à des questions.

Cela peut vous surprendre, mais *il existe cinq questions auxquelles la science des données répond* :

  * Est-ce A ou B ?
  * Est-ce étrange ?
  * Quelle quantité (ou combien) ?
  * Comment les données sont-elles organisées ?
  * Que dois-je faire ensuite ?

  Un groupe spécifique de méthodes d’apprentissage automatique, appelées algorithmes, permet de répondre à ces questions.


Le mieux est de considérer un algorithme comme une recette et vos données comme les ingrédients. Un algorithme indique comment combiner et associer les données afin d’obtenir une réponse. Les ordinateurs sont comme un mixeur. Ils prennent en charge une grande partie des tâches difficiles de l’algorithme, et ce assez rapidement.

## La question 1 (Est-ce A ou B ?) utilise des algorithmes de classification

Commençons par la question « Est-ce A ou B ? »

![Algorithmes de classification : est-ce A ou B ?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-classification-algorithms.png)

Cette famille d’algorithmes est appelée la classification double classe.

Elle est utile pour toute question qui n’a que deux réponses possibles.

Par exemple :

  *	Une défaillance du pneu pourra-t-elle se produire dans les prochains 1 600 km ? Oui ou non ?
  *	Qu’est-ce qui attire le plus les clients : un coupon de 5 $ ou une remise de 25 % ?

Cette question peut également être reformulée pour inclure plus de deux options : est-ce A ou B, ou C ou D, etc. ? On parle de classification multiclasse. Elle est utile lorsque vous avez plusieurs (ou plusieurs milliers) de réponses possibles. La classification multiclasse choisit celle qui est la plus probable.

## La question 2 (Est-ce bizarre ?) utilise des algorithmes de détection d’anomalies

La prochaine question à laquelle la science des données peut répondre est : est cette étrange ? Pour répondre à cette question, on utilise une famille d’algorithmes appelée détection des anomalies.

![Algorithmes de détection d’anomalies : est-ce bizarre ?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-anomaly-detection-algorithms.png)


Si vous disposez d’une carte de crédit, vous avez déjà bénéficié de la détection des anomalies. Votre société de carte de crédit analyse vos modèles d’achat, afin de vous alerter en cas de fraude possible. Les frais considérés comme « étranges » peuvent être liés à un achat dans un magasin dans lequel vous ne faites normalement aucun achat ou à un achat anormalement coûteux.

Cette question peut se révéler utile dans de nombreux cas. Exemple :

  *	Si vous disposez d’une voiture avec des indicateurs de pression, vous souhaitez peut-être savoir si les valeurs de ces indicateurs sont normales.
  *	Si vous utilisez Internet, vous voulez peut-être savoir si ce message Internet est normal.

La détection d’anomalies signale des événements ou comportements inattendus ou inhabituels. Elle donne des indications sur l’endroit où rechercher des problèmes.



## La question 3 (Quelle quantité ou combien ?) utilise des algorithmes de régression

L’apprentissage automatique peut également prédire une quantité ou un volume. La famille d’algorithmes qui répond à cette question est appelée régression.

![Algorithmes de régression : quelle quantité ou combien ?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-regression-algorithms.png)


Les algorithmes de régression effectuent des prédictions numériques, notamment :

  *	Quelle sera la température mardi prochain ?
  *	Quelles seront mes ventes au quatrième trimestre ?

Ils peuvent répondre à toute question dont la réponse est un chiffre.

## La question 4 (Comment les données sont-elles organisées ?) utilise des algorithmes de clustering

Les deux dernières questions sont plus avancées.

Parfois, vous souhaitez comprendre la structure d’un jeu de données : comment est-il organisé ? Pour cette question, vous ne disposez pas d’exemples dont vous connaissez déjà les résultats.

Il existe de nombreuses manières d’analyser la structure de données. L’une des approches est le clustering. Elle sépare les données en groupes naturels pour en simplifier l’interprétation. Avec le clustering, il n’existe pas une seule réponse.

![Algorithmes de clustering : comment les données sont-elles organisées ?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-clustering-algorithms.png)

Les exemples suivants sont des questions de clustering courantes :

  *	Quels spectateurs aiment quels types de films ?
  *	Quels modèles défaillent de la même manière ?

En comprenant comment les données sont organisées, vous pouvez mieux comprendre (et prédire) des événements et des comportements.

## La question 5 (Que dois-je faire ensuite ?) utilise des algorithmes d’apprentissage par renforcement

La dernière question (Que dois-je faire ensuite ?) utilise une famille d’algorithmes appelée apprentissage par renforcement.

L’apprentissage par renforcement est inspiré de la manière dont le cerveau de rats et d’humains répond aux punitions et aux récompenses. Ces algorithmes apprennent sur la base de résultats pour décider de l’action suivante.

En règle générale, l’apprentissage par renforcement est une solution parfaitement adaptée à des systèmes automatisés qui doivent prendre un grand nombre de petites décisions sans instructions humaines.

![Algorithmes d’apprentissage par renforcement : que dois-je faire ensuite ?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-reinforcement-learning-algorithms.png)

Les questions auxquelles il répond concernent toujours l’action à exécuter (généralement par une machine ou un robot). Voici quelques exemples :

  *	Si je suis un système de contrôle de température dans une maison : ajuster la température ou la laisser telle quelle ?
  *	Si je suis une voiture à conduite automatique : freiner ou accélérer au feu orange ?
  *	Pour un aspirateur : continuer à aspirer, ou revenir à la station de recharge ?

Les algorithmes d’apprentissage par renforcement collectent les données au fur et à mesure et apprend de ses essais et de ses erreurs.

Et voilà ! Ce sont les 5 questions auxquelles la science des données peut répondre.



## Étapes suivantes

  * [Menez votre première expérience de science des données avec Azure Machine Learning](machine-learning-create-experiment.md)
  * [Consultez la présentation de Machine Learning sur Microsoft Azure](machine-learning-what-is-machine-learning.md)

<!---HONumber=AcomDC_0720_2016-->