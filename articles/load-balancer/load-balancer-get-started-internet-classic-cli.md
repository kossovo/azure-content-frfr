<properties 
   pageTitle="Création d'un équilibreur de charge accessible sur Internet dans un modèle de déploiement classique à l'aide de l'interface de ligne de commande Azure | Microsoft Azure"
   description="Découvrez comment créer un équilibreur de charge accessible sur Internet dans un modèle de déploiement classique à l’aide de l’interface de ligne de commande Azure"
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
   ms.date="02/09/2016"
   ms.author="joaoma" />

# Création d'un équilibreur de charge accessible sur Internet (classique) dans l'interface de ligne de commande CLI Azure

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Cet article traite du modèle de déploiement classique. Vous pouvez également [découvrir comment créer un équilibreur de charge accessible sur Internet à l’aide d’Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]


## Création par étapes d’un équilibreur de charge accessible sur Internet à l’aide de l’interface de ligne de commande CLI

Ce guide indique comment créer un équilibreur de charge Internet selon le scénario ci-dessus.

1. Si vous n’avez jamais utilisé l’interface de ligne de commande Azure, consultez [Installation et configuration de l’interface de ligne de commande CLI Azure](../../articles/xplat-cli-install.md) et suivez les instructions jusqu’à l’étape vous invitant à sélectionner votre compte et votre abonnement Azure.

2. Exécutez la commande **azure config mode** pour passer en mode classique, comme illustré ci-dessous.

		azure config mode asm

	Sortie attendue :

		info:    New mode is asm


## Création d'un point de terminaison et d'un jeu d'équilibrage de charge 

Le scénario suppose que les machines virtuelles « web1 » et « web2 » ont été créées. Ce guide crée un ensemble d’équilibreurs de charge à l’aide du port 80 comme port public et du port 80 comme port local. Un port de sonde est également configuré sur le port 80 et le jeu d’équilibreur de charge est nommé « lbset ».


### Étape 1 

Créez le premier point de terminaison et le jeu d’équilibreur de charge avec `azure network vm endpoint create` pour la machine virtuelle « web1 ».

	azure vm endpoint create web1 80 -k 80 -o tcp -t 80 -b lbset 

Paramètres utilisés :

**-k** - port local de machine virtuelle<br> **-o** - protocole<BR> **-t -** - port de la sonde<BR> **b -** - nom de l’équilibreur de charge<BR>
 
## Étape 2 

Ajoutez une deuxième machine virtuelle « web2 » pour le jeu d'équilibrage de charge.

	azure vm endpoint create web2 80 -k 80 -o tcp -t 80 -b lbset

## Étape 3 

Vérifiez la configuration de l’équilibreur de charge à l’aide de `azure vm show`.

	azure vm show web1

La sortie se présente comme suit :

	data:    DNSName "contoso.cloudapp.net"
	data:    Location "East US"
	data:    VMName "web1"
	data:    IPAddress "10.0.0.5"
	data:    InstanceStatus "ReadyRole"
	data:    InstanceSize "Standard_D1"
	data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-2015
	6-en.us-127GB.vhd"
	data:    OSDisk hostCaching "ReadWrite"
	data:    OSDisk name "joaoma-1-web1-0-201509251804250879"
	data:    OSDisk mediaLink "https://XXXXXXXXXXXXXXX.blob.core.windows.
	/vhds/joaomatest-web1-2015-09-25.vhd"
	data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Se
	r-2012-R2-20150916-en.us-127GB.vhd"
	data:    OSDisk operatingSystem "Windows"
	data:    OSDisk iOType "Standard"
	data:    ReservedIPName ""
	data:    VirtualIPAddresses 0 address "XXXXXXXXXXXXXXXX"
	data:    VirtualIPAddresses 0 name "XXXXXXXXXXXXXXXXXXXX"
	data:    VirtualIPAddresses 0 isDnsProgrammed true
	data:    Network Endpoints 0 loadBalancedEndpointSetName "lbset"
	data:    Network Endpoints 0 localPort 80
	data:    Network Endpoints 0 name "tcp-80-80"
	data:    Network Endpoints 0 port 80
	data:    Network Endpoints 0 loadBalancerProbe port 80
	data:    Network Endpoints 0 loadBalancerProbe protocol "tcp"
	data:    Network Endpoints 0 loadBalancerProbe intervalInSeconds 15
	data:    Network Endpoints 0 loadBalancerProbe timeoutInSeconds 31
	data:    Network Endpoints 0 protocol "tcp"
	data:    Network Endpoints 0 virtualIPAddress "XXXXXXXXXXXX"
	data:    Network Endpoints 0 enableDirectServerReturn false
	data:    Network Endpoints 1 localPort 5986
	data:    Network Endpoints 1 name "PowerShell"
	data:    Network Endpoints 1 port 5986
	data:    Network Endpoints 1 protocol "tcp"
	data:    Network Endpoints 1 virtualIPAddress "XXXXXXXXXXXX"
	data:    Network Endpoints 1 enableDirectServerReturn false
	data:    Network Endpoints 2 localPort 3389
	data:    Network Endpoints 2 name "Remote Desktop"
	data:    Network Endpoints 2 port 58081
	info:    vm show command OK

## Création d'un point de terminaison de Bureau à distance pour une machine virtuelle

Vous pouvez créer un point de terminaison de Bureau à distance pour transférer le trafic réseau à partir d’un port public vers un port local pour une machine virtuelle spécifique à l’aide d’`azure vm endpoint create`.

	azure vm endpoint create web1 54580 -k 3389 


## Suppression d’une machine virtuelle de l’équilibreur de charge

Vous devez supprimer le point de terminaison associé à l'équilibreur de charge de la machine virtuelle. Une fois le point de terminaison supprimé, la machine virtuelle n'appartient plus au jeu d'équilibrage de charge.

 À l'aide de l'exemple ci-dessus, vous pouvez supprimer le point de terminaison créé pour la machine virtuelle « web1 » de l'équilibreur de charge « lbset » à l'aide de la commande `azure vm endpoint delete`.

	azure vm endpoint delete web1 tcp-80-80


>[AZURE.NOTE] Vous pouvez explorer d’autres options de gestion des points de terminaison à l’aide de la commande `azure vm endpoint --help`


## Étapes suivantes

[Prise en main de la configuration d’un équilibrage de charge interne](load-balancer-get-started-ilb-arm-ps.md)

[Configuration d'un mode de distribution d'équilibrage de charge](load-balancer-distribution-mode.md)

[Configuration des paramètres de délai d’expiration TCP inactif pour votre équilibrage de charge](load-balancer-tcp-idle-timeout.md)

 

<!---HONumber=AcomDC_0323_2016-->