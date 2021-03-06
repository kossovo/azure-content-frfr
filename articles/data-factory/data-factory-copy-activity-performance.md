<properties
	pageTitle="Guide sur les performances et le réglage de l’activité de copie | Microsoft Azure"
	description="En savoir plus sur les facteurs clés ayant une incidence sur les performances du déplacement de données dans Azure Data Factory via l’activité de copie."
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
	ms.topic="article"
	ms.date="06/03/2016"
	ms.author="spelluru"/>


# Guide sur les performances et le réglage de l’activité de copie
Cet article décrit les facteurs clés qui ont une incidence sur les performances de déplacement de données dans Azure Data Factory (activité de copie). Il répertorie également les performances observées lors des tests internes, et présente les différentes manières d’optimiser les performances de l’activité de copie.

L’activité de copie vous permet de bénéficier d’un débit de déplacement de données élevé, comme l’illustrent les exemples suivants :

- Réception de 1 To de données dans Azure Blob Storage à partir d’un système de fichiers local et d’Azure Blob Storage en moins de 3 heures (c.-a-d., à 100 Mbits/s)
- Réception de 1 To de données dans Azure Data Lake Store à partir d’un système de fichiers local et d’Azure Blob Storage en moins de 3 heures (c.-a-d., à 100 Mbits/s)
- Réception de 1 To de données dans Azure SQL Data Warehouse à partir d’Azure Blob Storage en moins de 3 heures (c.-a-d., à 100 Mbits/s)

Consultez les sections suivantes pour en savoir plus sur les performances de l’activité de copie et sur les astuces de réglage permettant de les améliorer.

> [AZURE.NOTE] Si vous ne disposez pas d’une connaissance générale de l’activité de copie, consultez [Activités de déplacement des données](data-factory-data-movement-activities.md) avant de lire cet article.

## Procédure de réglage des performances
Les étapes classiques que nous vous suggérons pour régler les performances de votre solution Azure Data Factory avec une activité de copie sont répertoriées ci-dessous.

1.	**Établir une ligne de base.** Pendant la phase de développement, testez votre pipeline avec l’activité de copie sur un échantillon de données représentatif. Pour limiter la quantité de données que vous utilisez, vous pouvez tirer parti du [modèle de découpage](data-factory-scheduling-and-execution.md#time-series-datasets-and-data-slices) d’Azure Data Factory.

	Récupérez le temps d’exécution et les caractéristiques de performances à l’aide de l’**Application de surveillance et gestion** : cliquez sur la vignette **Surveiller et gérer** dans la page d’accueil de votre fabrique de données, sélectionnez le **jeu de données de sortie** dans l’arborescence, puis sélectionnez l’activité de copie exécutée dans la liste **Fenêtres d’activité**. La durée de l’activité de copie doit s’afficher dans la liste **Fenêtres d’activité** et la taille des données copiées et le débit apparaissent dans la fenêtre **Explorateur de fenêtres d’activité** à droite. Consultez [Surveiller et gérer les pipelines Azure Data Factory à l’aide de l’application de surveillance et gestion](data-factory-monitor-manage-app.md) pour en savoir plus sur l’application.
	
	![Détails de l’exécution d’activité](./media/data-factory-copy-activity-performance/mmapp-activity-run-details.png)

	Vous pouvez comparer les performances et les configurations de votre scénario aux [Performances de référence](#performance-reference) de l’activité de copie publiées sur les observations internes.
2. **Diagnostic et optimisation des performances** Si les performances que vous observez sont inférieures à vos attentes, vous devez identifier les goulots d’étranglement et opérer des optimisations pour supprimer ou réduire l’impact des goulots d’étranglement. Une description complète du diagnostic des performances dépasserait la portée de cet article, mais vous trouverez quelques informations générales ci-après.
	- [Source](#considerations-on-source)
	- [Section sink](#considerations-on-sink)
	- [Sérialisation/désérialisation.](#considerations-on-serializationdeserialization)
	- [Compression](#considerations-on-compression)
	- [Mappage de colonnes](#considerations-on-column-mapping)
	- [Passerelle de gestion de données](#considerations-on-data-management-gateway)
	- [Autres considérations](#other-considerations)
	- [Copie en parallèle](#parallel-copy)
	- [Unités de déplacement de données cloud](#cloud-data-movement-units)

3. **Étendez la configuration à l’ensemble de vos données** Lorsque vous êtes satisfait des résultats et des performances de l’exécution, vous pouvez étendre la définition du jeu de données et de la période active du pipeline pour couvrir l’ensemble des données.

## Performances de référence
> [AZURE.IMPORTANT] **Exclusion :** les données ci-dessous ont été publiées dans le seul but de fournir des conseils et de proposer une planification de haut niveau. La bande passante, le matériel, la configuration et autres aspects sont supposés figurer parmi les meilleurs de leur catégorie. Utilisez ces données uniquement en guise de référence. Le débit de déplacement des données que vous observez dépend d’une série de variables. Reportez-vous aux sections ultérieurement pour découvrir comment vous pouvez éventuellement régler et obtenir de meilleures performances pour vos besoins en matière de déplacement des données. Ces données seront mises à jour au fur et à mesure des ajouts de fonctionnalités et améliorations des performances.

![Matrice des performances](./media/data-factory-copy-activity-performance/CopyPerfRef.png)

Points à noter :

- Le débit est calculé à l’aide de la formule suivante : [taille des données lues à partir de la source]/[durée d’exécution de l’activité de copie]
- Le jeu de données [TPC-H](http://www.tpc.org/tpch/) a été utilisé pour calculer les nombres ci-dessus.
- En ce qui concerne les magasins de données Microsoft Azure, la source et le récepteur sont dans la même région Azure.
- La propriété **cloudDataMovementUnits** a la valeur 1, **parallelCopies** n’est pas défini.
- Dans le cas du déplacement de données hybrides (de locales vers le cloud, ou de cloud vers locales), la passerelle de gestion des données (instance unique) était hébergée sur un ordinateur différent de celui du magasin de données local, à l’aide de la configuration suivante. Notez qu’avec une seule exécution d’activité sur la passerelle, l’opération de copie ne consommait qu’une petite partie de la bande passante réseau et des ressources de processeur et de mémoire de cet ordinateur.
	<table>
	<tr>
		<td>UC</td>
		<td>32 cœurs 2,20 GHz Intel Xeon® E5-2660 v2</td>
	</tr>
	<tr>
		<td>Mémoire</td>
		<td>128 GO</td>
	</tr>
	<tr>
		<td>Réseau</td>
		<td>Interface Internet : 10 Gbits/s&#160;; interface intranet : 40 Gbits/s</td>
	</tr>
	</table>

## Copie en parallèle
L’un des moyens d’améliorer le débit d’une opération de copie et de réduire le temps de déplacement des données est de lire les données à partir de la source et/ou d’écrire les données dans la destination **en parallèle dans une exécution d’activité de copie**.
 
Notez que ce paramètre est différent de la propriété **concurrency** au niveau de la définition d’activité. La propriété concurrency détermine le nombre d’**exécutions d’activité de copie simultanées** qui s’exécutent ensemble au moment de l’exécution pour traiter les données de différentes fenêtres d’activité (de 1h00 à 2h00, de 2h00 à 3h00, de 3h00 à 4h00, etc.). Elle est très utile quand il s’agit de procéder à un chargement historique. En revanche, la fonctionnalité de copie en parallèle dont il est question ici s’applique à une **exécution d’activité unique**.

Examinons un **exemple de scénario** : prenons l’exemple suivant dans lequel plusieurs tranches du passé doivent être traitées. Le service Data Factory exécute une instance de l’activité de copie (exécution d’activité) pour chaque tranche.

- tranche de données de la première fenêtre d’activité (de 1h00 à 2h00) == > exécution d’activité 1
- tranche de données de la deuxième fenêtre d’activité (de 2h00 à 3h00) == > exécution d’activité 2
- tranche de données de la troisième fenêtre d’activité (de 3h00 à 4h00) == > exécution d’activité 3
- et ainsi de suite.

Dans cet exemple, le fait que le paramètre **concurrency** ait la valeur **2** permet à l’**exécution d’activité 1 ** et à l’**exécution d’activité 2** de copier les données des deux fenêtres d’activité de façon **simultanée** et ainsi d’améliorer les performances de déplacement des données. Cependant, si plusieurs fichiers sont associés à l’exécution d’activité 1, un seul fichier est copié de la source vers la destination à la fois.

### parallelCopies
Vous pouvez utiliser la propriété **parallelCopies** pour indiquer le parallélisme que vous voulez appliquer à l’activité de copie. En termes simples, cette propriété représente le nombre maximal de threads dans une activité de copie qui lit dans votre source et/ou écrit dans vos magasins de données récepteurs en parallèle.

Pour chaque exécution d’activité de copie, Azure Data Factory détermine de façon intelligente le nombre de copies en parallèle à utiliser pour copier les données du magasin de données source vers le magasin de données de destination. Le nombre par défaut de copies en parallèle utilisé dépend des types de source et de récepteur :

Source et récepteur |	Nombre de copie en parallèle par défaut déterminé par le service
------------- | -------------------------------------------------
Copie de données entre **magasins basés sur des fichiers** (Azure Blob, Azure Data Lake, système de fichiers local, HDFS local) | Entre **1 et 32** selon la **taille des fichiers** et le **nombre d’unités de déplacement de données cloud** (voir la section suivante pour la définition) utilisés pour la copie de données entre les deux magasins de données cloud (ou) la configuration physique de l’ordinateur de passerelle utilisé pour la copie hybride (copie de données vers/à partir d’un magasin de données local)
Copie de données de **n’importe quel magasin de données source vers une table Azure** | 4
Toutes les autres paires de source et de récepteur | 1

Dans la majorité des cas, le comportement par défaut doit offrir le meilleur débit. Or, vous pouvez remplacer la valeur par défaut en spécifiant la valeur de la propriété **parallelCopies** de façon à contrôler la charge sur les ordinateurs hébergeant vos magasins de données ou optimiser les performances de copie. Cette valeur doit être comprise entre **1 et 32 (inclus)**. Au moment de l’exécution, l’activité de copie choisit une valeur inférieure ou égale à la valeur configurée pour offrir les meilleures performances.

	"activities":[  
	    {
	        "name": "Sample copy activity",
	        "description": "",
	        "type": "Copy",
	        "inputs": [{ "name": "InputDataset" }],
	        "outputs": [{ "name": "OutputDataset" }],
	        "typeProperties": {
	            "source": {
	                "type": "BlobSource",
	            },
	            "sink": {
	                "type": "AzureDataLakeStoreSink"
	            },
	            "parallelCopies": 8
	        }
	    }
	]

Notez les points suivants :

- Pour copier des données entre magasins basés sur des fichiers, le parallélisme intervient au niveau du fichier ; en d’autres termes, il n’y a pas de fragmentation au sein d’un même fichier. Le nombre réel de copies en parallèle utilisées pour l’opération de copie au moment de l’exécution ne peut pas être supérieur au nombre de fichiers dont vous disposez. Si le comportement de copie est mergeFile, le parallélisme n’est pas exploité.
- Quand vous spécifiez la valeur de la propriété parallelCopies, tenez compte de l’augmentation de la charge induite au niveau de vos magasins de données source et récepteur (et de la passerelle) dans le cas d’une copie hybride, en particulier quand plusieurs activités ou exécutions simultanées des mêmes activités sont exécutées sur un même magasin de données. Si vous remarquez que le magasin de données ou la passerelle est submergée par la charge, diminuez la valeur de parallelCopies pour alléger la charge.
- Quand la copie de données s’effectue de magasins non basés sur des fichiers vers des magasins basés sur des fichiers, la propriété parallelCopies est ignorée même si elle est spécifiée et que le parallélisme n’est pas exploité.

> [AZURE.NOTE] Pour tirer parti de la fonctionnalité parallelCopies dans le cas d’une copie hybride, vous devez utiliser la passerelle de gestion des données de version 1.11 ou supérieure.

### Unités de déplacement de données cloud
L’**unité de déplacement de données cloud** est une mesure qui représente la puissance (combinaison de l’allocation de ressources de processeur, de mémoire et de réseau) d’une même unité dans le service Azure Data Factory servant à exécuter une opération de copie de cloud à cloud. Elle n’intervient pas dans le cas d’une copie hybride. Par défaut, le service Azure Data Factory utilise une seule unité de déplacement de données cloud pour mener à bien une même exécution d’activité de copie. Vous pouvez remplacer cette valeur par défaut en spécifiant la valeur de la propriété **cloudDataMovementUnits**. À ce stade, le paramètre cloudDataMovementUnits est **pris en charge uniquement** pour la copie de données **entre deux emplacements de stockage d’objets blob Azure** ou entre un **emplacement de stockage d’objets blob Azure et un magasin Azure Data Lake Store**. De plus, il prend effet quand il y a plusieurs fichiers à copier dont la taille individuelle est supérieure ou égale à 16 Mo.

Si vous copiez un certain nombre de fichiers relativement volumineux, ce n’est pas en attribuant une valeur élevée à la propriété **parallelCopies** que vous bénéficierez de meilleurs performances en raison de la limitation des ressource d’une unité de déplacement de données cloud. Dans ce cas, vous avez tout intérêt à utiliser plusieurs unités de déplacement de données cloud pour pouvoir copier cet important volume de données à un débit élevé. Pour spécifier le nombre d’unités de déplacement de données cloud à utiliser dans l’activité de copie, définissez la valeur de la propriété **cloudDataMovementUnits** comme indiqué ci-dessous :

	"activities":[  
	    {
	        "name": "Sample copy activity",
	        "description": "",
	        "type": "Copy",
	        "inputs": [{ "name": "InputDataset" }],
	        "outputs": [{ "name": "OutputDataset" }],
	        "typeProperties": {
	            "source": {
	                "type": "BlobSource",
	            },
	            "sink": {
	                "type": "AzureDataLakeStoreSink"
	            },
	            "cloudDataMovementUnits": 4
	        }
	    }
	]

Les **valeurs autorisées** pour la propriété cloudDataMovementUnits sont les suivantes : 1 (par défaut), 2, 4 et 8. Si vous avez besoin d’un plus grand nombre d’unités de déplacement de données cloud pour bénéficier d’un débit plus élevé, contactez le [support Azure](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). Le **nombre effectif d’unités de déplacement de données cloud** utilisées pour l’opération de copie au moment de l’exécution est inférieur ou égal à la valeur configurée, selon le nombre de fichiers à copier à partir de la source qui remplissent les critères de taille.

> [AZURE.NOTE] parallelCopies doit avoir une valeur supérieure ou égale à celle de la propriété cloudDataMovementUnits si celle-ci est spécifiée ; et si la valeur de cloudDataMovementUnits est supérieure à 1, le déplacement de données en parallèle est réparti entre les cloudDataMovementUnits pour l’exécution d’activité de copie en question et le débit s’en trouve amélioré.

Quand la copie porte sur plusieurs fichiers volumineux et que la propriété **cloudDataMovementUnits** a la valeur 2, 4 et 8, les performances peuvent être multipliées par deux, quatre et sept par rapport aux valeurs de référence indiquées dans la section Performances de référence.

Reportez-vous aux [exemples de cas d’utilisation](#case-study---parallel-copy) ici pour mieux tirer parti des deux propriétés précédentes et ainsi améliorer le débit de vos déplacements de données.
 
Il est **important** de se rappeler que vous serez facturé selon la durée totale de l’opération de copie. Par conséquent, si un travail de copie prenait auparavant une heure avec une seule unité cloud et qu’il prend maintenant 15 minutes avec quatre unités cloud, le montant total de la facture sera identique. Voici un autre scénario : supposons que vous utilisez quatre unités cloud et que la première passe 10 minutes dans une exécution d’activité de copie, la deuxième 10 minutes, la troisième cinq minutes et la quatrième cinq minutes. Vous serez alors facturé pour la durée totale de la copie (déplacement des données), soit 10 + 10 + 5 + 5 = 30 minutes. L’utilisation de **parallelCopies** n’a aucun impact sur la facturation.

## Copie intermédiaire
Lorsque vous copiez des données entre un magasin de données source et un magasin de données cible, vous pouvez utiliser Azure Blob Storage comme magasin de stockage intermédiaire. Cette fonctionnalité intermédiaire est particulièrement utile dans les cas suivants :

1.	**Il peut être assez long parfois d’effectuer des déplacements de données hybrides (c’est-à-dire d’un magasin de données local vers un magasin de données cloud ou inversement) sur une connexion réseau lente.** Pour améliorer les performances de ce type de transfert de données, vous pouvez compresser les données en local afin d’accélérer le déplacement des données sur le réseau vers le magasin de données intermédiaire dans le cloud, puis décompresser les données du stockage intermédiaire avant de les charger dans le magasin de données de destination.
2.	**Vous ne souhaitez pas ouvrir de ports autres que 80 et 443 dans votre pare-feu en raison de vos stratégies informatiques.** Par exemple, lorsque vous copiez des données d’un magasin de données local vers un récepteur de base de données SQL Azure ou un récepteur Azure SQL Data Warehouse, vous devez activer les communications TCP sortantes sur le port 1433 pour le pare-feu Windows et le pare-feu d’entreprise. Dans ce scénario, vous pouvez utiliser la passerelle de gestion des données pour copier dans un premier temps les données dans une instance Azure Blob Storage intermédiaire, (via HTTP(s) c’est-à-dire sur le port 443), puis charger les données dans la base de données SQL ou dans SQL Data Warehouse à partir du stockage intermédiaire. Dans ce flux, il n’est pas nécessaire d’activer le port 1433.
3.	**Recevoir des données à partir de divers magasins de données dans Azure SQL Data Warehouse via PolyBase.** Azure SQL Data Warehouse utilise PolyBase comme un mécanisme de haut débit pour charger des données volumineuses dans SQL Data Warehouse. Ce processus suppose toutefois que les données source résident dans Azure Blob Storage et qu’elles répondent à certains critères supplémentaires. Lors du chargement des données à partir d’un magasin de données autre qu’Azure Blob Storage, vous pouvez activer la copie des données via une instance Azure Blob Storage intermédiaire, auquel cas Azure Data Factory effectuera les transformations requises sur les données pour faire en sorte qu’elles répondent aux exigences de PolyBase, avant d’utiliser PolyBase pour charger les données dans SQL Data Warehouse. Pour obtenir des informations supplémentaires et accéder à des exemples, consultez la section [Utiliser PolyBase pour charger des données dans Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).

### Principe de fonctionnement de la copie intermédiaire
Lorsque vous activez la fonctionnalité de stockage intermédiaire, les données sont d’abord copiées du magasin de données source dans le magasin de données intermédiaire (votre propre magasin), puis copiées du magasin de données intermédiaire dans le récepteur du magasin de données. Azure Data Factory gère automatiquement le flux en 2 étapes et nettoie également les données temporaires à partir du stockage intermédiaire une fois les données transférées.

Dans le **scénario de copie cloud** où les magasins de données source et cible résident dans le cloud et n’utilisent pas la passerelle de gestion des données, les opérations de copie sont effectuées par **le service Azure Data Factory**.

![Copie intermédiaire - scénario cloud](media/data-factory-copy-activity-performance/staged-copy-cloud-scenario.png)

En revanche, dans le **scénario de copie hybride** (magasin de données source en local et récepteur dans le cloud), le déplacement des données entre le magasin de données source et le magasin de données intermédiaire est assuré par la **passerelle de gestion des données** et le déplacement des données entre le magasin de données intermédiaire et le magasin de données récepteur est effectué par le **service Azure Data Factory**.

![Copie intermédiaire - scénario hybride](media/data-factory-copy-activity-performance/staged-copy-hybrid-scenario.png)

Lorsque vous activez le déplacement des données à l’aide du magasin de données intermédiaire, vous pouvez indiquer si vous souhaitez compresser les données avant de les déplacer du magasin de données source vers le magasin de données intermédiaire, et les décompresser avant leur transfert du magasin de données intermédiaire vers le magasin de données récepteur.

La copie de données entre un magasin de données cloud et un magasin de données en local ou entre deux magasins de données en local n’est pas prise en charge actuellement, mais le sera prochainement.

### Configuration
Vous pouvez configurer le paramètre **enableStaging** sur l’activité de copie pour spécifier si vous souhaitez que les données soient placées temporairement dans un stockage Azure Blob Storage avant d’être chargées dans un magasin de données de destination. Lorsque vous définissez enableStaging sur true, vous devez spécifier les propriétés supplémentaires répertoriées dans le tableau suivant. Si ce n’est déjà fait, vous devez également créer un service lié Azure Storage ou SAP Azure Storage.

Propriété | Description | Valeur par défaut | Requis
--------- | ----------- | ------------ | --------
enableStaging | Indiquez si vous souhaitez copier les données via un magasin de données intermédiaire. | False | Non
linkedServiceName | Spécifiez le nom d’un service lié [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) ou [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) faisant référence à votre compte Azure Storage qui sera utilisé comme magasin de données intermédiaire. <br/><br/> Notez qu’un stockage Azure avec SAP (signature d’accès partagé) ne peut pas être utilisé pour charger les données dans Azure SQL Data Warehouse via PolyBase. Il peut cependant être utilisé dans tous les autres scénarios. | N/A | Oui, quand enableStaging est défini sur true. 
path | Spécifiez le chemin d’accès dans Azure Blob Storage qui contiendra les données intermédiaires. Si vous ne renseignez pas le chemin d’accès, le service créera un conteneur pour stocker les données temporaires. <br/><br/> Vous n’avez pas besoin de spécifier le chemin d’accès, sauf si vous utilisez Azure Storage avec SAP ou si l’emplacement des données temporaires doit répondre à des exigences strictes. | N/A | Non
enableCompression | Spécifiez si les données doivent être compressées lorsqu’elles sont déplacées entre le magasin de données source et le magasin de données récepteur, afin de réduire le volume de données transférées sur le réseau. | False | Non

Voici un exemple de définition d’une activité de copie avec les propriétés ci-dessus :

	"activities":[  
	{
		"name": "Sample copy activity",
		"type": "Copy",
		"inputs": [{ "name": "OnpremisesSQLServerInput" }],
		"outputs": [{ "name": "AzureSQLDBOutput" }],
		"typeProperties": {
			"source": {
				"type": "SqlSource",
			},
			"sink": {
				"type": "SqlSink"
			},
	    	"enableStaging": true,
			"stagingSettings": {
				"linkedServiceName": "MyStagingBlob",
				"path": "stagingcontainer/path",
				"enableCompression": true
			}
		}
	}
	]


### Impact sur la facturation
Notez que vous serez facturé sur la base des deux étapes de la durée de la copie et du type de copie, autrement dit :

- Lorsque vous utilisez la copie intermédiaire lors d’une copie dans le cloud (copie de données à partir d’un magasin de données cloud vers un autre magasin de données cloud, par exemple d’Azure Data Lake vers Azure SQL Data Warehouse), vous serez facturé au prix de [somme de la durée de copie pour les étapes 1 et 2] x [prix unitaire de la copie dans le cloud]
- Lorsque vous utilisez la copie intermédiaire lors d’une copie hybride (copie de données à partir d’un magasin de données local vers un magasin de données cloud, par exemple, base de données de SQL Server en local vers Azure SQL Data Warehouse), vous serez facturé au prix de [durée de la copie hybride] x [prix unitaire de la copie hybride] + [durée de la copie cloud] x [prix unitaire de la copie cloud]


## Considérations sur la source
### Généralités
Vérifiez que le magasin de données sous-jacent n’est pas submergé par d’autres charges de travail s’exécutant dessus ou s’y rapportant, notamment une activité de copie.

Pour les magasins de données Microsoft, reportez-vous aux [rubriques relatives à la surveillance et au réglage](#appendix-data-store-performance-tuning-reference) spécifiques du magasin de données, qui peuvent vous aider à comprendre les caractéristiques de performances de celui-ci, à réduire les temps de réponse et à maximiser le débit.

Si vous copiez des données entre **Azure Blob Storage** et **Azure SQL Data Warehouse**, pensez à activer **PolyBase** pour améliorer les performances. Pour plus d’informations, consultez [Utiliser PolyBase pour charger des données dans Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md###use-polybase-to-load-data-into-azure-sql-data-warehouse).


### Magasins de données basés sur un fichier
*(Incluant les objets Blob Azure, Azure Data Lake et le système de fichiers local)*

- **Taille moyenne de fichier et nombre de fichiers** : l’activité de copie transfère des données fichier par fichier. Pour une même quantité de données à déplacer, le débit global sera plus lent si les données se composent d’un grand nombre de petits fichiers plutôt que d’un petit nombre de fichiers plus volumineux, en raison de la phase d’amorçage nécessaire pour chaque fichier. Par conséquent, vous devez autant que possible combiner plusieurs petits fichiers en fichiers plus volumineux pour augmenter le débit.
- **Format de fichier et compression** : pour d’autres méthodes permettant d’améliorer les performances, voir les sections [Considérations sur la sérialisation/désérialisation](#considerations-on-serializationdeserialization) et [Considérations sur la compression](#considerations-on-compression).
- En outre, pour le scénario de **système de fichiers local** où l’utilisation d’une **passerelle de gestion des données** est requise, voir la section [Considérations sur la passerelle](#considerations-on-data-management-gateway).

### Bases de données relationnelles
*(Incluant Base de données SQL Azure, Azure SQL Data Warehouse, Base de données SQL Server, Base de données Oracle, Base de données MySQL, Base de données DB2, Base de données Teradata, Base de données Sybase et Base de données PostgreSQL)*

- **Modèle de données** : le schéma de table a une incidence sur le débit de copie. Pour copier une même quantité de données, une taille de ligne importante produit de meilleures performances qu’une petite taille, car la base de données peut extraire plus efficacement moins de lots de données contenant moins de lignes.
- **Requête ou procédure stockée** : optimisez la logique de la requête ou de la procédure stockée que vous spécifiez dans la source d’activité de copie, afin d’extraire les données plus efficacement.
- En outre, pour des **bases de données relationnelles locales**, telles que SQL Server et Oracle, où l’utilisation d’une **passerelle de gestion des données** est requise, consultez la section [Considérations sur la passerelle](#considerations-on-data-management-gateway).

## Considérations sur le récepteur

### Généralités
Vérifiez que le magasin de données sous-jacent n’est pas submergé par d’autres charges de travail s’exécutant dessus ou s’y rapportant, notamment une activité de copie.

Pour les magasins de données Microsoft, reportez-vous aux [rubriques relatives à la surveillance et au réglage](#appendix-data-store-performance-tuning-reference) spécifiques du magasin de données, qui peuvent vous aider à comprendre les caractéristiques de performances de celui-ci, à réduire les temps de réponse et à maximiser le débit.

Si vous copiez des données entre **Azure Blob Storage** et **Azure SQL Data Warehouse**, pensez à activer **PolyBase** pour améliorer les performances. Pour plus d’informations, consultez [Utiliser PolyBase pour charger des données dans Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md###use-polybase-to-load-data-into-azure-sql-data-warehouse).


### Magasins de données basés sur un fichier
*(Incluant les objets Blob Azure, Azure Data Lake et le système de fichiers local)*

- **Comportement de copie** : si vous copiez des données d’un autre magasin de données basé sur un fichier, l’activité de copie fournit trois types de comportements via la propriété « copyBehavior » : conserver la hiérarchie, aplatir la hiérarchie et fusionner les fichiers. La conservation ou l’aplanissement de la hiérarchie entraîne peu ou pas de surcharge de performances, tandis que la fusion de fichiers entraîne une surcharge supplémentaire.
- **Format de fichier et compression** : pour d’autres méthodes permettant d’améliorer les performances, voir les sections [Considérations sur la sérialisation/désérialisation](#considerations-on-serializationdeserialization) et [Considérations sur la compression](#considerations-on-compression).
- Pour les **objets blob Azure**, nous ne prenons en charge actuellement que les objets blob de blocs pour l’optimisation du transfert et du débit de données.
- En outre, pour les scénarios de **système de fichiers local** où l’utilisation d’une **passerelle de gestion des données** est requise, voir la section [Considérations sur la passerelle](#considerations-on-data-management-gateway).

### Bases de données relationnelles
*(Incluant Base de données SQL Azure, Azure SQL Data Warehouse et Base de données SQL Server)*

- **Comportement de copie** : selon les propriétés configurées pour « sqlSink », l’activité de copie écrit des données dans la base de données de destination de différentes façons :
	- Par défaut, le service de déplacement de données utilise une API de copie en bloc pour insérer des données en mode Append, ce qui optimise les performances.
	- Si vous configurez une procédure stockée dans le récepteur, la base de données applique les données ligne par ligne, au lieu de les charger en bloc, ce qui entraîne une baisse sensible des performances. Si la taille des données est importante, autant que possible, songez à utiliser plutôt la propriété « sqlWriterCleanupScript » (voir ci-dessous).
	- Si vous configurez la propriété « sqlWriterCleanupScript », pour chaque exécution d’une activité de copie, le service déclenche d’abord le script, puis utilise l’API de copie en bloc pour insérer les données. Par exemple, pour remplacer les données de la table entière par les dernières données, vous pouvez spécifier un script qui supprime tous les enregistrements avant de charger en bloc les nouvelles données à partir de la source.
- **Modèle de données et taille de lot** :
	- Le schéma de table a une incidence sur le débit de copie. Pour copier une même quantité de données, une taille de ligne importante produit de meilleures performances qu’une petite taille, car la base de données peut valider plus efficacement moins de lots de données.
	- L’activité de copie insère les données dans une série de lots dont le nombre de lignes peut être défini à l’aide de la propriété de « writeBatchSize ». Si vos données comportent des lignes de petite taille, vous pouvez définir la propriété « writeBatchSize » sur une valeur plus élevée pour réduire la surcharge de traitement par lots et augmenter le débit. Si la taille de ligne de vos données est importante, soyez prudent en augmentant la valeur de writeBatchSize, car une valeur élevée peut faire échouer la copie en raison d’une surcharge de la base de données.
- En outre, pour des **bases de données relationnelles locales**, telles que SQL Server et Oracle, où l’utilisation d’une **passerelle de gestion des données** est requise, consultez la section [Considérations sur la passerelle](#considerations-on-data-management-gateway).


### Magasins NoSQL
*(Incluant table Azure et Azure DocumentDB)*

- Pour **Table Azure** :
	- **Partition** : l’écriture de données en partitions entrelacées réduit considérablement les performances. Vous pouvez classer vos données sources par clé de partition afin qu’elles soient insérées efficacement partition après partition, ou ajuster la logique pour écrire les données dans une seule partition.
- Pour **Azure DocumentDB** :
	- **Taille de lot** : la propriété de « writeBatchSize » indique le nombre de demandes parallèles adressées au service DocumentDB pour la création de documents. Vous pouvez vous attendre à de meilleures performances lorsque vous augmentez la valeur « writeBatchSize », car davantage de demandes parallèles sont envoyées à DocumentDB. Toutefois, prenez garde à la limitation lors de l’écriture dans DocumentDB (message d’erreur « Le taux de demandes est élevé »). Une limitation peut se produire en raison de divers facteurs, dont la taille des documents, le nombre de termes qu’ils contiennent, et la stratégie d’indexation de la collection cible. Pour obtenir un débit de copie plus élevé, songez à utiliser une meilleure collection (par exemple, S3).

## Considérations sur la sérialisation/désérialisation.
Une sérialisation et une désérialisation peuvent se produire quand le jeu de données d’entrée ou sortie est un fichier. L’activité de copie prend actuellement en charge les formats de données Avro et texte (par exemple, CSV et TSV).

**Comportements de copie :**

- Lors de la copie de fichiers entre magasins de données basés sur des fichiers :
	- Lorsque les jeux de données d’entrée et de sortie ont les mêmes paramètres de format de fichier ou n’en ont aucun, le service de déplacement de données exécute une copie binaire sans effectuer de sérialisation/désérialisation. Par conséquent, vous pouvez constater un meilleur débit par rapport au scénario où les paramètres de format de fichier source/récepteur diffèrent.
	- Lorsque les jeux de fichiers d’entrée et de sortie sont au format texte, et que seul le type d’encodage diffère, le service de déplacement de données opère uniquement une conversion de codage sans effectuer de sérialisation/désérialisation, ce qui entraîne une surcharge de performances par rapport à une copie binaire.
	- Lorsque les jeux de données d’entrée et de sortie diffèrent par leurs format de fichier ou leur configuration, par exemple au niveau des séparateurs, le service de déplacement de données désérialise les données sources en flux, les transforme, puis les sérialise au format de sortie souhaité. Cela entraîne une surcharge des performances beaucoup plus importante par rapport aux scénarios précédents.
- Lors de la copie de fichiers vers un magasin de données non basé sur un fichier ou à partir de celui-ci (par exemple, d’un magasin basé sur un fichier vers une base relationnelle), une étape de sérialisation ou de désérialisation est requise, ce qui entraîne une surcharge significative des performances.

**Format de fichier :** le choix du format de fichier peut avoir une incidence sur les performances de copie. Par exemple, Avro est un format de fichier binaire compact qui stocke des métadonnées avec les données, et est largement pris en charge dans l’écosystème Hadoop pour le traitement et les requêtes. En revanche, le format Avro étant plus coûteux en sérialisation/désérialisation, le débit de copie est inférieur par rapport au format texte. Le choix du format de fichier à utiliser ainsi que du flux de traitement doit être effectué de façon holistique, en considérant la forme sous laquelle les données sont stockées dans les magasins de données sources ou doivent être extraites de systèmes externes, le meilleur format pour le stockage, le traitement analytique et les requêtes, et le format dans lequel les données doivent être exportées dans des mini-data Warehouses pour les outils de rapport et de visualisation. Parfois, un format de fichier non optimal pour les performances en lecture et écriture peut s’avérer bien adapté pour le processus analytique général.

## Considérations relatives à la compression
Lorsque votre jeu de données d’entrée ou de sortie est un fichier, vous pouvez configurer l’activité de copie pour qu’elle effectue une compression ou une décompression lors de l’écriture de données dans la destination. L’activation de la compression constitue compromis entre les entrées/sorties (E/S) et l’utilisation du processeur : la compression des données nécessite des ressources de calcul supplémentaires mais réduit les E/S et le stockage réseau, ce qui, selon vos données, peut améliorer le débit global de copie.

**Codec :** les types de compressions GZIP, BZIP2 et Deflate sont pris en charge. Les trois types peuvent être utilisés par Azure HDInsight pour le traitement. Chaque codec de compression a sa spécificité. Par exemple, le codec BZIP2 présente le débit le plus bas, mais offre les meilleures performances de requête Hive, car il peut être fractionné pour le traitement. Le codec GZIP constitue l’option la plus équilibrée et la plus souvent utilisée. Vous devez choisir le codec le plus approprié pour votre scénario de bout en bout.

**Niveau :** pour chaque codec de compression, vous avec le choix entre deux options, la compression la plus rapide et la compression optimale. L’option de compression la plus rapide compresse les données aussi rapidement que possible, même si le fichier résultant de l’opération n’est pas compressé de façon optimale. L’option de compression optimale nécessite plus de temps, mais produit une quantité de données minimale. Vous pouvez tester les deux options pour voir celle qui offre les meilleures performances globales dans votre cas.

**Considération :** pour la copie d’un volume important de données entre un magasin local et le cloud, où la bande passante du réseau d’entreprise et Azure sont souvent le facteur de limitation, et où vous souhaitez que les jeux de données d’entrée et de sortie soient sous forme non compressée, vous pouvez envisager d’utiliser un **objet blob Azure temporaire** avec compression. Plus précisément, vous pouvez scinder une activité de copie en deux activités : la première copie à partir de la source dans un objet blob temporaire ou intermédiaire sous forme compressée, et la deuxième copie les données compressées de l’objet intermédiaire et les décompresse lors de l’écriture dans le récepteur.

## Considérations sur le mappage de colonnes
La propriété « columnMappings » dans l’activité de copie permet de mapper la totalité ou un sous-ensemble des colonnes d’entrée aux colonnes de sortie. Après avoir lu les données de la source, le service de déplacement de données doit effectuer un mappage de colonnes aux données avant d’écrire celles-ci sur le récepteur. Ce traitement supplémentaire réduit le débit de copie.

Si votre magasin de données source est interrogeable, qu’il s’agisse d’une base relationnelle de type SQL Azure/SQL Server ou d’un magasin NoSQL de type Table Azure/Azure DocumentDB, vous pouvez envisager d’envoyer la logique de filtrage/réorganisation de colonne à la propriété de requête au lieu d’utiliser un mappage de colonnes, ce qui a pour effet d’opérer la projection pendant la lecture des données à partir du magasin de données source, et est beaucoup plus efficace.

## Considérations sur la passerelle de gestion des données
Pour des recommandations relatives à la configuration de la passerelle, voir [Considérations relatives à l’utilisation de la passerelle de gestion des données](data-factory-move-data-between-onprem-and-cloud.md#Considerations-for-using-Data-Management-Gateway).

**Environnement de l’ordinateur de la passerelle :** nous vous recommandons d’utiliser un ordinateur dédié pour héberger la passerelle de gestion des données. Utilisez des outils tels que PerfMon pour examiner l’utilisation du processeur, de la mémoire et de la bande passante pendant une opération de copie sur l’ordinateur de la passerelle. Basculez vers un ordinateur plus puissant si la bande passante du processeur, de la mémoire ou du réseau forme un goulot d’étranglement.

**Exécutions d’activités de copie simultanées :** une seule instance de la passerelle de gestion des données peut traiter plusieurs exécutions d’activités de copie en même temps. Ainsi, une passerelle peut exécuter simultanément plusieurs travaux de copie, dont le nombre est calculé en fonction de la configuration matérielle de l’ordinateur de la passerelle. Les travaux de copie excédentaires sont placés en file d’attente jusqu’à ce que la passerelle les récupère ou jusqu’à ce qu’ils expirent, selon l’événement qui se produit en premier. Pour éviter tout conflit de ressources sur la passerelle, vous pouvez planifier vos activités par phases afin de réduire la quantité de travaux de copie mis en file d’attente en même temps, ou envisager de fractionner la charge sur plusieurs passerelles.


## Autres considérations
Si les données à copier sont très volumineuses, vous pouvez ajuster votre logique métier afin de partitionner davantage les données à l’aide du mécanisme de découpage d’Azure Data Factory, et planifier l’activité de copie plus fréquemment pour réduire la taille des données pour chaque exécution d’activité de copie.

Soyez prudent en ce qui concerne le nombre de jeux de données et d’activités de copie qui parviennent à une même base de données à un moment donné. Un grand nombre de travaux de copie simultanés peut limiter un magasin de données, et entraîner une dégradation des performances, une multiplication des tentatives internes de travail de copie et, dans certains cas, des échecs d’exécution.

## Étude de cas – copie d’un serveur SQL Server local vers un objet blob Azure
**Scénario :** un pipeline est conçu pour copier des données d’un serveur SQL Server local vers un objet blob Azure au format CSV. Pour accélérer la copie, il est spécifié que les fichiers CSV doivent être compressés au format BZIP2.

**Test et analyse :** il est constaté que le débit de l’activité de copie est inférieur à 2 Mo/s, ce qui est beaucoup plus lent que le test d’évaluation des performances.

**Analyse des performances et réglage :** pour résoudre le problème de performances, nous allons tout d’abord examiner pas à pas la manière dont les données sont traitées et déplacées :

1.	**Lecture des données :** la passerelle ouvre la connexion à SQL Server et envoie la requête. SQL Server répond en envoyant le flux de données à la passerelle via un intranet.
2.	La passerelle **sérialise** le flux de données au format CSV, et **compresse** les données dans un flux BZIP2.
3.	**Écriture des données :** la passerelle charge le flux BZIP2 vers l’objet blob Azure via Internet.

Comme vous pouvez le voir, les données sont traitées et déplacées de manière séquentielle par diffusion en continu : SQL Server -> Réseau local (LAN) -> Passerelle -> Réseau étendu (WAN) -> objet Blob Azure. **Les performances globales sont déterminées par le débit minimal du pipeline**.

![flux de données](./media/data-factory-copy-activity-performance/case-study-pic-1.png)

Un ou plusieurs des facteurs suivants peuvent occasionner un goulot d’étranglement des performances :

1.	**Source :** SQL Server offre lui-même un faible débit en raison de lourdes charges.
2.	**Passerelle de gestion de données :**
	1.	**LAN :** la passerelle est éloignée du serveur SQL Server auquel elle est reliée par une connexion à faible bande passante
	2.	La **charge sur l’ordinateur de la passerelle** a atteint ses limites pour l’exécution des opérations suivantes :
		1.	**Sérialisation :** la sérialisation du flux de données au format CSV présente un débit lent
		2.	**Compression :** le codec de compression choisi est lent (par exemple, BZIP2, avec un débit de 2,8 Mo/s avec Core i7)
	3.	**WAN :** faible bande passante entre le réseau d’entreprise et Azure (par exemple, T1 = 1 544 Kbits/s, T2 = 6 312 Kbits/s)
4.	**Récepteur :** l’objet blob Azure présente un faible débit (bien que ce soit très improbable étant donné que son contrat SLA garantit un minimum de 60 Mo/s).

Dans ce cas, la compression de données BZIP2 pourrait ralentir l’ensemble du pipeline. Un basculement vers le codec de compression GZIP peut résoudre ce goulot d’étranglement.


## Étude de cas : copie en parallèle  

**Scénario I :** copie de 1 000 fichiers de 1 Mo d’un système de fichiers local vers Azure Blob Storage

**Analyse et optimisation des performances :** supposons que vous avez installé la passerelle de gestion des données sur un ordinateur à quatre cœurs. Dans ce cas, Data Factory utilise par défaut 16 copies en parallèle pour déplacer simultanément les fichiers du système de fichiers vers Azure Blob Storage. Cette configuration doit assurer un bon débit. Si vous le souhaitez, vous pouvez aussi spécifier le nombre de copies en parallèle. Quand vous copiez un grand nombre de fichiers de petite taille, les copies en parallèle ont un effet très favorable sur le débit dans la mesure où les ressources en présence sont utilisées avec plus d’efficacité.

![Scénario 1](./media/data-factory-copy-activity-performance/scenario-1.png)

**Scénario II :** copie de 20 objets blob de 500 Mo chacun d’Azure Blob Storage vers Azure Data Lake Store Analysis et optimisation des performances.

**Analyse et optimisation des performances :** pour ce scénario, Data Factory copie par défaut les données d’Azure Blob vers Azure Data Lake en utilisant une seule copie (parallelCopies = 1) et une seule unité de déplacement de données cloud. Vous constaterez un débit proche de ce qui est indiqué dans la section précédente sur les [performances de référence](#performance-reference).

![Scénario 2](./media/data-factory-copy-activity-performance/scenario-2.png)

Quand la taille des fichiers individuels est supérieure à plusieurs dizaines de mégaoctets et que le volume total est important, le fait d’augmenter la valeur de la propriété parallelCopies ne donne pas de meilleures performances de copie compte tenu de la limitation des ressources d’une unité de déplacement de données cloud. Vous avez plutôt intérêt à spécifier plus d’unités de déplacement de données cloud pour profiter de davantage de ressources pour le déplacement de données. Ne spécifiez pas de valeur pour la propriété parallelCopies pour laisser Data Factory gérer le parallélisme. Dans ce cas, en attribuant à cloudDataMovementUnits la valeur 4, le débit sera multiplié par quatre.

![Scénario 3](./media/data-factory-copy-activity-performance/scenario-3.png)

## Référence sur le réglage des performances des magasins de données
Voici quelques références relatives à la surveillance et au réglage des performances pour quelques magasins de données pris en charge :

- Azure Storage (y compris les objets blob Azure et la table Azure) : [Objectifs d’évolutivité d’Azure Storage](../storage/storage-scalability-targets.md) et [Liste de contrôle des performances et de l’évolutivité d’Azure Storage](../storage//storage-performance-checklist.md)
- Base de données SQL Azure : vous pouvez [surveiller les performances](../sql-database/sql-database-service-tiers.md#monitoring-performance) et vérifier le pourcentage de l’unité de transaction de base de données (DTU).
- Azure SQL Data Warehouse : sa capacité est mesurée en Data Warehouse Units (DWU). Voir [Performances et mise à l’échelle élastiques avec SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md).
- Azure DocumentDB : [Niveaux de performances dans DocumentDB](../documentdb/documentdb-performance-levels.md).
- SQL Server local : [Surveillance et réglage des performances](https://msdn.microsoft.com/library/ms189081.aspx).
- Serveur de fichiers local : [Réglage des performances des serveurs de fichiers](https://msdn.microsoft.com/library/dn567661.aspx)

<!---HONumber=AcomDC_0706_2016-->