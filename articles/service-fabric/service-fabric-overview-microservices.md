<properties
   pageTitle="Présentation des microservices | Microsoft Azure"
   description="Présentation des raisons pour lesquelles créer des applications avec une approche de microservices revêt un caractère essentiel pour le développement des applications modernes, et de la manière dont Azure Service Fabric fournit une plateforme pour y parvenir"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor="bscholl"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/05/2016"
   ms.author="mfussell"/>

# Pourquoi une approche de microservices pour la conception d’applications ?
Pour les développeurs de logiciels que nous sommes, il n’y a rien de nouveau dans notre conception de l’affacturage d’une application en composants. Il s’agit du paradigme central de l’orientation des objets, des abstractions de logiciel et de la modularité. Aujourd’hui, cet affacturage a tendance à prendre la forme de classes et d’interfaces entre les bibliothèques partagées et les couches de technologie, généralement par le biais d’une approche hiérarchisée avec un magasin principal, une logique métier de niveau intermédiaire et une interface utilisateur frontale. Ce qui *a* changé au cours des dernières années, c’est que nous, en tant que développeurs, créons des applications distribuées pour le cloud, qui sont pilotées par l’entreprise.

Les nouveaux besoins de l’entreprise sont les suivants :

- Nécessité de créer et d’exploiter un service à l’échelle pour élargir la portée commerciale, par exemple dans de nouvelles régions géographiques ou sans avoir à effectuer de déploiement sur les sites des clients.
- Distribution plus rapide des fonctionnalités pour être en mesure de répondre aux demandes des clients d’une manière agile.
- Meilleure utilisation des ressources pour réduire les coûts.

Ces besoins de l’entreprise affectent *la manière* dont nous créons des applications.

Pour plus d’informations sur l’approche d’Azure à l’égard des microservices, voir [Microservices: An application revolution powered by the cloud](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) (Microservices : une approche révolutionnaire des applications reposant sur le cloud).

## Approches de conception monolithique et de microservices
Toutes les applications évoluent au fil du temps. Les applications réussies évoluent en étant utiles aux utilisateurs. Les applications ratées n’évoluent pas et sont, en fin de compte, déconseillées. La question est alors : « Que savez-vous de vos besoins aujourd’hui et comment pensez-vous qu’ils évolueront à l’avenir ? » Par exemple, si vous créez une application de création de rapports pour un service et que vous êtes sûr qu’elle restera dans la portée de votre entreprise et que les rapports auront une durée de vie courte, votre choix de l’approche sera différent, par exemple, de la création d’un service permettant de distribuer du contenu vidéo à des dizaines de millions de clients. Parfois, la sortie d’un élément en tant que preuve de concept constitue un facteur déterminant lorsque l’on sait que l’application peut être repensée ultérieurement. Il n’est pas très pertinent de lancer une ingénierie complexe pour quelque chose qui ne sera jamais utilisé. C’est un compromis habituel en ingénierie. Par ailleurs, lorsque des entreprises parlent de mettre en place le cloud, leurs attentes concernent la croissance et l’utilisation. Le problème, c’est que la croissance et l’évolutivité sont imprévisibles. Nous souhaitons pouvoir créer des prototypes rapidement, mais, dans le même temps, savoir que nous sommes sur la voie du succès. C’est l’approche d’une lean startup : créer, mesurer, apprendre, itérer.

À l’ère client-serveur, nous avions tendance à nous concentrer sur la création d’applications hiérarchisées à l’aide de technologies spécifiques dans chaque niveau. Le qualificatif « monolithique » a vu le jour pour ces applications. Les interfaces avaient tendance à se trouver entre les niveaux, et une conception plus étroitement couplée était généralement utilisée entre les composants de chaque niveau. Les développeurs concevaient et affacturaient avec des classes compilées dans des bibliothèques et liaient celles-ci ensemble dans quelques fichiers .exe et .dll. Une telle approche de conception monolithique présente des avantages. La conception peut se révéler plus simple, et les appels entre les composants plus rapides dans la mesure où ils sont souvent effectués via IPC (communication entre processus). Chacun teste également un produit unique, ce qui est plus efficace en termes de ressources de personnel. Les inconvénients sont les suivants : l’application est fortement couplée dans les couches hiérarchisées et vous ne pouvez pas mettre à l’échelle des composants individuels. Si vous devez effectuer des correctifs ou des mises à niveau, vous devez attendre que les autres terminent leurs tests, et il devient plus difficile de faire preuve d’agilité.

Les microservices gèrent ces inconvénients et s’alignent plus étroitement sur les exigences métier décrites ci-dessus, mais ils présentent également à la fois des avantages et des inconvénients. Avantages des microservices : en général, chacun encapsule des fonctionnalités métier plus simples, et ils peuvent être mis à l’échelle (à la hausse ou à la baisse), testés, déployés et gérés indépendamment. L’un des avantages importants d’une approche de microservices est que les équipes sont davantage orientées par les scénarios d’entreprise que par la technologie, ce qui est encouragé par l’approche hiérarchisée. Dans la pratique, cela signifie que les petites équipes développent un microservice en s’appuyant sur un scénario client, à l’aide des technologies de leur choix. En d’autres termes, l’organisation n’a pas besoin de standardiser la technologie pour maintenir les applications monolithiques. Par ailleurs, les équipes individuelles qui possèdent des services peuvent effectuer les actions les plus pertinentes pour elles en fonction de leur expertise ou de ce qui est mieux approprié pour le problème à résoudre. Dans la pratique, il est bien sûr préférable de disposer d’un ensemble de technologies recommandées, par exemple d’une banque NoSql particulière ou d’une infrastructure d’application web.

L’inconvénient des microservices tient à la gestion du nombre accru d’entités distinctes, à la gestion de déploiements plus complexes et au contrôle de version, dans un contexte de trafic réseau plus important entre les microservices, et de latences réseau correspondantes. Disposer d’un grand nombre de services très granulaires et bavards est la recette idéale pour des performances cauchemardesques. Sans les outils permettant d’afficher ces dépendances, il est difficile de « voir » le système dans son ensemble. En fin de compte, les normes permettent à l’approche de microservices de fonctionner, en définissant la façon de communiquer et en faisant preuve de tolérance à l’égard des seuls éléments dont vous avez besoin d’un service, au lieu d’adopter des contrats rigides. Il est important de définir ces contacts en amont dans la conception, car les services seront mis à jour indépendamment les uns des autres. Une autre description a été formulée pour la conception d’une approche de microservices : « la SOA affinée ».


***Dans sa forme la plus simple, l’approche de conception de microservices fait référence à une fédération de services découplée, avec des modifications indépendantes de chaque norme de communication.***


Tandis qu’un nombre croissant d’applications cloud est produit, de plus en plus de personnes découvrent que cette décomposition de l’application globale en services axés sur un scénario indépendant constitue une meilleure approche à long terme.

## Comparaison entre les approches de développement des applications

![Développement d’applications pour la plateforme Service Fabric][Image1]

1. Une application monolithique contient des fonctionnalités propres à un domaine. Elle est normalement divisée en couches fonctionnelles telles que web, métier et données.

2. Vous mettez à l’échelle une application monolithique en la clonant sur plusieurs serveurs/machines virtuelles/conteneurs.

3. Une application de microservices sépare les fonctionnalités en services plus petits distincts.

4. Cette approche monte en charge en déployant chaque service indépendamment, ce qui crée des instances de ces services sur les serveurs/machines virtuelles/conteneurs.


Concevoir avec une approche de microservices n’est pas la panacée pour tous les projets, mais cette vision se rapproche davantage des objectifs métier décrits précédemment. En outre, commencer par une approche monolithique est acceptable si vous savez que, plus tard, vous aurez la possibilité de retravailler le code en une conception de microservices si nécessaire. Le scénario le plus courant est le suivant : vous débutez avec une application monolithique que vous fractionnez lentement en étapes, en commençant par les zones fonctionnelles qui doivent être plus évolutives ou agiles.

Pour résumer, l’approche de microservices consiste à composer votre application de nombreux services plus petits exécutés dans des conteneurs déployés sur un cluster de machines. Chaque service est développé par une équipe réduite qui se concentre sur un scénario, et chaque service est testé indépendamment, géré en termes de version, déployé et mis à l’échelle afin que l’application dans son ensemble puisse évoluer.

## Que sont les microservices ?

Il existe différentes définitions des microservices ; une recherche sur Internet fournit de nombreuses ressources correctes qui proposent leurs propres points de vue et définitions. Néanmoins, la plupart des caractéristiques suivantes des microservices font l’objet d’un large consensus :

- Ils encapsulent un scénario client ou d’entreprise. Quel est le problème que vous cherchez à résoudre ?
- Ils sont développés par une petite équipe d’ingénierie.
- Ils peuvent être écrits dans tout langage de programmation et utiliser toute infrastructure.
- Ils se composent d’un code et éventuellement d’un état, indépendamment gérés en termes de version, déployés et mis à l’échelle.
- Ils interagissent avec les autres microservices sur des interfaces et protocoles bien définis.
- Ils possèdent des noms uniques (URL) qui peuvent être utilisés pour résoudre leur emplacement.
- Ils restent cohérents et disponibles en cas de défaillances.

Vous pouvez résumer ces éléments dans la définition suivante :

***Les applications de microservices sont composées de services d’envergure modeste, gérés indépendamment en termes de version et évolutifs en fonction des clients, qui communiquent entre eux sur des protocoles standard avec des interfaces bien définies.***


Nous avons abordé les deux premiers points ci-dessus dans la section précédente, et nous allons maintenant développer et clarifier les autres points.

### Ils peuvent être écrits dans tout langage de programmation et utiliser toute infrastructure
En tant que développeurs, nous devons être libres d’opter pour le langage ou l’infrastructure de notre choix, selon nos compétences ou les besoins du service. Dans certains services, les avantages en matière de performances du langage C++ sont peut-être encensés, tandis que dans d’autres, la simplicité du développement géré offerte par C# ou Java peut primer. Dans certains cas, vous devrez peut-être utiliser une bibliothèque tierce spécifique, une technologie de stockage de données ou un moyen d’exposer le service aux clients.

Une fois une technologie choisie, cela nous amène à la gestion du cycle de vie ou des opérations et à la mise à l’échelle du service.

### Le code et l’état peuvent être indépendamment gérés en termes de version, déployés et mis à l’échelle  

Toutefois, si vous choisissez d’écrire vos microservices, chacun doit être indépendamment déployé, mis à niveau et mis à l’échelle pour le code, et éventuellement pour l’état. Il s’agit en fait de l’un des problèmes les plus difficiles à résoudre dans la mesure où cela se résume à votre choix de technologies et, pour la mise à l’échelle, à comprendre comment partitionner le code et l’état. Lorsque le code et l’état utilisent des technologies distinctes (ce qui correspond à la tendance actuelle), les scripts de déploiement pour votre microservice doivent pouvoir faire face à la mise à l’échelle des deux. Cela concerne également l’agilité et la flexibilité afin que vous puissiez mettre à niveau certains des microservices sans avoir à les mettre à niveau tous en même temps. Revenons à la comparaison entre approche monolithique et approche de microservices pour un instant. Le schéma ci-dessous illustre les différences dans l’approche de stockage de l’état.

#### Stockage de l’état entre les styles d’application
![Stockage de l’état de la plateforme Service Fabric][Image2]

***Sur la gauche figure l’approche monolithique avec une base de données unique et des niveaux de technologies spécifiques.***

***Sur la droite figure l’approche des microservices, un graphique de microservices interconnectés où l’état est généralement limité au microservice, et où un éventail de technologies sont utilisées.***

Généralement, une approche monolithique compte une base de données utilisée par l’application. L’avantage est qu’il s’agit d’un seul emplacement, ce qui facilite le déploiement. Chaque composant peut avoir une seule table pour stocker son état. Le problème est que les équipes doivent être rigoureuses en séparant l’état et, inévitablement, il est tentant de se contenter d’ajouter une nouvelle colonne à une table des clients existante, de joindre des tables, et globalement, de créer des dépendances au niveau de la couche de stockage. Une fois cela effectué, vous ne pouvez pas mettre à l’échelle les composants individuels. Dans l’approche de microservices, chaque service gère et stocke son propre état, ce qui signifie qu’il est responsable de la mise à l’échelle à la fois du code et de l’état pour satisfaire aux demandes du service. L’inconvénient de cette approche apparaît lorsqu’il est nécessaire de créer des vues ou des requêtes relatives aux données de vos applications, car vous devez exécuter des requêtes dans des magasins d’état disparates. En règle générale, vous pouvez résoudre ce problème en disposant d’un microservice séparé qui génère une vue sur une collection de microservices. Si vous devez exécuter plusieurs requêtes ad hoc sur les données, chaque microservice doit envisager d’écrire ses données dans un service d’entreposage de données pour l’analyse hors connexion.


Le contrôle de version est propre à la version déployée d’un microservice. Il est requis pour que plusieurs versions différentes puissent être déployées et exécutées côte à côte. Le contrôle de version traite les scénarios où une version plus récente d’un microservice échoue au cours de la mise à niveau et doit être restaurée vers une version antérieure. L’autre scénario du contrôle de version se produit lorsque des tests de type A/B sont réalisés avec différents utilisateurs essayant différentes versions du service. Par exemple, il est courant de mettre à niveau un microservice pour un ensemble spécifique de clients afin qu’ils testent les nouvelles fonctionnalités avant de le déployer à plus grande échelle. Après la gestion du cycle de vie des microservices, nous arrivons à présent à la communication entre eux.


### Ils interagissent avec les autres microservices sur des interfaces et protocoles bien définis

Il n’y a pas grand-chose à dire sur ce sujet autre que recommander la lecture de la documentation abondante sur l’architecture orientée services publiée au cours des 10 dernières années, étant donné qu’une grande partie est destinée aux modèles de communication. En règle générale, ceci revient maintenant à utiliser une approche REST avec les protocoles HTTP et TCP, et les formats de sérialisation XML ou JSON. Du point de vue de l’interface, il s’agit d’adopter l’approche de conception web. Toutefois, rien ne vous empêche d’utiliser des protocoles binaires ou vos propres formats de données. Préparez-vous seulement à ce que les personnes rencontrent davantage de difficultés à utiliser vos microservices s’ils sont ouvertement disponibles.

### Ils possèdent un nom unique (URL) qui peut être utilisé pour résoudre leur emplacement

Vous n’avez pas oublié que nous répétons sans cesse que l’approche de microservices est similaire au web ? À l’instar du web, votre microservice doit être adressable partout où il est exécuté. Si vous pensez à des machines et à celles qui exécutent un microservice spécifique, les choses tourneront mal rapidement. De la même façon que le DNS résout une URL spécifique pour une machine spécifique, votre microservice doit avoir un nom unique pour découvrir son emplacement actuel. Les microservices ont besoin de noms adressables qui les rendent indépendants de l’infrastructure sur laquelle ils sont exécutés. Bien sûr, cela implique l’existence d’une interaction entre la façon dont votre service est déployé et la façon dont il est découvert dans la mesure où un Registre de service doit être disponible. De même, une interaction doit exister entre le moment où une défaillance de machine survient et ce qui se passe pour votre microservice, de sorte que le service de Registre puisse vous indiquer où il est exécuté actuellement. Ceci nous amène à la rubrique suivante sur la résilience et la cohérence.

### Ils restent cohérents et disponibles en cas de défaillances

La gestion des défaillances inattendues est l’un des problèmes les plus difficiles à résoudre, en particulier dans un système distribué. Une grande partie du code que nous écrivons en tant que développeurs consiste à gérer les exceptions, et c’est également le domaine dans lequel le plus de temps est consacré aux tests. Mais le problème est plus complexe que l’écriture de code pour gérer les défaillances. Que se passe-t-il lorsque la machine sur laquelle s’exécute le microservice échoue ? Non seulement, vous devez détecter cette défaillance du microservice (problème complexe en soi), mais des éléments sont également nécessaires au redémarrage de votre microservice. Un microservice doit résister aux défaillances et être doté de la capacité à redémarrer souvent sur une autre machine pour des raisons de disponibilité. Cela se rapporte également à l’état qui a été enregistré pour le compte du microservice : à partir d’où peut-il récupérer cet état et est-il capable de redémarrer correctement ? En d’autres termes, une résilience dans le calcul est nécessaire (par exemple, le processus est redémarré), ainsi qu’une résilience dans l’état ou les données (il n’y a eu aucune perte de données et les données restent cohérentes).

Les problèmes de résilience se compliquent au cours d’autres scénarios, par exemple lorsque des défaillances se produisent pendant une mise à niveau de l’application. En association avec le système de déploiement, le microservice ne doit pas seulement récupérer. Il doit également décider s’il peut continuer à avancer vers la version la plus récente, ou à la place restaurer une version précédente pour maintenir un état cohérent. Il faut prendre en compte les questions de savoir s’il y a suffisamment de machines disponibles pour continuer de progresser, et la manière de restaurer les versions précédentes du microservice. Pour prendre ces décisions, le microservice doit émettre des informations d’intégrité.

### Rapports d’intégrité et diagnostics

Cela peut paraître évident et pourtant la question est souvent négligée : il est essentiel qu’un microservice génère des rapports sur son intégrité et des diagnostics. Dans le cas contraire, les connaissances d’un point de vue opérationnel sont limitées. Le défi consiste à déterminer la corrélation des événements de diagnostic sur un ensemble de services indépendants, chacun journalisant indépendamment et gérant les variations d’horloges des machines afin de déterminer l’ordre des événements. De la même manière que vous interagissez avec un microservice selon des protocoles et formats de données convenus, un besoin de standardisation émerge quant à la manière de journaliser les événements d’intégrité et de diagnostic qui finissent dans un magasin d’événements que l’on peut interroger et afficher. Dans une approche de microservices, il est essentiel que les différentes équipes s’accordent sur un format de journalisation unique, étant donné qu’une approche cohérente d’affichage des événements de diagnostic dans l’application dans son ensemble est nécessaire.

L’intégrité diffère des diagnostics. L’intégrité fait référence aux rapports du microservice sur son état actuel de sorte que des actions puissent être prises. La plus évidente de ces actions est l’utilisation de mécanismes de mise à niveau et de déploiement pour garantir la disponibilité. Par exemple, un service peut être actuellement défectueux en raison d’un blocage de processus ou d’un redémarrage de machine, mais demeurer opérationnel. La dernière chose dont vous avez besoin est d’empirer les choses en effectuant une mise à niveau. La meilleure solution est de commencer par mener une enquête ou de laisser le temps au microservice de récupérer. Les événements d’intégrité d’un microservice nous permettent ainsi de prendre des décisions avisées et nous aident effectivement à créer des services de réparation spontanée.

## Service Fabric en tant que plateforme de microservices

Azure Service Fabric est né de la transition de Microsoft de la distribution de produits prêts à l’emploi, de style généralement monolithique, à la prestation de services. Service Fabric a été principalement piloté par l’expérience de création et d’utilisation de services d’envergure tels que les bases de données SQL Azure, DocumentDB et d’autres services Azure essentiels. Nous avons vu l’intégralité des besoins d’entreprise en ce qui concerne la mise à l’échelle, l’agilité, les équipes indépendantes, et nous avons permis à la plateforme d’évoluer au fil du temps à mesure que davantage de services l’adoptaient. Plus important, Service Fabric devait s’exécuter partout, pas uniquement dans Azure, mais également dans les déploiements Windows Server autonomes.

***Service Fabric est destiné à résoudre les problèmes complexes de conception et d’exécution d’un service, notamment les défaillances et les mises à niveau, ainsi que les problèmes d’utilisation efficace des ressources d’infrastructure, de façon à permettre aux équipes de résoudre les problèmes d’entreprise à l’aide d’une approche de microservices.***

Service Fabric fournit deux grands domaines pour vous aider à créer des applications avec une approche de microservices :

- Une plateforme composée d’un ensemble de services système qui prennent en charge les déploiements, les mises à niveau, la détection et le redémarrage des services ayant échoué, la détection de l’emplacement d’exécution des services, la gestion de l’état, la surveillance de l’intégrité, etc. Ces services système activent nombre des caractéristiques des microservices décrites ci-dessus.

-  Des API de programmation, ou infrastructures, pour vous aider à concevoir des applications en tant que microservices. Les API de programmation fournies sont appelées [des acteurs et des services fiables](service-fabric-choose-framework.md). Bien sûr, vous pouvez utiliser tout code de votre choix pour créer votre microservice, mais ces API simplifient votre travail et s’intègrent à la plateforme de manière plus approfondie. Ainsi, par exemple, vous pouvez obtenir des informations d’intégrité et des diagnostics, ou vous pouvez tirer parti de la haute disponibilité intégrée.

***Service Fabric est indifférent à la façon dont vous créez votre service, et vous pouvez utiliser la technologie de votre choix. Toutefois, il offre des API de programmation intégrées qui facilitent grandement la création de microservices.***

### Les microservices conviennent-ils à mon application ?

Peut-être. Nous avons constaté qu’étant donné que de plus en plus d’équipes Microsoft avaient pour consigne de créer en gardant le cloud à l’esprit pour des raisons liées aux entreprises, nombre d’entre elles avaient pris conscience des avantages d’une approche de microservices. Bing, par exemple, fait cela dans le domaine de la recherche depuis des années. Pour les autres équipes, c’était très nouveau. Elles ont trouvé qu’il y avait des problèmes complexes à résoudre, ce qui n’était pas leur point fort. C’est pourquoi Service Fabric est très populaire en tant que technologie pour créer des services.

L’objectif de Service Fabric consiste à réduire la complexité de la création d’applications avec une approche de microservices afin de vous éviter des refontes coûteuses. Commencez petit, mettez à l’échelle lorsque c’est nécessaire, déconseillez les services inutiles, ajoutez-en de nouveaux, évoluez avec l’utilisation des clients, voilà l’approche à adopter. Nous savons également qu’en réalité il existe de nombreux autres problèmes à résoudre pour que les microservices soient plus abordables pour la majorité des développeurs. Les conteneurs et le modèle de programmation Actor sont des exemples de petites étapes dans cette direction, et nous sommes persuadés que plusieurs innovations vont apparaître pour simplifier ce processus.  
<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Étapes suivantes

* Pour en savoir plus :
    * [Vue d'ensemble de Service Fabric](service-fabric-overview.md)
    * [Microservices: An application revolution powered by the cloud (Microservices : une approche révolutionnaire des applications reposant sur le cloud)](https://azure.microsoft.com/fr-FR/blog/microservices-an-application-revolution-powered-by-the-cloud/)


[Image1]: media/service-fabric-overview-microservices/monolithic-vs-micro.png
[Image2]: media/service-fabric-overview-microservices/statemonolithic-vs-micro.png

<!---HONumber=AcomDC_0713_2016-->