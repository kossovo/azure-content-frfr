<properties
   pageTitle="Créer votre première fonction Azure | Microsoft Azure"
   description="Générez votre première fonction Azure, une application sans serveur, en moins de deux minutes."
   services="functions"
   documentationCenter="na"
   authors="ggailey777"
   manager="erikre"
   editor=""
   tags=""
   />

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="hero-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="05/05/2016"
   ms.author="glenga"/>

# Créer votre première fonction Azure

##Vue d'ensemble
Azure Functions est une expérience de calcul à la demande pilotée par les événements, qui étend la plateforme d’application Azure existante avec des capacités d’implémenter du code déclenché par des événements se produisant dans d’autres services Azure, dans les produits SaaS et dans les systèmes locaux. Avec Azure Functions, vos applications sont mises à l’échelle à la demande, et vous payez uniquement pour les ressources que vous utilisez. Azure Functions vous permet de créer des unités de code planifiées ou déclenchées, et de les implémenter dans différents langages de programmation. Pour en savoir plus sur Azure Functions, consultez [Vue d’ensemble d’Azure Functions](functions-overview.md).

Cette rubrique vous montre comment utiliser le démarrage rapide d’Azure Functions dans le portail Azure Functions pour créer une fonction Node.js « Hello World » simple, appelée par un déclencheur HTTP. Vous pouvez également regarder une courte vidéo décrivant comment effectuer ces étapes dans le portail.

## Regarder la vidéo

La vidéo suivante montre comment effectuer les étapes de base de ce didacticiel.

[AZURE.VIDEO create-your-first-azure-function-simple]

##Créer une fonction à l’aide du démarrage rapide

Une application de fonction héberge l’exécution de vos fonctions dans Azure. Pour créer une application de fonction et la nouvelle fonction, procédez comme suit : Pour créer votre première fonction, vous devez avoir un compte Azure actif. Si tel n’est pas le cas, des [comptes gratuits sont disponibles](https://azure.microsoft.com/free/).

1. Accédez au [portail Azure Functions](https://functions.azure.com/signin) et connectez-vous avec votre compte Azure.

2. Dans **Nom**, indiquez le nom unique de votre nouvelle fonction ou acceptez le nom généré, sélectionnez votre **Région**, puis cliquez sur **Créer + Prise en main**.

3. Dans l’onglet **Démarrage rapide**, cliquez sur **WebHook + API** > **Créer une fonction**. Une fonction Node.js prédéfinie est créée.

4. (Facultatif) À ce stade du démarrage rapide, vous pouvez suivre une visite rapide des fonctionnalités Azure Functions dans le portail. Une fois la visite terminée ou ignorée, vous pouvez tester votre nouvelle fonction à l’aide du déclencheur HTTP.

##Tester la fonction

Comme les démarrages rapides Azure Functions contiennent du code fonctionnel, vous pouvez tester votre nouvelle fonction immédiatement.

1. Dans l’onglet **Développer**, examinez la fenêtre **Code** et notez que ce code Node.js attend une requête HTTP avec un *nom* transmis dans le corps du message ou dans une chaîne de requête. Lorsque la fonction s’exécute, cette valeur est renvoyée dans le message de réponse.

2. Faites défiler l’affichage jusqu’à la zone de texte **Corps de la requête**, remplacez la valeur de la propriété *Nom* par votre nom, puis cliquez sur **Exécuter**. Vous verrez que l’exécution est déclenchée par une requête HTTP de test, que des informations sont écrites dans les journaux de diffusion en continu, et que la réponse « Hello » s’affiche dans la **Sortie**.

3. Pour déclencher l’exécution de la même fonction à partir d’une autre fenêtre ou d’un autre onglet de navigateur, copiez la valeur de **URL de la fonction** de l’onglet **Développer** et collez-la dans la barre d’adresse d’un navigateur, puis ajoutez la valeur de chaîne de requête `&name=yourname` et appuyez sur Entrée. Les mêmes informations sont écrites dans les journaux et le navigateur affiche la réponse « Hello » comme avant.

##Étapes suivantes

Ce démarrage rapide montre l’exécution très simple d’une fonction HTTP de base déclenchée. Pour plus d’informations sur l’utilisation de la puissance des Azure Functions dans vos applications, consultez les rubriques suivantes.

+ [Référence du développeur Azure Functions](functions-reference.md) Référence du programmeur pour le codage de fonctions et la définition de déclencheurs et de liaisons.
+ Le didacticiel [Test d’Azure Functions](functions-test-a-function.md) décrit plusieurs outils et techniques permettant de tester vos fonctions.
+ La rubrique [Comment mettre à l’échelle Azure Functions](functions-scale.md) présente les plans de services disponibles dans Azure Functions, dont le plan de service dynamique, et explique comment choisir le plan adapté à vos besoins. 
+ [Qu'est-ce qu'Azure App Service ?](../app-service/app-service-value-prop-what-is.md) Azure Functions s’appuie sur la plateforme Azure App Service pour les fonctionnalités essentielles comme les déploiements, les variables d’environnement et les diagnostics. 

[AZURE.INCLUDE [Remarque relative à la prise en main](../../includes/functions-get-help.md)]

<!---HONumber=AcomDC_0511_2016-->