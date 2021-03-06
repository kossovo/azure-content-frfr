<properties
	pageTitle="Gestion d'Azure Key Vault avec Azure Automation | Microsoft Azure"
	description="Découvrez comment utiliser le service Azure Automation pour gérer Azure Key Vault."
	services="Key-Vault, automation"
	documentationCenter=""
	authors="csand-msft"
	manager="eamono"
	editor=""/>

<tags
	ms.service="key-vault"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/18/2016"
	ms.author="csand"/>



#Gestion d'Azure Key Vault avec Azure Automation

Ce guide vous présente le service Azure Automation et la manière de l'utiliser pour gérer plus simplement vos clés et secrets dans Azure Key Vault.

## Qu'est-ce qu'Azure Automation ?

[Azure Automation](https://azure.microsoft.com/services/automation/) est un service Azure destiné à simplifier la gestion du cloud via l'automatisation des processus et la configuration de l’état souhaité. Il automatise les tâches manuelles, répétitives, fastidieuses et sources d’erreurs pour accroître la fiabilité, l’efficacité et le retour sur investissement de votre organisation.

Azure Automation fournit un moteur d’exécution de workflow à haute fiabilité et à haute disponibilité, qui s’adapte à vos besoins. Dans Azure Automation, les processus peuvent être lancés manuellement, par des systèmes tiers ou à des intervalles planifiés, afin que les tâches interviennent exactement au moment opportun.

Réduisez les coûts opérationnels et libérez du temps pour que votre équipe d’informaticiens et de développeurs puisse se concentrer sur des tâches qui génèrent une valeur ajoutée pour l’entreprise, en déléguant l’exécution automatique de vos tâches de gestion de Cloud à Azure Automation.


## Comment Azure Automation peut-il aider à gérer Azure Key Vault ?

Key Vault peut être géré dans Azure Automation à l'aide des [applets de commande AzureRM Key Vault](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) et des [applets de commande Azure Classic Key Vault](https://msdn.microsoft.com/library/azure/dn868052.aspx). Le module Azure pour la gestion de Classic Key Vault est disponible automatiquement dans Azure Automation. Vous pouvez importer le [module AzureRM-KeyVault](https://www.powershellgallery.com/packages/AzureRM.KeyVault/1.1.4) dans Azure Automation pour pouvoir effectuer la plupart de vos tâches de gestion Key Vault dans le service. Dans Azure Automation, vous pouvez également associer ces applets de commande à des applets de commande d'autres services Azure, afin d'automatiser des tâches complexes entre des services Azure et des systèmes tiers.

Les applets de commande Azure Key Vault permettent d’effectuer, entre autres, les tâches suivantes : création et configuration d’un coffre de clés, création ou importation d’une clé, création ou mise à jour d’un secret, mise à jour des attributs d’une clé, obtention d’une clé ou d’un secret, suppression d’une clé ou d’un secret.

Voici quelques exemples d’utilisation de PowerShell pour Key Vault :
* [Azure Key Vault – Étape par étape](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step/)
* [Setting Up and Configuring an Azure Key Vault](https://www.simple-talk.com/cloud/platform-as-a-service/setting-up-and-configuring-an-azure-key-vault/) (Installation et configuration d’Azure Key Vault)


## Étapes suivantes

Maintenant que vous connaissez les bases d’Azure Automation et que vous savez l’utiliser pour gérer Azure Key Vault, cliquez sur les liens ci-dessous pour en savoir plus.

* Consultez le [didacticiel de mise en route](../automation/automation-first-runbook-graphical.md) d'Azure Automation.
* Consultez les [scripts PowerShell pour Azure Key Vault](https://gallery.technet.microsoft.com/scriptcenter/Azure-Key-Vault-Powershell-1349b091).

<!---HONumber=AcomDC_0518_2016-->