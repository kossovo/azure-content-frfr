<properties
	pageTitle="Utilisation du Cache Redis Azure avec Node.js | Microsoft Azure"
	description="Prise en main du Cache Redis Azure avec Node.js et node_redis"
	services="redis-cache"
	documentationCenter=""
	authors="steved0x"
	manager="douge"
	editor="v-lincan"/>

<tags
	ms.service="cache"
	ms.devlang="nodejs"
	ms.topic="hero-article"
	ms.tgt_pltfrm="cache-redis"
	ms.workload="tbd"
	ms.date="05/31/2016"
	ms.author="sdanie"/>

# Utilisation du Cache Redis Azure avec Node.js

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.JS](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Le Cache Redis Azure permet d'accéder à un cache Redis sécurisé et dédié, géré par Microsoft. Votre cache est accessible à partir de n'importe quelle application dans Microsoft Azure.

Cette rubrique montre comment utiliser le Cache Redis Azure avec Node.js. Pour obtenir un autre exemple d'utilisation du Cache Redis Azure avec Node.js, consultez la rubrique [Créer une application de conversation instantanée Node.js avec Socket.IO sur un site web Azure](../app-service-web/web-sites-nodejs-chat-app-socketio.md).


## Conditions préalables

Installez [node\_redis](https://github.com/mranney/node_redis) :

    npm install redis

Ce didacticiel utilise [node\_redis](https://github.com/mranney/node_redis), mais vous pouvez utiliser n'importe quel client Node.js parmi ceux répertoriés ici : [http://redis.io/clients](http://redis.io/clients).

## Créer un Cache Redis sur Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## Récupérer les clés d’accès et le nom hôte

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]


## Activer le point de terminaison non SSL

Certains clients Redis ne prennent pas en charge SSL. Le [port non-SSL est désactivé par défaut pour les nouvelles instances Cache Redis Azure.](cache-configure.md#access-ports) Au moment de la rédaction de cet article, le client [node\_redis](https://github.com/mranney/node_redis) ne prend pas en charge SSL.

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]


## Ajouter un élément au cache et le récupérer

	  var redis = require("redis");
	
	  // Add your cache name and access key.
	var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});
	
	client.set("key1", "value", function(err, reply) {
		    console.log(reply);
		});
	
	client.get("key1",  function(err, reply) {
		    console.log(reply);
		});

Output:

	OK
	value


## Étapes suivantes

- [Activez les diagnostics du cache](cache-how-to-monitor.md#enable-cache-diagnostics) afin de pouvoir [surveiller](cache-how-to-monitor.md) l’intégrité de votre cache.
- Lisez la [documentation Redis](http://redis.io/documentation) officielle.

<!---HONumber=AcomDC_0601_2016-->