<properties 
   pageTitle="Création d'un équilibreur de charge accessible sur Internet dans un modèle de déploiement classique à l'aide du portail Azure | Microsoft Azure"
   description="Découvrez comment créer un équilibreur de charge accessible sur Internet dans un modèle de déploiement classique à l’aide du portail Azure"
   services="load-balancer"
   documentationCenter="na"
   authors="joaoma"
   manager="carolz"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/17/2016"
   ms.author="joaoma" />

# Création d'un équilibreur de charge accessible sur Internet (classique) dans le portail Azure

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Cet article traite du modèle de déploiement classique. Vous pouvez également [découvrir comment créer un équilibreur de charge accessible sur Internet à l’aide d’Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).

 
[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]



## Créer un point de terminaison d’équilibreur de charge à l’aide du portail Azure	

Pour créer un modèle de déploiement (classique) d’équilibreur de charge accessible sur Internet à partir du portail Azure, suivez les étapes ci-dessous.

1. Dans un navigateur, accédez à http://portal.azure.com et, si nécessaire, connectez-vous avec votre compte Azure.

2. Accédez au panneau (classique) des machines virtuelles > sélectionnez une machine virtuelle.

3. Dans le panneau « Essentials » des machines virtuelles > sélectionnez « Tous les paramètres »

4. Cliquez sur Jeux d’équilibrage de charge.

5. Pour créer un équilibreur de charge, cliquez sur l'icône « Joindre » en haut du panneau des jeux d'équilibrage de charge.

6. Sélectionnez le « Type de jeu d'équilibrage de la charge » public pour l'équilibreur de charge accessible sur Internet.

7. Cliquez sur « Configurer les paramètres requis » pour ouvrir « Choisir un jeu d'équilibrage de charge », puis sur « Créer un jeu d'équilibrage de charge ».

8. Dans le volet « Créer un jeu d'équilibrage de charge », donnez un nom au jeu d'équilibrage de charge. Renseignez le nom, le port public, le protocole de sonde, le port de la sonde.

9. Si nécessaire, modifiez l'intervalle de sondage et de nouvelles tentatives.

10. (facultatif) Si vous le souhaitez, vous pouvez configurer des règles de contrôle d'accès dans le panneau de création de jeux d'équilibrage de charge.

11. Cliquez sur Ok pour revenir au panneau « Joindre le jeu d'équilibrage de charge ».

12. Cliquez sur Ok et attendez que la nouvelle ressource d'équilibrage de charge s'affiche dans le panneau « Jeux d'équilibrage de charge ».
 
## Étapes suivantes

[Prise en main de la configuration d’un équilibrage de charge interne](load-balancer-get-started-ilb-arm-ps.md)

[Configuration d'un mode de distribution d'équilibrage de charge](load-balancer-distribution-mode.md)

[Configuration des paramètres de délai d’expiration TCP inactif pour votre équilibrage de charge](load-balancer-tcp-idle-timeout.md)

<!---HONumber=AcomDC_0323_2016-->