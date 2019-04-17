---
title: "Gestion et Protection des informations d’identification"
description: "Sécurité de Windows Server"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="credentials-protection-and-management"></a>Gestion et Protection des informations d’identification

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique destinée aux professionnels de l’informatique décrit les fonctionnalités et méthodes introduites dans Windows Server2012R2 et Windows8.1 pour la protection des informations d’identification et les contrôles d’authentification de domaine pour réduire le vol d’informations d’identification.

## <a name="BKMK_CredentialsProtectionManagement"></a>
### <a name="restricted-admin-mode-for-remote-desktop-connection"></a>Mode d’administration restreinte pour la connexion Bureau à distance
Mode d’administration restreinte permet de connecter de manière interactive à un serveur hôte distant sans transmettre vos informations d’identification pour le serveur. Cela empêche vos informations d’identification de la collecte au cours du processus de connexion initiale si le serveur a été compromis.

À l’aide de ce mode avec les informations d’identification d’administrateur, le client Bureau à distance tente de connecter de manière interactive à un ordinateur hôte qui prend également en charge ce mode sans envoyer des informations d’identification. Quand l’hôte vérifie que le compte d’utilisateur y connecter dispose de droits d’administrateur et qu’il prend en charge le mode d’administration restreinte, la connexion aboutit. Sinon, la tentative de connexion échoue. Mode d’administration restreinte ne prend pas en texte brut point envoyer ou autre forme réutilisable des informations d’identification à des ordinateurs distants.

### <a name="lsa-protection"></a>Protection LSA
Le sécurité autorité locale (LSA), qui se trouve dans le processus de sécurité autorité Service LSASS (Local Security), valide les utilisateurs pour les connexions locales et distantes et applique les stratégies de sécurité locale. Le système d’exploitation Windows8.1 offre une protection supplémentaire pour l’autorité LSA en empêchant l’injection de code par des processus non protégés. Cela fournit plus de sécurité pour les informations d’identification qui l’autorité LSA stocke et gère. Ce paramètre de processus protégé pour LSA peut être configuré dans Windows8.1, mais est activée par défaut dans WindowsRT8.1 et ne peut pas être modifié.

Pour plus d’informations sur la configuration de la protection LSA, consultez [Configuring Additional LSA Protection](configuring-additional-lsa-protection.md).

### <a name="protected-users-security-group"></a>Groupe de sécurité utilisateurs protégés
Ce nouveau groupe global de domaine déclenche une nouvelle protection non configurable sur les périphériques et les ordinateurs hôtes exécutant Windows Server2012R2 et Windows8.1. Le groupe utilisateurs protégés active des protections supplémentaires pour les contrôleurs de domaine et les domaines dans les domaines Windows Server2012R2. Cela réduit considérablement les types d’informations d’identification disponibles quand les utilisateurs sont connectés aux ordinateurs sur le réseau à partir d’un ordinateur non compromis.

Les membres du groupe utilisateurs protégés sont limités en les méthodes d’authentification suivantes:

-   Un membre du groupe utilisateurs protégés ne peut se connecter à l’aide du protocole Kerberos. Le compte ne peut pas s’authentifier à l’aide de NTLM, l’authentification Digest ni CredSSP. Sur un appareil exécutant Windows8.1, les mots de passe ne sont pas mises en cache, d’afin de l’appareil qui utilise l’un de ces fournisseurs de prise en charge de sécurité (SSP) ne parviendra pas à s’authentifier à un domaine, lorsque le compte est membre du groupe utilisateurs protégés.

-   Le protocole Kerberos n’utilisera pas les types de chiffrement DES ou RC4 plus faibles dans le processus de pré-authentification. Cela signifie que le domaine doit être configuré pour prendre en charge au moins la suite de chiffrement AES.

-   Le compte d’utilisateur ne peut pas être délégué avec Kerberos contrainte ou non contrainte délégation. Cela signifie que les connexions antérieures à d’autres systèmes peuvent échouer si l’utilisateur est membre du groupe utilisateurs protégés.

-   Le paramètre de durée de vie des Tickets TGT Kerberos (TGT) par défaut de quatre heures est configurable à l’aide de stratégies d’authentification et Silos accédés par le biais de l’ActiveDirectory Administrative Center (ADAC). Cela signifie que lorsque les quatre heures est dépassée, l’utilisateur doit s’authentifier à nouveau.

> [!WARNING]
> Comptes de services et les ordinateurs ne doivent pas être membres du groupe utilisateurs protégés. Ce groupe ne fournit aucune protection locale car le mot de passe ou le certificat est toujours disponible sur l’ordinateur hôte. L’authentification échoue avec l’erreur "le nom d’utilisateur ou le mot de passe est incorrect" pour n’importe quel service ou d’un ordinateur qui est ajouté au groupe utilisateurs protégés.

Pour plus d’informations sur ce groupe, voir [groupe de sécurité utilisateurs protégés](protected-users-security-group.md).

### <a name="authentication-policy-and-authentication-policy-silos"></a>Stratégie d’authentification et Silos de stratégies d’authentification
Stratégies ActiveDirectory forêt sont introduites et peuvent être appliquées aux comptes dans un domaine avec un niveau fonctionnel du domaine Windows Server2012R2. Ces stratégies d’authentification peuvent contrôler les hôtes un utilisateur peut utiliser pour se connecter. Qu’ils fonctionnent avec le groupe de sécurité utilisateurs protégés, les administrateurs peuvent appliquer des conditions de contrôle d’accès pour l’authentification aux comptes. Ces stratégies d’authentification isolent les comptes connexes pour limiter la portée d’un réseau.

La nouvelle classe d’objet ActiveDirectory, stratégie d’authentification, vous permet d’appliquer la configuration de l’authentification aux classes de compte dans les domaines avec un niveau fonctionnel du domaine Windows Server2012R2. Stratégies d’authentification sont appliquées pendant la comme Kerberos ou TGS exchange. Classes de compte actives Directory sont:

-   Utilisateur

-   Ordinateur

-   Compte de Service administré

-   Compte de Service administré de groupe

Pour plus d’informations, voir [stratégies d’authentification et Silos de stratégies d’authentification](authentication-policies-and-authentication-policy-silos.md).

Pour plus d’informations sur la configuration des comptes protégés, consultez [comment configurer des comptes protégés](how-to-configure-protected-accounts.md).

## <a name="see-also"></a>Voir aussi
Pour plus d’informations sur l’autorité LSA et le processus LSASS, consultez le [ouverture de session Windows et l’authentification de la vue d’ensemble technique](https://technet.microsoft.com/library/dn169029(v=ws.10).aspx).



