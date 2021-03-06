<properties
	pageTitle="Version préliminaire des services de domaine Azure Active Directory : administrer DNS sur des domaines gérés | Microsoft Azure"
	description="Gérer DNS sur les domaines gérés à l’aide des services de domaine Azure Active Directory"
	services="active-directory-ds"
	documentationCenter=""
	authors="mahesh-unnikrishnan"
	manager="stevenpo"
	editor="curtand"/>

<tags
	ms.service="active-directory-ds"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/06/2016"
	ms.author="maheshu"/>

# Gérer le serveur DNS sur un domaine géré par les services de domaine Azure Active Directory
Les services de domaine Azure Active Directory incluent un serveur DNS (Domain Name Resolution) qui gère la résolution DNS du domaine géré. Parfois, il peut être nécessaire de configurer la fonction DNS sur le domaine géré, afin de créer des enregistrements DNS pour les machines qui ne sont pas jointes au domaine, les adresses IP virtuelles des équilibreurs de charge ou les redirecteurs DNS externes. Pour cette raison, les utilisateurs qui appartiennent au groupe « AAD DC Administrators » bénéficient de privilèges d’administration DNS sur le domaine géré.


## Avant de commencer
Pour exécuter les tâches détaillées dans cet article, vous avez besoin des éléments suivants :

1. Un **abonnement** Azure valide.

2. Un **répertoire Azure AD**, synchronisé avec un répertoire local ou un répertoire cloud uniquement.

3. Les **services de domaine Azure AD**, qui doivent être activés pour le répertoire Azure AD. Si ce n’est déjà fait, suivez l’ensemble des tâches décrites dans le [Guide de mise en route](./active-directory-ds-getting-started.md).

4. Une **machine virtuelle jointe au domaine**, qui vous permet d’administrer le domaine géré par les services de domaine Azure AD. Si vous ne disposez pas de cette machine, suivez toutes les tâches décrites dans l’article intitulé [Joindre une machine virtuelle Windows Server à un domaine géré](./active-directory-ds-admin-guide-join-windows-vm.md).

5. Vous devez vous procurer les informations d’identification d’un **compte d’utilisateur appartenant au groupe « AAD DC Administrators »** dans votre répertoire, afin d’administrer le serveur DNS de votre domaine géré.

<br>

## Tâche 1 : configurer une machine virtuelle jointe au domaine afin d’administrer DNS à distance pour le domaine géré
Vous pouvez administrer à distance les domaines gérés par les services de domaine Azure Active Directory (AD) par l’intermédiaire des outils d’administration familiers d’Active Directory, par exemple le Centre d’administration Active Directory (ADAC, Active Directory Administrative Center) ou AD PowerShell. De même, le serveur DNS du domaine géré peut être administré à distance via les outils d’administration du serveur DNS.

Les administrateurs du répertoire Azure AD ne disposent pas des privilèges permettant la connexion aux contrôleurs de domaine sur le domaine géré, via le Bureau à distance. Par conséquent, les membres du groupe « AAD DC Administrators » peuvent administrer DNS à distance pour les domaines gérés, à l’aide des outils de serveur DNS d’un ordinateur client/Windows Server joint au domaine géré. Les outils du serveur DNS peuvent être installés en tant que composants des Outils d’administration de serveur distant (fonction en option) sur les machines clientes et Windows Server jointes au domaine géré.

La première tâche consiste à configurer une machine virtuelle Windows Server jointe au domaine géré. Pour savoir comment procéder, vous pouvez consulter l’article [Joindre une machine virtuelle Windows Server à un domaine géré par les services de domaine Azure AD](active-directory-ds-admin-guide-join-windows-vm.md).


## Tâche 2 : installer les outils de serveur DNS sur la machine virtuelle
Procédez comme suit pour installer les outils d’administration DNS sur la machine virtuelle jointe au domaine. Pour en savoir plus sur [l’installation et l’utilisation des Outils d’administration de serveur distant](https://technet.microsoft.com/library/hh831501.aspx), voir TechNet.

1. Accédez au nœud **Virtual Machines** du portail Azure Classic. Sélectionnez la machine virtuelle que vous venez de créer et cliquez sur l’option **Connecter** de la barre de commandes, au bas de la fenêtre.

    ![Se connecter à une machine virtuelle Windows](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. Le portail Classic vous invite à ouvrir ou enregistrer un fichier .rdp, qui est utilisé pour la connexion à la machine virtuelle. Lorsqu’il est téléchargé, cliquez sur le fichier .rdp.

3. À l’invite de connexion, utilisez les informations d’identification d’un utilisateur appartenant au groupe « AAD DC Administrators ». Dans notre cas : bob@domainservicespreview.onmicrosoft.com, par exemple.

4. Dans l’écran d’accueil, ouvrez **Gestionnaire de serveur**. Dans le volet central de la fenêtre Gestionnaire de serveur, cliquez sur l’option **Ajout de rôles et de fonctionnalités**.

    ![Lancer le gestionnaire de serveur sur la machine virtuelle](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)

5. Sur la page **Avant de commencer** de l’**Assistant Ajout de rôles et de fonctionnalités**, cliquez sur **Suivant**.

    ![Page Avant de commencer](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)

6. Sur la page **Type d’installation**, laissez l’option **Installation basée sur un rôle ou une fonctionnalité** sélectionnée et cliquez sur **Suivant**.

	![Page Type d’installation](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)

7. Dans la page **Sélection du serveur**, choisissez la machine virtuelle actuelle dans le pool de serveurs, puis cliquez sur **Suivant**.

	![Page Sélection du serveur](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)

8. Sur la page **Rôles de serveurs**, cliquez sur **Suivant**. Nous allons ignorer cette page, car nous ne sommes pas en train d’installer des rôles sur le serveur.

9. Sur la page **Fonctionnalités**, cliquez sur le nœud **Outils d’administration de serveur distant** pour le développer, puis sur le nœud **Outils d’administration de rôles**, pour le développer. Sélectionnez la fonctionnalité **Outils du serveur DNS** dans la liste Outils d’administration de rôles, comme indiqué ci-dessous.

	![Page Fonctionnalités](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-tools.png)

10. Sur la page **Confirmation**, cliquez sur **Installer** pour installer la fonctionnalité Outils du serveur DNS sur la machine virtuelle. Une fois l’installation de la fonctionnalité terminée, cliquez sur **Fermer** afin de fermer l’Assistant **Ajout de rôles et de fonctionnalités**.

	![Page Confirmation](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-confirmation.png)


## Tâche 3: lancer la console Gestion du service DNS pour administrer le serveur DNS
Maintenant que la fonctionnalité Outils du serveur DNS est installée sur la machine virtuelle jointe au domaine, nous pouvons utiliser les outils DNS pour administrer le serveur DNS sur le domaine géré.

> [AZURE.NOTE] Vous devez être membre du groupe « AAD DC Administrators » pour pouvoir administrer le serveur DNS sur le domaine géré.

1. Dans l’écran d’accueil, cliquez sur **Outils d’administration**. Vous devez voir apparaître la console **DNS** installée sur la machine virtuelle.

	![Outils d’administration - Console DNS](./media/active-directory-domain-services-admin-guide/install-rsat-dns-tools-installed.png)

2. Cliquez sur **DNS** pour lancer la console Gestion du service DNS.

3. Dans la boîte de dialogue **Connexion au serveur DNS**, cliquez sur l’option **L’ordinateur suivant** et saisissez le nom DNS du domaine géré (par exemple : contoso100.com).

    ![Console DNS - Connexion au domaine](./media/active-directory-domain-services-admin-guide/dns-console-connect-to-domain.png)

4. La console DNS se connecte au domaine géré. Une vue similaire à celle-ci doit s’afficher.

    ![Console DNS - Administration du domaine](./media/active-directory-domain-services-admin-guide/dns-console-managed-domain.png)

5. Vous pouvez désormais utiliser la console DNS pour ajouter des entrées DNS correspondant aux ordinateurs dans le réseau virtuel au sein duquel les services de domaine Azure AD sont activés.

> [AZURE.WARNING] Soyez très prudent lorsque vous administrez DNS pour le domaine géré au moyen des outils d’administration DNS. Assurez-vous de ne pas **supprimer ou modifier les enregistrements DNS intégrés qu’utilisent les services de domaine au sein de domaine**. Cela inclut les enregistrements DNS du domaine, les enregistrements de serveur de noms et d’autres enregistrements utilisés pour le lieu du contrôleur de domaine. Si vous modifiez ces enregistrements, les services de domaine s’interrompent sur le réseau virtuel.


Pour en savoir plus sur la gestion du serveur DNS, consultez l’[article relatif à DNS sur Technet](https://technet.microsoft.com/library/cc753579.aspx).


## Contenu connexe

- [Services de domaine Azure AD : guide de mise en route](./active-directory-ds-getting-started.md)

- [Administrer un domaine géré par les services de domaine Azure Active Directory](active-directory-ds-admin-guide-administer-domain.md)

- [Joindre une machine virtuelle Windows Server à un domaine géré par les services de domaine Azure AD](active-directory-ds-admin-guide-join-windows-vm.md)

- [Outils d’administration DNS](https://technet.microsoft.com/library/cc753579.aspx)

<!---HONumber=AcomDC_0706_2016-->