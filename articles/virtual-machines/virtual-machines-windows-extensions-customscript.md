<properties
   pageTitle="Scripts personnalisés sur des machines virtuelles Windows utilisant des modèles | Microsoft Azure"
   description="Automatisation des tâches de configuration de machine virtuelle Windows à l’aide de l’extension de script personnalisé avec des modèles Resource Manager"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# Utilisation de l’extension de script personnalisé pour les machines virtuelles Windows avec les modèles Azure Resource Manager

[AZURE.INCLUDE [virtual-machines-common-extensions-customscript](../../includes/virtual-machines-common-extensions-customscript.md)]

## Exemple de modèle pour une machine virtuelle sous Windows

Définir la ressource suivante dans la section Ressource du modèle

       {
       "type": "Microsoft.Compute/virtualMachines/extensions",
       "name": "MyCustomScriptExtension",
       "apiVersion": "2015-05-01-preview",
       "location": "[parameters('location')]",
       "dependsOn": [
           "[concat('Microsoft.Compute/virtualMachines/',parameters('vmName'))]"
       ],
       "properties": {
           "publisher": "Microsoft.Compute",
           "type": "CustomScriptExtension",
           "typeHandlerVersion": "1.7",
           "autoUpgradeMinorVersion":true,
           "settings": {
               "fileUris": [
               "http://Yourstorageaccount.blob.core.windows.net/customscriptfiles/start.ps1"
           ],
           "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File start.ps1"
         }
       }
     }

Dans l’exemple ci-dessus, remplacez l’URL du fichier et le nom de fichier par vos propres paramètres.

Une fois le modèle créé, vous pouvez le déployer en utilisant Azure PowerShell.

Reportez-vous à l’exemple ci-dessous pour obtenir des exemples complets de configuration d’applications sur une machine virtuelle avec des extensions de script personnalisé.

* [Extension de script personnalisé sur une machine virtuelle Windows.](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/)

<!---HONumber=AcomDC_0601_2016-->