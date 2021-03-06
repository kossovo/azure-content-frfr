<properties
	pageTitle="Autorisation côté service des utilisateurs d’un service mobile principal .NET | Microsoft Azure"
	description="Découvrez comment limiter l’accès des utilisateurs autorisés d’un service mobile principal .NET."
	services="mobile-services"
	documentationCenter="windows"
	authors="krisragh"
	manager="dwrede"
	editor=""/>

<tags
	ms.service="mobile-services"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-multiple"
	ms.topic="article"
	ms.devlang="dotnet"
	ms.date="03/09/2016"
	ms.author="krisragh"/>

# Autorisation côté service des utilisateurs de Mobile Services
> [AZURE.SELECTOR]
- [Backend .NET](mobile-services-dotnet-backend-service-side-authorization.md)
- [Back-end Javascript](mobile-services-javascript-backend-service-side-authorization.md)

&nbsp;

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]
> Pour la version Mobile Apps de cette rubrique, consultez [Limiter l’accès aux données pour les utilisateurs autorisés](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#authorize) dans l’article Utiliser le Kit de développement logiciel (SDK) de serveur principal .NET pour Azure Mobile Apps.

Cette rubrique montre comment utiliser une logique côté serveur pour autoriser les utilisateurs. Dans ce didacticiel, vous modifiez les contrôleurs de table, filtrez des requêtes en fonction de l’ID des utilisateurs et permettez aux utilisateurs d’accéder uniquement à leurs propres données. Le filtrage des résultats de la requête d'un utilisateur par l'ID d'utilisateur est la forme d’autorisation la plus basique. Selon votre scénario spécifique, vous pouvez également créer des tables ou rôles utilisateurs pour effectuer le suivi d’informations d'autorisation utilisateur plus détaillées, telles que les points de terminaison auxquels un utilisateur donné peut accéder.

Ce didacticiel est basé sur le démarrage rapide de Mobile Services et s'appuie sur le didacticiel [Ajout de l'authentification à une application Mobile Services existante]. Veuillez suivre [Ajout de l'authentification à une application Mobile Services existante] tout d'abord.

## <a name="register-scripts"></a>Modification des méthodes d'accès aux données

1. Dans Visual Studio, ouvrez votre projet mobile, développez le dossier DataObjects, puis ouvrez **TodoItem.cs**. La classe **TodoItem** définit l'objet de données et vous devez ajouter une propriété **UserId** pour le filtrage. Ajoutez la nouvelle propriété UserId suivante à la classe **TodoItem** :

		public string UserId { get; set; }

	>[AZURE.NOTE] Pour modifier ce modèle de données et conserver les données existantes dans la base de données, vous devez utiliser les [migrations Code First](mobile-services-dotnet-backend-how-to-use-code-first-migrations.md).

2. Dans Visual Studio, développez le dossier Contrôleurs, ouvrez **TodoItemController.cs** et ajoutez l’instruction Using suivante :

		using Microsoft.WindowsAzure.Mobile.Service.Security;

3. Recherchez la méthode **PostTodoItem** et ajoutez le code suivant au début de celle-ci.

		// Get the logged in user
		var currentUser = User as ServiceUser;

		// Set the user ID on the item
		item.UserId = currentUser.Id;

	Ce code ajoute l’ID utilisateur de l’utilisateur authentifié à l’élément, avant son insertion dans la table TodoItem.

3. Recherchez la méthode **GetAllTodoItems** et remplacez l'instruction **return** existante par la ligne de code suivante :

		// Get the logged in user
		var currentUser = User as ServiceUser;

		return Query().Where(todo => todo.UserId == currentUser.Id);

	Cette requête filtre les objets TodoItem retournés de façon à ce que chaque utilisateur reçoive seulement les éléments qu'il a insérés.

4. Publiez à nouveau le projet de service mobile dans Azure.


## <a name="test-app"></a>Test de l'application

1. Remarquez que lorsque vous exécutez votre application côté client, bien que certains éléments provenant des didacticiels précédents figurent déjà dans la base de données, aucun élément n’est renvoyé. Cela se produit parce que les éléments précédents ont été insérés sans la colonne user ID et comportent maintenant des valeurs null.

2. Si vous disposez de comptes de connexion supplémentaires, assurez-vous que les utilisateurs peuvent uniquement afficher leurs propres données en fermant, puis supprimant l’application, avant de l’exécuter de nouveau. Lorsque la boîte de dialogue des informations d’identification apparaît, entrez une autre connexion, puis vérifiez que les éléments entrés lors de la session précédente ne s’affichent pas.



<!-- Anchors. -->
[Register server scripts]: #register-scripts
[Next Steps]: #next-steps

<!-- Images. -->

[3]: ./media/mobile-services-dotnet-backend-ios-authorize-users-in-scripts/mobile-quickstart-startup-ios.png

<!-- URLs. -->
[Ajout de l'authentification à une application Mobile Services existante]: mobile-services-dotnet-backend-ios-get-started-users.md

<!-----HONumber=AcomDC_0316_2016-->