<properties
   pageTitle="Créer un nom de domaine complet pour une machine virtuelle dans le portail Azure | Microsoft Azure"
   description="Apprenez à créer un nom de domaine complet pour une machine virtuelle Resource Manager dans le portail Azure."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="06/07/2016"
   ms.author="iainfou"/>

# Créer un nom de domaine complet dans le portail Azure
Lorsque vous créez une machine virtuelle dans le [portail Azure](https://portal.azure.com) à l’aide du modèle de déploiement Resource Manager, une ressource d’adresse IP publique est créée automatiquement pour la machine virtuelle. Vous pouvez utiliser cette adresse IP pour accéder à distance à la machine virtuelle. Bien que le portail ne crée pas de [nom de domaine complet](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) (FQDN) par défaut, il est très facile d’en générer un une fois la machine virtuelle créée. Cet article explique les étapes pour créer un nom DNS ou un nom de domaine complet.

[AZURE.INCLUDE [virtual-machines-common-portal-create-fqdn](../../includes/virtual-machines-common-portal-create-fqdn.md)]

Vous pouvez maintenant vous connecter à distance à la machine virtuelle à l’aide de ce nom DNS comme pour Remote Desktop Protocol (RDP).

## Étapes suivantes
Maintenant que votre machine virtuelle a un nom DNS et une IP publique, vous pouvez déployer des services ou des infrastructures d’applications tels que IIS, SQL, SharePoint, etc.

Vous pouvez également lire un autre article sur [l’utilisation de Resource Manager](../resource-group-overview.md) pour obtenir des conseils sur la création de vos déploiements Azure.

<!---HONumber=AcomDC_0608_2016-->