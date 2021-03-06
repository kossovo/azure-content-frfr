<properties
	pageTitle="Instructions pour les infrastructures réseau | Microsoft Azure"
	description="Découvrez-en plus sur les principales instructions de conception et d’implémentation pour le déploiement d’un réseau virtuel dans des services d’infrastructure Azure."
	documentationCenter=""
	services="virtual-machines-windows"
	authors="iainfoulds"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/30/2016"
	ms.author="iainfou"/>

# Instructions pour les infrastructures réseau

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

Cet article se concentre sur la compréhension des étapes de planification nécessaires pour un réseau virtuel dans Azure et la connectivité entre les environnements locaux existants.


## Instructions d’implémentation pour les réseaux virtuels

Décisions :

- Quel type de réseau virtuel devez-vous pour héberger votre charge de travail ou votre infrastructure informatique (cloud ou intersite) ?
- Pour les réseaux virtuels intersite, de quelle taille d’espace adressage avez-vous besoin pour héberger les sous-réseaux et les machines virtuelles maintenant et pour une extension future raisonnable ?
- Allez-vous créer des réseaux virtuels centralisés ou plutôt des réseaux virtuels pour chaque groupe de ressources ?

Tâches :

- Définissez l’espace d’adressage du réseau virtuel à créer.
- Définir l’ensemble de sous-réseaux et l’espace d’adressage pour chacun.
- Pour les réseaux virtuels intersite, définir l’ensemble des espaces d’adressage de réseau local pour les emplacements locaux auxquels les machines virtuelles doivent accéder dans le réseau virtuel.
- Travaillez avec l’équipe réseau locale pour assurer que le routage approprié est configuré lors de la création de réseaux virtuels intersite.
- Créer le réseau virtuel à l’aide de votre convention d’affectation de noms.


## Réseaux virtuels

Des réseaux virtuels sont nécessaires pour prendre en charge les communications entre les machines virtuelles. Vous pouvez définir des sous-réseaux, des adresses IP personnalisées, des paramètres DNS, des filtrages de sécurité et l’équilibrage de charge, tout comme avec les réseaux physiques. Avec un [VPN de site à site](../vpn-gateway/vpn-gateway-topology.md) ou un [Circuit Express Route](../expressroute/expressroute-introduction.md), vous pouvez connecter des réseaux virtuels Azure à vos réseaux locaux. Vous pouvez en savoir plus sur [les réseaux virtuels et leurs composants](../virtual-network/virtual-networks-overview.md).

Avec l’utilisation de groupes de ressources, vous disposez de plus de flexibilité dans la façon dont vous concevez vos composants de réseau virtuel. Les machines virtuelles peuvent se connecter à des réseaux virtuels en dehors de leur propre groupe de ressources. Une approche de conception courante consisterait à créer des groupes de ressources centralisés contenant votre infrastructure réseau de base, qui peut être gérée par une équipe commune, puis des machines virtuelles et leurs applications déployées sur des groupes de ressources distincts. Cela permet aux propriétaires d’applications d’accéder au groupe de ressources qui contient leurs machines virtuelles sans ouvrir l’accès à la configuration des ressources réseau virtuelles à plus grande échelle.

## Connectivité de site

### Réseaux virtuels cloud uniquement
Si les utilisateurs et les ordinateurs locaux ne nécessitent pas de connectivité continue aux machines virtuelles dans un réseau virtuel Azure, votre conception de réseau virtuel sera très simple :

![Diagramme d’un réseau virtuel de base uniquement dans le cloud](./media/virtual-machines-common-infrastructure-service-guidelines/vnet01.png)

Cette solution s’applique généralement à des charges de travail Internet, telles qu’un serveur web Internet. Vous pouvez gérer ces machines virtuelles par RDP ou connexion VPN point à site.

Les réseaux virtuels ne se connectant pas à votre réseau local, les réseaux virtuels uniquement basés sur Azure peuvent utiliser une partie de l’espace d’adressage IP privé, même si le même espace privé est utilisé localement.


### Réseaux virtuels intersite
Si les utilisateurs et ordinateurs locaux nécessitent une connectivité continue aux machines virtuelles dans un réseau virtuel Azure, créez un réseau virtuel intersite et connectez-le à votre réseau local avec une connexion ExpressRoute ou VPN de site à site.

![Diagramme de réseau virtuel entre sites locaux](./media/virtual-machines-common-infrastructure-service-guidelines/vnet02.png)

Dans cette configuration, le réseau virtuel Azure est essentiellement une extension cloud de votre réseau local.

Étant donné qu’ils se connectent à votre réseau local, les réseaux virtuels intersite doivent utiliser une partie de l’espace d’adressage utilisé par votre organisation, qui est unique. De la même façon que différents emplacements de l’entreprise se verront attribuer un sous-réseau IP spécifique, Azure devient un autre emplacement lorsque vous étendez votre réseau.

Pour autoriser les paquets à circuler de votre réseau virtuel intersite vers votre réseau local, vous devez configurer le jeu de préfixes d’adresses locales pertinents dans le cadre de la définition du réseau local pour le réseau virtuel. En fonction de l’espace d’adressage du réseau virtuel et de l’ensemble des emplacements locaux concernés, le réseau local peut comprendre de nombreux préfixes d’adresse.

Vous pouvez convertir un réseau virtuel cloud en un réseau virtuel intersite, mais il nécessitera probablement une renumérotation de l’IP de votre espace d’adressage de réseau virtuel, de vos sous-réseaux et de vos machines virtuelles qui utilisent des adresses IP Azure. Par conséquent, déterminez si un réseau virtuel devra se connecter à votre réseau local lorsque vous affectez un sous-réseau d’IP.

## Sous-réseaux
Les sous-réseaux vous permettent d’organiser des ressources apparentées, soit logiquement (par exemple, un sous-réseau pour les machines virtuelles associées à la même application), soit physiquement (par exemple, un sous-réseau par groupe de ressources), ou d’employer des techniques d’isolation de sous-réseaux pour renforcer la sécurité.

Pour les réseaux virtuels intersite, vous devez concevoir des sous-réseaux avec les conventions que vous utilisez pour les ressources locales, en gardant à l’esprit qu’**Azure utilise toujours les trois premières adresses IP de l’espace d’adressage pour chaque sous-réseau**. Pour déterminer le nombre d’adresses nécessaires pour le sous-réseau, comptez le nombre de machines virtuelles dont vous avez besoin maintenant, estimez sa croissance future, puis utilisez le tableau suivant pour déterminer la taille du sous-réseau.

Nombre de machines virtuelles nécessaires | Nombre de bits hôte nécessaires | Taille du sous-réseau
--- | --- | ---
1 à 3 | 3 | /29
4 à 11 | 4 | /28
12 à 27 | 5 | /27
28 à 59 | 6 | /26
60 à 123 | 7 | /25

> [AZURE.NOTE] Pour des sous-réseaux locaux normaux, le nombre maximal d’adresses d’hôte pour un sous-réseau avec n bits hôte est 2<sup>n</sup> – 2. Pour un sous-réseau Azure, le nombre maximal d’adresses d’hôte pour un sous-réseau avec n bits hôte est 2<sup>n</sup> – 5 (2 plus 3 pour les adresses qu’Azure utilise sur chaque sous-réseau).

Si vous choisissez une taille de sous-réseau trop petite, vous devrez renuméroter les IP et redéployer les machines virtuelles dans le sous-réseau.


## Groupes de sécurité réseau
Vous pouvez appliquer des règles de filtrage pour le trafic qui transite via vos réseaux virtuels à l’aide de groupes de sécurité réseau. Vous pouvez créer des règles de filtrage très granulaires pour sécuriser votre environnement de réseau virtuel, le trafic entrant et sortant du trafic, les plages d’IP de source et de destination, les ports autorisés, etc. Les groupes de sécurité réseau peuvent être appliqués à des sous-réseaux au sein d’un réseau virtuel, ou directement à une interface réseau virtuelle spécifiée. Il est recommandé d’avoir un certain niveau de filtrage du trafic sur vos groupes de sécurité réseau pour vos réseaux virtuels. Vous pouvez [en savoir plus sur les groupes de sécurité réseau](../virtual-network/virtual-networks-nsg.md).


## Composants réseau supplémentaires
Comme avec une infrastructure de réseau local physique, un réseau virtuel Microsoft Azure peut contenir plus que des sous-réseaux et des adresses IP. Lorsque vous concevez votre infrastructure d’application, vous souhaiterez incorporer certains de ces composants supplémentaires :

- [Des passerelles VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) : connectez des réseaux virtuels Azure à d’autres réseaux virtuels Azure, sur les réseaux locaux via une connexion VPN de site à site, fournissez aux utilisateurs un accès direct avec les connexions VPN point à site ou implémentez des connexions Express Route pour des connexions dédiées et sécurisées.
- [Un équilibreur de charge](../load-balancer/load-balancer-overview.md) - fournit l’équilibrage de la charge du trafic pour le trafic externe et interne de la façon dont vous le souhaitez.
- [Une Application Gateway](../application-gateway/application-gateway-introduction.md) - équilibrage de charge HTTP de la couche application, en fournissant des avantages supplémentaires pour les applications web par rapport au simple déploiement de l’équilibrage de charge Azure.
- [Traffic Manager](../traffic-manager/traffic-manager-overview.md) - distribution du trafic basée sur DNS aux utilisateurs finaux directs vers le point de terminaison de l’application disponible le plus proche, ce qui vous permet d’héberger votre application hors des centres de données Azure dans différentes régions.


## Étapes suivantes

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]

<!---HONumber=AcomDC_0706_2016-->