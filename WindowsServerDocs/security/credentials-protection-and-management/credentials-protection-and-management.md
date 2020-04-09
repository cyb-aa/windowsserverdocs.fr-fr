---
title: Gestion et protection des informations d'identification
description: Sécurité de Windows Server
ms.prod: windows-server
ms.technology: security-credential-protection
ms.topic: article
ms.assetid: e457229c-0126-40fe-948c-101c943e1b57
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: c836da8f83510e6547e0e182ac06fd2151dd9c41
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857062"
---
# <a name="credentials-protection-and-management"></a>Gestion et protection des informations d'identification

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique destinée aux professionnels de l’informatique traite des fonctionnalités et des méthodes introduites dans Windows Server 2012 R2 et Windows 8.1 pour la protection des informations d’identification et les contrôles d’authentification de domaine pour réduire le vol des informations d’identification.

## <a name="BKMK_CredentialsProtectionManagement"></a>
### <a name="restricted-admin-mode-for-remote-desktop-connection"></a>Mode d'administration restreinte pour la connexion Bureau à distance
Le mode d'administration restreinte permet de se connecter de manière interactive à un serveur hôte distant sans lui transmettre ses informations d'identification. Cela empêche la collecte de vos informations d'identification durant le processus de connexion initial, si la sécurité du serveur est compromise.

En utilisant ce mode avec des informations d'identification d'administrateur, le client Bureau à distance tente de se connecter de manière interactive à un hôte qui prend également ce mode en charge, sans envoyer d'informations d'identification. Quand l'hôte vérifie que le compte d'utilisateur qui se connecte dispose de droits d'administrateur et qu'il prend en charge le mode d'administration restreinte, la connexion réussit. Sinon, la tentative de connexion échoue. À aucun moment, le mode d'administration restreinte n'envoie d'informations d'identification en texte brut ou sous une autre forme réutilisable aux ordinateurs distants.

### <a name="lsa-protection"></a>Protection LSA
L'autorité de sécurité locale (LSA, Local Security Authority), qui est incluse dans le processus LSASS (Local Security Authority Security Service), valide les utilisateurs pour les connexions locales et à distance, et applique les stratégies de sécurité locales. Le système d’exploitation Windows 8.1 fournit une protection supplémentaire pour l’autorité LSA afin d’empêcher l’injection de code par des processus non protégés. La sécurité des informations d’identification stockées et gérées par l’autorité LSA est ainsi renforcée. Ce paramètre de processus protégé pour l’autorité LSA peut être configuré dans Windows 8.1, mais il est activé par défaut dans Windows RT 8,1 et ne peut pas être modifié.

Pour plus d’informations sur la configuration de la protection LSA, consultez [Configuring Additional LSA Protection](configuring-additional-lsa-protection.md).

### <a name="protected-users-security-group"></a>Groupe de sécurité Utilisateurs protégés
Ce nouveau groupe global de domaine déclenche une nouvelle protection non configurable sur les appareils et ordinateurs hôtes exécutant Windows Server 2012 R2 et Windows 8.1. Le groupe utilisateurs protégés active des protections supplémentaires pour les contrôleurs de domaine et les domaines dans les domaines Windows Server 2012 R2. Cela réduit considérablement les types d'informations d'identification disponibles quand les utilisateurs sont connectés aux ordinateurs du réseau à partir d'un ordinateur non compromis.

En outre, les membres du groupe Utilisateurs protégés sont limités par les méthodes d'authentification suivantes :

-   Un membre du groupe Utilisateurs protégés ne peut se connecter qu'à l'aide du protocole Kerberos. Le compte ne peut pas s'authentifier à l'aide des protocoles NTLM, Digest Authentication ou CredSSP. Sur un appareil exécutant Windows 8.1, les mots de passe ne sont pas mis en cache. par conséquent, l’appareil qui utilise l’un des fournisseurs SSP (Security Support Provider) ne peut pas s’authentifier auprès d’un domaine quand le compte est membre du groupe utilisateurs protégés.

-   Le protocole Kerberos n'utilise pas les types de chiffrement DES et RC4 plus faibles dans le processus de pré-authentification. Cela signifie que le domaine doit être configuré pour prendre en charge au moins la suite de chiffrement AES.

-   Le compte de l’utilisateur ne peut pas être délégué avec la délégation Kerberos contrainte ou non contrainte. Cela signifie que les anciennes connexions aux autres systèmes peuvent échouer si l'utilisateur est membre du groupe Utilisateurs protégés.

-   Le paramètre de durée de vie TGT (Ticket Granting Ticket) Kerberos par défaut de quatre heures est configurable à l'aide des silos et stratégies d'authentification, accessibles via le Centre d'administration Active Directory. Cela signifie qu'après quatre heures l'utilisateur doit s'authentifier de nouveau.

> [!WARNING]
> Les comptes de services et d'ordinateurs ne doivent pas être membres du groupe Utilisateurs protégés. Ce groupe n'offre pas de protection locale, car le mot de passe ou le certificat est toujours disponible sur l'hôte. L’authentification échoue avec l’erreur « le nom d’utilisateur ou le mot de passe est incorrect » pour tout service ou ordinateur ajouté au groupe utilisateurs protégés.

Pour plus d'informations sur ce groupe, voir [Groupe de sécurité Utilisateurs protégés](protected-users-security-group.md).

### <a name="authentication-policy-and-authentication-policy-silos"></a>Stratégie d'authentification et silos de stratégies d'authentification
Les stratégies de Active Directory basées sur une forêt sont introduites et peuvent être appliquées aux comptes d’un domaine avec un niveau fonctionnel de domaine Windows Server 2012 R2. Ces stratégies d’authentification peuvent contrôler les hôtes dont peut se servir un utilisateur pour se connecter. Elles fonctionnent en collaboration avec le groupe de sécurité Utilisateurs protégés. En outre, les administrateurs peuvent appliquer des conditions de contrôle d’accès pour l’authentification aux comptes. Ces stratégies d'authentification isolent les comptes connexes pour limiter la portée d'un réseau.

La nouvelle classe d’objets Active Directory, stratégie d’authentification, vous permet d’appliquer la configuration de l’authentification à des classes de compte dans des domaines avec un niveau fonctionnel de domaine Windows Server 2012 R2. Les stratégies d'authentification sont appliquées pendant l'échange du service d'authentification (AS) ou du service d'accord de tickets (TGS) Kerberos. Les classes de compte Active Directory sont les suivantes :

-   Utilisateur

-   Ordinateur

-   Compte de service administré

-   Compte de service administré de groupe

Pour plus d'informations, voir [Stratégies d'authentification et silos de stratégies d'authentification](authentication-policies-and-authentication-policy-silos.md).

Pour plus d'informations sur la configuration des comptes protégés, voir [Comment configurer des comptes protégés](how-to-configure-protected-accounts.md).

## <a name="see-also"></a>Voir aussi
Pour plus d’informations sur l’autorité LSA et le processus LSASS, consultez la [Vue d’ensemble technique de l’authentification et de l’ouverture de session Windows](https://technet.microsoft.com/library/dn169029(v=ws.10).aspx).



