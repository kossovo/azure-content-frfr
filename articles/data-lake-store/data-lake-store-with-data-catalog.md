<properties
   pageTitle="Référencer des données de Data Lake Store dans Azure Data Catalog | Azure"
   description="Référencer des données de Data Lake Store dans Azure Data Catalog"
   services="data-lake-store,data-catalog" 
   documentationCenter=""
   authors="nitinme"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/17/2016"
   ms.author="nitinme"/>

# Référencer des données de Data Lake Store dans Azure Data Catalog

Dans cet article, vous allez découvrir comment intégrer Azure Data Lake Store à Azure Data Catalog, pour rendre vos données détectable au sein d’une organisation en l’intégrant à Data Catalog. Pour plus d’informations sur le catalogage des données, consultez [Azure Data Catalog](../data-catalog/data-catalog-what-is-data-catalog.md). Pour comprendre les scénarios dans lesquels vous pouvez utiliser Data Catalog, consultez [Scénarios courants d’Azure Data Catalog](../data-catalog/data-catalog-common-scenarios.md).

## Composants requis

Avant de commencer ce didacticiel, vous devez disposer des éléments suivants :

- **Un abonnement Azure**. Consultez la page [Obtention d’un essai gratuit d’Azure](https://azure.microsoft.com/pricing/free-trial/).

- **Activez votre abonnement Azure** pour la version d'évaluation publique de Data Lake Store. Consultez les [instructions](data-lake-store-get-started-portal.md#signup).

- **Compte Azure Data Lake Store**. Suivez les instructions de [Prise en main d'Azure Data Lake Store avec le portail Azure](data-lake-store-get-started-portal.md). Pour ce didacticiel, créons un compte Data Lake Store appelé **datacatalogstore**.

	Une fois que vous avez créé le compte, chargez-y un exemple de jeu de données. Pour ce didacticiel, chargeons tous les fichiers .csv sous le dossier **AmbulanceData** dans le [dépôt Git Azure Data Lake](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/). Vous pouvez utiliser différents clients, comme [Explorateur de stockage Azure](http://storageexplorer.com/), pour charger des données dans un conteneur d’objets blob.

- **Azure Data Catalog**. Votre organisation doit déjà avoir un catalogue de données Azure créé pour votre organisation. Un seul catalogue est autorisé pour chaque organisation.

## Inscrire Data Lake Store comme source pour Data Catalog

>[AZURE.VIDEO adcwithadl] 

1. Accédez à `https://azure.microsoft.com/services/data-catalog`, puis cliquez sur **Prise en main**.

2. Connectez-vous au portail Azure Data Catalog, puis cliquez sur **Publier des données**.

	![Référencer une source de données](./media/data-lake-store-with-data-catalog/register-data-source.png "Référencer une source de données")

3. Sur la page suivante, cliquez sur **Lancer l’application**. Ceci télécharge le fichier manifeste d’application sur votre ordinateur. Double-cliquez sur le fichier manifeste pour démarrer l’application.

4. Sur la page Bienvenue, cliquez sur **Connexion**, puis entrez vos informations d’identification.

	![Écran d’accueil](./media/data-lake-store-with-data-catalog/welcome.screen.png "Écran d’accueil")

5. Sur la page Sélectionner une source de données, sélectionnez **Azure Data Lake**, puis cliquez sur **Suivant**.

	![Sélectionnez la source de données](./media/data-lake-store-with-data-catalog/select-source.png "Sélectionnez la source de données")

6. Sur la page suivante, spécifiez le nom du compte Data Lake Store que vous voulez inscrire dans Data Catalog. Laissez les autres options à leur valeur par défaut, puis cliquez sur **Connexion**.

	![Se connecter à une sources de données](./media/data-lake-store-with-data-catalog/connect-to-source.png "Se connecter à une sources de données")

7. La page suivante peut être divisée selon les segments suivants.

	a. La zone **Hiérarchie du serveur** représente la structure des dossiers du compte Data Lake Store. **$Root** représente la racine du compte Data Lake Store et **AmbulanceData** représente le dossier créé à la racine du compte Data Lake Store.

	b. La zone **Objets disponibles** répertorie les fichiers et les dossiers présents sous le dossier **AmbulanceData**.

	c. La zone **Objets à référencer** répertorie les fichiers et dossiers à référencer dans Azure Data Catalog.

	![Afficher la structure des données](./media/data-lake-store-with-data-catalog/view-data-structure.png "Afficher la structure des données")

8. Pour ce didacticiel, vous devez référencer tous les fichiers du répertoire. Pour cela, cliquez sur le bouton (![déplacer des objets](./media/data-lake-store-with-data-catalog/move-objects.png "Déplacer des objets")) pour déplacer tous les fichiers dans la zone **Objets à référencer**.

	Comme les données sont référencées dans un catalogue de données au niveau de l’organisation, il est recommandé d’ajouter des métadonnées que vous pouvez utiliser ultérieurement pour localiser rapidement les données. Par exemple, vous pouvez ajouter une adresse de messagerie pour le propriétaire des données (par exemple une personne qui charge les données) ou ajouter une étiquette pour identifier les données. La capture d’écran ci-dessous montre une étiquette que nous ajoutons aux données.

	![Afficher la structure des données](./media/data-lake-store-with-data-catalog/view-selected-data-structure.png "Afficher la structure des données")

	Cliquez sur **S'inscrire**.

8. La capture d’écran suivante montre que les données sont référencées avec succès dans le catalogue de données.

	![Référencement terminé](./media/data-lake-store-with-data-catalog/registration-complete.png "Afficher la structure des données")

9. Cliquez sur **Afficher le portail** pour revenir au portail Data Catalog et vérifiez que vous pouvez maintenant accéder aux données référencées à partir du portail. Pour rechercher les données, vous pouvez utiliser l’étiquette que vous avez utilisée lors du référencement des données.

	![Rechercher des données dans le catalogue](./media/data-lake-store-with-data-catalog/search-data-in-catalog.png "Rechercher des données dans le catalogue")

10. Vous pouvez maintenant effectuer des opérations comme ajouter des annotations et de la documentation aux données. Pour plus d’informations, consultez les liens suivants :
	* [Annoter des sources de données dans Data Catalog](../data-catalog/data-catalog-how-to-annotate.md)
	* [Documenter des sources de données dans Data Catalog](../data-catalog/data-catalog-how-to-documentation.md)

## Voir aussi

* [Annoter des sources de données dans Data Catalog](../data-catalog/data-catalog-how-to-annotate.md)
* [Documenter des sources de données dans Data Catalog](../data-catalog/data-catalog-how-to-documentation.md)
* [Intégrer Data Lake Store à d’autres services Azure](data-lake-store-integrate-with-other-services.md)

<!---HONumber=AcomDC_0518_2016-->