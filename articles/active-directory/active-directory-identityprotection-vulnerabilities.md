<properties
	pageTitle="Vulnérabilités détectées par Azure Active Directory Identity Protection | Microsoft Azure"
	description="Présentation des vulnérabilités détectées par Azure Active Directory Identity Protection."
	services="active-directory"
	keywords="azure active directory identity protection, cloud app discovery, gestion d’applications, sécurité, risque, niveau de risque, vulnérabilité, stratégie de sécurité"
	documentationCenter=""
	authors="markusvi"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/06/2016"
	ms.author="markvi"/>

# Vulnérabilités détectées par Azure Active Directory Identity Protection 

Les vulnérabilités sont des points faibles exploitables par un cybercriminel au sein de votre environnement. Nous vous recommandons de les résoudre afin d’améliorer la posture de sécurité de votre organisation et d’empêcher des personnes malveillantes d’en tirer parti. <br><br> ![vulnérabilités](./media/active-directory-identityprotection-vulnerabilities/101.png "vulnérabilités") <br>

Les sections suivantes fournissent une vue d’ensemble des vulnérabilités signalées par Identity Protection.

## Inscription à l’authentification multifacteur non configurée 

Cette vulnérabilité vous permet de contrôler le déploiement d’Azure Multi-Factor Authentication, service d’authentification multifacteur d’Azure, dans votre organisation.

L’authentification multifacteur d’Azure ajoute une deuxième couche de sécurité à l’authentification utilisateur. Elle contribue à sécuriser l’accès aux données et aux applications tout en répondant à la demande des utilisateurs souhaitant un processus d’authentification simple. Il fournit une authentification forte via diverses options de vérification simples : appel téléphonique, message texte, notification par application mobile ou code de vérification et jetons OATH tiers.

Nous vous recommandons d’exiger l’authentification multifacteur d’Azure pour les connexions des utilisateurs. L’authentification multifacteur joue un rôle clé dans les stratégies d’accès conditionnel en fonction des risques disponibles via Identity Protection.

Pour plus d’informations, consultez [Qu’est-ce qu’Azure Multi-Factor Authentication ?](../multi-factor-authentication/multi-factor-authentication.md)


## Applications cloud non gérées

Cette vulnérabilité vous permet d’identifier les applications cloud non gérées au sein de votre organisation.
 
Dans les entreprises modernes, les services informatiques n’ont souvent pas connaissance de toutes les applications cloud utilisées par les membres de l’organisation pour effectuer leur travail. Il est facile de comprendre pourquoi les administrateurs s’inquiètent d’un accès non autorisé aux données d'entreprise, de possibles fuites de données et autres risques de sécurité.

Nous vous recommandons de déployer Cloud App Discovery dans votre organisation pour détecter les applications cloud non gérées et de gérer ces applications à l’aide d’Azure Active Directory.

Pour plus d’informations, consultez l’article [Détection des applications cloud non gérées avec Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).



##Alertes de sécurité du service Privileged Identity Management

Cette vulnérabilité vous aide à détecter et à résoudre les alertes relatives aux identités privilégiées dans votre organisation.

Pour permettre aux utilisateurs d’effectuer des opérations privilégiées, les organisations doivent leur accorder un accès privilégié temporaire ou permanent à des ressources Azure AD, Azure ou Office 365 ou à d’autres applications SaaS. Chacun de ces utilisateurs privilégiés augmente la surface d’attaque de votre organisation. Cette vulnérabilité vous permet d’identifier les utilisateurs disposant d’un accès privilégié inutile et de prendre les mesures qui s’imposent pour réduire ou éliminer le risque associé.

Nous vous recommandons d’utiliser le service Azure AD Privileged Identity Management dans votre organisation pour gérer, contrôler et surveiller les identités privilégiées et leur accès aux ressources dans Azure AD et dans d’autres services en ligne Microsoft tels qu’Office 365 ou Microsoft Intune.

Pour plus d’informations, consultez l’article [Azure AD Privileged Identity Management](active-directory-privileged-identity-management-configure.md).



## Voir aussi

 - [Azure Active Directory Identity Protection](active-directory-identityprotection.md)

<!---HONumber=AcomDC_0608_2016-->