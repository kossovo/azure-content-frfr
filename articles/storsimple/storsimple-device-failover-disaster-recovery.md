<properties 
   pageTitle="Basculement et récupération d’urgence StorSimple | Microsoft Azure"
   description="Découvrez comment basculer votre appareil StorSimple vers lui-même, un autre appareil physique ou un appareil virtuel."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/31/2016"
   ms.author="alkohli" />

# Basculement et récupération d’urgence pour votre appareil StorSimple

## Vue d'ensemble

Ce didacticiel décrit les étapes nécessaires pour basculer un appareil StorSimple en cas d’urgence. Un basculement vous permet de migrer vos données à partir d’un appareil source dans le centre de données vers un autre appareil physique ou virtuel situé à un emplacement géographique identique ou différent.

Le basculement de l’appareil est orchestré via la fonctionnalité de récupération d’urgence et se lance à partir de la page **Appareils**. Cette page répertorie tous les appareils StorSimple connectés à votre service StorSimple Manager. Pour chaque appareil, le nom convivial, le statut, la capacité maximale et d’approvisionnement, le type et le modèle s’affichent.

![Page Appareils](./media/storsimple-device-failover-disaster-recovery/IC740972.png)

Les instructions de ce didacticiel concerne les appareils physiques et virtuels StorSimple pour toutes les versions du logiciel.



## Récupération d’urgence et basculement d’appareil

Dans un scénario de récupération d’urgence, l’appareil principal cesse de fonctionner. Dans ce cas, vous pouvez déplacer les données de cloud associées à l’appareil défaillant vers un autre appareil en utilisant l’appareil principal en tant que *source* et en spécifiant un autre appareil en tant que *cible*. Vous pouvez sélectionner un ou plusieurs conteneurs de volume à migrer vers l’appareil cible. Ce processus est appelé le *basculement*. Pendant le basculement, la propriété des conteneurs de volume de l’appareil source change et ceux-ci sont transférés vers l’appareil cible.

## Considérations relatives au basculement d’appareil

En cas de sinistre, vous pouvez choisir de basculer votre appareil StorSimple :

- vers un appareil physique ; 
- vers lui-même ;
- vers un appareil virtuel.

Pour tout basculement d’appareil, tenez compte des éléments suivants :

- Pour la récupération d’urgence, tous les volumes dans les conteneurs de volume doivent être hors connexion et les conteneurs de volume doivent être associés à un instantané de cloud. 
- Les appareils cibles disponibles pour la récupération d’urgence possèdent un espace suffisant pour accueillir les conteneurs de volume sélectionnés. 
- Les appareils connectés à votre service mais ne répondant pas aux critères d’espace ne seront pas disponibles en tant qu’appareils cibles.

#### Basculement de l'appareil entre les versions du logiciel

Un service StorSimple Manager dans un déploiement peut avoir plusieurs appareils physiques et virtuels, exécutant tous des versions différentes du logiciel. Selon la version du logiciel, les types de volumes sur les appareils peuvent également être différents. Par exemple, un appareil exécutant Update 2 ou une version ultérieure aurait des volumes épinglés localement à plusieurs niveaux (l'archivage étant un sous-ensemble des niveaux). Un appareil pré-Update 2 en revanche peut avoir des volumes à plusieurs niveaux et d'archivage.

Utilisez le tableau suivant pour déterminer si vous pouvez basculer vers un autre appareil exécutant une autre version logicielle et le comportement des types de volumes pendant la récupération d'urgence.

| Basculement à partir de | Autorisé pour un appareil physique | Autorisé pour un appareil virtuel |
|----------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------|
| Update 2 à pré-Update 1 (version 0.1, 0.2, 0.3) | Non | Non |
| Update 2 à Update 1 (1, 1.1, 1.2) | Oui <br></br>En cas d'utilisation de volumes épinglés localement ou à plusieurs niveaux ou un mélange des deux, les volumes sont toujours basculés sous la forme à plusieurs niveaux. | Oui <br></br>En cas d'utilisation de volumes épinglés localement, ceux-ci sont basculés sous la forme à plusieurs niveaux. |
| Update 2 à Update 2 (version ultérieure) | Oui <br></br>En cas d'utilisation de volumes épinglés localement ou à plusieurs niveaux ou un mélange des deux, les volumes sont toujours basculés sous le type de volume de départ ; à plusieurs niveaux pour à plusieurs niveaux et épinglés localement pour épinglés localement. | Oui <br></br>En cas d'utilisation de volumes épinglés localement, ceux-ci sont basculés sous la forme à plusieurs niveaux. |


#### Basculement partiel entre les versions du logiciel

Suivez ces instructions si vous envisagez d’effectuer un basculement partiel à l’aide d’un appareil source StorSimple exécutant la Mise à jour préliminaire 1 vers une cible exécutant la Mise à jour 1 ou une version ultérieure.


| Basculement partiel à partir de | Autorisé pour un appareil physique | Autorisé pour un appareil virtuel |
|----------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------|
|Mise à jour préliminaire 1 (Version 0.1, 0.2, 0.3) à la Mise à jour 1 ou version ultérieure | Oui, voici le meilleur conseil pratique. | Oui, voici le meilleur conseil pratique. |


>[AZURE.TIP] Une modification des métadonnées du cloud et du format des données dans la Mise à jour 1 et les versions ultérieures. Par conséquent, nous ne recommandons pas un basculement partiel de la Mise à jour préliminaire 1 à la Mise à jour 1 ou à une version ultérieure. Si vous devez effectuer un basculement partiel, nous vous recommandons de commencer par appliquer la Mise à jour 1 ou une version ultérieure sur les deux les appareils (source et cible), puis effectuez le basculement.

## Basculement vers un autre appareil physique

Procédez comme suit pour restaurer votre appareil vers un appareil physique.

1. Vérifiez que le conteneur de volume que vous souhaitez basculer est associé à des instantanés de cloud.

1. Sur la page **Appareils**, cliquez sur l’onglet **Conteneurs de volume**.

1. Sélectionnez un conteneur de volume que vous souhaitez basculer vers un autre appareil. Cliquez sur le conteneur de volume pour afficher la liste des volumes dans ce conteneur. Sélectionnez un volume et cliquez sur **Déconnecter** pour mettre le volume hors connexion. Répétez ce processus pour tous les volumes dans le conteneur de volume.

1. Répétez l’étape précédente pour tous les conteneurs de volume que vous souhaitez basculer vers un autre appareil.

1. Sur la page **Appareils**, cliquez sur **Basculement**.

1. Dans l’Assistant qui s’ouvre, sous **Choisir le conteneur de volume pour le basculement** :

	1. dans la liste des conteneurs de volume, sélectionnez les conteneurs de volume que vous souhaitez basculer.
	

		>[AZURE.NOTE] **Seuls les conteneurs de volumes associés à des instantanés cloud et des volumes hors connexion sont affichés.**

	1. Sous **Choisir un appareil cible** pour les volumes dans les conteneurs sélectionnés, sélectionnez un appareil cible dans la liste déroulante des appareils disponibles. Seuls les appareils possédant la capacité disponible sont affichés dans la liste déroulante.

	1. Enfin, passez en revue tous les paramètres de basculement sous **Confirmer le basculement**. Cliquez sur l’icône en forme de coche ![Icône en forme de coche](./media/storsimple-device-failover-disaster-recovery/IC740895.png).

1. Un travail de basculement est créé, qui peut être contrôlé via la page **Travaux**. Si le conteneur de volumes sur lequel vous avez basculé possède des volumes locaux, vous verrez les travaux de restauration individuels pour chaque volume local (et non pour les volumes à plusieurs niveaux) dans le conteneur. Ces tâches de restauration peuvent prendre un certain temps. Il est probable que le travail de basculement se termine plus tôt. Notez que ces volumes auront des garanties locales uniquement une fois les travaux de restauration terminés. Une fois le basculement terminé, accédez à la page **Appareils**.

	1. Sélectionnez l’appareil qui a été utilisé en tant qu’appareil cible pour le processus de basculement.

	1. Accédez à la page **Conteneurs de volumes**. Tous les conteneurs de volume, ainsi que les volumes de l’ancien appareil, doivent être répertoriés.

## Basculement à l’aide d’un seul appareil

Procédez comme suit si vous disposez d’un seul appareil et devez effectuer un basculement.

1. Prenez des instantanés de cloud de tous les volumes de votre appareil.

1. Réinitialisez votre appareil aux valeurs par défaut. Suivez les instructions détaillées dans la section [Rétablissement des paramètres par défaut de l’appareil](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).

1. Configurez votre appareil et réenregistrez-le avec votre service StorSimple Manager.

1. Sur la page **Appareils**, l’ancien appareil doit s’afficher en **Hors connexion**. L’appareil récemment enregistré doit s’afficher en tant que **En ligne**.

1. Effectuez tout d’abord la configuration minimale du nouvel appareil.
												
	>[AZURE.IMPORTANT] **Si vous n’effectuez pas cette action en premier, la récupération d’urgence échoue en raison d’un bogue dans l’implémentation actuelle. Ce problème sera résolu dans une version ultérieure.**

1. Sélectionnez l’ancien appareil (état hors connexion) et cliquez sur **Basculement**. Dans l’Assistant qui s’affiche, basculez cet appareil et spécifiez l’appareil cible en tant qu’appareil nouvellement inscrit. Pour obtenir des instructions détaillées, reportez-vous à la section [Basculer vers un autre appareil physique](#fail-over-to-another-physical-device).

1. Un travail de restauration de l’appareil sera créé ; vous pouvez le surveiller à partir de la page **Travaux**.

1. Une fois la tâche terminée, accédez au nouvel appareil et à la page **Conteneurs de volume**. Tous les conteneurs de volume de l’ancien appareil doivent désormais être migrés vers le nouvel appareil.

## Basculement vers un appareil virtuel StorSimple

Vous devez avoir créé et configuré un appareil virtuel StorSimple avant d’exécuter cette procédure. En cas d'exécution d'Update 2, pensez à utiliser un appareil virtuel 8020 pour la récupération d'urgence avec 64 To et le stockage Premium.
 
Procédez comme suit pour restaurer votre appareil vers un appareil virtuel StorSimple cible.

1. Vérifiez que le conteneur de volume que vous souhaitez basculer est associé à des instantanés de cloud.

1. Sur la page **Appareils**, cliquez sur l’onglet **Conteneurs de volume**.

1. Sélectionnez un conteneur de volume que vous souhaitez basculer vers un autre appareil. Cliquez sur le conteneur de volume pour afficher la liste des volumes dans ce conteneur. Sélectionnez un volume et cliquez sur **Déconnecter** pour mettre le volume hors connexion. Répétez ce processus pour tous les volumes dans le conteneur de volume.

1. Répétez l’étape précédente pour tous les conteneurs de volume que vous souhaitez basculer vers un autre appareil.

1. Sur la page **Appareils**, cliquez sur **Basculement**.

1. Dans l’Assistant qui s’ouvre, sous **Choisir le conteneur de volume pour le basculement**, procédez comme suit :
													
	a. dans la liste des conteneurs de volume, sélectionnez les conteneurs de volume que vous souhaitez basculer.

	>[AZURE.NOTE] **Seuls les conteneurs de volumes associés à des instantanés cloud et des volumes hors connexion sont affichés.**

	b. Sous **Choisir un périphérique cible pour les volumes dans les conteneurs sélectionnés**, sélectionnez un appareil virtuel StorSimple dans la liste déroulante des appareils disponibles. Seuls les appareils possédant la capacité suffisante sont affichés dans la liste déroulante.
	

1. Enfin, passez en revue tous les paramètres de basculement sous **Confirmer le basculement**. Cliquez sur l’icône en forme de coche ![Icône en forme de coche](./media/storsimple-device-failover-disaster-recovery/IC740895.png).

1. Une fois le basculement terminé, accédez à la page **Appareils**.
													
	a. Sélectionnez l’appareil virtuel StorSimple qui a été utilisé en tant qu’appareil cible pour le processus de basculement.
	
	b. Accédez à la page **Conteneurs de volumes**. Tous les conteneurs de volume, ainsi que les volumes de l’ancien appareil, doivent désormais être répertoriés.

![Vidéo disponible](./media/storsimple-device-failover-disaster-recovery/Video_icon.png) **Vidéo disponible**

Pour visionner une vidéo expliquant comment restaurer un appareil physique basculé vers un appareil virtuel dans le cloud, cliquez [ici](https://azure.microsoft.com/documentation/videos/storsimple-and-disaster-recovery/).

## Continuité d’activité et récupération d’urgence (Business Continuity Disaster Recovery - BCDR)

Un scénario de continuité d’activité et récupération d’urgence (BCDR) se produit lorsque l’ensemble du centre de données Azure cesse de fonctionner. Cela peut affecter votre service StorSimple Manager et les appareils StorSimple associés.

S’il existe des appareils StorSimple inscrits juste avant un incident, ces périphériques StorSimple devront peut-être subir une réinitialisation des paramètres. Après l’incident, le périphérique StorSimple s’affichera comme étant hors connexion. Le périphérique StorSimple doit être supprimé à partir du portail, et une réinitialisation des paramètres doit être effectuée, suivie d’une nouvelle inscription.


## Étapes suivantes

- Après avoir effectué un basculement, vous devrez peut-être [désactiver ou supprimer votre appareil StorSimple](storsimple-deactivate-and-delete-device.md).

- Pour plus d’informations sur l’utilisation du service StorSimple Manager, consultez [Utiliser le service StorSimple Manager pour gérer votre appareil StorSimple](storsimple-manager-service-administration.md).
 

<!---HONumber=AcomDC_0601_2016-->