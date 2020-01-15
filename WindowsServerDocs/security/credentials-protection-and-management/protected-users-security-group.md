---
title: Groupe de sécurité Utilisateurs protégés
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b0b5180-f65a-43ac-8ef3-66014116f296
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 227d66dafffd67b0b2e4f67158498cf43c7b59f8
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950368"
---
# <a name="protected-users-security-group"></a>Groupe de sécurité Utilisateurs protégés

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique destinée aux professionnels de l'informatique décrit le groupe de sécurité Active Directory nommé Utilisateurs protégés et explique son fonctionnement. Ce groupe a été introduit dans les contrôleurs de domaine Windows Server 2012 R2.

## <a name="BKMK_ProtectedUsers"></a>Vue d’ensemble

Ce groupe de sécurité est conçu dans le cadre d’une stratégie de gestion de l’exposition des informations d’identification au sein de l’entreprise. Les membres de ce groupe disposent automatiquement de protections non configurables qui sont appliquées à leurs comptes. L'appartenance au groupe Utilisateurs protégés est censée être restrictive et sécurisée de manière proactive par défaut. La seule méthode permettant de modifier ces protections pour un compte consiste à supprimer le compte du groupe de sécurité.

> [!WARNING]
> Les comptes de services et d’ordinateurs ne doivent jamais être membres du groupe utilisateurs protégés. Ce groupe fournit néanmoins une protection incomplète, car le mot de passe ou le certificat est toujours disponible sur l’ordinateur hôte. L’authentification échoue avec l’erreur \"le nom d’utilisateur ou le mot de passe est incorrect\" pour tout service ou ordinateur ajouté au groupe utilisateurs protégés.

Ce groupe global lié au domaine déclenche une protection non configurable sur les appareils et ordinateurs hôtes exécutant Windows Server 2012 R2 et Windows 8.1 ou version ultérieure pour les utilisateurs des domaines avec un contrôleur de domaine principal exécutant Windows Server 2012 R2. Cela réduit considérablement l’encombrement de mémoire par défaut des informations d’identification quand les utilisateurs se connectent aux ordinateurs avec ces protections.

Pour plus d’informations, voir fonctionnement [du groupe utilisateurs protégés](#BKMK_HowItWorks) dans cette rubrique.


## <a name="BKMK_Requirements"></a>Conditions requises pour le groupe utilisateurs protégés
Les conditions requises pour fournir des protections d’appareils aux membres du groupe utilisateurs protégés sont les suivantes :

- Le groupe de sécurité global Utilisateurs protégés est répliqué vers tous les contrôleurs du domaine du compte.

- Windows 8.1 et Windows Server 2012 R2 ajoutent la prise en charge par défaut. L' [avis de sécurité Microsoft 2871997](https://technet.microsoft.com/library/security/2871997) ajoute la prise en charge de Windows 7, windows Server 2008 R2 et windows server 2012.

Les conditions requises pour fournir une protection de contrôleur de domaine aux membres du groupe Utilisateurs protégés sont notamment les suivantes :

- Les utilisateurs doivent se trouver dans des domaines qui sont des niveaux fonctionnels de domaine Windows Server 2012 R2 ou version ultérieure.

### <a name="adding-protected-user-global-security-group-to-down-level-domains"></a>Ajout d’un groupe de sécurité global utilisateur protégé à des domaines de niveau supérieur

Les contrôleurs de domaine qui exécutent un système d’exploitation antérieur à Windows Server 2012 R2 peuvent prendre en charge l’ajout de membres au nouveau groupe de sécurité utilisateur protégé. Cela permet aux utilisateurs de bénéficier des protections de l’appareil avant la mise à niveau du domaine. 

> [!Note]
> Les contrôleurs de domaine ne prennent pas en charge les protections de domaine. 

Le groupe utilisateurs protégés peut être créé en [transférant le rôle d’émulateur de contrôleur de domaine principal (PDC)](https://technet.microsoft.com/library/cc816944(v=ws.10).aspx) sur un contrôleur de domaine qui exécute Windows Server 2012 R2. Une fois l'objet de groupe répliqué sur d'autres contrôleurs de domaine, le rôle de l'émulateur PDC peut être hébergé sur un contrôleur de domaine qui exécute une version antérieure de Windows Server.

### <a name="BKMK_ADgroup"></a>Propriétés AD du groupe utilisateurs protégés

Le tableau suivant spécifie les propriétés du groupe Utilisateurs protégés.

|Attribut|Value|
|-------|-----|
|SID/RID connu|S-1-5-21-<domain>-525|
|Tapez|Global du domaine|
|Conteneur par défaut|CN=Utilisateurs, DC=<domain>, DC=|
|Membres par défaut|Aucun(e)|
|Membre par défaut de|Aucun(e)|
|Protégé par ADMINSDHOLDER ?|non|
|Sortie du conteneur par défaut sécurisée ?|Oui|
|Délégation de la gestion de ce groupe à des administrateurs extérieurs au service sécurisée ?|non|
|Droits d’utilisateur par défaut|Aucun droit d’utilisateur par défaut|

## <a name="BKMK_HowItWorks"></a>Fonctionnement du groupe utilisateurs protégés
Cette section décrit le fonctionnement du groupe Utilisateurs protégés quand :

- Signé sur un appareil Windows

- Le domaine du compte d’utilisateur se trouve dans un niveau fonctionnel de domaine Windows Server 2012 R2 ou version supérieure

### <a name="device-protections-for-signed-in-protected-users"></a>Protections des appareils pour les utilisateurs protégés
Lorsque l’utilisateur connecté est membre du groupe utilisateurs protégés, les protections suivantes sont appliquées :

- La délégation des informations d’identification (CredSSP) ne met pas en cache les informations d’identification de texte brut de l’utilisateur, même lorsque le paramètre **autoriser la délégation des informations d’identification par défaut** stratégie de groupe est activé.

- À partir de Windows 8.1 et de Windows Server 2012 R2, Windows Digest ne met pas en cache les informations d’identification en texte brut de l’utilisateur, même quand Windows Digest est activé.

> [!Note]
> Après l’installation de l' [avis de sécurité Microsoft 2871997](https://technet.microsoft.com/library/security/2871997) , Windows Digest continue de mettre en cache les informations d’identification jusqu’à ce que la clé de registre soit configurée. Consultez l' [avis de sécurité Microsoft : mise à jour pour améliorer la protection et la gestion des informations d’identification : 13 mai, 2014](https://support.microsoft.com/help/2871997/microsoft-security-advisory-update-to-improve-credentials-protection-a) pour obtenir des instructions.

- NTLM ne met pas en cache les informations d’identification en texte brut de l’utilisateur ou la fonction unidirectionnelle NT (NTOWF).

- Kerberos ne crée plus de clés DES ou RC4. En outre, il ne met pas en cache les informations d’identification de texte brut de l’utilisateur ou les clés à long terme après l’acquisition du ticket TGT initial.

- Aucun vérificateur mis en cache n’est créé lors de la connexion ou du déverrouillage. la connexion hors connexion n’est donc plus prise en charge.

Une fois le compte d’utilisateur ajouté au groupe utilisateurs protégés, la protection commence lorsque l’utilisateur se connecte à l’appareil.

### <a name="domain-controller-protections-for-protected-users"></a>Protection du contrôleur de domaine pour les utilisateurs protégés
Les comptes qui sont membres du groupe utilisateurs protégés qui s’authentifient auprès d’un domaine Windows Server 2012 R2 ne peuvent pas :

- s'authentifier avec l'authentification NTLM ;

- utiliser les types de chiffrement DES ou RC4 dans la pré-authentification Kerberos ;

- être délégués en utilisant la délégation non contrainte ou contrainte ;

- renouveler les tickets TGT Kerberos au-delà de la durée de vie initiale de 4 heures.

Des paramètres non configurables pour l'expiration des tickets TGT sont établis pour chaque compte dans le groupe Utilisateurs protégés. Normalement, le contrôleur de domaine définit la durée de vie et le renouvellement des tickets TGT en fonction des stratégies de domaine **Durée de vie maximale du ticket d'utilisateur** et **Durée de vie maximale pour le renouvellement du ticket utilisateur**. Pour le groupe Utilisateurs protégés, la valeur 600 minutes est définie pour ces stratégies de domaine.

Pour plus d'informations, voir [How to Configure Protected Accounts](how-to-configure-protected-accounts.md).

## <a name="troubleshooting"></a>Dépannage
Deux journaux d'administration opérationnels sont disponibles pour résoudre les problèmes associés aux événements concernant les utilisateurs protégés. Ces nouveaux journaux se trouvent dans l'Observateur d'événements, sont désactivés par défaut et sont situés sous **Journaux des applications et des services\Microsoft\Windows\Microsoft\Authentification**.

|ID d'événement et journal|Description|
|----------|--------|
|104<br /><br />**ProtectedUser-Client**|Cause : Le package de sécurité sur le client ne contient pas les informations d'identification.<br /><br />L'erreur est consignée sur l'ordinateur client quand le compte est membre du groupe de sécurité Utilisateurs protégés. Cet événement indique que le package de sécurité ne met pas en cache les informations d'identification nécessaires pour une authentification auprès du serveur.<br /><br />Affiche le nom du package, le nom d'utilisateur, le nom du domaine et le nom du serveur.|
|304<br /><br />**ProtectedUser-Client**|Raison : le package de sécurité ne stocke pas les informations d’identification de l’utilisateur protégé.<br /><br />Un événement d’information est consigné dans le client pour indiquer que le package de sécurité ne met pas en cache les informations d’identification de connexion de l’utilisateur. Normalement, Digest (WDigest), la délégation des informations d'identification (CredSSP) et NTLM ne devraient pas pouvoir obtenir les informations d'identification de connexion pour les utilisateurs protégés. Les applications peuvent quand même réussir si elles demandent des informations d'identification.<br /><br />Affiche le nom du package, le nom d'utilisateur et le nom du domaine.|
|100<br /><br />**ProtectedUserFailures-DomainController**|Cause : Un échec de connexion NTLM se produit pour un compte qui figure dans le groupe de sécurité Utilisateurs protégés.<br /><br />Une erreur est consignée dans le contrôleur de domaine pour indiquer l'échec de l'authentification NTLM en raison de l'appartenance du compte au groupe de sécurité Utilisateurs protégés.<br /><br />Affiche le nom du compte et le nom de l'appareil.|
|104<br /><br />**ProtectedUserFailures-DomainController**|Cause : Les types de chiffrement DES ou RC4 sont utilisés pour l'authentification Kerberos et un échec de connexion se produit pour un utilisateur dans le groupe de sécurité Utilisateurs protégés.<br /><br />La pré-authentification Kerberos a échoué, car les types de chiffrement DES et RC4 ne peuvent pas être utilisés quand le compte est membre du groupe de sécurité Utilisateurs protégés.<br /><br />(AES est acceptable.)|
|303<br /><br />**ProtectedUserSuccesses-DomainController**|Cause : Un ticket TGT Kerberos a été correctement émis pour un membre du groupe Utilisateurs protégés.|


## <a name="additional-resources"></a>Ressources supplémentaires

- [Gestion et protection des informations d'identification](credentials-protection-and-management.md)

- [Stratégies d’authentification et silos de stratégies d’authentification](authentication-policies-and-authentication-policy-silos.md)

- [Comment configurer des comptes protégés](how-to-configure-protected-accounts.md)
