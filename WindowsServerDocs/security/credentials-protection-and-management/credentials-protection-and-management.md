---
title: Gestion et protection des informations d’identification
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e457229c-0126-40fe-948c-101c943e1b57
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 16cc1f2260bf0552da6902dc3e97de65d29c7931
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812200"
---
# <a name="credentials-protection-and-management"></a>Gestion et protection des informations d’identification

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique destinée aux professionnels de l’informatique décrit les fonctionnalités et les méthodes introduites dans Windows Server 2012 R2 et Windows 8.1 pour la protection des informations d’identification et les contrôles d’authentification de domaine pour réduire le vol d’informations d’identification.

## <a name="BKMK_CredentialsProtectionManagement"></a>
### <a name="restricted-admin-mode-for-remote-desktop-connection"></a>Mode d'administration restreinte pour la connexion Bureau à distance
Le mode d'administration restreinte permet de se connecter de manière interactive à un serveur hôte distant sans lui transmettre ses informations d'identification. Cela empêche la collecte de vos informations d'identification durant le processus de connexion initial, si la sécurité du serveur est compromise.

En utilisant ce mode avec des informations d'identification d'administrateur, le client Bureau à distance tente de se connecter de manière interactive à un hôte qui prend également ce mode en charge, sans envoyer d'informations d'identification. Quand l'hôte vérifie que le compte d'utilisateur qui se connecte dispose de droits d'administrateur et qu'il prend en charge le mode d'administration restreinte, la connexion réussit. Sinon, la tentative de connexion échoue. À aucun moment, le mode d'administration restreinte n'envoie d'informations d'identification en texte brut ou sous une autre forme réutilisable aux ordinateurs distants.

### <a name="lsa-protection"></a>Protection LSA
L'autorité de sécurité locale (LSA, Local Security Authority), qui est incluse dans le processus LSASS (Local Security Authority Security Service), valide les utilisateurs pour les connexions locales et à distance, et applique les stratégies de sécurité locales. Le système d’exploitation Windows 8.1 offre une protection supplémentaire pour l’autorité LSA en empêchant l’injection de code par des processus non protégés. La sécurité des informations d’identification stockées et gérées par l’autorité LSA est ainsi renforcée. Ce paramètre de processus protégé pour LSA peut être configuré dans Windows 8.1, mais est activée par défaut dans Windows RT 8.1 et ne peut pas être modifié.

Pour plus d’informations sur la configuration de la protection LSA, consultez [Configuring Additional LSA Protection](configuring-additional-lsa-protection.md).

### <a name="protected-users-security-group"></a>Groupe de sécurité Utilisateurs protégés
Ce nouveau groupe global de domaine déclenche une nouvelle protection non configurable sur les appareils et ordinateurs hôtes exécutant Windows Server 2012 R2 et Windows 8.1. Le groupe utilisateurs protégés active des protections supplémentaires pour les contrôleurs de domaine et de domaines dans des domaines Windows Server 2012 R2. Cela réduit considérablement les types d'informations d'identification disponibles quand les utilisateurs sont connectés aux ordinateurs du réseau à partir d'un ordinateur non compromis.

En outre, les membres du groupe Utilisateurs protégés sont limités par les méthodes d'authentification suivantes :

-   Un membre du groupe Utilisateurs protégés ne peut se connecter qu'à l'aide du protocole Kerberos. Le compte ne peut pas s'authentifier à l'aide des protocoles NTLM, Digest Authentication ou CredSSP. Sur un appareil exécutant Windows 8.1, les mots de passe ne sont pas mis en cache, l’appareil qui utilise l’un de ces fournisseurs de prise en charge de sécurité (SSP) pourront pas être authentifiés à un domaine lorsque le compte est membre du groupe utilisateurs protégés.

-   Le protocole Kerberos n'utilise pas les types de chiffrement DES et RC4 plus faibles dans le processus de pré-authentification. Cela signifie que le domaine doit être configuré pour prendre en charge au moins la suite de chiffrement AES.

-   Le compte d’utilisateur ne peut pas être délégué avec le protocole Kerberos contrainte ou sans contrainte délégation. Cela signifie que les anciennes connexions aux autres systèmes peuvent échouer si l'utilisateur est membre du groupe Utilisateurs protégés.

-   Le paramètre de durée de vie TGT (Ticket Granting Ticket) Kerberos par défaut de quatre heures est configurable à l'aide des silos et stratégies d'authentification, accessibles via le Centre d'administration Active Directory. Cela signifie qu'après quatre heures l'utilisateur doit s'authentifier de nouveau.

> [!WARNING]
> Les comptes de services et d'ordinateurs ne doivent pas être membres du groupe Utilisateurs protégés. Ce groupe n'offre pas de protection locale, car le mot de passe ou le certificat est toujours disponible sur l'hôte. L’authentification échoue avec l’erreur « le nom d’utilisateur ou mot de passe est incorrect » pour n’importe quel service ou d’un ordinateur qui est ajouté au groupe utilisateurs protégés.

Pour plus d'informations sur ce groupe, voir [Groupe de sécurité Utilisateurs protégés](protected-users-security-group.md).

### <a name="authentication-policy-and-authentication-policy-silos"></a>Stratégie d'authentification et silos de stratégies d'authentification
Stratégies de Active Directory en fonction de forêt sont introduites, et elles peuvent être appliquées aux comptes d’un domaine avec un niveau fonctionnel de domaine Windows Server 2012 R2. Ces stratégies d’authentification peuvent contrôler les hôtes dont peut se servir un utilisateur pour se connecter. Elles fonctionnent en collaboration avec le groupe de sécurité Utilisateurs protégés. En outre, les administrateurs peuvent appliquer des conditions de contrôle d’accès pour l’authentification aux comptes. Ces stratégies d'authentification isolent les comptes connexes pour limiter la portée d'un réseau.

La nouvelle classe d’objet Active Directory, la stratégie d’authentification vous permet d’appliquer la configuration de l’authentification aux classes de compte dans les domaines avec un niveau fonctionnel de domaine Windows Server 2012 R2. Les stratégies d'authentification sont appliquées pendant l'échange du service d'authentification (AS) ou du service d'accord de tickets (TGS) Kerberos. Les classes de compte Active Directory sont les suivantes :

-   Utilisateur

-   Computer

-   Compte de service administré

-   Compte de service administré de groupe

Pour plus d'informations, voir [Authentication Policies and Authentication Policy Silos](authentication-policies-and-authentication-policy-silos.md).

Pour plus d’informations sur la configuration des comptes protégés, consultez [How to Configure Protected Accounts](how-to-configure-protected-accounts.md).

## <a name="see-also"></a>Voir aussi
Pour plus d’informations sur l’autorité LSA et le processus LSASS, consultez la [Vue d’ensemble technique de l’authentification et de l’ouverture de session Windows](https://technet.microsoft.com/library/dn169029(v=ws.10).aspx).



