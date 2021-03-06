<properties 
   pageTitle="Gérer les enregistrements de contrôle d’accès dans StorSimple | Microsoft Azure"
   description="Décrit comment utiliser les enregistrements de contrôle d’accès pour déterminer les hôtes qui peuvent se connecter à un volume sur l’appareil StorSimple."
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
   ms.date="05/24/2016"
   ms.author="alkohli" />

# Utiliser le service StorSimple Manager pour gérer les enregistrements de contrôle d’accès

## Vue d’ensemble

Les enregistrements de contrôle d’accès vous permettent de spécifier les hôtes qui peuvent se connecter à un volume sur l’appareil StorSimple. Les enregistrements de contrôle d’accès sont définis pour un volume spécifique et contiennent les noms complets iSCSI (IQN) des ordinateurs hôtes. Lorsqu’un hôte essaie de se connecter à un volume, l’appareil vérifie l’enregistrement de contrôle d’accès associé à ce volume pour le nom complet iSCSI (IQN) et s’il existe une correspondance, la connexion est établie. La section des enregistrements de contrôle d’accès de la page **Configurer** affiche tous les enregistrements de contrôle d’accès avec les noms complets iSCSI (IQN) des hôtes correspondants.

Le didacticiel décrit les tâches courantes liées aux enregistrements de contrôle d’accès :

- Ajouter un enregistrement de contrôle d’accès 
- Modifier un enregistrement de contrôle d’accès 
- Supprimer un enregistrement de contrôle d’accès 

> [AZURE.IMPORTANT] 
> 
> - Lorsque vous attribuez un enregistrement de contrôle d’accès à un volume, veillez à ce que plusieurs hôtes non cluster n’accèdent pas simultanément au volume, sans quoi celui-ci pourrait être endommagé. 
> - Lorsque vous supprimez un enregistrement de contrôle d’accès d’un volume, assurez-vous que l’hôte correspondant n’accède pas au volume, car la suppression pourrait entraîner une perturbation des opérations de lecture-écriture.

## Ajouter un enregistrement de contrôle d’accès

Utilisez la page **Configuration** du service StorSimple Manager pour ajouter des enregistrements de contrôle d’accès. En général, vous associez un enregistrement de contrôle d’accès à un volume.

Effectuez les opérations suivantes pour ajouter un enregistrement de contrôle d’accès.

#### Pour ajouter un enregistrement de contrôle d’accès

1. Dans la page d’accueil du service, sélectionnez votre service, double-cliquez sur son nom, puis cliquez sur l’onglet **Configurer**.

2. Dans le tableau sous **Enregistrements de contrôle d’accès**, attribuez un **Nom** à votre enregistrement de contrôle d’accès.

3. Sous **Nom d’initiateur iSCSI**, indiquez le nom IQN de votre hôte Windows. Pour obtenir le nom IQN de l’hôte Windows Server, procédez comme suit :

   - Démarrez l’initiateur Microsoft iSCSI sur l’hôte Windows.
   - Dans la fenêtre **Propriétés de l’initiateur iSCSI**, sous l’onglet **Configuration**, sélectionnez et copiez la chaîne affichée dans le champ **Nom de l’initiateur**.
   - Collez la chaîne du champ **Nom d’initiateur iSCSI** dans la table des enregistrements de contrôle d’accès du portail Azure Classic.

4. Cliquez sur **Enregistrer** pour sauvegarder l’enregistrement de contrôle d’accès nouvellement créé. La liste de la table est mise à jour pour refléter les modifications.

## Modifier un enregistrement de contrôle d’accès

Utilisez la page **Configurer** du portail Azure Classic pour modifier les enregistrements de contrôle d’accès.

> [AZURE.NOTE] Vous pouvez modifier uniquement les enregistrements de contrôle d’accès qui ne sont pas en cours d’utilisation. Pour modifier un enregistrement de contrôle d’accès associé à un volume en cours d’utilisation, vous devez d’abord placer le volume hors connexion.

Effectuez les opérations suivantes pour modifier un enregistrement de contrôle d’accès.

#### Pour modifier un enregistrement de contrôle d’accès

1. Dans la page d’accueil du service, sélectionnez votre service, double-cliquez sur son nom, puis cliquez sur l’onglet **Configurer**.

2. Dans la liste de la table des enregistrements de contrôle d’accès, pointez sur l’enregistrement que vous souhaitez modifier.

3. Attribuez un nouveau nom et/ou nom IQN à l’enregistrement de contrôle d’accès.

4. Cliquez sur **Enregistrer** pour sauvegarder l’enregistrement de contrôle d’accès modifié. La liste de la table est mise à jour pour refléter la modification.

## Supprimer un enregistrement de contrôle d’accès

Utilisez la page **Configurer** du portail Azure Classic pour supprimer les enregistrements de contrôle d’accès.

> [AZURE.NOTE] Vous pouvez uniquement supprimer les enregistrements de contrôle d’accès qui ne sont pas en cours d’utilisation. Pour supprimer un enregistrement de contrôle d’accès associé à un volume en cours d’utilisation, vous devez d’abord placer le volume hors connexion.

Pour supprimer un enregistrement de contrôle d’accès, procédez comme suit.

#### Pour supprimer un enregistrement de contrôle d’accès

1. Dans la page d’accueil du service, sélectionnez votre service, double-cliquez sur son nom, puis cliquez sur l’onglet **Configurer**.

2. Dans la liste de la table des enregistrements de contrôle d’accès, pointez sur l’enregistrement que vous souhaitez supprimer.

3. Une icône de suppression (**x**) apparaît dans la colonne la plus à droite, en regard de l’enregistrement de contrôle d’accès que vous sélectionnez. Cliquez sur l’icône **x** pour supprimer l’enregistrement de contrôle d’accès.

4. Lorsque vous êtes invité à confirmer la suppression, cliquez sur **Oui** pour continuer. La liste de la table est mise à jour pour refléter la suppression.

## Étapes suivantes

- En savoir plus sur la [gestion des volumes StorSimple](storsimple-manage-volumes.md).

- En savoir plus sur [l’utilisation du service StorSimple Manager pour gérer votre appareil StorSimple](storsimple-manager-service-administration.md).
 

<!---HONumber=AcomDC_0525_2016-->