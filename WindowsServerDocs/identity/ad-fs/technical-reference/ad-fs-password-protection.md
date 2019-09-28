---
title: Protection contre les attaques de mot de passe AD FS
description: Ce document explique comment protéger les utilisateurs AD FS contre les attaques de mot de passe
author: billmath
manager: mtillman
ms.reviewer: andandyMSFT
ms.date: 11/15/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 68624e2307e5ddefc6e32160cabfcd140f609966
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407249"
---
# <a name="ad-fs-password-attack-protection"></a>Protection contre les attaques de mot de passe AD FS

## <a name="what-is-a-password-attack"></a>Qu’est-ce qu’une attaque de mot de passe ?

Une condition requise pour l’authentification unique fédérée est la disponibilité des points de terminaison pour l’authentification sur Internet. La disponibilité des points de terminaison d’authentification sur Internet permet aux utilisateurs d’accéder aux applications même s’ils ne se trouvent pas sur un réseau d’entreprise. 

Toutefois, cela signifie également que certains mauvais acteurs peuvent tirer parti des points de terminaison fédérés disponibles sur Internet et utiliser ces points de terminaison pour essayer de déterminer les mots de passe ou pour créer des attaques par déni de service. L’une de ces attaques qui devient plus courante est appelée **attaque de mot de passe**. 

Il existe 2 types d’attaques de mot de passe courantes. Attaque par pulvérisation de mot de passe & attaque de mot de passe en force brute. 

### <a name="password-spray-attack"></a>Attaque par pulvérisation de mot de passe
Dans le cas d’une attaque par pulvérisation de mot de passe, ces mauvais acteurs vont essayer les mots de passe les plus courants entre de nombreux comptes et services différents pour accéder aux ressources protégées par mot de passe qu’ils peuvent trouver. Généralement, ceux-ci s’étendent à de nombreuses organisations et fournisseurs d’identité. Par exemple, une personne malveillante utilise une boîte à outils couramment disponible pour énumérer tous les utilisateurs de plusieurs organisations, puis essayer « P @ $ $w 0rd » et « Password1 » pour tous ces comptes. Pour vous donner l’idée, une attaque peut se présenter comme suit :


|  Utilisateur cible   | Mot de passe cible |
|----------------|-----------------|
| User1@org1.com |    Password1    |
| User2@org1.com |    Password1    |
| User1@org2.com |    Password1    |
| User2@org2.com |    Password1    |
|       …        |        …        |
| User1@org1.com |    P @ $ $w 0rd     |
| User2@org1.com |    P @ $ $w 0rd     |
| User1@org2.com |    P @ $ $w 0rd     |
| User2@org2.com |    P @ $ $w 0rd     |

Ce modèle d’attaque échappe à la plupart des techniques de détection, car du point de bourré d’un utilisateur ou d’une société individuel, l’attaque ressemble à une connexion ayant échoué isolée.

Pour les attaquants, il s’agit d’un jeu de chiffres : ils savent qu’il y a des mots de passe très courants.  L’attaquant obtiendra quelques réussites pour chaque millier de comptes attaqués, ce qui est suffisant pour être efficace. Ils utilisent les comptes pour obtenir des données à partir de messages électroniques, collecter des informations de contact et envoyer des liens d’hameçonnage ou simplement développer le groupe cible de pulvérisation de mot de passe. Les attaquants ne se soucient pas de l’identité des cibles initiales, mais elles ont un certain succès qu’elles peuvent exploiter.

Toutefois, en suivant quelques étapes pour configurer correctement la AD FS et le réseau, AD FS points de terminaison peuvent être sécurisés contre ces types d’attaques. Cet article traite des 3 domaines qui doivent être configurés correctement pour vous aider à sécuriser ces attaques.

### <a name="brute-force-password-attack"></a>Attaque par force brute de mot de passe 
Dans ce type d’attaque, une personne malveillante tentera plusieurs tentatives de mot de passe sur un ensemble de comptes ciblé. Dans de nombreux cas, ces comptes sont ciblés sur les utilisateurs qui ont un niveau d’accès plus élevé au sein de l’organisation. Il peut s’agir de dirigeants au sein de l’organisation ou des administrateurs qui gèrent l’infrastructure critique.  

Ce type d’attaque peut également entraîner des modèles DOS. Il peut s’agir d’un niveau de service dans lequel ADFS ne peut pas traiter un grand nombre de requêtes en raison d’un nombre insuffisant de serveurs ou d’un niveau utilisateur où un utilisateur est verrouillé à partir de son compte.  

## <a name="securing-ad-fs-against-password-attacks"></a>Sécurisation des AD FS contre les attaques par mot de passe 

Toutefois, en suivant quelques étapes pour configurer correctement la AD FS et le réseau, AD FS points de terminaison peuvent être sécurisés contre ces types d’attaques. Cet article traite des 3 domaines qui doivent être configurés correctement pour vous aider à sécuriser ces attaques. 


- Niveau 1, ligne de base : Il s’agit des paramètres de base qui doivent être configurés sur un serveur AD FS pour s’assurer que les mauvais acteurs ne peuvent pas forcer les utilisateurs fédérés malveillants. 
- Niveau 2, protection de l’extranet : Il s’agit des paramètres qui doivent être configurés pour garantir que l’accès extranet est configuré pour utiliser des protocoles sécurisés, des stratégies d’authentification et des applications appropriées. 
- Niveau 3, passer au mot de passe sans mot de passe pour l’accès extranet : Il s’agit de paramètres avancés et de recommandations pour permettre l’accès aux ressources fédérées avec des informations d’identification plus sécurisées plutôt que des mots de passe susceptibles d’être attaqués. 

## <a name="level-1-baseline"></a>Niveau 1 : Baseline

1. Si ADFS 2016, implémentez le verrouillage intelligent extranet de [verrouillage intelligent](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md) qui effectue le suivi des emplacements familiers et permettra à un utilisateur valide de traverser s’il s’est déjà connecté avec succès à partir de cet emplacement. En utilisant le verrouillage intelligent extranet, vous pouvez vous assurer que les mauvais acteurs ne seront pas en mesure de forcer les utilisateurs à effectuer des attaques en force brute et de laisser à l’utilisateur légitime la productivité.
    - Si vous n’êtes pas sur AD FS 2016, nous vous recommandons vivement d' [effectuer la mise à niveau](../../ad-fs/deployment/upgrading-to-ad-fs-in-windows-server.md) vers AD FS 2016. Il s’agit d’un chemin de mise à niveau simple de AD FS 2012 R2. Si vous êtes sur AD FS 2012 R2, implémentez le [verrouillage extranet](../../ad-fs/operations/Configure-AD-FS-Extranet-Soft-Lockout-Protection.md). L’un des inconvénients de cette approche est que les utilisateurs valides peuvent être bloqués pour l’accès extranet si vous êtes en force brute. AD FS sur le serveur 2016 ne présente pas cet inconvénient.

2. Surveiller & bloquer les adresses IP suspectes 
    - Si vous avez Azure AD Premium, implémentez Connect Health pour ADFS et utilisez les notifications de [rapport d’adresse IP risquées](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-adfs#risky-ip-report-public-preview) qu’il fournit.

        a. La gestion des licences n’est pas destinée à tous les utilisateurs et nécessite 25 licences/ADFS/WAP, ce qui peut être facile pour un client.

        b. Vous pouvez maintenant examiner les adresses IP qui génèrent de nombreux échecs de connexion

        c. Pour cela, vous devez activer l’audit sur vos serveurs ADFS.

3.  Bloquez les adresses IP suspectes.  Cela bloque potentiellement les attaques DOS.

    a. Si le 2016, utilisez la fonctionnalité d' [*adresses IP interdites extranet*](../../ad-fs/operations/configure-ad-fs-banned-ip.md) pour bloquer toutes les demandes provenant de l’adresse IP signalées par #3 (ou une analyse manuelle).

    b. Si vous êtes sur AD FS 2012 R2 ou une version antérieure, bloquez l’adresse IP directement sur Exchange Online et éventuellement sur votre pare-feu.

4. Si vous avez Azure AD Premium, utilisez [Azure ad protection par mot de passe](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises) pour empêcher que les mots de passe devinés ne soient Azure ad  

    a. Notez que si vous avez des mots de passe devinables, vous pouvez les deviner avec seulement 1-3 tentatives. Cette fonctionnalité empêche ces derniers d’être définis. 

    b. À partir de nos statistiques préliminaires, presque 20-50% des nouveaux mots de passe sont bloqués. Cela implique que% des utilisateurs sont vulnérables aux mots de passe facilement devinés.

## <a name="level-2-protect-your-extranet"></a>Niveau 2 : Protéger votre extranet

5. Passez à l’authentification moderne pour tous les clients accédant à partir de l’extranet. Les clients de messagerie constituent une partie importante de cette fonctionnalité. 

    a. Vous devrez utiliser Outlook Mobile pour les appareils mobiles. La nouvelle application de messagerie Native iOS prend également en charge l’authentification moderne. 

    b. Vous devrez utiliser Outlook 2013 (avec les derniers correctifs CU) ou Outlook 2016.

6. Activez MFA pour tous les accès extranet. Vous bénéficiez ainsi d’une protection supplémentaire pour tous les accès extranet.

   a.  Si vous avez Azure AD Premium, utilisez [Azure ad des stratégies d’accès conditionnel](https://docs.microsoft.com/azure/active-directory/conditional-access/overview) pour contrôler cela.  C’est mieux que d’implémenter les règles sur AD FS.  Cela est dû au fait que les applications clientes modernes sont appliquées de manière plus fréquente.  Cela se produit, au Azure AD, lors de la demande d’un nouveau jeton d’accès (généralement toutes les heures) à l’aide d’un jeton d’actualisation.  

   b.  Si vous n’avez pas Azure AD Premium ou si vous disposez d’applications supplémentaires sur AD FS que vous autorisez l’accès à Internet, implémentez l’authentification MFA (l’authentification multifacteur peut également être Azure MFA sur AD FS 2016) et effectuez une [stratégie d’authentification multifacteur globale](../../ad-fs/operations/configure-authentication-policies.md#to-configure-multi-factor-authentication-globally) pour tous les accès extranet.

## <a name="level-3-move-to-password-less-for-extranet-access"></a>Niveau 3 : Passer au mot de passe moins pour l’accès extranet

7. Accédez à la fenêtre 10 et utilisez [Hello entreprise](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification).

8. Pour les autres appareils, si vous utilisez AD FS 2016, vous pouvez utiliser le mot de passe à [usage unique Azure MFA](../../ad-fs/operations/configure-ad-fs-and-azure-mfa.md) comme premier facteur et mot de passe comme second facteur. 

9. Pour les appareils mobiles, si vous autorisez uniquement les appareils gérés par MDM, vous pouvez utiliser des [certificats](../../ad-fs/operations/configure-user-certificate-authentication.md) pour connecter l’utilisateur. 

## <a name="urgent-handling"></a>Gestion urgente

Si l’environnement de AD FS est en cours d’attaque, les étapes suivantes doivent être implémentées au plus tôt :

 - Désactivez le point de terminaison U/P dans ADFS et demandez à tout le monde d’accéder au VPN ou à l’intérieur de votre réseau. Pour cela, vous devez disposer de l’étape **2 #1a** terminée. Dans le cas contraire, toutes les demandes Outlook internes seront toujours acheminées via le Cloud via l’authentification proxy EXO.
 - Si l’attaque provient uniquement de EXO, vous pouvez désactiver l’authentification de base pour les protocoles Exchange (POP, IMAP, SMTP, EWS, etc.) à l’aide de stratégies d’authentification, ces protocoles et méthodes d’authentification sont utilisés sur la plupart de ces attaques. En outre, les règles d’accès client dans EXO et l’activation des protocoles par boîte aux lettres sont évaluées après l’authentification et ne permettent pas d’atténuer les attaques. 
 - Offrez de manière sélective un accès extranet à l’aide du niveau 3 #1-3.

## <a name="next-steps"></a>Étapes suivantes

- [Mettre à niveau vers AD FS Server 2016](../../ad-fs/deployment/upgrading-to-ad-fs-in-windows-server.md) 
- [Verrouillage intelligent extranet dans AD FS 2016](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md)
- [Configurer des stratégies d’accès conditionnel](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
- [Protection par mot de passe Azure AD](https://docs.microsoft.com/azure/active-directory/authentication/howto-password-ban-bad-on-premises)
