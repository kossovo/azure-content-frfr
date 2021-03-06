<properties
	pageTitle="Dans quels cas utiliser NoSQL et SQL | Microsoft Azure"
	description="Comparez les avantages liés à l’utilisation de solutions NoSQL non relationnelles par rapport aux solutions SQL. Déterminez si votre scénario se prête plus à l’un des services de Microsoft Azure NoSQL ou à SQL Server."
	keywords="noSQL comparé à sql, quand utiliser NoSQL, sql comparé à nosql"
	services="documentdb"
	documentationCenter=""
	authors="mimig1"
	manager="jhubbard"
	editor=""/>

<tags
	ms.service="documentdb"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="dotnet"
	ms.topic="article" 
	ms.date="06/24/2016"
	ms.author="mimig"/>

# NoSQL et SQL

SQL Server et les bases de données relationnelles (SGBDR) ont pendant plus de 20 ans été les bases de données incontournables. Or, l’évolution des besoins liés au traitement de plus grands volumes et de plus grandes variétés de données à un rythme soutenu a modifié la nature des besoins en stockage de données pour les développeurs d’applications. Pour répondre à cette situation, les bases de données NoSQL, qui permettent de stocker des données non structurées et hétérogènes à l’échelle appropriée, ont gagné en popularité.

NoSQL est une catégorie de base de données très différente de celle des bases de données SQL. Le terme NoSQL est souvent utilisé pour désigner des systèmes de gestion de données « différents de SQL » ou une approche de gestion des données qui n’intègre « pas seulement SQL ». La catégorie NoSQL englobe un certain nombre de technologies, notamment des bases de données de documents, des magasins de clés/valeurs, des magasins de familles de colonnes et des bases de données de graphiques, qui sont les plus courantes parmi les applications de jeux, de réseaux sociaux et IoT.

![Tableau comparatif de NoSQL et SQL présentant des scénarios et des modèles de données courants](./media/documentdb-nosql-vs-sql/nosql-vs-sql-overview.png)

L’objectif de cet article est de vous permettre de découvrir les différences entre NoSQL et SQL et de vous présenter les offres NoSQL et SQL de Microsoft.

## Dans quels cas utiliser NoSQL ?

Imaginons que vous créez un nouveau site de réseau social. Les utilisateurs peuvent créer des publications et y ajouter des images, des vidéos et de la musique. Les autres utilisateurs peuvent commenter les publications et les noter en leur attribuant des points (« J’aime »). La page d’accueil présente un flux de messages que les utilisateurs peuvent partager et sur lesquels ils peuvent interagir.

Comment stocker ces données ? Si vous connaissez SQL, vous commencerez certainement par un schéma de ce type :

![Tableau comparatif de NoSQL et SQL illustrant le modèle de données relationnelles d’un site de réseau social](./media/documentdb-nosql-vs-sql/nosql-vs-sql-social.png)

Jusque-là, tout va bien. Mais réfléchissez maintenant à la structure d’une publication et à la façon de l’afficher. Si vous voulez afficher la publication ainsi que les images, les sons, les vidéos, les commentaires, les points et les informations utilisateur associés d’un site web ou d’une application, vous devrez exécuter une requête avec huit jointures de table seulement pour récupérer le contenu. À présent, imaginez que vous avez un flux de publications à charger et à afficher à l’écran dynamiquement. Vous pouvez facilement pronostiquer qu’il faudra plusieurs milliers de requêtes et beaucoup de jointures pour effectuer cette tâche.

Vous pourriez très bien avoir recours à une solution relationnelle comme SQL Server pour stocker les données, mais il existe une autre option, une option NoSQL qui simplifie l’approche. En transformant la publication en document JSON, tel que celui illustré ci-dessous, et en le stockant dans DocumentDB, service de base de données de documents NoSQL Azure, vous pouvez accroître les performances et récupérer la publication entière avec une seule requête et aucune jointure. Le résultat sera plus simple, plus direct et plus performant.

    {
        "id":"ew12-res2-234e-544f",
        "title":"post title",
        "date":"2016-01-01",
        "body":"this is an awesome post stored on NoSQL",
        "createdBy":User,
        "images":["http://myfirstimage.png","http://mysecondimage.png"],
        "videos":[
            {"url":"http://myfirstvideo.mp4", "title":"The first video"},
            {"url":"http://mysecondvideo.mp4", "title":"The second video"}
        ],
        "audios":[
            {"url":"http://myfirstaudio.mp3", "title":"The first audio"},
            {"url":"http://mysecondaudio.mp3", "title":"The second audio"}
        ]
    }

Ces données peuvent aussi être partitionnées par ID de publication pour permettre une montée en charge naturelle des données et tirer parti des caractéristiques de mise à l’échelle NoSQL. De même, les systèmes NoSQL permettent aux développeurs de fluidifier la cohérence et de proposer des applications extrêmement disponibles. Enfin, cette solution dispense les développeurs de définir, gérer et tenir à jour le schéma dans la couche Données, ce qui autorise une itération rapide.

Ensuite, vous pouvez compléter cette solution avec d’autres services Azure :

- [Azure Search](https://azure.microsoft.com/services/search/) peut être utilisé via l’application web pour permettre aux utilisateurs de rechercher des publications.
- [Azure App Services](https://azure.microsoft.com/services/app-service/) peut être utilisé pour héberger des applications et des processus d’arrière-plan.
- [Azure Blob Storage](https://azure.microsoft.com/services/storage/) peut être utilisé pour stocker des profils utilisateur complets, notamment des images.
- [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) peut être utilisé pour stocker des quantités de données énormes, telles que des informations de connexion et des données destinées à l’analyse d’utilisation.
- [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) peut être utilisé pour créer des informations et des données décisionnelles dans le but d’affiner le processus et de contribuer à proposer un contenu adapté aux différents utilisateurs.

Ce site de réseau social est un scénario parmi tant d’autres où une base de données NoSQL est le modèle de données adapté à la tâche. Si vous voulez en savoir plus sur ce scénario et sur la modélisation de vos données pour DocumentDB dans des applications de réseaux sociaux, consultez [Going social with DocumentDB](documentdb-social-media-apps.md) (Exploiter les réseaux sociaux avec DocumentDB).

## Comparaison de NoSQL et SQL

Le tableau suivant compare les principales caractéristiques de NoSQL et SQL.

![Tableau comparatif de NoSQL et SQL expliquant les critères de choix de NoSQL et de SQL. Comparaison de SQL et NoSQL](./media/documentdb-nosql-vs-sql/nosql-vs-sql-comparison.png)

Si une base de données NoSQL correspond mieux à vos critères, passez à la section suivante pour découvrir les services NoSQL disponibles dans Azure. Si au contraire une base de données SQL correspond mieux à vos besoins, passez à la section [Offres SQL de Microsoft](#what-are-the-microsoft-sql-offerings)

## Offres Microsoft Azure NoSQL

Azure propose quatre services NoSQL entièrement gérés :

- [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/)
- [Azure Table Storage](https://azure.microsoft.com/services/storage/)
- [Azure HBase dans le cadre de HDInsight](https://azure.microsoft.com/services/hdinsight/)
- [Cache Redis Azure](https://azure.microsoft.com/services/cache/)

Le tableau comparatif décrit les principaux atouts de chaque service. Quel est celui qui correspond le mieux aux besoins de votre application ?

![Tableau comparatif de NoSQL et SQL indiquant les critères de choix des offres NoSQL de Microsoft Azure, notamment DocumentDB, Table Storage, HBase dans le cadre de HDInsight et le Cache Redis](./media/documentdb-nosql-vs-sql/nosql-vs-sql-documentdb-storage-hbase-hdinsight-redis-cache.png)

Si un ou plusieurs de ces services peut répondre aux besoins de votre application, découvrez-les plus en détail en consultant les ressources suivantes :

- [Parcours d’apprentissage de DocumentDB](https://azure.microsoft.com/documentation/learning-paths/documentdb/) et [Cas d’utilisation de DocumentDB](documentdb-use-cases.md)
- [Prise en main du stockage de tables Azure](../storage/storage-dotnet-how-to-use-tables.md)
- [Présentation de HBase dans HDInsight](../hdinsight/hdinsight-hbase-overview.md)
- [Parcours d’apprentissage du Cache Redis](https://azure.microsoft.com/documentation/learning-paths/redis-cache/)

Accédez ensuite à la section [Étapes suivantes](#next-steps) pour plus d’informations sur les versions d’évaluation gratuites.

## Offres SQL de Microsoft

Microsoft propose cinq offres SQL :

- [Azure SQL Database](https://azure.microsoft.com/services/sql-database/)
- [SQL Server sur les machines virtuelles Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/)
- [SQL Server](https://www.microsoft.com/server-cloud/products/sql-server-2016/)
- [Azure SQL Data Warehouse (version préliminaire)](https://azure.microsoft.com/services/sql-data-warehouse/)
- [Système de plateforme d’analyse (équipement local)](https://www.microsoft.com/fr-FR/server-cloud/products/analytics-platform-system/)

Si vous êtes intéressé par l’utilisation de SQL Server sur une machine virtuelle ou une base de données SQL, consultez [Choisir une option de SQL Server cloud : Base de données SQL Azure (PaaS) ou SQL Server sur des machines virtuelles Azure (IaaS)](../sql-database/data-management-azure-sql-database-and-sql-server-iaas.md) pour découvrir plus en détail ce qui différencie les deux options.

Si SQL vous semble la meilleure option, accédez à [SQL Server](https://www.microsoft.com/server-cloud/products/) pour en savoir plus sur les offres de produits et de services Microsoft SQL.

Accédez ensuite à la section [Étapes suivantes](#next-steps) pour obtenir des liens vers les versions d’évaluation gratuites.

## Étapes suivantes

Pour mieux découvrir nos produits SQL et NoSQL, nous vous invitons à les essayer gratuitement.

- Pour tous les services Azure, vous pouvez vous abonner pour une [durée d’évaluation gratuite d’un mois](https://azure.microsoft.com/pricing/free-trial/) et bénéficier d’un crédit de 200 USD pour l’achat de l’un des services Azure.
    - [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/)
    - [Azure HBase dans le cadre de HDInsight](https://azure.microsoft.com/services/hdinsight/)
    - [Cache Redis Azure](https://azure.microsoft.com/services/cache/)
    - [Azure SQL Data Warehouse (version préliminaire)](https://azure.microsoft.com/services/sql-data-warehouse/)
    - [Azure SQL Database](https://azure.microsoft.com/services/sql-database/)
    - [Azure Table Storage](https://azure.microsoft.com/services/storage/)

- Vous pouvez exécuter une [version d’évaluation de SQL Server 2016 sur une machine virtuelle](https://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2016ctp33evaluationwindowsserver2012r2/) ou télécharger une [version d’évaluation de SQL Server](https://www.microsoft.com/fr-FR/evalcenter/evaluate-sql-server-2016).
    - [SQL Server](https://www.microsoft.com/server-cloud/products/sql-server-2016/)
    - [SQL Server sur les machines virtuelles Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/)

<!---HONumber=AcomDC_0629_2016-->