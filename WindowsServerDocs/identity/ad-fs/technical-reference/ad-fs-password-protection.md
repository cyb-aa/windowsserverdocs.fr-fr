---
title: Protection contre les attaques de mot de passe FS de AD
description: Ce document décrit comment protéger les utilisateurs AD FS contre les attaques de mot de passe
author: billmath
manager: mtillman
ms.reviewer: andandyMSFT
ms.date: 11/15/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: de1af9712b54c977c591953c68eec506c80d3cdd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822000"
---
# <a name="ad-fs-password-attack-protection"></a>Protection contre les attaques de mot de passe FS de AD

## <a name="what-is-a-password-attack"></a>Qu’est une attaque de mot de passe ?

Une exigence pour fédérés l’authentification unique est la disponibilité des points de terminaison à s’authentifier via internet. La disponibilité des points de terminaison d’authentification sur internet permet aux utilisateurs d’accéder aux applications, même lorsqu’ils ne sont pas sur un réseau d’entreprise. 

Toutefois, cela signifie également que certains mauvais acteurs peut tirer parti des points de terminaison fédérés disponibles sur internet et utiliser ces points de terminaison pour essayer de déterminer les mots de passe ou pour créer par déni de service. Une telle attaque est de plus en plus courante est appelée un **attaque de mot de passe**. 

Il existe 2 types d’attaques de mot de passe courants. Mot de passe pulvérisation attaque & brute force attaque de mot de passe. 

### <a name="password-spray-attack"></a>Attaque de pulvérisation de mot de passe
Dans une attaque par pulvérisation de mot de passe, ces mauvais acteurs va tenter les mots de passe courants sur de nombreux comptes différents et les services pour accéder à n’importe quel il peut trouver des ressources protégé par mot de passe. Généralement Cela englobe les nombreux différentes organisations et les fournisseurs d’identité. Par exemple, une personne malveillante utilisera un kit de ressources généralement disponible pour énumérer tous les utilisateurs dans plusieurs organisations et essayez « P@$$w0rd » et « Password1 » par rapport à tous ces comptes. Pour vous donner l’idée, une attaque pourrait ressembler à :

|Utilisateur cible|Cible de mot de passe|
|-----|-----|-----|
|User1@org1.com|Password1|
|User2@org1.com|Password1|
|User1@org2.com|Password1|
|User2@org2.com|Password1|
|…|…|
|User1@org1.com|P@$$w0rd|
|User2@org1.com|P@$$w0rd|
|User1@org2.com|P@$$w0rd|
|User2@org2.com|P@$$w0rd|

Ce modèle d’attaque échappe à la plupart des techniques de détection, car à partir du point de vue d’un utilisateur individuel ou d’entreprise, l’attaque ressemble simplement à une connexion ayant échouée isolée.

Pour les attaquants, c’est un jeu de nombres : ils savent qu’il n’y a certains mots de passe ici sont très courantes.  L’attaquant obtiendra quelques réussites pour tous les milliers comptes attaqués, et c’est suffisant pour être efficace. Ils utilisent les comptes pour obtenir des données à partir des e-mails, collecter des informations de contact et envoyer des liens de phishing ou développez simplement le groupe de cibles de pulvérisation de mot de passe. Les pirates ne vous souciez beaucoup qui sont les cibles initiales, juste qu’ils disposent de succès qui peuvent tirer parti.

Ils utilisent les comptes pour obtenir des données à partir des e-mails, collecter des informations de contact et envoyer des liens de phishing ou développez simplement le groupe de cibles de pulvérisation de mot de passe. Les pirates ne vous souciez beaucoup qui sont les cibles initiales, juste qu’ils disposent de succès qui peuvent tirer parti.

Mais, en prenant quelques étapes pour configurer les services AD FS et réseau correctement, les points de terminaison AD FS peuvent être sécurisés contre les attaques de ce type. Cet article couvre des 3 domaines qui doivent être configurés correctement pour vous aider à sécuriser contre ces attaques.

### <a name="brute-force-password-attack"></a>Attaque de mot de passe par Force brute 
Dans ce type d’attaque, un attaquant va tenter de plusieurs tentatives de mot de passe par rapport à un ensemble ciblé de comptes. Dans de nombreux cas, ces comptes sont ciblés sur les utilisateurs qui ont un niveau plus élevé d’accès au sein de l’organisation. Il peut s’agir cadres au sein de l’organisation ou les administrateurs qui gèrent des infrastructures critiques.  

Ce type d’attaque peut également entraîner des modèles de déni de service. Cela peut être au niveau du service où AD FS ne peut pas traiter un grand nombre de requêtes en raison d’un nombre insuffisant de serveurs ou peut être à un niveau de l’utilisateur où un utilisateur est verrouillé à leur compte.  

## <a name="securing-ad-fs-against-password-attacks"></a>Sécurisation d’AD FS contre les attaques de mot de passe 
 
Mais, en prenant quelques étapes pour configurer les services AD FS et réseau correctement, les points de terminaison AD FS peuvent être sécurisées par rapport à ces types d’attaques. Cet article couvre des 3 domaines qui doivent être configurés correctement pour vous aider à sécuriser contre ces attaques. 


- Niveau 1, ligne de base : Voici les paramètres de base qui doivent être configurés sur un serveur AD FS pour vous assurer que les mauvais acteurs ne peut pas les utilisateurs attaque fédéré de force brute. 
- Niveau 2, protection de l’extranet : Ce sont les paramètres qui doivent être configurés pour garantir que l’accès extranet est configuré pour utiliser des protocoles sécurisés, les stratégies d’authentification et les applications appropriées. 
- Niveau 3, accédez sans mot de passe pour l’accès extranet : Ceux-ci sont des paramètres et des instructions pour activer l’accès aux ressources fédérés avec la plus sécurisée identification, plutôt que les mots de passe qui sont vulnérables aux attaques. 

## <a name="level-1-baseline"></a>Niveau 1 : Baseline

1. Si AD FS 2016, implémentez [extranet verrouillage intelligent](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md) Extranet verrouillage intelligent effectue le suivi des emplacements connus et permet à un utilisateur valid à transiter par si elles ont précédemment connecté avec succès à partir de cet emplacement. À l’aide de verrouillage intelligent extranet, vous pouvez vous assurer que les mauvais acteurs ne sera pas en mesure de force brute attaquer les utilisateurs et en même temps vous permettent d’utilisateur légitime être productifs.
    - Si vous n’êtes pas sur AD FS 2016, nous vous recommandons fortement [mise à niveau](../../ad-fs/deployment/upgrading-to-ad-fs-in-windows-server.md) à AD FS 2016. Il est une simple mise à niveau à partir d’AD FS 2012 R2. Si vous utilisez AD FS 2012 R2, implémenter [verrouillage extranet](../../ad-fs/operations/Configure-AD-FS-Extranet-Soft-Lockout-Protection.md). L’un des inconvénients de cette approche sont que les utilisateurs valides peuvent être bloqués à partir de l’accès extranet si vous êtes dans un modèle de force brute. AD FS sur Server 2016 n’a pas cet inconvénient.

2. Surveiller et bloquer des adresses IP suspectes 
    - Si vous avez Azure AD Premium, implémenter Connect Health pour AD FS et utilisez le [rapport d’adresse IP risquée](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-adfs#risky-ip-report-public-preview) notifications qu’il fournit.
        
        a. Gestionnaire de licences n’est pas pour tous les utilisateurs et nécessite 25 licences/ADFS/WAP du serveur qui peut être facile pour un client.
    
        b. Vous pouvez commencer à étudier l’adresse IP qui génèrent le grand nombre d’échecs de connexion
    
        c. Vous devrez activer l’audit sur vos serveurs AD FS.

3.  Bloquer l’adresse IP suspecte.  Cela bloque potentiellement des attaques de déni de service.

    a. Si vous utilisez 2016, utilisez le [ *des adresses IP d’interdites Extranet* ](../../ad-fs/operations/configure-ad-fs-banned-ip.md) pour bloquer toutes les demandes provenant d’adresses IP indiquées par #3 (ou une analyse manuelle).

    b. Si vous utilisez AD FS 2012 R2 ou antérieur, bloquer l’adresse IP directement à Exchange Online et, éventuellement, sur votre pare-feu.

4. Si vous avez Azure AD Premium, utilisez [Protection de mot de passe Azure AD](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises) pour empêcher les mots de passe à deviner d’accéder à Azure AD  

    a. Notez que si vous avez des mots de passe à deviner, vous pouvez facilement les décoder simplement 1 à 3 tentatives. Cette fonctionnalité évite à partir de la configuration. 

    b. À partir des statistiques de notre version préliminaire, presque 20 à 50 % de nouveaux mots de passe être bloqué d’être définie. Cela implique que % des utilisateurs sont vulnérables aux mots de passe faciles à deviner.

## <a name="level-2-protect-your-extranet"></a>Niveau 2 : Protéger votre extranet

5. Déplacer vers l’authentification moderne pour les clients qui accèdent à partir de l’extranet. Les clients de messagerie sont une grande partie de ce. 

    a. Vous devez utiliser Outlook Mobile pour les appareils mobiles. La nouvelle application de messagerie native iOS prend en charge l’authentification moderne. 

    b. Vous devez utiliser Outlook 2013 (avec les derniers correctifs CU) ou Outlook 2016.

6.  Activer l’authentification Multifacteur pour tous les accès extranet. Ce vous offre plus de protection pour l’accès extranet.

    a.  Si vous avez Azure AD premium, utilisez [des stratégies d’accès conditionnel Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/overview) pour contrôler cela.  Cela est préférable à l’application des règles au niveau AD FS.  Il s’agit, car les applications clientes modernes sont appliquées de manière plus fréquente.  Cela se produit, sur Azure AD, lorsque vous demandez un nouveau jeton d’accès (généralement toutes les heures) à l’aide d’un jeton d’actualisation.  

    b.  Si vous n’avez Azure AD premium ou d’autres applications sur AD FS que vous autorisez internet après accès, mettre en œuvre MFA (peut également Azure MFA sur AD FS 2016) et effectuez un [stratégie globale de MFA](../../ad-fs/operations/configure-authentication-policies.md#to-configure-multi-factor-authentication-globally) pour tous les accès extranet.
 
## <a name="level-3-move-to-password-less-for-extranet-access"></a>Niveau 3 : Déplacement au mot de passe moins pour l’accès extranet

7. Déplacez vers Windows 10 et utilisez [Hello For Business](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification).

8. Pour d’autres périphériques, si vous utilisez AD FS 2016, que vous pouvez utiliser [Azure MFA OTP](../../ad-fs/operations/configure-ad-fs-and-azure-mfa.md) en tant que le premier facteur et mot de passe comme facteur 2nd. 

9.  Pour les appareils mobiles, si vous autorisez uniquement les périphériques gérés MDM, vous pouvez utiliser [certificats](../../ad-fs/operations/configure-user-certificate-authentication.md) pour vous connecter l’utilisateur. 
 
## <a name="urgent-handling"></a>Gestion d’urgence

Si l’environnement AD FS sous attaque active, les étapes suivantes doivent être implémentés au plus tôt :

 - Désactiver le point de terminaison U/P dans ADFS et nécessitent tout le monde au VPN pour accéder à ou être à l’intérieur de votre réseau. Cela nécessite que vous disposiez d’étape **2 de niveau 1 a #** terminée. Dans le cas contraire, toutes les demandes Outlook internes seront toujours routés via le cloud via l’authentification de proxy EXO
 - Si l’attaque est uniquement disponible via EXO, vous pouvez désactiver l’authentification de base pour les protocoles d’échange (POP, IMAP, SMTP, EWS, etc.) à l’aide de stratégies d’authentification, ces protocoles et méthodes d’authentification sont utilisés sur la grande majorité de ces attaques. En outre, les règles d’accès Client dans l’activation de protocole EXO et par boîte aux lettres sont évaluée après l’authentification et ne sont pas aider à atténuer les attaques. 
 - Sélectivement offrent un accès extranet à l’aide de niveau 3 #1-3.

## <a name="next-steps"></a>Étapes suivantes

- [Mise à niveau vers le serveur AD FS 2016](../../ad-fs/deployment/upgrading-to-ad-fs-in-windows-server.md) 
- [Verrouillage intelligent extranet dans AD FS 2016](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md)
- [Configurer des stratégies d’accès conditionnel](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
- [Protection de mot de passe Azure AD](https://docs.microsoft.com/azure/active-directory/authentication/howto-password-ban-bad-on-premises)
