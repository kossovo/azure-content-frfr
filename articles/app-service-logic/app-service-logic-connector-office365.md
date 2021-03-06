<properties
   pageTitle="Utilisation du connecteur Office 365 dans des applications logiques | Microsoft Azure App Service"
   description="Comment créer et configurer le connecteur Office 365 ou une application API et l'utiliser dans une application logique d’Azure App Service"
   services="app-service\logic"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="03/16/2016"
   ms.author="sameerch"/>


# Utilisation et ajout du connecteur Office 365 dans votre application logique
>[AZURE.NOTE] Cette version de l’article s’applique à la version du schéma 2014-12-01-preview des applications logiques. Pour la version de schéma 2015-08-01-preview, cliquez sur [API Outlook Office 365](../connectors/connectors-create-api-office365-outlook.md) ou [API Utilisateurs Office 365](../connectors/connectors-create-api-office365-users.md).

Connectez-vous à votre compte Office 365 pour envoyer et recevoir du courrier électronique et gérer votre calendrier et vos contacts. Vous pouvez effectuer diverses actions comme envoyer, recevoir et obtenir des messages messages électroniques, créer et supprimer des événements dans votre calendrier et créer, mettre à jour, obtenir et supprimer vos contacts.

Les applications logiques peuvent se déclencher selon diverses sources de données et proposent des connecteurs pour obtenir et traiter les données dans le cadre du flux. Vous pouvez ajouter le connecteur Office 365 à votre flux d’entreprise et traiter les données dans le cadre de ce flux de travail dans une application logique.

**Actions de base**

- Nouveau message (déclencheur)
- Envoyer du courrier électronique
- Répondre au courrier
- Envoyer un événement
- Ajouter un contact

## Création de l’application API du connecteur O365
Un connecteur peut être créé dans une application logique ou directement à partir d'Azure Marketplace. Pour créer un connecteur à partir de Marketplace :

1. Dans le tableau d'accueil Azure, sélectionnez **Marketplace**.
2. Recherchez « Connecteur Office 365 », sélectionnez-le et sélectionnez **Créer**.
3.	Configurez le connecteur Office 365 en indiquant les détails du plan d’hébergement, le groupe de ressources et en sélectionnant le nom de l’application API : ![][21]


## Créer une application logique
Créons une application logique simple qui se déclenche quand un message électronique est reçu à votre ID de messagerie pour les demandes de renseignements commerciaux, par exemple sales@contoso.com). Elle crée un événement, ajoute un contact avec les détails de l'expéditeur, envoie un message électronique à votre compte personnel et enfin envoie une réponse avec un accusé de réception.

1.	Connectez-vous au portail Azure et cliquez sur Nouveau -> Web + mobile -> Application logique : ![][1]

2.	Dans la page Créer une application logique, fournissez les informations requises, telles que le nom, le plan de service de l'application et l'emplacement : ![][2]

3.	Cliquez sur Déclencheurs et actions. L'éditeur d'application logique s'ouvre : ![][3]

4.	Sélectionnez le déclencheur Office 365 dans la section « Applications API dans ce groupe de ressources » dans la galerie pour l’ajouter au flux : ![][4]

6.	Pour la connexion à Office 365, vous devez autoriser l’application logique à accéder à votre compte. Une fois l’application créée, cliquez sur Autoriser pour indiquer les informations d’identification Office 365 : ![][5]

7.	Vous êtes redirigé vers la page de connexion Office 365, puis vous pouvez vous authentifier avec vos informations d'identification Office 365 : ![][6]![][7]

8.	Une fois que l'autorisation est terminée, les déclencheurs Office 365 s'affichent : ![][8]

9.	Sélectionnez le déclencheur Nouveau message et les paramètres d'entrée s'affichent.


10.	Remplacez la fréquence de déclenchement par Minutes et cliquez sur ✓ : ![][9]

11. Le déclencheur Office 365 Nouvel e-mail est configuré et vous pouvez également voir les paramètres de sortie : ![][10]

12.	Sélectionnez Connecteur Office 365 dans la section des éléments récemment utilisés dans la galerie et une nouvelle action Office 365 est ajoutée.

13.	Sélectionnez Envoyer un événement dans la liste des actions et les paramètres d'entrée de cette action s'affichent : ![][11]

14.	Spécifiez les détails de l'événement, puis cliquez sur ✓ : ![][12]

15.	Sélectionnez Connecteur Office 365 dans la section des éléments récemment utilisés dans la galerie et une nouvelle action Office 365 est ajoutée.

16.	Sélectionnez Ajouter un contact dans la liste des actions et les paramètres d'entrée de cette action s'affichent : ![][13]

17.	Cliquez sur + en regard du champ Adresse électronique, puis sélectionnez la valeur du champ de sortie De à partir du déclencheur : ![][14]

18. Cliquez sur ✓. La configuration de l'action est terminée : ![][15]

19.	Sélectionnez Connecteur Office 365 dans la section des éléments récemment utilisés dans la galerie et une nouvelle action Office 365 est ajoutée.


20.	Sélectionnez Envoyer un message dans la liste des actions et les paramètres d'entrée de cette action s'affichent : ![][19]

21.	Indiquez les détails requis pour envoyer le message électronique. Vous pouvez rédiger un message en tapant quelque chose comme ci-dessous. Une fois l’action d’envoi du message configurée, cliquez sur ✓ :

		Body - @concat('You got a new sales enquiry from',triggers().output.body.From)

	![][20]
22.	Sélectionnez Connecteur Office 365 dans la section des éléments récemment utilisés dans la galerie et une nouvelle action Office 365 est ajoutée.


23.	Sélectionnez Répondre dans la liste des actions et les paramètres d'entrée de cette action s'affichent : ![][16]

24.	Cliquez sur + en regard du champ De et sélectionnez la valeur de l’ID de message de sortie du déclencheur, puis cliquez sur ✓ : ![][17]

25. Cliquez sur OK dans l’éditeur d’application logique, puis cliquez sur Créer. Environ 30 secondes sont nécessaires pour terminer la création.

26. Envoyez un message électronique au compte avec lequel vous avez configuré votre déclencheur. Un message électronique doit apparaître dans votre compte de messagerie personnel et un événement de calendrier ainsi qu'un contact doivent apparaître dans votre compte de messagerie professionnel. De plus, vous devez obtenir une réponse confirmant que la demande de renseignements commerciaux sera traitée dans les meilleurs délais.

## En faire plus avec votre connecteur
Maintenant que le connecteur est créé, vous pouvez l'ajouter à un flux d'entreprise à l'aide d'une application logique. Voir [Que sont les applications logiques ?](app-service-logic-what-are-logic-apps.md).

> [AZURE.NOTE] Si vous voulez vous familiariser avec Azure Logic Apps avant d'ouvrir un compte Azure, accédez à la page [Essayer Logic App](https://tryappservice.azure.com/?appservice=logic), où vous pourrez créer immédiatement une application logique temporaire dans App Service. Aucune carte de crédit n’est requise ; vous ne prenez aucun engagement.

Affichez la référence d’API REST Swagger sur [Référence de connecteurs et d’applications API](http://go.microsoft.com/fwlink/p/?LinkId=529766).

Vous pouvez également consulter les statistiques de performances et contrôler la sécurité du connecteur. Consultez la page [Gestion et contrôle de vos connecteurs et applications API intégrés](app-service-logic-monitor-your-connectors.md).

<!--Image references-->
[1]: ./media/app-service-logic-connector-office365/1_New_Logic_App.png
[2]: ./media/app-service-logic-connector-office365/2_Logic_App_Settings.png
[3]: ./media/app-service-logic-connector-office365/3_Logic_App_Editor.png
[4]: ./media/app-service-logic-connector-office365/4_Select_Office365_Gallery.png
[5]: ./media/app-service-logic-connector-office365/5_Office365_Authorize.png
[6]: ./media/app-service-logic-connector-office365/6_Office365_Login.png
[7]: ./media/app-service-logic-connector-office365/7_Office365_User_Consent.png
[8]: ./media/app-service-logic-connector-office365/8_Office365_Trigger.png
[9]: ./media/app-service-logic-connector-office365/9_Office365_Trigger_Settings.png
[10]: ./media/app-service-logic-connector-office365/10_Office365_Trigger_Configured.png
[11]: ./media/app-service-logic-connector-office365/11_Office365_Actions_List.png
[12]: ./media/app-service-logic-connector-office365/12_Office365_Create_Event_Inputs.png
[13]: ./media/app-service-logic-connector-office365/13_Office365_Add_Contact_Inputs.png
[14]: ./media/app-service-logic-connector-office365/14_Office365_Add_Contact_Email_FromTrigger.png
[15]: ./media/app-service-logic-connector-office365/15_Office365_Add_Contacts_Configured.png
[16]: ./media/app-service-logic-connector-office365/16_Office365_Reply_To_Inputs.png
[17]: ./media/app-service-logic-connector-office365/17_Office365_Reply_To_MessageId.png
[18]: ./media/app-service-logic-connector-office365/18_Office365_Reply_To_Configured.png
[19]: ./media/app-service-logic-connector-office365/19_Office365_Send_Inputs.png
[20]: ./media/app-service-logic-connector-office365/20_Office365_Send_Configured.png
[21]: ./media/app-service-logic-connector-office365/21-create-new-o365-api-app.png

<!---HONumber=AcomDC_0323_2016-->