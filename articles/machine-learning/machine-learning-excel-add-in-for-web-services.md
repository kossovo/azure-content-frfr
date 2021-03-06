<properties
	pageTitle="Complément Excel pour les services web Machine Learning | Microsoft Azure"
	description="Comment utiliser les services web Azure Machine Learning directement dans Excel sans écrire de code."
	services="machine-learning"
	documentationCenter=""
	authors="tedway"
	manager="paulettm"
	editor="cgronlun"
    tags=""/>

<tags
	ms.service="machine-learning"
    ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.workload="data-services"
	ms.date="07/06/2016"
	ms.author="tedway;garye" />

# Complément Excel de services web Azure Machine Learning

Excel permet d'appeler facilement des services web directement sans qu'il soit nécessaire d'écrire du code.

## Étapes pour utiliser un fichier de service web existant dans le classeur

1. Ouvrez l’[exemple de fichier Excel](http://aka.ms/amlexcel-sample-2), qui contient le complément Excel et les données concernant les passagers sur le Titanic.
2. Choisissez le service web en cliquant dessus : « Titanic Survivor Predictor (exemple de complément Excel) [Score] » dans cet exemple.

    ![Sélectionner un site Web][01]

3. Vous accéderez alors à la section **Prédire**. Ce classeur contient déjà des exemples de données mais, pour un classeur vide, vous pouvez également sélectionner une cellule dans Excel et cliquer sur **Utiliser les exemples de données**.
4. Sélectionnez les données avec les en-têtes et cliquez sur l'icône de plage de données d'entrée. Assurez-vous que la case « Mes données ont des en-têtes » est activée.
5. Sous **Sortie**, saisissez le numéro de la cellule dans laquelle vous souhaitez placer le résultat, par exemple « H1 » ici.
6. Cliquez sur **Prédire**.

	![Section Prédire][02]

## Opérations pour ajouter un nouveau Service Web

Publier un service web ([cette page](machine-learning-walkthrough-5-publish-web-service.md) explique comment le faire) ou utiliser un service web existant.

Obtenez la clé API de votre service web. L’endroit duquel vous la récupérez varie selon que vous avez publié un service web classique ou un nouveau service web.

**Service web classique**

1. Dans Machine Learning Studio, cliquez sur la section **WEB SERVICES** située sur le volet de gauche, puis sélectionnez le service web.

	![Studio Sélectionner un site Web][04]

2. Copiez la clé API du service web.

	![Studio clé API][05]

3. Dans l'onglet **TABLEAU DE BORD** pour le service web, cliquez sur le lien **REQUÊTE-RÉPONSE**.
4. Recherchez la section **URI de requête**. Copiez et enregistrez l’URL.

**Nouveau service web**

1. Dans le portail Services web Azure Machine Learning, cliquez sur **Services web**, puis sélectionnez votre service web.
2. Cliquez sur **Consommer**.
3. Recherchez la section **des informations de base sur la consommation**. Copiez et enregistrez la **clé primaire** et l’URL **Demande-requête**.


## Opérations pour ajouter un nouveau Service Web

1. Publier un service web ([cette page](machine-learning-walkthrough-5-publish-web-service.md) explique comment le faire) ou utiliser un service web existant.
2. Dans Excel, accédez à la section **Services web** (si vous vous trouvez dans la section **Prédire**, cliquez sur la flèche Précédent pour accéder à la liste des services web).

	![Accéder à la sélection du service web][03]
3. Cliquez sur **Ajouter un service web**.
4. Collez l’URL dans la zone de texte du complément Excel intitulée **URL**.
5. Collez l’API/Clé primaire dans la zone de texte intitulée **Clé API**.
6. Cliquez sur **Add**.

	![URL et clé API d’un service web classique.][06]

10.	Pour utiliser le service web, suivez les instructions ci-dessus, « Étapes pour utiliser un Service web existant ».

## Partage de votre classeur

Si vous enregistrez votre classeur, l'API/la clé primaire pour les services web que vous avez ajoutés seront également enregistrés. Cela signifie que vous devez uniquement partager le classeur avec des personnes de confiance.

Posez des questions ci-dessous ou sur notre [forum](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409).

[01]: ./media/machine-learning-excel-add-in-for-web-services/image1.png
[02]: ./media/machine-learning-excel-add-in-for-web-services/image2.png
[03]: ./media/machine-learning-excel-add-in-for-web-services/image3.png
[04]: ./media/machine-learning-excel-add-in-for-web-services/image4.png
[05]: ./media/machine-learning-excel-add-in-for-web-services/image5.png
[06]: ./media/machine-learning-excel-add-in-for-web-services/image6.png

<!---HONumber=AcomDC_0720_2016-->