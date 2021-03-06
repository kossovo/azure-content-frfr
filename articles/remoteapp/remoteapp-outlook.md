<properties
    pageTitle="Utilisation d’Outlook dans Azure RemoteApp | Microsoft Azure" 
    description="Configurer et utiliser Outlook dans Azure RemoteApp | Microsoft Azure"
    services="remoteapp"
    documentationCenter=""
    authors="pavithir"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/20/2016"
    ms.author="elizapo" />

# Utilisation de Microsoft Outlook dans Azure RemoteApp

Azure RemoteApp prend en charge Microsoft Outlook O365. En savoir plus sur la façon dont [Office fonctionne dans Azure RemoteApp](remoteapp-officesubscription.md). Il existe quelques paramètres recommandés pour Outlook lors de l’utilisation dans Azure RemoteApp.

## Mode mis en cache
Le mode mis en cache est une configuration recommandée lorsque vous utilisez Outlook dans Azure RemoteApp. Lorsque vous configurez un compte Outlook 2013 pour utiliser le mode Exchange mis en cache, Outlook 2013 fonctionne à partir d’une copie locale de la boîte aux lettres Microsoft Exchange de l’utilisateur qui est stockée dans un fichier de données hors connexion (fichier OST) sur l’ordinateur de l’utilisateur, avec le carnet d’adresses en mode hors connexion (OAB). La boîte aux lettres mise en cache et le carnet d’adresses en mode hors connexion sont régulièrement mis à jour à partir du service O365. En savoir plus sur [les différences entre les modes mis en cache et en ligne](https://technet.microsoft.com/library/jj683103.aspx).

L’utilisateur peut sélectionner le **mode Exchange mis en cache** ou le **mode en ligne** lors de la configuration du compte ou en modifiant les paramètres du compte. Vous pouvez également déployer le mode de votre choix à l’aide de l’Outil de personnalisation Office (OPO) ou de la stratégie de groupe.

Lisez [des instructions étape par étape sur l’activation du mode mis en cache](https://technet.microsoft.com/library/c6f4cad9-c918-420e-bab3-8b49e1885034#proc).

## Search
Dans Azure RemoteApp, l’utilisation de la recherche dans Outlook présente des limites. Azure RemoteApp utilise des machines virtuelles mises en pool pour héberger les sessions utilisateur. L’indexation de la recherche dépend de l’ID de la machine, qui est différent pour les différentes machines virtuelles. Il est possible que chaque fois qu’un utilisateur se connecte à Azure RemoteApp, il soit dirigé vers une nouvelle machine virtuelle. Cela signifierait que, si nous activons la recherche locale, l’indexeur sera exécuté chaque fois que l’ID de machine change (lorsque l’utilisateur est sur une machine virtuelle différente). Selon la taille du fichier .OST, l’indexeur peut prendre beaucoup de temps pour terminer et épuiser les ressources nécessaires aux autres applications. La recherche ne serait pas seulement ralentie, mais elle ne renverrait pas de résultats. L’utilisation d’un profil de compte Mode en ligne permettrait de contourner ce problème, mais compromettrait les performances globales en raison de l’absence d’un cache local (pour plus d’informations sur les différences entre le mode mis en cache et le mode en ligne, voir le lien ci-dessus). Malheureusement, la recherche indexée/locale ne peut pas être désactivée et la recherche en ligne ne peut pas être activée par défaut dans Outlook 2013.

Outlook 2016 propose une solution pour contrer ce problème en mode mis en cache, en offrant une nouvelle expérience de recherche de service pour les boîtes aux lettres hébergées sur Exchange 2016 (ou dans Office 365). Cette solution utilise les résultats de la recherche de service sur le cache local (OST). Outlook peut revenir à l’utilisation de l’indexeur de recherche local dans certains scénarios, mais la plupart des recherches utiliseront cette nouvelle fonctionnalité de recherche de service. Recommandation d’Azure RemoteApp : utiliser Outlook 2016 si la recherche d’e-mails est un scénario très important.

<!---HONumber=AcomDC_0727_2016-->