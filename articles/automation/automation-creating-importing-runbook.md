<properties
	pageTitle="Création ou importation d’un runbook dans Azure Automation"
	description="Cet article explique comment créer un runbook dans Azure Automation ou comment en importer un à partir d’un fichier."
	services="automation"
	documentationCenter=""
	authors="mgoedtel"
	manager="jwhit"
	editor="tysonn" />
<tags
	ms.service="automation"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.workload="infrastructure-services"
	ms.date="05/31/2016"
	ms.author="magoedte;bwren" />

# Création ou importation d’un runbook dans Azure Automation

Vous pouvez ajouter un runbook à Azure Automation en [en créant un](#creating-a-new-runbook) ou en important un runbook existant à partir d’un fichier ou de la [galerie de runbooks](automation-runbook-gallery.md). Cet article fournit des informations sur la création et l’importation des runbooks à partir d’un fichier. Vous pouvez obtenir toutes les informations sur l’accès aux runbooks et modules communautaires dans [Galeries de runbooks et de modules pour Azure Automation](automation-runbook-gallery.md).

## Création d’un runbook

Vous pouvez créer un runbook dans Azure Automation à l’aide de l’un des portails Azure ou de Windows PowerShell. Une fois que le runbook a été créé, vous pouvez le modifier à l’aide des informations disponibles dans [Apprentissage du workflow Windows PowerShell](automation-powershell-workflow.md) et [Création de graphiques dans Azure Automation](automation-graphical-authoring-intro.md).

### Pour créer un runbook Azure Automation avec le portail Azure Classic

Vous pouvez uniquement travailler avec des [runbooks de workflow PowerShell](automation-runbook-types.md#powershell-workflow-runbooks) dans le portail Azure.

1. Dans le portail Azure Classic, cliquez sur **Nouveau**, **Services d’application**, **Automation**, **Runbook**, **Création rapide**.
2. Entrez les informations requises, puis cliquez sur **Créer**. Le nom du runbook doit commencer par une lettre et peut contenir des lettres, des chiffres, des traits de soulignement et des tirets.
3. Si vous souhaitez modifier le runbook maintenant, cliquez sur **Modifier le runbook**. Sinon, cliquez sur **OK**.
4. Votre nouveau runbook apparaît sur l’onglet **Runbooks**.


### Pour créer un runbook Azure Automation avec le portail Azure

1. Dans le portail Azure, ouvrez votre compte Automation.
2. Cliquez sur la vignette **Runbooks** pour ouvrir la liste des runbooks.
3. Cliquez sur le bouton **Ajouter un runbook**, puis sur **Créer un runbook**.
2. Saisissez un **Nom** pour le runbook et sélectionnez son [Type](automation-runbook-types.md). Le nom du runbook doit commencer par une lettre et peut contenir des lettres, des chiffres, des traits de soulignement et des tirets.
3. Cliquez sur **Créer** pour créer le runbook et ouvrez l’éditeur.


### Pour créer un runbook Azure Automation avec Windows PowerShell

Vous pouvez utiliser l’applet de commande [New-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt619376.aspx) pour créer un [runbook de workflow PowerShell](automation-runbook-types.md#powershell-workflow-runbooks) vide. Vous pouvez spécifier le paramètre **Name** pour créer un runbook vide que vous pouvez modifier ultérieurement, ou vous pouvez spécifier le paramètre **Path** pour importer un fichier de runbook. Le paramètre **Type** doit être également inclus pour spécifier l’un des quatre types de runbook.

Les exemples de commandes suivants montrent comment créer un runbook vide.

    New-AzureRmAutomationRunbook -AutomationAccountName MyAccount `
    -Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell

## Importation d’un runbook à partir d’un fichier dans Azure Automation

Vous pouvez créer un runbook dans Azure Automation en important un script PowerShell, un workflow PowerShell (extension .ps1) ou un runbook graphique exporté (.graphrunbook). Vous devez spécifier le [type de runbook](automation-runbook-types.md) qui sera créé à partir de l’importation en tenant compte des considérations suivantes.

- Un fichier .graphrunbook peut uniquement être importé dans un nouveau [runbook graphique](automation-runbook-types.md#graphical-runbooks), et les runbooks graphiques ne peuvent être créés qu’à partir d’un fichier .graphrunbook.
- Un fichier .ps1 contenant un workflow PowerShell peut uniquement être importé dans un [runbook de workflow PowerShell](automation-runbook-types.md#powershell-workflow-runbooks). Si le fichier contient plusieurs workflows PowerShell, l’importation échoue. Vous devez enregistrer chaque workflow dans son propre fichier, puis les importer séparément.
- Un fichier .ps1 qui ne contient pas de workflow peut être importé dans un [runbook PowerShell](automation-runbook-types.md#powershell-runbooks) ou un [runbook de workflow PowerShell](automation-runbook-types.md#powershell-workflow-runbooks). S’il est importé dans un runbook de workflow PowerShell, il est ensuite converti en workflow et les commentaires sont inclus dans le runbook spécifiant les modifications qui ont été apportées.

### Pour importer un runbook à partir d’un fichier avec le portail Azure Classic
Vous pouvez utiliser la procédure suivante pour importer un fichier de script dans Azure Automation. Notez que vous pouvez uniquement importer un fichier .ps1 dans un runbook de workflow PowerShell à l’aide de ce portail. Vous devez utiliser le portail Azure pour les autres types.

1. Dans le portail de gestion Azure, sélectionnez **Automation**, puis sélectionnez un compte Automation.
2. Cliquez sur **Importer**.
3. Cliquez sur **Rechercher le fichier** et recherchez le fichier de script à importer.
4. Si vous souhaitez modifier le runbook maintenant, cliquez sur **Modifier le runbook**. Sinon, cliquez sur OK.
5. Le nouveau runbook apparaît sur l’onglet **Runbooks** pour le compte Automation.
6. Vous devez [Publier le runbook](#publishing-a-runbook) avant de pouvoir l’exécuter.


### Pour importer un runbook à partir d’un fichier avec le portail Azure
Vous pouvez utiliser la procédure suivante pour importer un fichier de script dans Azure Automation.

>[AZURE.NOTE] Notez que vous pouvez uniquement importer un fichier .ps1 dans un runbook de workflow PowerShell à l’aide de ce portail.

1. Dans le portail Azure, ouvrez votre compte Automation.
2. Cliquez sur la vignette **Runbooks** pour ouvrir la liste des runbooks.
3. Cliquez sur le bouton **Ajouter un runbook**, puis sur **Importer**.
4. Cliquez sur **Fichier runbook** pour sélectionner le fichier à importer.
2. Si le champ **Nom** est activé, vous avez la possibilité de modifier le nom. Le nom du runbook doit commencer par une lettre et peut contenir des lettres, des chiffres, des traits de soulignement et des tirets.
3. Le [type de runbook](automation-runbook-types.md) est sélectionné automatiquement, mais vous pouvez le modifier après avoir pris en compte les restrictions applicables.
3. Le nouveau runbook apparaît sur la liste des runbooks pour le compte Automation.
4. Vous devez [Publier le runbook](#publishing-a-runbook) avant de pouvoir l’exécuter.

>[AZURE.NOTE] Après avoir importé un runbook graphique ou un runbook de workflow PowerShell graphique, vous pouvez effectuer une conversion dans l’autre type le cas échéant. Vous ne pouvez pas effectuer de conversion textuelle.

### Pour importer un runbook à partir d’un fichier de script avec Windows PowerShell

Vous pouvez utiliser l’applet de commande [Import-AzureRMAutomationRunbook](https://msdn.microsoft.com/library/mt603735.aspx) pour importer un fichier de script en tant que brouillon de runbook de workflow PowerShell. Si le runbook existe déjà, l’importation échoue, sauf si vous utilisez le paramètre *-Force*.

Les exemples de commandes suivants montrent comment importer un fichier de script dans un runbook.

    $automationAccountName =  "AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $scriptPath = "C:\Runbooks\Sample_TestRunbook.ps1"
    $RGName = "ResourceGroup"

    Import-AzureRMAutomationRunbook -Name $runbookName -Path $scriptPath `
    -ResourceGroupName $RGName -AutomationAccountName $automationAccountName `
    -Type PowerShellWorkflow 


## Publication d’un runbook

Lorsque vous créez ou importez un runbook, vous devez le publier avant de pouvoir l’exécuter. Dans Automation, chaque Runbook a un brouillon et une version publiée. Seule la version publiée est disponible à l'exécution, et seul le brouillon peut être modifié. La version publiée n'est pas affectée par les modifications apportées à la version Bouillon. Lorsque le brouillon doit être rendu disponible, vous le publiez, ce qui remplace la version publiée par le brouillon.

## Pour publier un runbook à l’aide du portail Azure Classic

1. Ouvrez le runbook dans le portail Azure Classic.
1. En haut de l’écran, cliquez sur **Auteur**.
1. En bas de l’écran, cliquez sur **Publier**, puis sur **Oui** pour le message de vérification.

## Pour publier un runbook à l’aide du portail Azure

1. Ouvrez le runbook dans le portail Azure.
1. Cliquez sur le bouton **Edit**.
1. Cliquez sur le bouton **Publier**, puis sur **Oui** pour le message de vérification.


## Pour publier un runbook à l’aide de Windows PowerShell

Vous pouvez utiliser l’applet de commande [Publish-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603705.aspx) pour publier un runbook avec Windows PowerShell. Les exemples de commandes suivants montrent comment publier un exemple de runbook.

	$automationAccountName =  "AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $RGName = "ResourceGroup"

	Publish-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName `
    -Name $runbookName -ResourceGroupName $RGName


## Étapes suivantes
- Pour savoir comment vous pouvez tirer parti de la galerie de runbooks et de modules PowerShell, consultez [Galeries de runbooks et de modules pour Azure Automation](automation-runbook-gallery.md)
- Pour en savoir plus sur la modification des runbooks PowerShell et de workflow PowerShell avec un éditeur de texte, consultez [Modifier des runbooks textuels dans Azure Automation](automation-edit-textual-runbook.md)
- Pour en savoir plus sur la création de Runbooks graphiques, consultez [Création de graphiques dans Azure Automation](automation-graphical-authoring-intro.md)

<!---HONumber=AcomDC_0713_2016-->