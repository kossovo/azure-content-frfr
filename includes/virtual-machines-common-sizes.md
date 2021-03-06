
Pour connaître les limites générales des machines virtuelles Azure, consultez [Abonnement Azure et limites, quotas et contraintes du service](../articles/azure-subscription-service-limits.md).

Les tailles standard sont constituées de plusieurs séries : A, D, DS, F, Fs, G et GS. Voici des considérations quant à certaines de ces tailles :

*   Les machines virtuelles de la série D sont conçues pour exécuter des applications qui nécessitent une puissance de calcul et des performances de disque temporaire supérieures. Ces machines virtuelles se caractérisent par des processeurs plus rapides, un rapport mémoire-cœur plus élevé et un disque SSD pour le disque temporaire. Pour plus d’informations, voir l’annonce suivante sur le blog Azure : [Nouvelles tailles de machines virtuelles de la série D](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/) (en anglais).

*   La série Dv2, suite de la série D d’origine, comprend un processeur plus puissant. Le processeur de la série Dv2 est environ 35 % plus rapide que le processeur de la série D. Il est basé sur la dernière génération de processeur 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) et comporte la technologie 2.0 Intel Turbo Boost, et peut atteindre 3,1 GHz. La série Dv2 a les mêmes configurations de disque et de mémoire que la série D.

* La série F est basée sur le processeur Intel Xeon® E5-2673 v3 (Haswell) de 2,4 GHz pouvant aller jusqu’à 3,1 GHz avec Intel Turbo Boost Technology 2.0. Ce sont les mêmes performances d’UC que la série Dv2 de machines virtuelles. Affichant le coût le plus bas par heure, la série F offre le meilleur rapport prix-performances de la gamme Azure si l’on considère les unités de calcul Azure (ACU) par cœur.

	La série F introduit également un nouveau standard de nom de taille de machine virtuelle pour Azure. Pour les tailles de machines virtuelles de cette série et à venir, la valeur numérique après la lettre du nom de la gamme correspond au nombre de cœurs de processeur. Les fonctionnalités supplémentaires, notamment l’optimisation pour le stockage premium, seront désignées par les lettres suivant le nombre de cœurs de processeur. Ce format d’affectation de noms sera utilisé pour les tailles de machines virtuelles commercialisées à l’avenir mais ne changera pas rétroactivement le nom des tailles de machines virtuelles qui sont déjà commercialisées.


*   Les machines virtuelles de la série G offrent le plus de mémoire et s’exécutent sur des hôtes équipés de processeurs de la famille Intel Xeon E5 V3.


*   Les machines virtuelles des séries DS, DSv2 F et GS peuvent utiliser Premium Storage, qui offre un stockage hautes performances à faible latence pour les charges de travail utilisant une quantité importante d’E/S. Ces machines virtuelles utilisent des disques SSD pour héberger les disques de la machine virtuelle et offrent également un cache de disque SSD local. Le stockage Premium est disponible dans certaines régions. Pour plus d’informations, consultez l’article [Stockage Premium : stockage hautes performances pour les charges de travail des machines virtuelles Azure](../articles/storage/storage-premium-storage.md).


*   Les machines virtuelles de la série A peuvent être déployées sur différents types de matériels et processeurs. La taille est limitée, en fonction du matériel, pour offrir des performances de processeur cohérentes pour l’instance en cours d’exécution, quel que soit le matériel sur lequel elle est déployée. Pour déterminer le matériel physique sur lequel cette taille est déployée, interrogez le matériel virtuel à partir de la machine virtuelle.

*   La taille A0 est trop sollicitée sur le matériel physique. Pour cette taille spécifique uniquement, les autres déploiements de clients peuvent affecter les performances de la charge de travail en cours d’exécution. Les performances relatives sont décrites ci-dessous comme référence attendue, soumises à une variation approximative de 15 pour cent.


La taille de la machine virtuelle a une incidence sur la tarification. La taille influe également sur les capacités de traitement, de mémoire et de stockage de la machine virtuelle. Les coûts de stockage sont calculés séparément en fonction des pages utilisées dans le compte de stockage. Pour plus d’informations, voir les pages [Machines virtuelles Tarification](https://azure.microsoft.com/pricing/details/virtual-machines/) et [Tarification Azure Storage](https://azure.microsoft.com/pricing/details/storage/).


Les considérations ci-dessous peuvent vous aider à choisir une taille :


* Les tailles A8 à A11 sont également connues comme étant des *instances nécessitant beaucoup de ressources système*. Le matériel qui exécute ces tailles a été conçu et optimisé pour les applications nécessitant beaucoup de ressources système et réseau, notamment les applications en cluster pour des calculs complexes, la modélisation et les simulations. Pour plus d’informations et pour connaître les éléments à prendre en considération sur l’utilisation de ces tailles, consultez l’article [À propos des instances de calcul intensif A8, A9, A10 et A11](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md).


*	Les séries Dv2, D, G et DS/GS sont idéales pour les applications qui exigent des processeurs plus rapides, de meilleures performances de disque local, ou qui ont des exigences de mémoire plus élevées. Elles offrent une combinaison puissante pour de nombreuses applications professionnelles.

* Les machines virtuelles de série F sont un excellent choix pour les charges de travail qui exigent des processeurs plus rapides, mais n’ont pas besoin d’autant de mémoire ou de SSD local par cœur de processeur. Les charges de travail telles que l’analyse, les serveurs de jeux, les serveurs web et le traitement par lots tireront avantage de la série F.

*   Certains hôtes physiques des centres de données Azure ne prennent pas en charge les tailles de machines virtuelles élevées, comme A5 à A11. Ainsi, vous pouvez obtenir le message d’erreur **Échec de la configuration de la machine virtuelle <nom de la machine>** ou **Échec de la création de la machine virtuelle <nom de la machine>** pendant le redimensionnement d’une machine virtuelle existante, la création d’une machine virtuelle dans un réseau virtuel créé avant le 16 avril 2013 ou l’ajout d’une nouvelle machine virtuelle à un service cloud existant. Pour découvrir les solutions de contournement pour chaque scénario de déploiement, consultez [Erreur : « Échec de la configuration de la machine virtuelle »](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) (en anglais) sur le forum d'assistance.


## Considérations relatives aux performances

Nous avons créé le concept d’unité de calcul Azure (ACU) pour permettre de comparer les performances de calcul (UC) des références Azure. Cela vous aidera à identifier facilement la référence la plus à même de répondre à vos besoins en termes de performances. L’unité ACU est actuellement normalisée sur une machine virtuelle de petite taille (Standard\_A1) correspondant à 100, et toutes les autres références représentent ensuite approximativement la rapidité avec laquelle la référence en question peut exécuter un test d’évaluation.

>[AZURE.IMPORTANT] Cette unité ACU est fournie uniquement à titre indicatif. Les résultats de votre charge de travail peuvent varier.

<br>

|Famille de références |ACU/Cœur |
|---|---|
|[Standard\_A0](#a-series) |50 |
|[Standard\_A1-4](#a-series) |100 |
|[Standard\_A5-7](#a-series) |100 |
|[A8-A11](#a-series) |225*|
|[D1-14](#d-series) |160 |
|[D1-15v2](#dv2-series) |210 - 250*|
|[DS1-14](#ds-series) |160 |
|[DS1-15v2](#dsv2-series) |210-250* |
|[F1-F16](#f-series) | 210-250*|
|[F1s-F16s](#fs-series) | 210-250*|
|[G1-5](#g-series) |180 - 240*|
|[GS1-5](#gs-series) |180 - 240*|


Les unités ACU signalées par un astérisque (*) utilisent la technologie Intel ® Turbo pour augmenter la fréquence du processeur et améliorer les performances. Cette amélioration des performances peut varier en fonction de la taille et de la charge de travail de la machine virtuelle, et des autres charges de travail en cours d’exécution sur le même hôte.

## Tableaux des tailles

Les tableaux ci-après indiquent les tailles et les capacités qu’elles offrent.

* La capacité de stockage est représentée avec 1024^3 octets comme unité de mesure pour les Go. Cette unité est parfois appelée gibioctet ou définition en base 2. Quand vous comparez des tailles qui utilisent des systèmes en base différente, n’oubliez pas que les tailles en base 2 peuvent paraître plus petites que celles en base 10, mais que pour une taille spécifique (par exemple, 1 Go), un système en base 2 offre une capacité plus élevée qu’un système en base 10, car 1 024^3 est supérieur à 1 000^3.

* La bande passante réseau maximale est la bande passante maximale agrégée allouée et affectée par type de machine virtuelle. La bande passante maximale fournit des recommandations pour la sélection du bon type de machine virtuelle afin de garantir une capacité réseau adéquate. Lors du déplacement entre Faible, Modéré, Élevé et Très élevé, le débit augmente en conséquence. Les performances réseau réelles dépendent de nombreux facteurs, notamment les charges du réseau et de l’application, ainsi que les paramètres réseau de l’application.


## Série A

|Taille |Cœurs d’unité centrale|Mémoire|Cartes réseau (max)|Taille maximale du disque|Nombre maximal de disques de données (1 023 Go chacun)|Bande passante Nombre maximal d’opérations d’E/S par seconde (500 par disque)| Bande passante réseau maximale |
|---|---|---|---|---|---|---|---|
|Standard\_A0 |1|768 Mo|1| Temporaire = 20 Go |1|1 x 500| faible |
|Standard\_A1 |1|1,75 Go|1|Temporaire = 70 Go |2|2 x 500| Modéré |
|Standard\_A2 |2|3,5 Go|1|Temporaire = 135 Go |4|4 x 500| Modéré |
|Standard\_A3 |4|7 Go|2|Temporaire = 285 Go |8|8 x 500| élevé |
|Standard\_A4 |8|14 Go|4|Temporaire = 605 Go |16|16 x 500| élevé |
|Standard\_A5 |2|14 Go|1|Temporaire = 135 Go |4|4 x 500| Modéré |
|Standard\_A6 |4|28 Go|2|Temporaire = 285 Go |8|8 x 500| élevé |
|Standard\_A7 |8|56 Go|4|Temporaire = 605 Go |16|16 x 500| élevé |



## Série A - Instances de calcul intensif

Pour plus d’informations et pour connaître les éléments à prendre en compte pour l’utilisation de ces tailles, consultez la page [À propos des instances de calcul intensif A8, A9, A10 et A11](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md).

|Taille |Cœurs d’unité centrale|Mémoire|Cartes réseau (max)|Taille maximale du disque|Nombre maximal de disques de données (1 023 Go chacun)|Bande passante Nombre maximal d’opérations d’E/S par seconde (500 par disque)| Bande passante réseau maximale |
|---|---|---|---|---|---|---|---|
|Standard\_A8|8|56 Go|2| Temporaire = 382 Go |16|16 x 500| élevé |
|Standard\_A9|16|112 Go|4| Temporaire = 382 Go |16|16 x 500| très élevé |
|Standard\_A10|8|56 Go|2| Temporaire = 382 Go |16|16 x 500| élevé |
|Standard\_A11|16|112 Go|4| Temporaire = 382 Go |16|16 x 500| très élevé |

## Série D

|Taille |Cœurs d’unité centrale|Mémoire|Cartes réseau (max)|Taille maximale du disque|Nombre maximal de disques de données (1 023 Go chacun)|Bande passante Nombre maximal d’opérations d’E/S par seconde (500 par disque)| Bande passante réseau maximale |
|---|---|---|---|---|---|---|---|
|D1 standard |1|3,5 Go|1|Temporaire (SSD) = 50 Go |2|2 x 500| Modéré |
|D2 standard |2|7 Go|2|Temporaire (SSD) = 100 Go |4|4 x 500| élevé |
|D3 standard |4|14 Go|4|Temporaire (SSD) = 200 Go |8|8 x 500| élevé |
|D4 standard |8|28 Go|8|Temporaire (SSD) = 400 Go |16|16 x 500| élevé |
|D11 standard |2|14 Go|2|Temporaire (SSD) = 100 Go |4|4 x 500| élevé |
|D12 standard |4|28 Go|4|Temporaire (SSD) = 200 Go |8|8 x 500| élevé |
|D13 standard |8|56 Go|8|Temporaire (SSD) = 400 Go |16|16 x 500| élevé |
|D14 standard |16|112 Go|8|Temporaire (SSD) = 800 Go |32|32 x 500| très élevé |


## Série Dv2

|Taille |Cœurs d’unité centrale|Mémoire|Cartes réseau (max)|Taille maximale du disque|Nombre maximal de disques de données (1 023 Go chacun)|Bande passante Nombre maximal d’opérations d’E/S par seconde (500 par disque)| Bande passante réseau maximale |
|---|---|---|---|---|---|---|---|
|Standard\_D1\_v2 |1|3,5 Go|1|Temporaire (SSD) = 50 Go |2|2 x 500| Modéré |
|Standard\_D2\_v2 |2|7 Go|2|Temporaire (SSD) = 100 Go |4|4 x 500| élevé |
|Standard\_D3\_v2 |4|14 Go|4|Temporaire (SSD) = 200 Go |8|8 x 500| élevé |
|Standard\_D4\_v2 |8|28 Go|8|Temporaire (SSD) = 400 Go |16|16 x 500| élevé |
|Standard\_D5\_v2 |16|56 Go|8|Temporaire (SSD) = 800 Go |32|32 x 500| extrêmement élevé |
|Standard\_D11\_v2 |2|14 Go|2|Temporaire (SSD) = 100 Go |4|4 x 500| élevé |
|Standard\_D12\_v2 |4|28 Go|4|Temporaire (SSD) = 200 Go |8|8 x 500| élevé |
|Standard\_D13\_v2 |8|56 Go|8|Temporaire (SSD) = 400 Go |16|16 x 500| élevé |
|Standard\_D14\_v2 |16|112 Go|8|Temporaire (SSD) = 800 Go |32|32 x 500| extrêmement élevé |
|Standard\_D15\_v2 |20|140 Go|10|Temporaire (SSD) = 1 To |40|40 x 500| extrêmement élevé |


## Séries DS*

|Taille |Cœurs d’unité centrale|Mémoire|Cartes réseau (max)|Taille maximale du disque|Nombre maximal de disques de données (1 023 Go chacun)|Taille de cache (Go)|Nombre maximal d’opérations d’E/S par seconde du disque et bande passante| Bande passante réseau maximale |
|---|---|---|---|---|---|---|---|---|
|Standard\_DS1 |1|3,5|1|Disque SSD local = 7 Go |2|43| 3 200 32 Mo par seconde | Modéré |
|Standard\_DS2 |2|7|2|Disque SSD local = 14 Go |4|86| 6 400 64 Mo par seconde | élevé |
|Standard\_DS3 |4|14|4|Disque SSD local = 28 Go |8|172| 12 800 128 Mo par seconde | élevé |
|Standard\_DS4 |8|28|8|Disque SSD local = 56 Go |16|344| 25 600 256 Mo par seconde | élevé |
|Standard\_DS11 |2|14|2|Disque SSD local = 28 Go |4|72| 6 400 64 Mo par seconde | élevé |
|Standard\_DS12 |4|28|4|Disque SSD local = 56 Go |8|144| 12 800 128 Mo par seconde | élevé |
|Standard\_DS13 |8|56|8|Disque SSD local = 112 Go |16|288| 25 600 256 Mo par seconde | élevé |
|Standard\_DS14 |16|112|8|Disque SSD local = 224 Go |32|576| 51 200 512 Mo par seconde | très élevé |

* Le nombre maximal d’opérations d’entrée/sortie par seconde (IOPS) et le débit (bande passante) possibles avec une machine virtuelle de la série DS sont affectés par la taille du disque. Pour plus d’informations, consultez l’article [Stockage Premium : stockage hautes performances pour les charges de travail des machines virtuelles Azure](../articles/storage/storage-premium-storage.md).


## Séries DSv2*

|Taille |Cœurs d’unité centrale|Mémoire|Cartes réseau (max)|Taille maximale du disque|Nombre maximal de disques de données (1 023 Go chacun)|Taille de cache (Go)|Nombre maximal d’opérations d’E/S par seconde du disque et bande passante| Bande passante réseau maximale |
|---|---|---|---|---|---|---|---|---|
|Standard\_DS1\_v2 |1|3,5|1|Disque SSD local = 7 Go |2|43| 3 200 48 Mo par seconde | Modéré |
|Standard\_DS2\_v2 |2|7|2|Disque SSD local = 14 Go |4|86| 6 400 96 Mo par seconde | élevé |
|Standard\_DS3\_v2 |4|14|4|Disque SSD local = 28 Go |8|172| 12 800 192 Mo par seconde | élevé |
|Standard\_DS4\_v2 |8|28|8|Disque SSD local = 56 Go |16|344| 25 600 384 Mo par seconde | élevé |
|Standard\_DS5\_v2 |16|56|8|Disque SSD local = 112 Go |32|688| 51 200 768 Mo par seconde | extrêmement élevé |
|Standard\_DS11\_v2 |2|14|2|Disque SSD local = 28 Go |4|72| 6 400 96 Mo par seconde | élevé |
|Standard\_DS12\_v2 |4|28|4|Disque SSD local = 56 Go |8|144| 12 800 192 Mo par seconde | élevé |
|Standard\_DS13\_v2 |8|56|8|Disque SSD local = 112 Go |16|288| 25 600 384 Mo par seconde | élevé |
|Standard\_DS14\_v2 |16|112|8|Disque SSD local = 224 Go |32|576| 51 200 768 Mo par seconde | extrêmement élevé |
|Standard\_DS15\_v2 |20|140 Go|10|Disque SSD local = 280 Go |40| 720|64 000 960 Mo par seconde | extrêmement élevé |


* Le nombre maximal d’opérations d’entrée/sortie par seconde (IOPS) et le débit (bande passante) possibles avec une machine virtuelle de la série DS sont affectés par la taille du disque. Pour plus d’informations, consultez l’article [Stockage Premium : stockage hautes performances pour les charges de travail des machines virtuelles Azure](../articles/storage/storage-premium-storage.md).


## Série F


| Taille | Cœurs d’unité centrale | Mémoire | Cartes réseau (max) | Taille du disque | Nombre maximal de disques de données (1 023 Go chacun) | Nombre maximal d’opérations d’E/S par seconde (500 par disque) | Bande passante réseau maximale |
|--------------|-----------|--------|------------|-------------------------|--------------------------|--------------------------|-------------|
| Standard\_F1 | 1 | 2 Go | 1 | Temporaire (SSD) = 16 Go | 2 | 2 x 500 | Modéré |
| Standard\_F2 | 2 | 4 Go | 2 | Temporaire (SSD) = 32 Go | 4 | 4 x 500 | élevé |
| Standard\_F4 | 4 | 8 Go | 4 | Temporaire (SSD) = 64 Go | 8 | 8 x 500 | élevé |
| Standard\_F8 | 8 | 16 Go | 8 | Temporaire (SSD) = 128 Go | 16 | 16 x 500 | élevé |
| Standard\_F16 | 16 | 32 Go | 8 | Temporaire (SSD) = 256 Go | 32 | 32 x 500 | extrêmement élevé |



## Série Fs*

| Taille | Cœurs d’unité centrale | Mémoire | Cartes réseau (max) | Taille du disque | Nombre maximal de disques de données (1 023 Go chacun) | Taille de cache (Go) | Nombre maximal d’opérations d’E/S par seconde du disque et bande passante | Bande passante réseau maximale |
|---------------|-----------|--------|------------|------------------------|-----------|-----------|----------------------------|------------|
| Standard\_F1s | 1 | 2 | 1 | Disque SSD local = 4 Go | 2 | 12 | 3 200 48 Mo par seconde | Modéré |
| Standard\_F2s | 2 | 4 | 2 | Disque SSD local = 8 Go | 4 | 24 | 6 400 96 Mo par seconde | élevé |
| Standard\_F4s | 4 | 8 | 4 | Disque SSD local = 16 Go | 8 | 48 | 12 800 192 Mo par seconde | élevé |
| Standard\_F8s | 8 | 16 | 8 | Disque SSD local = 32 Go | 16 | 96 | 25 600 384 Mo par seconde | élevé |
| Standard\_F16s | 16 | 32 | 8 | Disque SSD local = 64 Go | 32 | 192 | 51 200 768 Mo par seconde | extrêmement élevé |



* Le nombre maximal d’opérations d’entrée/sortie par seconde (IOPS) et le débit (bande passante) possibles avec une machine virtuelle de la série Fs sont affectés par la taille du disque. Pour plus d’informations, consultez l’article [Stockage Premium : stockage hautes performances pour les charges de travail des machines virtuelles Azure](../articles/storage/storage-premium-storage.md).





## Série G

|Taille |Cœurs d’unité centrale|Mémoire|Cartes réseau (max)|Taille maximale du disque|Nombre maximal de disques de données (1 023 Go chacun)|Bande passante Nombre maximal d’opérations d’E/S par seconde (500 par disque)| Bande passante réseau maximale |
|---|---|---|---|---|---|---|---|
|Standard\_G1 |2|28 Go|1|Disque SSD local = 384 Go |4|4 x 500| élevé |
|Standard\_G2 |4|56 Go|2|Disque SSD local = 768 Go |8|8 x 500| élevé |
|Standard\_G3 |8|112 Go|4|Disque SSD local = 1 536 Go |16|16 x 500| très élevé | 
|Standard\_G4 |16|224 Go|8|Disque SSD local = 3 072 Go |32|32 x 500| extrêmement élevé |
|Standard\_G5 |32|448 Go|8|Disque SSD local = 6 144 Go |64| 64 x 500 | extrêmement élevé |




## Série GS

|Taille |Cœurs d’unité centrale|Mémoire|Cartes réseau (max)|Taille maximale du disque|Nombre maximal de disques de données (1 023 Go chacun)|Taille de cache (Go)|Nombre maximal d’opérations d’E/S par seconde du disque et bande passante| Bande passante réseau maximale |
|---|---|---|---|---|---|---|---|---|
|Standard\_GS1|2|28|1|Disque SSD local = 56 Go |4|264| 5 000 125 Mo par seconde | élevé |
|Standard\_GS2|4|56|2|Disque SSD local = 112 Go |8|528| 10 000 250 Mo par seconde | élevé | 
|Standard\_GS3|8|112|4|Disque SSD local = 224 Go |16|1 056| 20 000 500 Mo par seconde | très élevé |
|Standard\_GS4|16|224|8|Disque SSD local = 448 Go |32|2 112| 40 000 1 000 Mo par seconde | extrêmement élevé |
|Standard\_GS5|32|448|8|Disque SSD local = 896 Go |64|4 224| 80 000 2 000 Mo par seconde | extrêmement élevé |




## Remarques : Standard A0 - A4 à l’aide de l’interface de ligne de commande et de Powershell 


Dans le modèle de déploiement classique, certains noms de tailles de machines virtuelles sont légèrement différents dans Powershell et dans l’interface de ligne de commande :

* Standard\_A0 est ExtraSmall (Très petit)
* Standard\_A1 est Small (Petit)
* Standard\_A2 est Medium (Moyen)
* Standard\_A3 est Large (Grand)
* Standard\_A4 est ExtraLarge (Très grand)


## Étapes suivantes

- En savoir plus sur l’[abonnement Azure et les limites, quotas et contraintes des services](../articles/azure-subscription-service-limits.md).
- En savoir plus sur [à propos des instances de calcul intensif A8, A9, A10 et A11](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) pour les charges de travail telles que le calcul haute performance (HPC).

<!---HONumber=AcomDC_0720_2016-->