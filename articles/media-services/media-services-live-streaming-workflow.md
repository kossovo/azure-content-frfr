<properties 
	pageTitle="Diffusion vidéo en flux continu avec Azure Media Services" 
	description="Cette rubrique présente une vue d’ensemble des principaux composants impliqués dans la vidéo en flux continu." 
	services="media-services" 
	documentationCenter="" 
	authors="Juliako" 
	manager="erikre" 
	editor=""/>

<tags 
	ms.service="media-services" 
	ms.workload="media" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="06/22/2016"
	ms.author="juliako"/>


#Diffusion d’événements en direct en continu avec Azure Media Services

##Vue d'ensemble

Lorsque vous utilisez la vidéo en flux continu, les composants suivants sont généralement impliqués :

- Une caméra utilisée pour diffuser un événement.
- Un encodeur vidéo live qui convertit les signaux de la caméra en flux de données qui sont envoyés vers un service de vidéo en flux continu.

Éventuellement, plusieurs encodeurs live. Pour certains événements en direct critiques qui exigent une disponibilité et une qualité d’expérience très élevées, il est recommandé d’utiliser des encodeurs redondants en mode actif-actif pour obtenir un basculement transparent sans perte de données.
- Service de vidéo en flux continu qui vous permet d’effectuer les opérations suivantes :
- Recevoir du contenu en direct à l’aide de différents protocoles de diffusion de vidéo en flux continu (par exemple RTMP ou Smooth Streaming),
- Encoder votre flux en flux à débit adaptatif
- Afficher un aperçu de votre flux en direct
- Stocker le contenu reçu pour le diffuser ultérieurement (vidéo à la demande)
- Fournir le contenu via des protocoles de diffusion communs (par exemple, MPEG DASH, Smooth, TLS, HDS) directement à vos clients ou à un réseau de distribution de contenu (CDN) pour une distribution supplémentaire


**Microsoft Azure Media Services** (AMS) offre la possibilité de recevoir, d’encoder, d’afficher, de stocker et de distribuer votre contenu vidéo en flux continu.

Quand vous distribuez votre contenu aux clients, votre objectif est de distribuer une vidéo de haute qualité à divers appareils dans des conditions de réseau différentes. Pour prendre en charge les conditions de qualité et de réseau, utilisez des encodeurs dynamiques pour encoder votre flux dans un flux vidéo à débit binaire multiple (débit binaire adaptatif). Pour prendre en charge la diffusion en continu sur différents appareils, utilisez l’[empaquetage dynamique](media-services-dynamic-packaging-overview.md) Media Services pour empaqueter de manière dynamique votre flux dans différents protocoles. Media Services prend en charge la distribution des technologies de diffusion en continu à débit binaire adaptatif suivantes : HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH et HDS (pour licences Adobe PrimeTime/Access uniquement).

Dans Azure Media Sercices, les **canaux**, les **programmes** et le **point de terminaison de diffusion en continu** gèrent toutes les fonctionnalités vidéo en flux continu, notamment la réception, le formatage, le DVR, la sécurité, l’extensibilité et la redondance.

Un **canal** représente un pipeline de traitement du contenu vidéo en flux continu. Actuellement, un canal peut recevoir des flux d’entrée live de l’une des manières suivantes :


- Un encodeur live envoie un flux à vitesse de transmission unique vers le canal activé pour effectuer un encodage en direct avec Media Services dans l’un des formats suivants : RTP (MPEG-TS), RTMP ou Smooth Streaming (MP4 fragmenté). Le canal procède ensuite à l’encodage en temps réel du flux à débit binaire unique entrant en flux vidéo à débit binaire multiple (adaptatif). Lorsqu’il y est invité, Media Services fournit le flux aux clients.

- Un encodeur en direct local envoie au canal un paquet **RTMP** ou **Smooth Streaming** (MP4 fragmenté) à débit binaire multiple. Vous pouvez utiliser les encodeurs live suivants qui produisent un flux Smooth Streaming à débit binaire multiple : Elemental, Envivio, Cisco. Les encodeurs live suivants produisent un flux au format RTMP : Adobe Flash Live, Telestream Wirecast et transcodeurs Tricaster. Les flux reçus transitent par les **canaux** sans traitement supplémentaire. Votre encodeur live peut également envoyer un flux à débit binaire unique vers un canal qui n’est pas activé pour le codage en direct, mais ce n’est pas recommandé. Lorsqu’il y est invité, Media Services fournit le flux aux clients.


##Utilisation de canaux activés pour effectuer un encodage en temps réel avec Azure Media Services


Le schéma suivant illustre les principales parties de la plateforme AMS impliquées dans le flux de travail de vidéo en flux continu où un canal est activé pour effectuer un encodage live avec Media Services.

![Flux de travail en direct][live-overview1]

Pour plus d’informations, consultez [Utilisation de canaux activés pour effectuer un encodage en direct avec Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).


##Utilisation des canaux recevant un flux live à débit binaire multiple provenant d’encodeurs locaux


Le diagramme suivant présente les principaux composants de la plateforme AMS impliqués dans ce flux de travail de vidéo en flux continu.

![Flux de travail en direct][live-overview2]

Pour plus d’informations, consultez [Utilisation des canaux recevant un flux live à débit binaire multiple provenant d’encodeurs locaux](media-services-live-streaming-with-onprem-encoders.md).



##Parcours d’apprentissage de Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Fournir des commentaires

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##Rubriques connexes

[Concepts Azure Media Services](media-services-concepts.md)

[Spécification d’ingestion en direct au format MP4 fragmenté Azure Media Services](media-services-fmp4-live-ingest-overview.md)





[live-overview1]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png

[live-overview2]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png

<!---HONumber=AcomDC_0629_2016-->