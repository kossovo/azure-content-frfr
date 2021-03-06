<properties 
	pageTitle="Environnement de test de batterie de serveurs SharePoint 2013 | Microsoft Azure" 
	description="Découvrez comment créer une batterie de serveurs SharePoint Server 2013 intranet à deux niveaux dans un environnement de cloud hybride pour le développement ou le test." 
	services="virtual-machines-windows" 
	documentationCenter="" 
	authors="JoeDavies-MSFT" 
	manager="timlt" 
	editor=""
	tags="azure-resource-manager"/>

<tags 
	ms.service="virtual-machines-windows" 
	ms.workload="infrastructure-services" 
	ms.tgt_pltfrm="vm-windows" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/19/2016" 
	ms.author="josephd"/>

# Configuration d’une batterie de serveurs SharePoint intranet dans un cloud hybride à des fins de test

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] modèle de déploiement classique.

Les étapes de cette rubrique vous aident à créer un environnement de cloud hybride pour le test d’une batterie de serveurs SharePoint 2013 ou 2016 intranet hébergée dans Microsoft Azure. Voici la configuration obtenue.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sp/virtual-machines-windows-ps-hybrid-cloud-test-env-sp-ph3.png)
 
Cette configuration simule un SharePoint dans l'environnement de production Azure à partir de votre emplacement sur Internet. Elle comprend :

- Un réseau local simplifié (sous-réseau de réseau d'entreprise).
- Un réseau virtuel entre sites hébergé dans Microsoft Azure (TestVNET).
- Une connexion VPN de site à site.
- Une batterie de serveurs SharePoint à deux niveaux et un contrôleur de domaine secondaire dans le réseau virtuel TestVNET.

Cette configuration fournit une base et un point de départ commun à partir duquel vous pouvez :

- Développer et tester des applications sur une batterie de serveurs SharePoint intranet dans un environnement de cloud hybride.
- Tester cette charge de travail dans un cloud hybride.

Il existe trois principales étapes pour configurer cet environnement de test de cloud hybride :

1.	Configurer l’environnement de cloud hybride pour le test.
2.	Configurer l'ordinateur du serveur SQL (SQL1).
3.	Configurer le serveur SharePoint (SP1), exécutant SharePoint 2013 ou SharePoint 2016.

Cette charge de travail nécessite un abonnement Azure. Si vous avez un abonnement MSDN ou Visual Studio, consultez [Crédit Azure mensuel pour les abonnés Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

## Phase 1 : configuration de l’environnement de cloud hybride

Suivez les instructions de la rubrique [Configuration d’un environnement de cloud hybride à des fins de test](virtual-machines-windows-ps-hybrid-cloud-test-env-base.md). Étant donné que cet environnement de test ne nécessite pas la présence du serveur APP1 sur le sous-réseau de réseau d’entreprise, n’hésitez pas à l’arrêter pour le moment.

Ceci est votre configuration actuelle.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sp/virtual-machines-windows-ps-hybrid-cloud-test-env-sp-ph1.png)

> [AZURE.NOTE] Pour la phase 1, vous pouvez également configurer la [simulation d’environnement de test de cloud hybride](virtual-machines-windows-ps-hybrid-cloud-test-env-sim.md).
 
## Phase 2 : configurer l’ordinateur du serveur SQL (SQL1)

Dans le portail Azure, démarrez l’ordinateur DC2 si nécessaire.

Dans le portail Azure, connectez-vous à DC2 avec les informations d’identification CORP\\User1.

Ensuite, créez un compte d'administrateur de batterie de serveurs SharePoint. Ouvrez une invite de commandes Windows PowerShell de niveau administrateur sur DC2 et exécutez cette commande.

	New-ADUser -SamAccountName SPFarmAdmin -AccountPassword (read-host "Set user password" -assecurestring) -name "SPFarmAdmin" -enabled $true -ChangePasswordAtLogon $false

Lorsque vous êtes invité à fournir le mot de passe du compte SPFarmAdmin, tapez un mot de passe fort et notez-le dans un endroit sûr.

Ensuite, créez une machine virtuelle Azure pour SQL1 avec ces commandes à l'invite de commandes Azure PowerShell sur votre ordinateur local. Avant d’exécuter ces commandes, renseignez les valeurs des variables et supprimez les caractères < et >.

	$rgName="<your resource group name>"
	$locName="<the Azure location of your resource group>"
	$saName="<your storage account name>"
	
	$vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
	$subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
	$pip=New-AzureRMPublicIpAddress -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
	$nic=New-AzureRMNetworkInterface -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
	$vm=New-AzureRMVMConfig -VMName SQL1 -VMSize Standard_D4
	$storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
	$vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-SQLDataDisk.vhd"
	Add-AzureRMVMDataDisk -VM $vm -Name "Data" -DiskSizeInGB 100 -VhdUri $vhdURI  -CreateOption empty
	
	$cred=Get-Credential -Message "Type the name and password of the local administrator account for the SQL Server computer." 
	$vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName SQL1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
	$vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSQLServer -Offer SQL2014-WS2012R2 -Skus Standard -Version "latest"
	$vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
	$storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
	$osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-OSDisk.vhd"
	$vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
	New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Lorsque vous avez terminé, utilisez le portail Azure pour vous connecter à SQL1 à l’aide du compte d’administrateur local.

Ensuite, configurez les règles de pare-feu Windows pour autoriser le trafic pour le test de la connectivité de base et de SQL Server. À partir d'une invite de commandes Windows PowerShell de niveau administrateur sur SQL1, exécutez ces commandes.

	New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action allow 
	Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
	ping dc2.corp.contoso.com

La commande ping doit aboutir à quatre réponses réussies à partir de l’adresse IP 192.168.0.4.

Ensuite, ajoutez le disque de données supplémentaire comme nouveau volume avec la lettre de lecteur F:.

1.	Dans le volet gauche du Gestionnaire de serveur, cliquez sur **Services de fichiers et de stockage**, puis sur **Disques**.
2.	Dans le volet Contenu, dans le groupe **Disques**, cliquez sur **disque 2** (avec la **Partition** définie sur **Inconnue**).
3.	Cliquez sur **Tâches**, puis sur **Nouveau volume**.
4.	Dans la page Avant de commencer de l’Assistant Nouveau volume, cliquez sur **Suivant**.
5.	Dans la page Sélectionner le serveur et le disque, cliquez sur **Disque 2**, puis sur **Suivant**. À l’invite, cliquez sur **OK**.
6.	Dans la page Spécifier la taille du volume, cliquez sur **Suivant**.
7.	À la page Affecter à la lettre d'un lecteur ou à un dossier, cliquez sur **Suivant**.
8.	À la page Sélectionner les paramètres du système de fichiers, cliquez sur **Suivant**.
9.	À la page Confirmer les sélections, cliquez sur **Créer**.
10.	Lorsque vous avez terminé, cliquez sur **Fermer**.

Exécutez ces commandes à l’invite de commandes Windows PowerShell sur SQL1 :

	md f:\Data
	md f:\Log
	md f:\Backup

Ensuite, joignez SQL1 au domaine Active Directory CORP avec les commandes suivantes dans l’invite Windows PowerShell.

	Add-Computer -DomainName corp.contoso.com
	Restart-Computer

Utilisez le compte CORP\\User1 lorsque vous êtes invité à fournir vos informations d’identification du compte de domaine pour la commande **Add-Computer**.

Après le redémarrage, utilisez le portail Azure pour vous connecter à SQL1 à l’aide *du compte d’administrateur local*.

Ensuite, configurez SQL Server 2014 pour qu'il utilise le lecteur F: pour les nouvelles bases de données et pour les autorisations de compte d'utilisateur.

1.	Dans l’écran d’accueil, tapez **SQL Server Management**, puis cliquez sur **SQL Server 2014 Management Studio**.
2.	Dans **Se connecter au serveur**, cliquez sur **Connecter**.
3.	Dans le volet d’arborescence de l’Explorateur d’objets, cliquez avec le bouton droit sur **SQL1**, puis cliquez sur **Propriétés**.
4.	Dans la fenêtre **Propriétés du serveur**, cliquez sur **Paramètres de base de données**.
5.	Recherchez les **Emplacements de la base de données par défaut** et définissez les valeurs suivantes :
	- Pour**Data**, tapez le chemin d’accès **f:\\Data**.
	- Pour **Log**, tapez le chemin d’accès **f:\\Log**.
	- Pour **Backup**, tapez le chemin d’accès **f:\\Backup**.
	- Notez que seules les nouvelles bases de données utilisent ces emplacements.
6.	Cliquez sur **OK** pour fermer la fenêtre.
7.	Dans le volet d’arborescence de l’**Explorateur d’objets**, ouvrez **Sécurité**.
8.	Cliquez avec le bouton droit sur **Connexions** et sélectionnez **Nouvelle connexion**.
9.	Dans **Nom de connexion**, tapez **CORP\\User1**.
10.	Dans la page **Rôles de serveur**, cliquez sur **sysadmin**, puis sur **OK**.
11.	Dans le volet d’arborescence de l’**Explorateur d’objets**, cliquez avec le bouton droit sur **Connexions**, puis cliquez sur **Nouvelle connexion**.
12.	Dans la page **Général**, dans **Nom de connexion**, tapez **CORP\\SPFarmAdmin**.
13.	Dans la page **Rôles serveur**, sélectionnez **dbcreator**, puis cliquez sur **OK**.
14.	Fermez Microsoft SQL Server Management Studio.

Ceci est votre configuration actuelle.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sp/virtual-machines-windows-ps-hybrid-cloud-test-env-sp-ph2.png)

Passez à la phase 3 appropriée pour configurer un serveur SharePoint 2013 ou SharePoint 2016.

## Phase 3 : configuration du serveur SharePoint 2013 (SP1)

Commencez par créer une machine virtuelle Azure pour SP1 avec ces commandes à l'invite de commandes Azure PowerShell sur votre ordinateur local.

	$rgName="<your resource group name>"
	$locName="<the Azure location of your resource group>"
	$saName="<your storage account name>"
	
	$vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
	$subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
	$pip=New-AzureRMPublicIpAddress -Name SP1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
	$nic=New-AzureRMNetworkInterface -Name SP1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
	$vm=New-AzureRMVMConfig -VMName SP1 -VMSize Standard_D3_V2
	$cred=Get-Credential -Message "Type the name and password of the local administrator account for the SharePoint 2013 server." 
	$vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName SP1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
	$vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSharePoint -Offer MicrosoftSharePointServer -Skus 2013 -Version "latest"
	$vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
	$storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
	$osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SP1-OSDisk.vhd"
	$vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
	New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Ensuite, utilisez le portail Azure pour vous connecter à la machine virtuelle SP1 avec les informations d’identification du compte d’administrateur local.

Ensuite, configurez une règle de pare-feu Windows pour autoriser le trafic pour le test de la connectivité de base. À l’invite de commandes Windows PowerShell sur SP1, exécutez les commandes suivantes :

	Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
	ping dc2.corp.contoso.com

La commande ping doit aboutir à quatre réponses réussies à partir de l’adresse IP 192.168.0.4.

Ensuite, joignez SP1 au domaine Active Directory CORP avec les commandes suivantes dans l’invite Windows PowerShell.

	Add-Computer -DomainName corp.contoso.com
	Restart-Computer

Utilisez le compte CORP\\User1 lorsque vous êtes invité à fournir vos informations d’identification du compte de domaine pour la commande **Add-Computer**.

Après le redémarrage, utilisez le portail Azure pour vous connecter à SP1 avec le compte et le mot de passe CORP\\User1.

Ensuite, configurez le SP1 pour la nouvelle batterie de serveurs SharePoint 2013 et un site d’équipe par défaut.

1.	Dans l’écran d’accueil, tapez **produits SharePoint 2013**, puis cliquez sur **Assistant Configuration des produits SharePoint 2013**. Lorsque vous êtes invité à autoriser le programme à apporter des modifications à l’ordinateur, cliquez sur **Oui**.
2.	Sur la page d’accueil des produits SharePoint, cliquez sur **Suivant**.
3.	Dans la boîte de dialogue qui vous informe que certains services peuvent avoir besoin d’être redémarrés pendant la configuration, cliquez sur **Oui**.
4.	Dans la page Se connecter à une batterie de serveurs, cliquez sur **Créer une batterie de serveurs**, puis sur **Suivant**.
5.	Dans la page Spécifier les paramètres de la base de données de configuration, tapez **sql1.corp.contoso.com** dans **Serveur de base de données**, tapez **CORP\\SPFarmAdmin** dans **Nom d’utilisateur**, tapez le mot de passe du compte SPFarmAdmin dans **Mot de passe**, puis cliquez sur **Suivant**.
6.	Dans la page Spécifier les paramètres de sécurité de la batterie de serveurs, tapez **P@ssphrase** dans les champs **Phrase secrète** et **Confirmer la phrase secrète**, puis cliquez sur **Suivant**.
7.	Sur la page Configurer l’application Web de l’Administration centrale de SharePoint, cliquez sur **Suivant**.
8.	Dans la page Fin de l’Assistant Configuration des produits SharePoint, cliquez sur **Suivant**. L'exécution de l'Assistant Configuration des produits SharePoint peut prendre quelques minutes.
9.	Sur la page Configuration réussie, cliquez sur **Terminer**. Internet Explorer s'ouvre ensuite avec un onglet appelé Assistant Configuration de batterie de serveurs initiale.
10.	Dans la boîte de dialogue **Contribuer à l’amélioration de SharePoint**, cliquez sur **Non, je ne souhaite pas participer**, puis cliquez sur **OK**.
11.	Dans **Comment voulez-vous configurer votre batterie SharePoint**, cliquez sur **Démarrer l’Assistant**.
12.	Dans la page Configuration de votre batterie SharePoint, dans **Compte de service**, cliquez sur **Utiliser un compte géré existant**.
13.	Dans **Services**, décochez toutes les cases, sauf la case **Service d’états**, puis cliquez sur **Suivant**. La page Opération en cours peut rester affichée un moment le temps que l'opération se termine.
14.	Dans la page Créer une collection de sites, dans **Titre et description**, tapez **Contoso Corporation** dans **Titre**, spécifiez l’URL **http://sp1**/, puis cliquez sur **OK**. La page Opération en cours peut rester affichée un moment le temps que l'opération se termine. Cette étape crée un site d’équipe dont l’URL est http://sp1.
15.	Dans la page Cette page est la dernière de l’Assistant Configuration de batterie de serveurs, cliquez sur **Terminer**. L'onglet Internet Explorer affiche le site Administration centrale de SharePoint 2013.
16.	Ouvrez une session sur l'ordinateur CLIENT1 avec les informations d'identification du compte CORP\\User1, puis démarrez Internet Explorer.
17.	Dans la barre d’adresses, tapez **http://sp1/**, puis appuyez sur Entrée. Vous devriez voir le site d'équipe SharePoint de la société Contoso Corporation. L'affichage du site peut prendre un certain temps.

Ceci est votre configuration actuelle.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sp/virtual-machines-windows-ps-hybrid-cloud-test-env-sp-ph3.png)
 
Votre une batterie de serveurs SharePoint 2013 intranet dans un cloud hybride est maintenant prêt pour les tests.


## Phase 3 : configuration du serveur SharePoint 2016 (SP1)

Commencez par créer une machine virtuelle Azure pour SP1 avec ces commandes à l'invite de commandes Azure PowerShell sur votre ordinateur local.

	$rgName="<your resource group name>"
	$locName="<the Azure location of your resource group>"
	$saName="<your storage account name>"
	
	$vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
	$subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
	$pip=New-AzureRMPublicIpAddress -Name SP1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
	$nic=New-AzureRMNetworkInterface -Name SP1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
	$vm=New-AzureRMVMConfig -VMName SP1 -VMSize Standard_D3_V2
	$cred=Get-Credential -Message "Type the name and password of the local administrator account for the SharePoint 2016 server." 
	$vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName SP1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
	$vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSharePoint -Offer MicrosoftSharePointServer -Skus 2016 -Version "latest"
	$vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
	$storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
	$osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SP1-OSDisk.vhd"
	$vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
	New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Ensuite, utilisez le portail Azure pour vous connecter à la machine virtuelle SP1 avec les informations d’identification du compte d’administrateur local.

Ensuite, configurez une règle de pare-feu Windows pour autoriser le trafic pour le test de la connectivité de base. À l’invite de commandes Windows PowerShell sur SP1, exécutez les commandes suivantes :

	Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
	ping dc2.corp.contoso.com

La commande ping doit aboutir à quatre réponses réussies à partir de l’adresse IP 192.168.0.4.

Ensuite, joignez SP1 au domaine Active Directory CORP avec les commandes suivantes dans l’invite Windows PowerShell.

	Add-Computer -DomainName corp.contoso.com
	Restart-Computer

Utilisez le compte CORP\\User1 lorsque vous êtes invité à fournir vos informations d’identification du compte de domaine pour la commande **Add-Computer**.

Après le redémarrage, utilisez le portail Azure pour vous connecter à SP1 avec le compte et le mot de passe CORP\\User1.

Ensuite, configurez le SP1 pour la nouvelle batterie à un seul serveur SharePoint 2016 et un site d’équipe par défaut.

1. Dans l’écran d’accueil, tapez **SharePoint**, puis cliquez sur **Assistant Configuration des produits SharePoint 2016**.
2. Sur la page d’accueil des produits SharePoint, cliquez sur **Suivant**.
3. Une boîte de dialogue **Assistant Configuration des produits SharePoint** s’affiche et indique que les services (tels qu’IIS) seront redémarrés ou réinitialisés. Cliquez sur **Oui**.
4. Sur la page Se connecter à une batterie de serveurs, sélectionnez sur **Créer une batterie de serveurs**, puis cliquez sur **Suivant**.
5. Sur la page Spécifier les paramètres de la base de données de configuration :
	- Dans **Serveur de base de données**, tapez **SQL1**.
	- Dans **Nom d’utilisateur**, tapez **CORP\\SPFarmAdmin**.
	- Dans **Mot de passe**, tapez le mot de passe associé au compte SPFarmAdmin.
6. Cliquez sur **Next**.
7. Dans la page Spécifier les paramètres de sécurité de la batterie de serveurs, tapez **P@ssphrase** deux fois, puis cliquez sur **Suivant**.
8. 	Dans la page Specify Server Role (Spécifier le rôle du serveur), sous **Single-Server Farm** (Batterie à un seul serveur), cliquez sur **Single-Server Farm** (Batterie à un seul serveur), puis cliquez sur **Suivant**.
9. Sur la page Configurer l’application Web de l’Administration centrale de SharePoint, cliquez sur **Suivant**.
10. La page Fin de l’Assistant Configuration des produits SharePoint s’affiche. Cliquez sur **Next**.
11. La page Configuration des produits SharePoint s’affiche. Attendez la fin du processus de configuration.
12. Sur la page Configuration réussie, cliquez sur **Terminer.** Le nouveau site Web d’administration démarre.
13. Sur la page Contribuer à l’amélioration de SharePoint, cliquez pour indiquer que vous souhaitez participer au Programme d’amélioration de l’expérience utilisateur, puis cliquez sur **OK**.
14. Dans la page d’accueil, cliquez sur **Démarrer l’Assistant**.
15. Dans la page Service Applications and Services (Services et applications de service), sous **Compte de service**, cliquez sur **Utiliser un compte géré existant**, puis cliquez sur **Suivant**. L’affichage de la page suivante peut prendre quelques minutes.
16. Dans la page Créer une collection de sites, tapez **Contoso** dans **Titre**, puis cliquez sur **OK**.
17. Dans la page Cette page est la dernière de l’Assistant Configuration de batterie de serveurs, cliquez sur **Terminer**. La page web de l’Administration centrale de SharePoint s’affiche.
18. Ouvrez une session sur l'ordinateur CLIENT1 avec les informations d'identification du compte CORP\\User1, puis démarrez Internet Explorer.
19.	Dans la barre d’adresses, tapez **http://sp1/**, puis appuyez sur Entrée. Vous devriez voir le site d'équipe SharePoint de la société Contoso Corporation. L'affichage du site peut prendre un certain temps.

Ceci est votre configuration actuelle.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-sp/virtual-machines-windows-ps-hybrid-cloud-test-env-sp-ph3.png)
 
Votre batterie à un seul serveur SharePoint 2016 intranet dans un cloud hybride est maintenant prête pour les tests.


## Étapes suivantes

- [Configurer](https://technet.microsoft.com/library/ee836142.aspx) la batterie de serveurs SharePoint 2013.

<!---HONumber=AcomDC_0720_2016-->