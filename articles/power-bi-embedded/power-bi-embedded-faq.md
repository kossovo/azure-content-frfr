<properties
   pageTitle="Forum Aux Questions"
   description="Forum Aux Questions de Power BI Embedded"
   services="power-bi-embedded"
   documentationCenter=""
   authors="minewiskan"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="07/05/2016"
   ms.author="owend"/>

# Forum Aux Questions de Power BI Embedded

## Quelle est votre annonce la plus récente sur Power BI Embedded ?

Le service Microsoft Power BI Embedded est désormais disponible avec un contrat SLA version grand public standard. Pour en savoir plus sur cette version, voir [Nouveautés](power-bi-embedded-whats-new.md).

## Pouvez-vous nous présenter le service Microsoft Power BI Embedded ?

Microsoft Power BI Embedded est désormais mis à la disposition générale. Power BI Embedded est un service Azure qui permet aux développeurs d’applications d’incorporer de superbes visualisations et rapports interactifs dans vos applications destinées aux clients, sans que vous ayez à créer vos propres contrôles de A à Z. Imaginez le gain de temps et d’argent que cela représente ! Power BI Embedded est désormais disponible avec un contrat SLA dans 9 centres de données dans le monde entier. Nous avons aussi amélioré les fonctionnalités de services tels que le support pour la sécurité des données via la sécurité au niveau des lignes (RLS) dans Power BI Embedded pour le filtrage avancé. Nous avons simplifié et mis à jour le modèle de tarification de Power BI Embedded. Pour plus d’informations, voir [Power BI Embedded](https://azure.microsoft.com/services/power-bi-embedded/).

## À qui et en quoi Microsoft Power BI Embedded peut-il être utile ?

Microsoft Power BI Embedded s’adresse aux développeurs d’applications qui souhaitent permettre à leurs utilisateurs d’afficher des visualisations de données attrayantes et interactives, sur tous les types d’appareils, sans avoir à les créer eux-mêmes. Grâce aux requêtes directes de Power BI Embedded, les développeurs fournissent des visualisations de données toujours actualisées. Ils peuvent aussi utiliser les API d’ARM et les API de Power BI pour déployer, gérer et automatiser Power BI par programmation. Comme c’est toujours le cas avec Power BI, le service incorporé est automatiquement mis à l’échelle en fonction de l’usage et des besoins de votre application. Le service Power BI Embedded applique un modèle de tarification basé sur le paiement à l’utilisation.

## Quelle est la relation entre Power BI Embedded et le service Power BI ?

Power BI Embedded et le service Power BI sont des offres distinctes. Power BI Embedded applique un modèle de facturation basée sur la consommation réelle et est déployé via le portail Azure. Il est conçu pour permettre aux éditeurs de logiciels indépendants d’incorporer des visualisations de données dans les applications destinées à leurs clients. Le service Power BI, facturé et déployé via le portail O365, est une offre BI autonome à usage général, principalement destinée à une utilisation interne en entreprise. Pour plus d’informations sur le service Power BI, voir [www.powerbi.com](https://powerbi.microsoft.com).

## De quelle façon Power BI Embedded améliore-t-il mon application ?

Votre application sera beaucoup plus utile aux utilisateurs si elle leur fournit directement des visualisations de données interactives et saisissantes qui les aident à prendre les bonnes décisions. Utilisez Power BI Embedded pour enrichir votre application avec des visualisations de données attrayantes, interactives et actualisées en temps réel. Vous pourrez ainsi proposer une application offrant plus d’intérêt, et donc satisfaire et fidéliser davantage vos utilisateurs, et fournir facilement des analyses contextuelles sur tous les appareils.

## L’utilisation de Power BI Embedded dans mon application est-elle soumise à certaines règles ou limitations ?

Power BI Embedded est conçu pour vos applications fournies pour une utilisation par des tiers. Si vous voulez utiliser le service Power BI Embedded pour créer une application métier interne, vos utilisateurs internes ont besoin d’une licence d’abonnement utilisateur Power BI Pro. En outre, votre organisation est facturée pour leur consommation du service Power BI Embedded ainsi que pour leurs licences d’abonnement utilisateur Power BI Pro. Pour ne pas payer aussi bien les frais de licences d’abonnement utilisateur Power BI Pro que les frais de consommation Power BI Embedded pour les applications internes, le service Power BI offre ses propres fonctionnalités d’incorporation de contenu en dehors de Power BI Embedded sans coût supplémentaire pour les détenteurs de licences d’abonnement utilisateur Power BI (dev.powerbi.com).

## Est-il possible d’utiliser Power BI Embedded pour créer des applications internes ?

Non, Power BI Embedded est uniquement destiné à un usage par des utilisateurs externes. Il ne doit pas être utilisé dans des applications métier internes. Pour incorporer et utiliser du contenu Power BI dans des applications professionnelles internes, vous devez utiliser le service Power BI, et toutes les personnes utilisant ce contenu doivent avoir une licence d’abonnement utilisateur valide à Power BI Gratuit ou à Power BI Pro. Pour plus d’informations sur l’intégration d’applications internes au service Power BI, voir [https://dev.powerbi.com](https://dev.powerbi.com).

## Ce service est-il disponible partout dans le monde ?

Le service Power BI Embedded est désormais disponible dans la plupart des centres de données. Vous pouvez vérifier les dernières disponibilités [ici](https://azure.microsoft.com/status/).

## Quel est le contrat SLA disponible pour le service ?

Power BI Embedded avec contrat SLA Azure standard. Pour plus d’informations, consultez la page [Contrats de niveau de service](https://azure.microsoft.com/support/legal/sla/).

## Comment ce service sera-t-il facturé ?

Pour connaître la tarification, consultez la page [Tarification de Power BI Embedded](http://go.microsoft.com/fwlink/?LinkId=760527).

## Qu’est-ce qu’un rendu et comment est-il facturé ?

>[AZURE.NOTE] Suite aux commentaires reçus des utilisateurs, la tarification par rendu (remise incluse) qui était offerte pendant la version préliminaire de Power BI Embedded sera remplacée par une tarification par session. La transition de la tarification par rendu à la tarification par session sera effective le 1er septembre 2016.

Un rendu est un élément visuel qui s’affiche à un utilisateur final suite à une requête adressée au service. Par exemple, si un utilisateur consulte un rapport contenant 4 éléments visuels, l’opération génère 4 rendus. Si l’utilisateur actualise le rapport et que davantage de requêtes sont envoyées au service, il en résulte 4 rendus supplémentaires. Pour limiter l’exposition du coût et réduire les coûts dans des scénarios d’analyse de données statiques, le propriétaire du service peut contrôler le nombre de requêtes engendrant des rendus payants que les utilisateurs finaux peuvent émettre.

Les rendus sont facturés par tranches de 1 000 unités. Pour les quantités de rendus inférieures à 1 000, la facturation est établie au pro rata. Les clients reçoivent 1 000 rendus gratuits par mois. Les clients qui achètent le service via des contrats de licence en volume doivent consulter leur partenaire ou revendeur Microsoft pour obtenir les informations de tarification.

## Qu’est-ce qu’une session de rapport et comment est-elle facturée ?

Une session est un ensemble d’interactions entre un utilisateur final et un rapport Power BI Embedded. À chaque fois qu’un rapport Power BI Embedded est affiché par un utilisateur, une session est initiée et facturée au détenteur de l’abonnement. Les sessions sont facturées sur une base forfaitaire, indépendamment du nombre d’éléments visuels du rapport ou de la fréquence d’actualisation du contenu du rapport. Une session se termine lorsque l’utilisateur ferme le rapport ou lorsque la session expire après une heure.

## Fournissez-vous des outils ou une assistance pour m’aider à estimer le nombre de rendus/session ? Comment suivre le nombre de rendus réalisés ?

Vous trouverez sur le portail Azure la facturation détaillée des rendus/sessions de rapport comptabilisés dans le cadre de votre abonnement.

## Ai-je besoin d’un abonnement Power BI pour développer des applications avec Power BI Embedded ? Comment faire pour démarrer ?

En tant que développeur d’applications, vous n’avez pas besoin d’abonnement Power BI pour créer les rapports et les visualisations que vous souhaitez incorporer dans votre application. Vous avez besoin d’un abonnement Microsoft Azure et de l’application gratuite Power BI Desktop.

Consultez notre documentation sur le service Power BI Embedded pour en savoir plus sur son utilisation.

## J’ai un abonnement Azure. Puis-je utiliser Power BI Embedded dans le cadre de cet abonnement ?

Oui. Vous pouvez utiliser votre abonnement Azure existant pour approvisionner et utiliser le service Microsoft Power BI Embedded.

## Les utilisateurs de mon application ont-ils besoin d’une licence Power BI ?

Non. Les utilisateurs finaux de votre application n’ont pas besoin d’acheter un abonnement Power BI distinct pour afficher les visualisations de données incorporées à votre application. Dans le modèle Power BI Embedded, l’utilisation du service est facturée au fournisseur de l’application sur la base de l’indicateur de consommation Azure. Pour plus d’informations, consultez la [page sur les licences et la tarification](http://go.microsoft.com/fwlink/?LinkId=760527).

## Comment les utilisateurs sont-ils authentifiés par Power BI Embedded ?

Le service Power BI Embedded utilise des jetons d’application pour l’authentification et l’autorisation des utilisateurs finaux. Il n’utilise pas l’authentification explicite. Dans ce modèle de jetons d’application, votre application gère l’authentification et l’autorisation de vos utilisateurs finaux. Ensuite, quand cela est nécessaire, votre application crée

et envoie les jetons d’application à notre service pour lui indiquer d’afficher le rendu du rapport demandé. Avec cette conception, votre application peut gérer l’authentification et l’autorisation des utilisateurs sans passer par Azure AD. Cette option est toutefois possible. Pour plus d’informations sur les jetons d’application, cliquez [ici](power-bi-embedded-app-token-flow.md). Nous avons également introduit la fonctionnalité de sécurité au niveau des lignes (RLS) dans Power BI Embedded pour les scénarios de filtrage de sécurité avancés.

## Quelles sont les sources de données actuellement prises en charge avec Power BI Embedded ?

Nous prenons en charge l’accès aux sources de données cloud qui utilisent des informations d’identification de base via une requête directe. Cela signifie que les sources telles qu’Azure SQL DB et Azure SQL DW sont prises en charge dès à présent. Nous allons étendre la prise en charge d’autres sources de données et types d’accès dans les prochains mois. Nous indiquerons les nouvelles sources de données prises en charge sur le forum de développement Power BI à l’adresse [https://dev.powerbi.com](https://dev.powerbi.com/).

## Comment le modèle d’architecture cliente fonctionne-t-il pour Power BI Embedded ?

Le modèle Power BI Embedded n’impose pas explicitement que vos utilisateurs soient dans des clients Azure AD. Vous avez le choix d’exiger ou non Azure AD pour vos clients. Vous devez donc déterminer le modèle d’architecture cliente nécessaire pour Power BI Embedded par rapport à l’architecture de votre application et de votre infrastructure.

Les développeurs ou employés qui conçoivent ou utilisent votre application doivent avoir un compte d’utilisateur AAD pour pouvoir gérer votre abonnement Azure et les collections d’espaces de travail via le portail Azure. Les API de programmation utilisées par les développeurs pour importer des rapports, modifier des chaînes de connexion et obtenir des URL incorporées se servent de jetons d’application pour l’authentification. Elles ne nécessitent donc pas AAD. Vous trouverez plus de détails sur l’utilisation de nos API et du portail Azure sur la [page de documentation de Power BI Embedded](https://azure.microsoft.com/documentation/services/power-bi-embedded/) sur Azure.com.

## Où en savoir plus ?

Vous pouvez consulter la [page de documentation de Power BI Embedded](http://go.microsoft.com/fwlink/?LinkId=760526). Vous pouvez aussi rester informé des dernières actualités concernant ce service en consultant le [blog des développeurs Power BI](http://blogs.msdn.com/powerbidev) ou le centre de développement Power BI à l’adresse dev.powerbi.com. Vous pouvez également poser vos questions sur le site [Stackoverflow](http://stackoverflow.com/questions/tagged/powerbi).

## Comment faire pour démarrer ?

Vous pouvez démarrer gratuitement dès maintenant ! Si vous avez un abonnement Azure, vous pouvez approvisionner Power BI Embedded directement à partir du portail Azure. Si vous n’en avez pas, vous pouvez créer votre [compte Azure gratuit](https://azure.microsoft.com/free/). Une fois que vous avez approvisionné le service Power BI Embedded, vous pouvez facilement utiliser directement les API REST de Power BI ou utiliser le kit de développement logiciel (SDK) disponible sur [GitHub](http://go.microsoft.com/fwlink/?LinkID=746472). Plusieurs exemples sont fournis pour vous aider à utiliser le Kit SDK.

## Voir aussi

- [Présentation de Microsoft Power BI Embedded](power-bi-embedded-what-is-power-bi-embedded.md)
- [Prise en main de Microsoft Power BI Embedded](power-bi-embedded-get-started.md)

<!---HONumber=AcomDC_0713_2016-->