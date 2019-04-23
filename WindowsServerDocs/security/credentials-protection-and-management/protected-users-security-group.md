---
title: Groupe de sécurité Utilisateurs protégés
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: bd6b53c0febdfb2d344136097a9654c981405568
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862360"
---
# <a name="protected-users-security-group"></a>Groupe de sécurité Utilisateurs protégés

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique destinée aux professionnels de l'informatique décrit le groupe de sécurité Active Directory nommé Utilisateurs protégés et explique son fonctionnement. Ce groupe a été introduit dans les contrôleurs de domaine Windows Server 2012 R2.

## <a name="BKMK_ProtectedUsers"></a>Vue d’ensemble

Ce groupe de sécurité est conçu dans le cadre d’une stratégie pour gérer l’exposition des informations d’identification au sein de l’entreprise. Les membres de ce groupe disposent automatiquement de protections non configurables qui sont appliquées à leurs comptes. L'appartenance au groupe Utilisateurs protégés est censée être restrictive et sécurisée de manière proactive par défaut. La seule méthode permettant de modifier ces protections pour un compte consiste à supprimer le compte du groupe de sécurité.

> [!WARNING]
> Comptes de services et les ordinateurs ne doivent jamais être membres du groupe utilisateurs protégés. Ce groupe est offre une protection incomplète malgré tout, car le mot de passe ou le certificat est toujours disponible sur l’ordinateur hôte. L’authentification échoue avec l’erreur \"le nom d’utilisateur ou le mot de passe est incorrect\" pour n’importe quel service ou d’un ordinateur qui est ajouté au groupe utilisateurs protégés.

Ce groupe global lié au domaine déclenche une protection non configurable sur les appareils et ordinateurs hôtes exécutant Windows Server 2012 R2 et Windows 8.1 ou version ultérieure pour les utilisateurs dans des domaines avec un contrôleur de domaine principal exécutant Windows Server 2012 R2. Cela réduit considérablement l’encombrement de mémoire par défaut des informations d’identification quand les utilisateurs connectez-vous aux ordinateurs avec ces protections.

Pour plus d’informations, consultez [comment les utilisateurs protégés groupe works](#BKMK_HowItWorks) dans cette rubrique.



## <a name="BKMK_Requirements"></a>Besoins de groupe utilisateurs protégés
La configuration requise pour fournir des protections de périphérique pour les membres du groupe utilisateurs protégés :

- Le groupe de sécurité global Utilisateurs protégés est répliqué vers tous les contrôleurs du domaine du compte.

- Windows 8.1 et Windows Server 2012 R2 prise en charge par défaut. [Microsoft Security Advisory 2871997](https://technet.microsoft.com/library/security/2871997) prend désormais en charge Windows 7, Windows Server 2008 R2 et Windows Server 2012.

Les conditions requises pour fournir une protection de contrôleur de domaine aux membres du groupe Utilisateurs protégés sont notamment les suivantes :

- Les utilisateurs doivent être dans des domaines Windows Server 2012 R2 ou plus élevé niveau fonctionnel du domaine.

### <a name="adding-protected-user-global-security-group-to-down-level-domains"></a>Ajout du groupe global de sécurité utilisateurs protégés à des domaines de bas niveau

Les contrôleurs de domaine qui exécutent un système d’exploitation antérieurs à Windows Server 2012 R2 peuvent prendre en charge l’ajout de membres au nouveau groupe de sécurité utilisateurs protégés. Cela permet aux utilisateurs de tirer parti des protections de périphérique avant que le domaine est mis à niveau. 

> [!Note]
> Les contrôleurs de domaine ne prendra pas en charge les protections de domaine. 

Groupe utilisateurs protégés peut être créé par [transférer le rôle d’émulateur de contrôleur principal de domaine](https://technet.microsoft.com/library/cc816944(v=ws.10).aspx) à un contrôleur de domaine qui exécute Windows Server 2012 R2. Une fois l'objet de groupe répliqué sur d'autres contrôleurs de domaine, le rôle de l'émulateur PDC peut être hébergé sur un contrôleur de domaine qui exécute une version antérieure de Windows Server.

### <a name="BKMK_ADgroup"></a>Propriétés du groupe AD les utilisateurs protégées

Le tableau suivant spécifie les propriétés du groupe Utilisateurs protégés.

|Attribut|Value|
|-------|-----|
|SID/RID connu|S-1-5-21-<domain>-525|
|Type|Global du domaine|
|Conteneur par défaut|CN=Utilisateurs, DC=<domain>, DC=|
|Membres par défaut|Aucune|
|Membre par défaut de|Aucune|
|Protégé par ADMINSDHOLDER ?|Non|
|Sortie du conteneur par défaut sécurisée ?|Oui|
|Délégation de la gestion de ce groupe à des administrateurs extérieurs au service sécurisée ?|Non|
|Droits d’utilisateur par défaut|Aucun droit d’utilisateur par défaut|

## <a name="BKMK_HowItWorks"></a>Fonctionne du groupe utilisateurs protégés
Cette section décrit le fonctionnement du groupe Utilisateurs protégés quand :

-   Connecter un appareil Windows

-   Domaine de compte d’utilisateur est dans un Windows Server 2012 R2 ou le plus élevé niveau fonctionnel du domaine

### <a name="device-protections-for-signed-in-protected-users"></a>Protections d’appareil pour connecter des utilisateurs protégés
Lorsque l’utilisateur connecté est membre du groupe utilisateurs protégés, les protections suivantes sont appliquées :

-   Va délégation (CredSSP) pas mettre en cache les informations d’identification en texte brut même lorsque des informations d’identification du **autoriser la délégation des informations d’identification par défaut** paramètre de stratégie de groupe est activé.

-   Depuis Windows 8.1 et Windows Server 2012 R2, Windows Digest ne mettra pas en cache les informations d’identification en texte brut même quand Windows Digest est activé.

> [!Note]
> Après avoir installé [2871997 avis de sécurité Microsoft](https://technet.microsoft.com/library/security/2871997) Windows Digest continue aux informations d’identification du cache jusqu'à ce que la clé de Registre est configurée. Consultez [avis de sécurité Microsoft : Mise à jour pour améliorer la gestion et la protection des informations d’identification : Le 13 mai 2014](https://support.microsoft.com/en-us/help/2871997/microsoft-security-advisory-update-to-improve-credentials-protection-a) pour obtenir des instructions.

-   NTLM ne sera pas mettre en cache les informations d’identification de l’utilisateur en texte brut ou fonction unidirectionnelle NT (NTOWF).

-   Kerberos n’est plus créé les clés DES ou RC4. Également il ne mettra pas en cache informations d’identification en texte brut ou des clés à long terme de l’utilisateur une fois que le ticket TGT initial est acquis.

-   Aucun vérificateur en cache n’est pas créé à la connexion ou de déverrouiller, donc dans l’authentification en mode hors connexion n’est plus pris en charge.

Une fois que le compte d’utilisateur est ajouté au groupe utilisateurs protégés, protection commence lorsque l’utilisateur se connecte à l’appareil.

### <a name="domain-controller-protections-for-protected-users"></a>Protections de contrôleur de domaine pour les utilisateurs protégés
Comptes qui sont membres du groupe utilisateurs protégés qui s’authentifient auprès d’un domaine Windows Server 2012 R2 ne parvenez pas à :

-   s'authentifier avec l'authentification NTLM ;

-   utiliser les types de chiffrement DES ou RC4 dans la pré-authentification Kerberos ;

-   être délégués en utilisant la délégation non contrainte ou contrainte ;

-   renouveler les tickets TGT Kerberos au-delà de la durée de vie initiale de 4 heures.

Des paramètres non configurables pour l'expiration des tickets TGT sont établis pour chaque compte dans le groupe Utilisateurs protégés. Normalement, le contrôleur de domaine définit la durée de vie et le renouvellement des tickets TGT en fonction des stratégies de domaine **Durée de vie maximale du ticket d'utilisateur** et **Durée de vie maximale pour le renouvellement du ticket utilisateur**. Pour le groupe Utilisateurs protégés, la valeur 600 minutes est définie pour ces stratégies de domaine.

Pour plus d'informations, voir [Comment configurer des comptes protégés](how-to-configure-protected-accounts.md).

## <a name="troubleshooting"></a>Résolution des problèmes
Deux journaux d'administration opérationnels sont disponibles pour résoudre les problèmes associés aux événements concernant les utilisateurs protégés. Ces nouveaux journaux se trouvent dans l'Observateur d'événements, sont désactivés par défaut et sont situés sous **Journaux des applications et des services\Microsoft\Windows\Microsoft\Authentification**.

|ID d'événement et journal|Description|
|----------|--------|
|104<br /><br />**ProtectedUser-Client**|Cause : Le package de sécurité sur le client ne contient pas les informations d'identification.<br /><br />L'erreur est consignée sur l'ordinateur client quand le compte est membre du groupe de sécurité Utilisateurs protégés. Cet événement indique que le package de sécurité ne met pas en cache les informations d'identification nécessaires pour une authentification auprès du serveur.<br /><br />Affiche le nom du package, le nom d'utilisateur, le nom du domaine et le nom du serveur.|
|304<br /><br />**ProtectedUser-Client**|Cause : Le package de sécurité ne stocke pas les informations d’identification de l’utilisateur protégé.<br /><br />Un événement d’information est consigné dans le client pour indiquer que le package de sécurité ne met pas de cache des identifiants de connexion de l’utilisateur. Normalement, Digest (WDigest), la délégation des informations d'identification (CredSSP) et NTLM ne devraient pas pouvoir obtenir les informations d'identification de connexion pour les utilisateurs protégés. Les applications peuvent quand même réussir si elles demandent des informations d'identification.<br /><br />Affiche le nom du package, le nom d'utilisateur et le nom du domaine.|
|100<br /><br />**ProtectedUserFailures-DomainController**|Cause : Un échec de connexion NTLM se produit pour un compte qui figure dans le groupe de sécurité Utilisateurs protégés.<br /><br />Une erreur est consignée dans le contrôleur de domaine pour indiquer l'échec de l'authentification NTLM en raison de l'appartenance du compte au groupe de sécurité Utilisateurs protégés.<br /><br />Affiche le nom du compte et le nom de l'appareil.|
|104<br /><br />**ProtectedUserFailures-DomainController**|Cause : Les types de chiffrement DES ou RC4 sont utilisés pour l'authentification Kerberos et un échec de connexion se produit pour un utilisateur dans le groupe de sécurité Utilisateurs protégés.<br /><br />La pré-authentification Kerberos a échoué, car les types de chiffrement DES et RC4 ne peuvent pas être utilisés quand le compte est membre du groupe de sécurité Utilisateurs protégés.<br /><br />(AES est acceptable.)|
|303<br /><br />**ProtectedUserSuccesses-DomainController**|Cause : Un ticket TGT Kerberos a été correctement émis pour un membre du groupe Utilisateurs protégés.|



## <a name="additional-resources"></a>Ressources supplémentaires

-   [Gestion et protection des informations d'identification](credentials-protection-and-management.md)

-   [Stratégies d’authentification et Silos de stratégies](authentication-policies-and-authentication-policy-silos.md)

-   [Comment configurer des comptes protégés](how-to-configure-protected-accounts.md)


