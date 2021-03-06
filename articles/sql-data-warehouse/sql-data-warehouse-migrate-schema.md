<properties
   pageTitle="Migration de votre schéma vers SQL Data Warehouse | Microsoft Azure"
   description="Conseils relatifs à la migration de votre schéma vers Microsoft Azure SQL Data Warehouse, dans le cadre du développement de solutions."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# Migration de votre schéma vers SQL Data Warehouse#

Les synthèses suivantes vous permettront de mieux comprendre les différences entre SQL Server et SQL Data Warehouse afin de faciliter la migration de votre base de données.

### Fonctionnalités de tables
SQL Data Warehouse n’utilise ou ne prend pas en charge les fonctionnalités suivantes :

- Clés primaires  
- Clés étrangères  
- Contraintes CHECK  
- Contraintes uniques  
- Index uniques  
- Colonnes calculées  
- Colonnes éparses  
- Types définis par l’utilisateur  
- Vues indexées  
- Identités  
- Séquences  
- Déclencheurs  
- Synonymes  


### Différences entre les types de données
SQL Data Warehouse prend en charge les types de données métiers les plus courants :

- bigint
- binaire
- bit
- char
- date
- datetime
- datetime2
- datetimeoffset
- décimal
- float
- int
- money
- nchar
- nvarchar
- numérique
- real
- smalldatetime
- smallint
- smallmoney
- sysname
- time
- tinyint
- varbinary
- varchar
- uniqueidentifier

Vous pouvez identifier les colonnes de votre entrepôt de données qui contiennent des types incompatibles, via la requête suivante :

```sql
SELECT  t.[name]
,       c.[name]
,       c.[system_type_id]
,       c.[user_type_id]
,       y.[is_user_defined]
,       y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN
                (   'geography'
                ,   'geometry'
                ,   'hierarchyid'
                ,   'image'
                ,   'ntext'
                ,   'sql_variant'
                ,   'text'
                ,   'timestamp'
                ,   'xml'
                )
AND  y.[is_user_defined] = 1
;

```

La requête inclut les types de données définis par l’utilisateur qui ne sont également pas pris en charge.

Si votre base de données inclut des types non pris en charge, ne vous inquiétez pas. Voici d’autres solutions possibles :

Au lieu du paramètre...

- **geometry** : utilisez un type « varbinary » ;
- **geography** : utilisez un type « varbinary » ;
- **hierarchyid** : ce type CLR n’est pas pris en charge ;
- **image**, **text**, **ntext** : utilisez une valeur «  varchar/nvarchar » (la plus petite possible) ;
- **sql\_variant** : fractionnez la colonne en plusieurs colonnes fortement typées ;
- **table** : appliquez une conversion vers des tables temporaires ;
- **timestamp** : modifiez le code afin d’utiliser le paramètre « datetime2 » et la fonction `CURRENT_TIMESTAMP`. Remarque : vous ne pouvez pas utiliser le paramètre « current\_timestamp » en tant que contrainte par défaut ; la valeur ne sera pas automatiquement mise à jour. Si vous devez migrer les valeurs « rowversion » à partir d’une colonne de type horodatage, utilisez le paramètre binary(8) ou varbinary(8) pour les valeurs de version de ligne NOT NULL ou NULL ;
- **types définis par l’utilisateur** : appliquez à nouveau les types natifs, le cas échéant ;
- **xml** : utilisez la valeur varchar(max) ou une valeur inférieure pour optimiser les performances. Fractionner sur plusieurs colonnes si nécessaire.

Pour optimiser les performances, au lieu de :

- nvarchar(max), utilisez le paramètre nvarchar (4000) ou une valeur inférieure pour optimiser les performances ;
- varchar(max), utilisez la valeur varchar(8000) ou une valeur inférieure pour optimiser les performances.

Prise en charge partielle :

- Les contraintes par défaut prennent uniquement en charge des littéraux et des constantes. Les expressions ou fonctions non déterministes comme `GETDATE()` ou `CURRENT_TIMESTAMP` ne sont pas prises en charge.

> [AZURE.NOTE] Si vous utilisez PolyBase pour charger vos tables, définissez ces dernières de sorte que la taille de ligne maximale (longueur totale des colonnes à longueur variable comprise) ne dépasse pas 32 767 octets. Vous pouvez définir des lignes incluant des données d’une longueur variable, susceptibles de dépasser cette valeur maximale, et charger des lignes avec BCP. Toutefois, vous ne pouvez pas encore utiliser PolyBase pour charger ces données. La prise en charge de PolyBase pour les lignes larges sera bientôt disponible. Essayez également de limiter la taille de vos colonnes à longueur variable, afin d’optimiser le débit lors de l’exécution des requêtes.

## Étapes suivantes
Une fois que vous avez migré le schéma de base de données vers SQL Data Warehouse, vous pouvez parcourir l’un des articles suivants :

- [Migration de vos données][]
- [Migration de votre code][]

Pour obtenir des conseils supplémentaires sur le développement, consultez la [vue d’ensemble sur le développement][].

<!--Image references-->

<!--Article references-->
[Migration de votre code]: sql-data-warehouse-migrate-code.md
[Migration de vos données]: sql-data-warehouse-migrate-data.md
[vue d’ensemble sur le développement]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->


<!--Other Web references-->

<!---HONumber=AcomDC_0518_2016-->