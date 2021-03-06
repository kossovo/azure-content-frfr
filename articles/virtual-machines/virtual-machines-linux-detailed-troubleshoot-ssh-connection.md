<properties
	pageTitle="Dépannage détaillé de SSH pour une machine virtuelle Azure | Microsoft Azure"
	description="Étapes supplémentaires de dépannage détaillées des problèmes de connexion à une machine virtuelle Azure"
	keywords="refus de la connexion ssh,erreur ssh,ssh azure,échec de la connexion ssh"
	services="virtual-machines-linux"
	documentationCenter=""
	authors="iainfoulds"
	manager="timlt"
	editor=""
	tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="support-article"
	ms.date="06/16/2016"
	ms.author="iainfou"/>

# Étapes de dépannage détaillées pour SSH

Il existe de nombreuses raisons pour lesquelles le client SSH peut ne pas pouvoir accéder au service SSH sur la machine virtuelle. Si vous avez suivi les [étapes de dépannage générales pour SSH](virtual-machines-linux-troubleshoot-ssh-connection.md), vous devrez aussi résoudre le problème de connexion. Cet article vous guide tout au long des étapes de dépannage détaillées pour déterminer où la connexion SSH échoue et comment résoudre le problème.

## Commencer par les étapes préliminaires

La figure suivante montre les composants concernés.

![Diagramme qui affiche les composants d’un service SSH](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot1.png)

Les étapes suivantes vous aident à isoler la source du problème et à déterminer différentes solutions.

Tout d’abord, vérifiez l’état de la machine virtuelle sur le portail.

Dans le [portail Azure Classic](https://manage.windowsazure.com), pour les machines virtuelles créées à l’aide du modèle de déploiement Classic :

1. Cliquez sur **Machines virtuelles** > *Nom de la machine virtuelle*.
2. Sélectionnez le **tableau de bord** de la machine virtuelle pour vérifier son état.
3. Sélectionnez **Surveiller** pour afficher l’activité récente des ressources de calcul, de stockage et réseau.
4. Sélectionnez **Points de terminaison** pour vérifier qu’il existe un point de terminaison pour le trafic SSH.

Dans le [portail Azure](https://portal.azure.com) :

1. Pour les machines virtuelles créées avec le modèle de déploiement Classic, sélectionnez **Parcourir** > **Machines virtuelles (classiques)** > *Nom de la machine virtuelle*.

	OU

	Pour les machines virtuelles créées avec le modèle de déploiement Resource Manager, sélectionnez **Parcourir** > **Machines virtuelles** > *Nom de la machine virtuelle*.

	Le volet d’état de la machine virtuelle doit afficher **En cours d’exécution**. Faites défiler vers le bas pour voir l’activité récente des ressources de calcul, de stockage et réseau.

2. Sélectionnez **Paramètres** pour examiner les points de terminaison, les adresses IP et les autres paramètres.

	Pour identifier les points de terminaison dans les machines virtuelles créées à l’aide de Resource Manager, vérifiez qu’un [groupe de sécurité réseau](../virtual-network/virtual-networks-nsg.md) a été défini. Vérifiez également que les règles ont été appliquées au groupe de sécurité réseau et qu’elles sont référencées dans le sous-réseau.

Pour vérifier la connectivité réseau, contrôlez les points de terminaison configurés et déterminez si vous pouvez atteindre la machine virtuelle par le biais d’un autre protocole, comme HTTP ou un autre service.

Une fois ces étapes effectuées, essayez à nouveau la connexion SSH.


## Découvrir la source du problème

Le client SSH sur votre ordinateur n’a peut-être pas pu accéder au service SSH sur la machine virtuelle Azure en raison de problèmes ou de mauvaises configurations :

- Ordinateur client SSH
- Appareil du périmètre de l’organisation
- point de terminaison de service cloud et liste de contrôle d’accès (ACL) ;
- groupes de sécurité réseau ;
- Machine virtuelle Linux Azure

## Source 1 : ordinateur client SSH

Pour vérifier que votre ordinateur n’est pas la source du problème, vérifiez qu’il peut établir des connexions SSH avec un autre ordinateur Linux local.

![Diagramme qui indique les composants de l’ordinateur client SSH](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot2.png)

Si cette opération échoue, recherchez sur votre ordinateur :

- un paramètre de pare-feu local qui bloque le trafic SSH entrant ou sortant (TCP 22) ;
- un logiciel de proxy client installé localement qui empêche les connexions SSH ;
- un logiciel de surveillance réseau installé localement qui empêche les connexions SSH ;
- d’autres types de logiciels de sécurité qui surveillent le trafic ou autorisent/interdisent des types spécifiques de trafic.

Si une de ces conditions s’applique, désactivez temporairement le logiciel et tentez d’établir une connexion SSH à un ordinateur local pour connaître le motif du blocage de la connexion sur votre ordinateur. Ensuite, contactez votre administrateur réseau pour corriger les paramètres du logiciel et autoriser les connexions SSH.

Si vous utilisez l’authentification par certificat, vérifiez que vous avez ces autorisations sur le dossier .ssh dans votre répertoire de base :

- Chmod 700 ~/.ssh
- Chmod 644 ~/.ssh/*.pub
- Chmod 600 ~/.ssh/id\_rsa (ou tout autre fichier contenant vos clés privées)
- Chmod 644 ~/.ssh/known\_hosts (contient les hôtes auxquels vous vous êtes connecté via SSH)

## Source 2 : appareil du périmètre de l’organisation

Pour vérifier que votre appareil de périmètre de l’organisation n’est pas la cause du problème, vérifiez qu’un ordinateur directement connecté à Internet peut établir des connexions SSH à votre machine virtuelle Azure. Si vous accédez à la machine virtuelle via un VPN de site à site ou une connexion Azure ExpressRoute, passez à [Source 4 : groupes de sécurité réseau](#nsg).

![Diagramme qui met en évidence un appareil du périmètre de l’organisation](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot3.png)

Si votre ordinateur n’est pas directement connecté à Internet, vous pouvez facilement créer une machine virtuelle Azure dans son propre groupe de ressources ou service cloud, et l’utiliser. Pour plus d’informations, consultez [Créer une machine virtuelle exécutant Linux dans Azure](virtual-machines-linux-quick-create-cli.md). Une fois le test terminé, supprimez le groupe de ressources ou la machine virtuelle et le service cloud.

Si vous pouvez créer une connexion SSH avec un ordinateur directement connecté à Internet, vérifiez sur l’appareil de périmètre de l’organisation :

- un pare-feu interne qui bloque le trafic SSH avec Internet ;
- un serveur proxy qui empêche les connexions SSH ;
- un logiciel de détection d’intrusion ou de surveillance réseau s’exécutant sur les appareils de votre réseau de périmètre qui empêche les connexions SSH.

Contactez votre administrateur réseau pour corriger les paramètres de vos appareils du périmètre de l’organisation pour permettre le trafic SSH avec Internet.

## Source 3 : Point de terminaison de service cloud et liste de contrôle d’accès

> [AZURE.NOTE] Cette source ne s’applique qu’aux machines virtuelles créées à l’aide du modèle de déploiement Classic. Pour les machines virtuelles créées à l’aide de Resource Manager, passez à [Source 4 : groupes de sécurité réseau](#nsg).

Pour éliminer le point de terminaison du service cloud et la liste de contrôle d’accès comme source du problème, vérifiez qu’une autre machine virtuelle Azure dans le même réseau virtuel peut établir des connexions SSH à votre machine virtuelle.

![Diagramme qui met en évidence un point de terminaison de service cloud et une liste de contrôle d’accès](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot4.png)

Si le réseau virtuel ne contient pas une autre machine virtuelle, vous pouvez facilement en créer une. Pour plus d’informations, consultez [Création d’une machine virtuelle Linux sur Azure à l’aide de l’interface de ligne de commande (CLI)](virtual-machines-linux-quick-create-cli.md). Une fois le test terminé, supprimez la machine virtuelle supplémentaire.

Si vous pouvez créer une connexion SSH avec une machine virtuelle dans le même réseau virtuel, vérifiez :

- **la configuration du point de terminaison pour le trafic SSH sur la machine virtuelle cible ;** le port TCP privé du point de terminaison doit correspondre au port TCP écouté par le service SSH de la machine virtuelle. (Par défaut, il s’agit du port 22.) Pour les machines virtuelles créées à l’aide du modèle de déploiement Resource Manager, vérifiez le numéro de port TCP SSH dans le portail Azure en sélectionnant **Parcourir** > **Machines virtuelles (v2)** > *Nom de la machine virtuelle* > **Paramètres** > **Points de terminaison**.

- **La liste de contrôle d’accès du point de terminaison du trafic SSH sur la machine virtuelle cible.** Une liste de contrôle d’accès vous permet de spécifier le trafic Internet entrant autorisé ou interdit, en fonction de l’adresse IP source. Une mauvaise configuration des listes de contrôle d’accès peut empêcher le trafic SSH entrant d’accéder au point de terminaison. Consultez vos listes de contrôle d’accès et vérifiez que le trafic entrant provenant des adresses IP publiques de votre proxy ou d’un autre serveur de périmètre est autorisé. Pour plus d'informations, consultez [À propos des listes de contrôle d'accès (ACL) réseau](../virtual-network/virtual-networks-acl.md).

Pour vérifier que le point de terminaison n’est pas la source du problème, supprimez le point de terminaison actuel, créez-en un autre et spécifiez le nom SSH (port TCP 22 comme port public et privé). Pour plus d’informations, consultez [Configuration des points de terminaison sur une machine virtuelle dans Azure](virtual-machines-windows-classic-setup-endpoints.md).

<a id="nsg"></a>
## Source 4 : groupes de sécurité réseau

Les groupes de sécurité réseau vous permettent de mieux contrôler le trafic entrant et sortant autorisé. Vous pouvez créer des règles qui s’étendent aux sous-réseaux et aux services cloud d’un réseau virtuel Azure. Vérifiez les règles de votre groupe de sécurité réseau pour vous assurer que le trafic SSH vers et depuis Internet est autorisé. Pour plus d'informations, consultez [À propos des groupes de sécurité réseau](../virtual-network/virtual-networks-nsg.md).

## Source 5 : machine virtuelle Azure Linux

La dernière source des problèmes possibles est la machine virtuelle Azure elle-même.

![Diagramme qui met en évidence une machine virtuelle Azure Linux](./media/virtual-machines-linux-detailed-troubleshoot-ssh-connection/ssh-tshoot5.png)

Si ce n’est déjà fait, suivez les instructions permettant de [réinitialiser un mot de passe ou SSH pour les machines virtuelles basées sur Linux](virtual-machines-linux-classic-reset-access.md).

Essayez une nouvelle fois de vous connecter à partir de votre ordinateur. Si l’échec se reproduit, l’une des raisons suivantes en est peut-être la cause :

- Le service SSH n’est pas en cours d’exécution sur la machine virtuelle cible.
- Le service SSH n’est pas à l’écoute sur le port TCP 22. Pour tester cela, installez un client telnet sur votre ordinateur local et exécutez « telnet *NomServiceCloud*.cloudapp.net 22 ». Ceci permet de déterminer si la machine virtuelle autorise les communications entrantes et sortantes avec le point de terminaison SSH.
- Le pare-feu local sur la machine virtuelle cible a des règles qui empêchent le trafic SSH entrant ou sortant.
- Un logiciel de détection d’intrusion ou de surveillance réseau s’exécutant sur la machine virtuelle Azure empêche les connexions SSH.


## Ressources supplémentaires
Pour plus d’informations sur la résolution des problèmes d’accès aux applications, consultez la page [Résolution des problèmes d’accès à une application exécutée sur une machine virtuelle Azure](virtual-machines-linux-troubleshoot-app-connection.md)

<!---HONumber=AcomDC_0622_2016-->