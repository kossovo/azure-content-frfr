<properties
   	pageTitle="Installer des applications Hadoop sur HDInsight | Microsoft Azure"
   	description="Découvrez comment installer des applications Hadoop sur des applications HDInsight."
   	services="hdinsight"
   	documentationCenter=""
   	authors="mumian"
   	manager="paulettm"
   	editor="cgronlun"
	tags="azure-portal"/>

<tags
   	ms.service="hdinsight"
   	ms.devlang="na"
   	ms.topic="hero-article"
   	ms.tgt_pltfrm="na"
   	ms.workload="big-data"
   	ms.date="06/29/2016"
   	ms.author="jgao"/>

# Installer des applications HDInsight

Une application HDInsight est une application que les utilisateurs peuvent installer sur un cluster HDInsight sous Linux. Ces applications peuvent être développées par Microsoft, par des éditeurs de logiciels indépendants (ISV) ou par vous-même. Dans cet article, vous allez apprendre à installer une application publiée. Pour installer votre propre application, consultez la page [Installer des applications HDInsight personnalisées](hdinsight-apps-install-custom-applications.md).

Actuellement, il existe une application publiée :

- **Datameer** : [Datameer](http://www.datameer.com/documentation/display/DAS50/Home?ls=Partners&lsd=Microsoft&c=Partners&cd=Microsoft) offre aux analystes une manière interactive de détecter, analyser et visualiser les résultats sur Big Data. Extrayez facilement des sources de données supplémentaires pour découvrir de nouvelles relations et obtenir les réponses dont vous avez besoin rapidement.

>[AZURE.NOTE] Datameer est actuellement pris en charge uniquement dans les clusters Azure HDInsight version 3.2.

Les instructions fournies dans cet article utilisent le Portail Azure. Vous pouvez également exporter le modèle Azure Resource Manager à partir du portail ou obtenir une copie de ce modèle auprès des fournisseurs et utiliser Azure PowerShell et l’interface de ligne de commande Azure pour déployer le modèle. Consultez [Création de clusters Hadoop basés sur Linux dans HDInsight à l’aide de modèles Resource Manager | Microsoft Azure](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

## Composants requis

Pour installer des applications HDInsight sur un cluster HDInsight existant, vous devez disposer d’un cluster HDInsight. Pour en créer un, consultez [Créer des clusters](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster). Vous pouvez également installer des applications HDInsight lorsque vous créez un cluster HDInsight.

## Installer des applications dans des clusters existants

La procédure suivante montre comment installer des applications HDInsight dans un cluster HDInsight existant.

**Pour installer une application HDInsight**

1. Connectez-vous au [portail Azure](https://portal.azure.com).
2. Cliquez sur **Clusters HDInsight** dans le menu de gauche. Si vous ne le voyez pas, cliquez sur **Parcourir**, puis sur **Clusters HDInsight**.
3. Cliquez sur un cluster HDInsight. Si vous ne disposez pas d’un tel cluster, vous devez d’abord en créer un. Consultez la page [Créer des clusters](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).
4. Dans le panneau **Paramètres**, cliquez sur **Applications** sous la catégorie **Général**. Le panneau **Applications installées** répertorie toutes les applications installées.

    ![applications HDInsight - menu du portail](./media/hdinsight-apps-install-applications/hdinsight-apps-portal-menu.png)

5. Cliquez sur **Ajouter** dans le menu du panneau.

    ![applications hdinsight - applications installées](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps.png)

	Une liste des applications HDInsight existantes doit s’afficher.

	![applications hdinsight - applications disponibles](./media/hdinsight-apps-install-applications/hdinsight-apps-list.png)

6. Cliquez sur une des applications, acceptez les conditions juridiques, puis cliquez sur **Sélectionner**.

Vous pouvez vérifier l’état de l’installation de l’application dans les notifications du portail (cliquez sur l’icône en forme de cloche en haut du portail). Une fois l’application installée, celle-ci s’affiche dans le panneau Applications installées.

## Installer des applications lors de la création du cluster

Vous pouvez également installer des applications HDInsight lorsque vous créez un cluster. Au cours du processus, les applications HDInsight sont installées une fois le cluster créé et en cours d’exécution. La procédure suivante montre comment installer des applications HDInsight lors de la création d’un cluster.

**Pour installer une application HDInsight**

1. Connectez-vous au [portail Azure](https://portal.azure.com).
2. Cliquez sur **NOUVEAU**, sur **Analyse des données**, puis sur **HDInsight**.
3. Entrez le **nom du cluster** : ce nom doit être globalement unique.
4. Cliquez sur **Abonnement** pour sélectionner l’abonnement Azure qui sera utilisé pour le cluster.
5. Cliquez sur **Sélectionner un type de cluster**, puis choisissez :

    - **Type de cluster** : si vous ne savez pas lequel choisir, sélectionnez **Hadoop**. Il s’agit du type de cluster le plus répandu.
    - **Système d’exploitation** : sélectionnez **Linux**.
    - **Version** : utilisez la version par défaut si vous ne savez pas laquelle choisir. Pour plus d’informations, consultez [Versions de clusters HDInsight](hdinsight-component-versioning.md).
    - **Niveau de cluster** : Azure HDInsight propose deux catégories d’offres de cloud Big Data : Niveau Standard et Niveau Premium. Pour plus d’informations, consultez [Niveaux de cluster](hdinsight-hadoop-provision-linux-clusters.md#cluster-tiers).
6. Cliquez sur **Applications**, sur l’une des applications publiées, puis sur **Sélectionner**.
6. Cliquez sur **Informations d’identification**, puis entrez un mot de passe pour l’utilisateur admin. Vous devez également entrer un **Nom d’utilisateur SSH** et un **MOT DE PASSE** ou une **CLÉ PUBLIQUE**, qui seront utilisés pour authentifier l’utilisateur SSH. L'utilisation d'une clé publique est l'approche recommandée. Cliquez sur **Sélectionner** en bas de l’écran pour enregistrer la configuration des informations d’identification.
8. Cliquez sur **Source de données**, sélectionnez un des comptes existants ou créez un nouveau compte existant à utiliser comme compte de stockage par défaut pour le cluster.
9. Cliquez sur **Groupe de ressources** pour sélectionner un groupe de ressources existant, ou sur **Nouveau** pour créer un nouveau groupe de ressources.

10. Dans le panneau **Nouveau cluster HDInsight**, vérifiez que l’option **Épingler au tableau d’accueil** est sélectionnée, puis cliquez sur **Créer**.

## Répertorier les applications HDInsight installées et les propriétés

Le portail affiche une liste des applications HDInsight installées pour un cluster et les propriétés de chaque application installée.

**Pour répertorier une application HDInsight et afficher des propriétés**

1. Connectez-vous au [portail Azure](https://portal.azure.com).
2. Cliquez sur **Clusters HDInsight** dans le menu de gauche. Si vous ne le voyez pas, cliquez sur **Parcourir**, puis sur **Clusters HDInsight**.
3. Cliquez sur un cluster HDInsight.
4. Dans le panneau **Paramètres**, cliquez sur **Applications** sous la catégorie **Général**. Le panneau Applications installées répertorie toutes les applications installées.

	![applications hdinsight - applications installées](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps-with-apps.png)

5. Cliquez sur une des applications installées pour en afficher la propriété. Le panneau des propriétés répertorie les éléments suivants :

    - Nom de l’application : nom de l’application.
    - État : état de l’application.
    - Page web : l’URL de l’application web que vous avez déployée sur le nœud de périmètre s’il existe. Les informations d’identification sont les mêmes que celles de l’utilisateur HTTP configurées pour le cluster.
    - Point de terminaison HTTP : les informations d’identification sont les mêmes que celles de l’utilisateur HTTP configuré pour le cluster.
    - Point de terminaison SSH : vous pouvez utiliser [SSH](hdinsight-hadoop-linux-use-ssh-unix.md) pour la connexion au nœud de périmètre. Les informations d’identification sont les mêmes que celles de l’utilisateur SSH configurées pour le cluster.

6. Pour supprimer une application, cliquez avec le bouton droit sur l’application, puis sur **Supprimer** dans le menu contextuel.

## Connexion au nœud de périmètre

Vous pouvez vous connecter au nœud de périmètre à l’aide de HTTP et SSH. Vous trouverez les informations de point de terminaison sur le [portail](#list-installed-hdinsight-apps-and-properties). Pour plus d’informations sur l’utilisation de SSH avec HDInsight, consultez la rubrique [Utilisation de SSH avec Hadoop dans HDInsight sur Linux à partir de Linux, Unix ou OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

Les informations d’identification du point de terminaison HTTP sont les informations d’identification de l’utilisateur HTTP configurées pour le cluster HDInsight ; les informations d’identification du point de terminaison SSH sont les informations d’identification SSH configurées pour le cluster HDInsight.

## Résolution des problèmes

Consultez [Dépanner l’installation](hdinsight-apps-install-custom-applications.md#troubleshoot-the-installation).

## Étapes suivantes

- [Installer des applications HDInsight personnalisées](hdinsight-apps-install-custom-applications.md) : découvrez comment déployer des applications HDInsight inédites vers HDInsight.
- [Publish HDInsight applications](hdinsight-apps-publish-applications.md) (Publier des applications HDInsight) : découvrez comment publier vos applications HDInsight personnalisées sur Azure Marketplace.
- [MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx) (MSDN : installer une application HDInsight) : découvrez comment définir des applications HDInsight.
- [Personnalisation de clusters HDInsight basés sur Linux à l’aide d’une action de script](hdinsight-hadoop-customize-cluster-linux.md) : apprenez à utiliser l’action de script pour installer des applications supplémentaires.
- [Créer des clusters Hadoop sous Linux dans HDInsight à l’aide de modèles Resource Manager](hdinsight-hadoop-create-linux-clusters-arm-templates.md) : apprenez à appeler des modèles Resource Manager pour la création de clusters HDInsight.

<!---HONumber=AcomDC_0706_2016-->