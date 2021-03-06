<properties 
	pageTitle="Améliorer votre compte DocumentDB S1 | Microsoft Azure" 
	description="Tirez parti d’un débit plus élevé dans votre compte DocumentDB S1 en apportant quelques modifications simples dans le portail Azure." 
	services="documentdb" 
	authors="mimig1" 
	manager="jhubbard" 
	editor="monicar" 
	documentationCenter=""/>

<tags 
	ms.service="documentdb" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="06/27/2016" 
	ms.author="mimig"/>

# Améliorer votre compte DocumentDB

Suivez ces étapes pour tirer parti d’un débit plus élevé pour votre compte Azure DocumentDB S1. Avec peu ou pas de frais supplémentaires, vous pouvez augmenter le débit de votre compte S1 existant 250 à 400 [unités de requête/s](documentdb-request-units.md) ou plus !

> [AZURE.VIDEO changedocumentdbcollectionperformance]

## Passer aux performances définies par l’utilisateur dans le portail Azure

1. Dans votre navigateur, accédez au [**portail Azure**](https://portal.azure.com).
2. Cliquez sur **Parcourir** -> **Comptes DocumentDB**, puis sélectionnez le compte DocumentDB à modifier.
3. Dans le filtre **Bases de données**, sélectionnez la base de données à modifier, puis, dans le panneau **Base de données**, sélectionnez la collection dont le niveau tarifaire est S1.

      ![Capture d’écran de la collection S1 dans le panneau Base de données](./media/documentdb-supercharge-your-account/documentdb-change-performance-S1.png)

4. Dans le panneau **Collections**, cliquez sur **Paramètres** dans la barre supérieure.
5. Dans le panneau **Paramètres**, cliquez sur **Niveau tarifaire** et notez que l’estimation des coûts mensuels pour chaque plan est affichée. Dans le panneau **Choisir votre niveau tarifaire**, cliquez sur **Standard**, puis cliquez sur **Sélectionner** pour enregistrer vos modifications.

      ![Capture d’écran des panneaux Paramètres DocumentDB et Choisir votre niveau tarifaire](./media/documentdb-supercharge-your-account/documentdb-change-performance.png)

6. De retour dans le panneau **Paramètres**, le **Niveau tarifaire** est défini sur **Standard** et la zone **Débit (unités de requête/s)** est affichée avec une valeur par défaut de 400. Cliquez sur **OK** pour enregistrer vos modifications.

    > [AZURE.NOTE] Définissez le débit entre 400 et 10 000 [unités de requête](../articles/documentdb/documentdb-request-units.md)/seconde (UR/s). Le **Résumé de la tarification** au bas de la page se met à jour automatiquement pour donner une estimation du coût mensuel.
    
	![Capture d’écran du panneau Paramètres montrant où modifier la valeur de débit](./media/documentdb-supercharge-your-account/documentdb-change-performance-set-thoughput.png)

8. Sur le panneau **Base de données**, vous pouvez vérifier le débit amélioré de la collection.

	![Capture d’écran d’une collection modifiée dans le panneau Base de données](./media/documentdb-supercharge-your-account/documentdb-change-performance-confirmation.png)

Pour plus d’informations sur les modifications liées aux débits définis par l’utilisateur ou prédéfinis, consultez l’article de blog [DocumentDB : tout ce que vous devez savoir sur l’utilisation des nouvelles options de tarification](https://azure.microsoft.com/blog/documentdb-use-the-new-pricing-options-on-your-existing-collections/).

## Étapes suivantes

Si vous déterminez que vous avez besoin d’un débit supérieur (plus de 10 000 unités de requête/s) ou d’une plus grande capacité de stockage (plus de 10 Go), vous pouvez créer une collection partitionnée. Pour créer une collection partitionnée, consultez [Créer une collection](documentdb-create-collection.md).

<!---HONumber=AcomDC_0720_2016-->