---
title: "Groupe de sécurité utilisateurs protégés"
description: "Sécurité de Windows Server"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="protected-users-security-group"></a>Groupe de sécurité utilisateurs protégés

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique destinée aux professionnels de l’informatique décrit le groupe de sécurité ActiveDirectory nommé utilisateurs protégés et explique son fonctionnement. Ce groupe a été introduit dans les contrôleurs de domaine Windows Server2012R2.

## <a name="BKMK_ProtectedUsers"></a>Vue d’ensemble

Ce groupe de sécurité est conçu dans le cadre d’une stratégie pour gérer l’exposition des informations d’identification de l’entreprise. Les membres de ce groupe ont automatiquement appliquées à leurs comptes de protections non configurables. L’appartenance au groupe utilisateurs protégés est censée être restrictive et sécurisée de manière proactive par défaut. La seule méthode pour modifier ces protections pour un compte consiste à supprimer le compte du groupe de sécurité.

> [!WARNING]
> Comptes de services et les ordinateurs ne doivent jamais être membres du groupe utilisateurs protégés. Ce groupe serait offre une protection incomplète malgré tout, car le mot de passe ou le certificat est toujours disponible sur l’ordinateur hôte. L’authentification échoue avec l’erreur \"the nom d’utilisateur ou le mot de passe est incorrect\» pour n’importe quel service ou d’un ordinateur qui est ajouté au groupe utilisateurs protégés.

Ce groupe global lié au domaine déclenche une protection non configurable sur les périphériques et les ordinateurs hôtes exécutant Windows Server2012R2 et Windows8.1 ou versions ultérieures pour les utilisateurs dans les domaines avec un contrôleur de domaine principal exécutant Windows Server2012R2. Cela réduit considérablement l’encombrement de mémoire par défaut des informations d’identification quand les utilisateurs connectez-vous aux ordinateurs avec ces protections.

Pour plus d’informations, voir [comment les utilisateurs protégés groupe fonctionne](#BKMK_HowItWorks) dans cette rubrique.



## <a name="BKMK_Requirements"></a>Exigences en matière de groupe utilisateurs protégés
Conditions requises pour fournir les protections de périphérique pour les membres du groupe utilisateurs protégés sont les suivantes:

- Le groupe de sécurité global utilisateurs protégés est répliqué sur tous les contrôleurs de domaine dans le domaine du compte.

- Windows8.1 et Windows Server2012R2 prise en charge par défaut. [Avis de sécurité Microsoft2871997](https://technet.microsoft.com/library/security/2871997) ajoute la prise en charge pour Windows7, Windows Server2008R2 et Windows Server2012.

Conditions requises pour fournir la protection de contrôleur de domaine pour les membres du groupe utilisateurs protégés sont les suivantes:

- Les utilisateurs doivent être dans les domaines qui sont Windows Server2012R2 ou supérieur niveau fonctionnel du domaine.

### <a name="adding-protected-user-global-security-group-to-down-level-domains"></a>Groupe de sécurité global utilisateurs protégés ajout à des domaines de niveau inférieur

Les contrôleurs de domaine qui exécutent un système d’exploitation antérieurs à Windows Server2012R2 peuvent prendre en charge l’ajout de membres au nouveau groupe de sécurité utilisateurs protégés. Cela permet aux utilisateurs de bénéficier de protections périphérique avant que le domaine est mis à niveau. 

> [!Note]
> Les contrôleurs de domaine ne seront pas en charge les protections de domaine. 

Groupe utilisateurs protégés peut être créé en [transférant le rôle d’émulateur de contrôleur principal de domaine](https://technet.microsoft.com/library/cc816944(v=ws.10).aspx) à un contrôleur de domaine qui exécute Windows Server2012R2. Une fois l’objet de groupe est répliqué vers d’autres contrôleurs de domaine, le rôle d’émulateur PDC peut être hébergé sur un contrôleur de domaine qui exécute une version antérieure de Windows Server.

### <a name="BKMK_ADgroup"></a>Propriétés du groupe ActiveDirectory utilisateurs protégées

Le tableau suivant spécifie les propriétés du groupe utilisateurs protégés.

|Attribut|Valeur|
|-------|-----|
|SID/RID connu|S-1-5-21 -<domain>-525|
|Type|Global du domaine|
|Conteneur par défaut|CN = Users, DC =<domain>, DC =|
|Membres par défaut|None|
|Membre par défaut de|None|
|Protégé par ADMINSDHOLDER?|N°|
|Sortie du conteneur par défaut sécurisée?|Oui|
|Sécurisée pour déléguer la gestion de ce groupe pour les administrateurs non-service?|N°|
|Droits d’utilisateur par défaut|Sans droits d’utilisateur par défaut|

## <a name="BKMK_HowItWorks"></a>Fonctionne du groupe utilisateurs protégés
Cette section explique comment le groupe utilisateurs protégés fonctionne quand:

-   Enregistré dans un appareil Windows

-   Domaine de compte d’utilisateur est dans un Windows Server2012R2 ou le niveau fonctionnel du domaine supérieur

### <a name="device-protections-for-signed-in-protected-users"></a>Des protections pour appareil connecté utilisateurs protégés
Lorsque l’utilisateur connecté est un membre du groupe utilisateurs protégés, les protections suivantes sont appliquées:

-   Des informations d’identification seront délégation (CredSSP) pas mettre en cache les informations d’identification en texte brut même lorsque le **autoriser la délégation des informations d’identification par défaut** paramètre de stratégie de groupe est activé.

-   À compter de Windows8.1 et Windows Server2012R2, Windows Digest ne met pas en cache les informations d’identification en texte brut même quand Windows Digest est activé.

> [!Note]
> Après l’installation [avis de sécurité Microsoft2871997](https://technet.microsoft.com/library/security/2871997) Windows Digest continue des informations d’identification du cache jusqu'à ce que la clé de Registre est configurée. Voir [avis de sécurité Microsoft: informations d’identification de mise à jour pour améliorer la gestion et protection: 13 mai 2014](https://support.microsoft.com/en-us/help/2871997/microsoft-security-advisory-update-to-improve-credentials-protection-a) pour obtenir des instructions.

-   NTLM ne sera pas mettre en cache les informations d’identification en texte brut ou fonction à sens unique NT (NTOWF).

-   Kerberos n’est plus crée DES ou RC4 clés. Également il ne mettra pas en cache informations d’identification en texte brut ou des clés à long terme de l’utilisateur une fois que le ticket TGT initial est obtenu.

-   Aucun vérificateur en cache n’est pas créé à la connexion ou déverrouiller, connectez-vous en mode hors connexion n’est n’est plus prise en charge.

Une fois que le compte d’utilisateur est ajouté au groupe utilisateurs protégés, protection commence lorsque l’utilisateur se connecte à l’appareil.

### <a name="domain-controller-protections-for-protected-users"></a>Protections de contrôleur de domaine pour les utilisateurs protégés
Comptes qui sont membres du groupe utilisateurs protégés qui s’authentifier auprès d’un domaine Windows Server2012R2 ne peuvent pas:

-   S’authentifier avec l’authentification NTLM.

-   Utilisez les types de chiffrement DES ou RC4 dans la pré-authentification Kerberos.

-   Être délégués avec délégation non contrainte ou contrainte.

-   Renouveler les tickets TGT Kerberos au-delà de la durée de vie initiale de 4heures.

Les paramètres non configurables à l’expiration des tickets TGT sont établis pour chaque compte dans le groupe utilisateurs protégés. Normalement, le contrôleur de domaine définit la durée de vie des tickets TGT et de renouvellement, en fonction des stratégies de domaine, **durée de vie maximale du ticket utilisateur** et **durée de vie maximale pour le renouvellement du ticket utilisateur**. Pour le groupe utilisateurs protégés, 600minutes est définie pour ces stratégies de domaine.

Pour plus d’informations, voir [comment configurer des comptes protégés](how-to-configure-protected-accounts.md).

## <a name="troubleshooting"></a>Résolution des problèmes
Deux journaux d’administration opérationnels sont disponibles pour résoudre les événements qui sont associés aux utilisateurs protégés. Ces nouveaux journaux se trouvent dans l’Observateur d’événements et sont désactivées par défaut et se trouvent sous **Applications et des services\microsoft\windows\microsoft\authentification**.

|ID d’événement et journal|Description|
|----------|--------|
|104<br /><br />**ProtectedUser-Client**|Cause: Le package de sécurité sur le client ne contient pas les informations d’identification.<br /><br />L’erreur est enregistrée sur l’ordinateur client lorsque le compte est membre du groupe de sécurité utilisateurs protégés. Cet événement indique que le package de sécurité ne met pas en cache les informations d’identification qui sont nécessaires pour s’authentifier auprès du serveur.<br /><br />Affiche le nom du package, le nom d’utilisateur, le nom de domaine et le nom du serveur.|
|304<br /><br />**ProtectedUser-Client**|Cause: Le package de sécurité ne stocke pas les informations d’identification de l’utilisateur protégé.<br /><br />Un événement d’information est consigné dans le client pour indiquer que le package de sécurité ne met pas en cache identification de connexion de l’utilisateur. Il est prévu que NTLM, Digest (WDigest) et la délégation d’informations d’identification (CredSSP) n’ont pas d’informations d’identification de connexion pour les utilisateurs protégés. Les applications peuvent quand même réussir si elles demandent des informations d’identification.<br /><br />Affiche le nom du package, le nom d’utilisateur et le nom de domaine.|
|100<br /><br />**ProtectedUserFailures-DomainController.**|Cause: Un échec de connexion NTLM se produit pour un compte qui se trouve dans le groupe de sécurité utilisateurs protégés.<br /><br />Une erreur est enregistrée dans le contrôleur de domaine pour indiquer que l’authentification NTLM a échoué car le compte est membre du groupe de sécurité utilisateurs protégés.<br /><br />Affiche le nom de compte et le nom de l’appareil.|
|104<br /><br />**ProtectedUserFailures-DomainController.**|Raison: Les types de chiffrement DES ou RC4 sont utilisés pour l’authentification Kerberos et un échec de connexion se produit pour un utilisateur dans le groupe de sécurité utilisateurs protégés.<br /><br />La pré-authentification Kerberos a échoué car les types de chiffrement DES et RC4 ne peuvent pas être utilisés lorsque le compte est membre du groupe de sécurité utilisateurs protégés.<br /><br />(AES est acceptable).|
|303<br /><br />**ProtectedUserSuccesses-DomainController.**|Cause: Un ticket-granting-ticket Kerberos (TGT) a été correctement émis pour un membre du groupe utilisateurs protégés.|



## <a name="additional-resources"></a>Ressources supplémentaires

-   [Gestion et Protection des informations d’identification](credentials-protection-and-management.md)

-   [Stratégies d’authentification et Silos de stratégies d’authentification](authentication-policies-and-authentication-policy-silos.md)

-   [Comment configurer des comptes protégés](how-to-configure-protected-accounts.md)


