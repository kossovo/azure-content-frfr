<properties 
	pageTitle="Qu’est-ce que Data Factory ? Service d’intégration de données | Microsoft Azure" 
	description="Découvrez Azure Data Factory : un service d’intégration de données cloud qui gère et automatise le déplacement et la transformation des données." 
	keywords="intégration de données, intégration de données cloud, description d’azure data factory"
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="get-started-article" 
	ms.date="07/12/2016" 
	ms.author="spelluru"/>

# Présentation du service Azure Data Factory, un service d’intégration de données dans le cloud

## qu'est-ce qu'Azure Data Factory ? 
Data Factory est un service d’intégration de données dans le cloud qui gère et automatise le déplacement et la transformation des données. À la manière d’une usine de fabrication, qui utilise des machines visant à transformer des matières premières en produits manufacturés, Data Factory orchestre des services existants qui collectent des données brutes et les transforment en informations prêtes à l'emploi.

Data Factory fonctionne pour des sources de données locales et dans le cloud et avec les SaaS pour réceptionner, préparer, transformer, analyser et publier vos données. Utilisez Data Factory pour constituer des services dans des pipelines de flux de données gérés afin de transformer vos données à l'aide de services tels qu’[Azure HDInsight (Hadoop)](http://azure.microsoft.com/documentation/services/hdinsight/) et [Azure Batch](https://azure.microsoft.com/documentation/services/batch/) pour vos besoins en calcul de Big Data, et avec [Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) pour configurer vos solutions d'analyse. Car une simple vue tabulaire pour surveiller vos projets ne suffit plus, utilisez les affichages élaborés de Data Factory et visualisez d’un seul coup d’œil le lignage et les dépendances de vos pipelines de données. Analysez tous vos pipelines de flux de données à partir d'une vue unifiée pour identifier les problèmes et configurer des alertes d'analyse en toute simplicité.

![Diagramme : présentation de Data Factory, un service d’intégration de données](./media/data-factory-introduction/what-is-azure-data-factory.png)

**Figure 1.** Collectez les données de nombreuses sources de données locales, réceptionnez-les et préparez-les, organisez-les et analysez-les en vous appuyant sur toute une série de transformations. Publiez ensuite ces données prêtes à l’emploi en vue de leur consommation.

Vous pouvez utiliser Data Factory à tout moment dès que vous en avez besoin pour collecter des données de différentes formes et tailles, les transformer et les publier pour extraire des informations détaillées. Le tout, avec un calendrier fiable. Data Factory permet de créer des pipelines de flux de données hautement disponibles pour de nombreux scénarios, à travers des secteurs différents, pour leurs besoins en matière de pipeline d’analyse. Les détaillants en ligne peuvent l’utiliser pour générer des [recommandations de produits](data-factory-product-reco-usecase.md) personnalisées basées sur le comportement de navigation du client. Des studios de jeux l'utilisent pour comprendre [l'efficacité de leurs campagnes](data-factory-customer-profiling-usecase.md) marketing. Découvrez directement auprès de nos clients comment et pourquoi ils utilisent Data Factory en consultant les [Études de cas client](data-factory-customer-case-studies.md).

> [AZURE.VIDEO azure-data-factory-overview]

## Concepts clés

Azure Data Factory contient quelques entités clés qui fonctionnent conjointement pour définir les données d’entrée et de sortie, les événements de traitement et le calendrier et les ressources nécessaires pour exécuter le flux de données souhaité.

![Diagramme : Data Factory, un service d’intégration de données cloud - Concepts clés](./media/data-factory-introduction/data-integration-service-key-concepts.png)

**Figure 2 :** Relations entre jeu de données, activité, pipeline et service lié


### Activités
Les activités définissent les actions à effectuer sur les données. Chaque activité accepte ou non des [jeux de données](data-factory-create-datasets.md) en tant qu’entrées et produit au moins un jeu de données en tant que sortie. Une activité est une unité d'orchestration dans Azure Data Factory. Par exemple, vous pouvez utiliser une [activité de copie](data-factory-data-movement-activities.md) pour orchestrer la copie de données d’un jeu de données vers un autre. De même, vous pouvez utiliser une [activité Hive](data-factory-data-transformation-activities.md) qui exécutera une requête Hive sur un cluster Azure HDInsight afin de convertir ou d’analyser vos données. Azure Data Factory fournit un large éventail d'activités de transformation, d’analyse et de déplacement de données.

### Pipelines
Les [pipelines](data-factory-create-pipelines.md) constituent un regroupement logique d’activités. Ils sont utilisés pour regrouper des activités dans une unité. Ces activités exécutent alors conjointement une tâche. Par exemple, une séquence de plusieurs activités de transformation peut être nécessaire pour nettoyer les données d’un fichier journal. Cette séquence peut présenter un calendrier et des dépendances complexes qui doivent être orchestrés et automatisés. Toutes ces activités pourraient être regroupées en un seul pipeline nommé « CleanLogFiles ». « CleanLogFiles » pourrait ensuite être déployé, planifié ou supprimé sous la forme d'une seule unité. Plus besoin de gérer chaque activité de manière indépendante.

### Jeux de données
Les [jeux de données](data-factory-create-datasets.md) englobent des références/liens nommés vers les données que vous souhaitez utiliser en tant qu’entrées ou sorties d’activités. Les jeux de données identifient les structures de données dans différents magasins de données, notamment des tables, des fichiers, des dossiers et des documents.

### Service lié
Les services liés définissent les informations nécessaires à Data Factory pour se connecter à des ressources externes. Data Factory fait appel aux services liés pour deux raisons :

- Pour représenter un magasin de données et notamment un partage de fichiers SQL Server, Oracle DB local ou un compte de stockage Azure Blob. Comme expliqué ci-dessus, les jeux de données représentent les structures que l’on retrouve dans des magasins de données connectés à Data Factory via un service lié.
- Pour représenter une ressource de calcul qui peut héberger l'exécution d'une activité. Par exemple, l'activité « HDInsightHive » s'exécute sur un cluster HDInsight Hadoop.

Avec ces quatre concepts simples de jeux de données, activités, pipelines et services liés, plus instant à perdre pour vous y mettre ! Vous pouvez [créer votre premier pipeline](data-factory-build-your-first-pipeline.md) de A à Z ou déployer un exemple prêt à l’emploi en suivant les instructions présentées dans l’article [Azure Data Factory - Exemples](data-factory-samples.md).

## Régions prises en charge
Vous pouvez créer actuellement des fabriques de données aux **États-Unis de l’Ouest**, aux **États-Unis de l’Est** et en **Europe du Nord**. Une fabrique de données peut toutefois accéder à des magasins de données et à des services de calcul situés dans d’autres régions Azure pour déplacer des données entre des magasins de données ou pour traiter des données à l’aide des services de calcul.

Azure Data Factory ne permet pas en soi de stocker des données. Il vous permet de créer des flux pilotés par les données afin d’orchestrer le déplacement de données entre les [magasins de données pris en charge ](data-factory-data-movement-activities.md#supported-data-stores) ainsi que le traitement des données à l’aide [des services de calcul](data-factory-compute-linked-services.md) situés dans d’autres régions ou dans un environnement local. Il vous permet également de [surveiller et gérer des workflows](data-factory-monitor-manage-pipelines.md) au moyen de programmes et à l’aide des mécanismes de l’interface utilisateur.

Notez que même si Azure Data Factory est disponible uniquement aux **États-Unis de l’Ouest**, aux **États-Unis de l’Est** ainsi qu’en **Europe du Nord**, le service de déplacement des données intégré à Data Factory est [mondialement](data-factory-data-movement-activities.md#global) disponible dans plusieurs régions. Lorsqu’un magasin de données se trouve derrière un pare-feu, le déplacement des données est assuré au moyen d’une [passerelle de gestion des données](data-factory-move-data-between-onprem-and-cloud.md) installée dans votre environnement local.

Supposons que vos environnements de calcul (cluster Azure HDInsight et Azure Machine Learning, par exemple) s’exécutent hors de la région Europe de l’ouest. Vous pouvez dans ce cas créer et exploiter une instance Azure Data Factory en Europe du Nord et l’utiliser pour planifier des tâches sur vos environnements de calcul en Europe de l’ouest. Quelques millisecondes suffisent au service Data Factory pour déclencher la tâche dans votre environnement de calcul, mais l’heure d’exécution du travail dans votre environnement informatique ne change pas.

Nous avons l’intention d’étendre la prise en charge d’Azure Data Factory à toutes les zones géographiques.
  
## Étapes suivantes
Suivez les instructions pas à pas des didacticiels suivants pour découvrir comment créer des fabriques de données avec des pipelines de données.

Didacticiel | Description
-------- | -----------
[Créer un pipeline de données qui traite les données à l’aide du cluster Hadoop](data-factory-build-your-first-pipeline.md) | Dans ce didacticiel, vous allez créer votre première fabrique de données Azure avec un pipeline de données qui **traite les données** en exécutant le script Hive sur un cluster Azure HDInsight (Hadoop). |
[Build a data pipeline to move data between two cloud data stores (Créer un pipeline de données pour déplacer des données entre deux banques de données cloud)](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) | Dans ce didacticiel, vous allez créer une fabrique de données avec un pipeline qui **déplace des données** de Blob Storage vers la base de données SQL.
[Build a data pipeline to move data between an on-premises data store and a cloud data store using Data Management Gateway (Créer un pipeline de données pour déplacer des données entre une banque de données locale et une banque de données cloud à l’aide de la passerelle de gestion des données)](data-factory-move-data-between-onprem-and-cloud.md) | Dans ce didacticiel, vous allez créer une fabrique de données avec un pipeline qui **déplace des données** d’une base de données SQL Server **locale** vers un objet blob Azure. Dans le cadre de la procédure pas à pas, vous allez installer et configurer la passerelle de gestion des données sur votre ordinateur. 

<!---HONumber=AcomDC_0713_2016-->