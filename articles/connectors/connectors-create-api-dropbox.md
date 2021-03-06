<properties
    pageTitle="Ajouter le connecteur Dropbox à PowerApps Enterprise ou des applications logiques | Microsoft Azure"
    description="Vue d’ensemble du connecteur Dropbox avec les paramètres d’API REST"
    services=""
    suite=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="05/20/2016"
   ms.author="mandia"/>

# Prise en main du connecteur Dropbox 
Connectez-vous à Dropbox pour effectuer des tâches de gestion de fichiers, telles que la création ou la récupération de fichiers. Le connecteur Dropbox peut être utilisé dans :

- Logic Apps 
- PowerApps

> [AZURE.SELECTOR]
- [Logic Apps](../articles/connectors/connectors-create-api-dropbox.md)
- [PowerApps Enterprise](../articles/power-apps/powerapps-create-api-dropbox.md)

&nbsp;

>[AZURE.NOTE] Cette version de l'article s'applique à la version de schéma 2015-08-01-preview des applications logiques.


Avec Dropbox, vous pouvez effectuer les opérations suivantes :

- Créer votre flux d’activité en fonction des données que vous obtenez de Dropbox. 
- Utiliser des déclencheurs quand un fichier est créé ou mis à jour.
- Utiliser des actions pour créer un fichier, supprimer un fichier et bien plus encore. Ces actions obtiennent une réponse, puis mettent la sortie à la disposition d’autres actions. Par exemple, quand un fichier est créé dans Dropbox, vous pouvez l’envoyer par courrier électronique à l’aide d’Office 365.
- Ajoutez le connecteur Dropbox à PowerApps Enterprise. Vos utilisateurs peuvent ensuite utiliser ce connecteur dans leurs applications. 

Pour plus d’informations sur l’ajout d’un connecteur à PowerApps Enterprise, consultez [Register connector in PowerApps](../power-apps/powerapps-register-from-available-apis.md) (Inscrire un connecteur dans PowerApps).

Pour ajouter une opération aux applications logiques, consultez [Créer une application logique](../app-service-logic/app-service-logic-create-a-logic-app.md).

## Déclencheurs et actions
Dropbox inclut les déclencheurs et les actions suivants.

Déclencheurs | Actions
--- | ---
<ul><li>Quand un fichier est créé</li><li>Quand un fichier est modifié</li></ul> | <ul><li>Créer un fichier</li><li>Quand un fichier est créé</li><li>Copier un fichier</li><li>Supprimer un fichier</li><li>Extraire une archive dans un dossier</li><li>Obtenir le contenu d’un fichier à l’aide de l’identifiant</li><li>Obtenir un fichier à l’aide du chemin</li><li>Obtenir les métadonnées d’un fichier à l’aide de l’identifiant</li><li>Obtenir les métadonnées d’un fichier à l’aide du chemin</li><li>Mettre à jour un fichier</li><li>Quand un fichier est modifié</li></ul>

Tous les connecteurs prennent en charge les données aux formats JSON et XML.

## Créer la connexion à Dropbox

Quand vous ajoutez ce connecteur à vos applications logiques, vous devez autoriser celles-ci à se connecter à votre compte Dropbox.

>[AZURE.INCLUDE [Procédure de création d’une connexion à Dropbox](../../includes/connectors-create-api-dropbox.md)]

Après avoir créé la connexion, vous entrez les propriétés Dropbox, telles que le chemin du dossier ou le nom du fichier. La section **Informations de référence sur l’API REST** dans cette rubrique décrit ces propriétés.

>[AZURE.TIP] Vous pouvez utiliser cette même connexion Dropbox dans d’autres applications logiques.

## Informations de référence sur l'API REST Swagger
S'applique à la version 1.0.

### Créer un fichier    
Charge un fichier sur Dropbox. ```POST: /datasets/default/files```

| Nom| Type de données|Requis|Emplacement|Valeur par défaut|Description|
| ---|---|---|---|---|---|
|folderPath|string|yes|query|(aucun) |Chemin du dossier Dropbox sur lequel charger le fichier|
|name|string|yes|query|(aucun) |Nom du fichier à créer dans Dropbox|
|body|string(binary) |yes|body|(aucun) |Contenu du fichier à charger sur Dropbox|

#### Response
|Nom|Description|
|---|---|
|200|OK|
|default|L’opération a échoué.|


### Quand un fichier est créé    
Déclenche un flux quand un fichier est créé dans un dossier Dropbox. ```GET: /datasets/default/triggers/onnewfile```

| Nom| Type de données|Requis|Emplacement|Valeur par défaut|Description|
| ---|---|---|---|---|---|
|folderId|string|yes|query|(aucun) |Identificateur unique du dossier dans Dropbox|

#### Response
|Nom|Description|
|---|---|
|200|OK|
|default|L’opération a échoué.|


### Copier un fichier    
Copie un fichier vers Dropbox. ```POST: /datasets/default/copyFile```

| Nom| Type de données|Requis|Emplacement|Valeur par défaut|Description|
| ---|---|---|---|---|---|
|source|string|yes|query|(aucun) |URL du fichier source|
|destination|string|yes|query| (aucun)|Chemin de destination du fichier dans Dropbox, y compris le nom de fichier cible|
|overwrite|booléenne|no|query|(aucun) |Remplace le fichier de destination si la valeur est « true »|

#### Response
|Nom|Description|
|---|---|
|200|OK|
|default|L’opération a échoué.|


### Supprimer un fichier    
Supprime un fichier de Dropbox. ```DELETE: /datasets/default/files/{id}```

| Nom| Type de données|Requis|Emplacement|Valeur par défaut|Description|
| ---|---|---|---|---|---|
|id|string|yes|path|(aucun)|Identificateur unique du fichier à supprimer de Dropbox|

#### Response
|Nom|Description|
|---|---|
|200|OK|
|default|L’opération a échoué.|


### Extraire une archive dans un dossier    
Extrait un fichier d’archive dans un dossier de Dropbox (exemple : .zip). **```POST: /datasets/default/extractFolderV2```**

| Nom| Type de données|Requis|Emplacement|Valeur par défaut|Description|
| ---|---|---|---|---|---|
|source|string|yes|query|(aucun) |Chemin du fichier d'archive|
|destination|string|yes|query|(aucun) |Chemin dans Dropbox indiquant où extraire le contenu de l’archive|
|overwrite|booléenne|no|query|(aucun) |Remplace les fichiers de destination si la valeur est « true »|

#### Response
|Nom|Description|
|---|---|
|200|OK|
|default|L’opération a échoué.|


### Obtenir le contenu d’un fichier à l’aide de l’identifiant    
Récupère le contenu d’un fichier de Dropbox à partir de son identifiant. ```GET: /datasets/default/files/{id}/content```

| Nom| Type de données|Requis|Emplacement|Valeur par défaut|Description|
| ---|---|---|---|---|---|
|id|string|yes|path|(aucun) |Identificateur unique du fichier dans Dropbox|

#### Response
|Nom|Description|
|---|---|
|200|OK|
|default|L’opération a échoué.|


### Obtenir le contenu d’un fichier à l’aide du chemin    
Récupère le contenu d’un fichier de Dropbox à l’aide du chemin d’accès. ```GET: /datasets/default/GetFileContentByPath```

| Nom| Type de données|Requis|Emplacement|Valeur par défaut|Description|
| ---|---|---|---|---|---|
|path|string|yes|query|(aucun) |Chemin unique du fichier dans Dropbox|

#### Response
|Nom|Description|
|---|---|
|200|OK|
|default|L’opération a échoué.|


### Obtenir les métadonnées d’un fichier à l’aide de l’identifiant    
Récupère les métadonnées d’un fichier de Dropbox à l’aide de son identifiant. ```GET: /datasets/default/files/{id}```

| Nom| Type de données|Requis|Emplacement|Valeur par défaut|Description|
| ---|---|---|---|---|---|
|id|string|yes|path|(aucun) |Identificateur unique du fichier dans Dropbox|

#### Response
|Nom|Description|
|---|---|
|200|OK|
|default|L’opération a échoué.|


### Obtenir les métadonnées d’un fichier à l’aide du chemin    
Récupère les métadonnées d’un fichier de Dropbox à l’aide du chemin d’accès. ```GET: /datasets/default/GetFileByPath```

| Nom| Type de données|Requis|Emplacement|Valeur par défaut|Description|
| ---|---|---|---|---|---|
|path|string|yes|query|(aucun) |Chemin unique du fichier dans Dropbox|

#### Response
|Nom|Description|
|---|---|
|200|OK|
|default|L’opération a échoué.|


### Mettre à jour un fichier    
Met à jour un fichier dans Dropbox. ```PUT: /datasets/default/files/{id}```

| Nom| Type de données|Requis|Emplacement|Valeur par défaut|Description|
| ---|---|---|---|---|---|
|id|string|yes|path| (aucun)|Identificateur unique du fichier à mettre à jour dans Dropbox|
|body|string(binary) |yes|body|(aucun) |Contenu du fichier à mettre à jour dans Dropbox|

#### Response
|Nom|Description|
|---|---|
|200|OK|
|default|L’opération a échoué.|


### Quand un fichier est modifié    
Déclenche un flux quand un fichier est modifié dans un dossier Dropbox. ```GET: /datasets/default/triggers/onupdatedfile```

| Nom| Type de données|Requis|Emplacement|Valeur par défaut|Description|
| ---|---|---|---|---|---|
|folderId|string|yes|query|(aucun) |Identificateur unique du dossier dans Dropbox|

#### Response
|Nom|Description|
|---|---|
|200|OK|
|default|L’opération a échoué.|


## Définitions d'objet

#### DataSetsMetadata

|Nom de la propriété | Type de données | Requis|
|---|---|---|
|tabular|non défini|no|
|objet blob|non défini|no|

#### TabularDataSetsMetadata

|Nom de la propriété | Type de données |Requis|
|---|---|---|
|source|string|no|
|displayName|string|no|
|urlEncoding|string|no|
|tableDisplayName|string|no|
|tablePluralName|string|no|

#### BlobDataSetsMetadata

|Nom de la propriété | Type de données |Requis|
|---|---|---|
|source|string|no|
|displayName|string|no|
|urlEncoding|string|no|

#### BlobMetadata

|Nom de la propriété | Type de données |Requis|
|---|---|---|
|ID|string|no|
|Nom|string|no|
|DisplayName|string|no|
|Path|string|no|
|LastModified|string|no|
|Taille|integer|no|
|MediaType|string|no|
|IsFolder|booléenne|no|
|ETag|string|no|
|FileLocator|string|no|

## Étapes suivantes

[Créer une application logique](../app-service-logic/app-service-logic-create-a-logic-app.md).

Revenir à la [liste des API](apis-list.md).


<!--References-->
[1]: https://www.dropbox.com/login
[2]: https://www.dropbox.com/developers/apps/create
[3]: https://www.dropbox.com/developers/apps
[8]: ./media/connectors-create-api-dropbox/dropbox-developer-site.png
[9]: ./media/connectors-create-api-dropbox/dropbox-create-app.png
[10]: ./media/connectors-create-api-dropbox/dropbox-create-app-page1.png
[11]: ./media/connectors-create-api-dropbox/dropbox-create-app-page2.png

<!---HONumber=AcomDC_0525_2016-->