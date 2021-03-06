<properties
	pageTitle="Création et téléchargement d'un VHD Linux | Microsoft Azure"
	description="Création et téléchargement d’un disque dur virtuel (VHD) Azure avec le modèle de déploiement classique contenant le système d'exploitation Linux."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="iainfoulds"
	manager="timlt"
	editor="tysonn"
	tags="azure-service-management"/>

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/14/2016"
	ms.author="iainfou"/>

# Création et téléchargement d'un disque dur virtuel contenant le système d'exploitation Linux

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Cet article vous montre comment créer et télécharger un disque dur virtuel pour que vous puissiez l'utiliser comme image personnelle pour créer des machines virtuelles dans Azure. Vous apprendrez comment préparer le système d'exploitation pour créer plusieurs machines virtuelles basées sur cette image.

**Important** : l'accord de niveau de service de la plateforme Azure s'applique aux machines virtuelles exécutant le système d'exploitation Linux uniquement lorsqu'une des distributions reconnues est utilisée avec les détails de configuration définis sous « Versions prises en charge » dans [Linux dans les distributions approuvées par Azure](virtual-machines-linux-endorsed-distros.md). Toutes les distributions Linux de la galerie d'images Azure sont des distributions reconnues répondant à la configuration requise.


## Conditions préalables
Cet article part du principe que vous disposez des éléments suivants :

- **Un système d’exploitation Linux installé dans un fichier .vhd** : vous avez installé une [distribution Linux approuvée par Azure](virtual-machines-linux-endorsed-distros.md) (ou consultez les [informations relatives aux distributions non approuvées](virtual-machines-linux-create-upload-generic.md)) sur un disque virtuel au format de disque dur virtuel (VHD). Il existe de nombreux outils de création de fichiers .vhd. Par exemple, vous pouvez utiliser une solution de virtualisation telle que Hyper-V pour créer le fichier .vhd et installer le système d’exploitation. Pour obtenir des instructions, consultez la page [Installation du rôle Hyper-V et configuration d'une machine virtuelle](http://technet.microsoft.com/library/hh846766.aspx).

	> [AZURE.NOTE] Azure ne prend pas en charge le nouveau format VHDX. Vous pouvez convertir un VHDX au format VHD à l’aide du Gestionnaire Hyper-V ou de l’applet de commande `Convert-VHD`. De plus, Azure ne prend pas en charge le chargement de disques durs virtuels dynamiques. Vous devez convertir ces disques en disques durs virtuels statiques avant le chargement. Vous pouvez utiliser des outils tels que [les utilitaires de disque dur virtuel Azure pour GO](https://github.com/Microsoft/azure-vhd-utils-for-go) pour convertir les disques dynamiques.

- **Interface de ligne de commande Azure** : installez [l’interface de ligne de commande Azure](../virtual-machines-command-line-tools.md) la plus récente pour télécharger le disque dur virtuel.

<a id="prepimage"> </a>
## Étape 1 : préparation de l'image pour le téléchargement

Azure prend en charge diverses distributions de Linux (voir [Distributions Linux approuvées](virtual-machines-linux-endorsed-distros.md)). Les articles suivants vous montrent comment préparer les diverses distributions de Linux prises en charge dans Azure. Après avoir suivi toutes les étapes indiquées dans les guides suivants, revenez ici et vous devriez disposer d’un fichier VHD prêt pour le téléchargement dans Azure :

- **[Distributions CentOS](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Red Hat Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES et openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Autres - Distributions non approuvées](virtual-machines-linux-create-upload-generic.md)**

Consultez également les **[Notes d’installation Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)** pour obtenir d’autres conseils généraux sur la préparation d’images Linux pour Azure.


<a id="connect"> </a>
## Étape 2 : préparation de la connexion à Azure

Assurez-vous que vous utilisez l’interface de ligne de commande Azure dans le modèle de déploiement Classic (`azure config mode asm`), puis connectez-vous à votre compte :

```
azure login
```


<a id="upload"> </a>
## Étape 3 : chargement de l’image dans Azure

Vous aurez besoin d’un compte de stockage pour charger votre fichier de disque dur virtuel. Vous pouvez en choisir un existant ou [en créer un](../storage/storage-create-storage-account.md).

Utilisez l’interface de ligne de commande Azure pour charger l’image, à l’aide de la commande suivante :

		azure vm image create <ImageName> --blob-url <BlobStorageURL>/<YourImagesFolder>/<VHDName> --os Linux <PathToVHDFile>

Dans l’exemple précédent :

- **BlobStorageURL** correspond à l’URL du compte de stockage que vous envisagez d’utiliser
- **YourImagesFolder** est le conteneur au sein du stockage d’objets blob dans lequel vous souhaitez stocker vos images
- **VHDName** est l’étiquette affichée dans le portail pour identifier le disque dur virtuel.
- **PathToVHDFile** est le chemin d’accès complet et le nom du fichier .vhd sur votre machine.

Pour plus d’informations, consultez [Informations de référence sur l’interface de ligne de commande Azure pour Azure Service Management](../virtual-machines-command-line-tools.md).

[Step 1: Prepare the image to be uploaded]: #prepimage
[Step 2: Prepare the connection to Azure]: #connect
[Step 3: Upload the image to Azure]: #upload

<!---HONumber=AcomDC_0629_2016-->