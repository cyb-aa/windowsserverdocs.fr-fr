---
ms.assetid: eefcc989-8763-45ee-8a64-3a97b4397160
title: Opérations d'AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 2db9cd83ed08673835a38e443e90c5eb092f43ac
ms.sourcegitcommit: b649047f161cb605df084f18b573f796a584753b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76162480"
---
# <a name="ad-fs-operations"></a>Opérations d'AD FS



Ce document contient une liste de toutes les opérations de documentation pour AD FS. 

## <a name="service-configuration"></a>Configuration de service
- [Mettre à jour des certificats SSL dans AD FS et WAP 2016](../ad-fs/operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)
- [Outil de restauration rapide AD FS](../ad-fs/operations/AD-FS-Rapid-Restore-Tool.md)
- [Configurer une autre liaison de nom d’hôte pour l’authentification par certificat dans AD FS](../ad-fs/operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)
- [Ajouter un magasin d’attributs](../ad-fs/operations/Add-an-Attribute-Store.md)
- [Personnaliser les en-têtes de réponse de sécurité HTTP avec AD FS 2019](../ad-fs/operations/customize-http-security-headers-ad-fs.md)
- [Déléguer l’accès aux applets de commande Powershell AD FS aux utilisateurs non-administrateurs](../ad-fs/operations/delegate-ad-fs-pshell-access.md)
- [Ajuster la latence SQL et d’adresse](../ad-fs/operations/adfs-sql-latency.md)
- [Groupes de disponibilité AlwaysOn](../ad-fs/operations/ad-fs-always-on.md) 


## <a name="authentication-configuration"></a>Configuration de l'authentification
### <a name="strong-authentication-mfa--password-less"></a>Authentification renforcée (MFA) & sans mot de passe
- [Configurer les fournisseurs d’authentification externes comme principaux dans AD FS (2019 ou version ultérieure)](../ad-fs/operations/Additional-Authentication-Methods-AD-FS.md)
- [Configurer AD FS (2016 ou version ultérieure) et Azure MFA](../ad-fs/operations/Configure-AD-FS-2016-and-Azure-MFA.md)
- [Configurer des méthodes d’authentification supplémentaires pour AD FS](../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)

### <a name="lockout-protection"></a>Verrouillage de la protection
- [Configurer la protection du verrouillage souple extranet AD FS](../ad-fs/operations/Configure-AD-FS-Extranet-Soft-Lockout-Protection.md)
- [Configurer la protection du verrouillage intelligent extranet AD FS](../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md)
- [Configurer des IP interdites extranet AD FS](../ad-fs/operations/Configure-AD-FS-Banned-IP.md)

### <a name="policy-configuration"></a>Configuration de la stratégie
- [Configurer des stratégies d’authentification](../ad-fs/operations/Configure-Authentication-Policies.md)
- [Configuration des ID de connexion alternatif](../ad-fs/operations/Configuring-Alternate-Login-ID.md)
- [Configurer Azure AD comportement de connexion prompt pour qu’il fonctionne avec AD FS stratégie](../ad-fs/operations/AD-FS-Prompt-Login.md)

### <a name="kerberos--certificate-authentication"></a>Authentification par certificat de & Kerberos
- [Activer AD DS les revendications & l’authentification composée Kerberos dans AD FS](../ad-fs/operations/AD-FS-Compound-Authentication-and-AD-DS-claims.md)
- [Configurer AD FS pour l’authentification par certificat utilisateur](../ad-fs/operations/Configure-User-Certificate-Authentication.md)
- [Configurer une autre liaison de nom d’hôte pour l’authentification par certificat dans AD FS](../ad-fs/operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)


### <a name="device"></a>Périphérique
- [Contrôles d’authentification des appareils dans AD FS](../ad-fs/operations/device-authentication-controls-in-AD-FS.md) 


## <a name="authorization-configuration"></a>Configuration de l’autorisation
- [Configurer des stratégies de Access Control dans AD FS](../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)
- [Configurer l’accès conditionnel local basé sur un périphérique](../ad-fs/operations/Configure-Device-based-Conditional-Access-on-Premises.md)

## <a name="rpt--cpt-configuration"></a>Configuration de la & de CPT dans RPT
- [Configuration d’AD FS pour authentifier les utilisateurs stockées dans des annuaires LDAP](../ad-fs/operations/Configure-AD-FS-to-authenticate-users-stored-in-LDAP-directories.md)
- [Configurer les règles de revendication](../ad-fs/operations/Configure-Claim-Rules.md) 
- [Créer une approbation de fournisseur de revendications](../ad-fs/operations/Create-a-Claims-Provider-Trust.md) 
- [Créer une approbation de partie de confiance qui ne prend pas en charge les revendications](../ad-fs/operations/Create-a-Non-Claims-Aware-Relying-Party-Trust.md)
- [Créer une partie de confiance](../ad-fs/operations/Create-a-Relying-Party-Trust.md)
- [Configurer AD FS pour qu’il fonctionne avec le fournisseur de Fédération agrégé (par exemple, inhabituel)](../ad-fs/operations/Improved-interoperability-with-SAML-2.0.md)

## <a name="sign-in-experience-configuration"></a>Configuration de l’expérience de connexion
- [Configurer les paramètres d’authentification unique AD FS 2016](../ad-fs/operations/AD-FS-2016-Single-Sign-On-Settings.md)
- [Configurer AD FS connexion paginée](../ad-fs/operations/AD-FS-paginated-sign-in.md)
- [Configurer AD FS la personnalisation de la connexion de l’utilisateur](../ad-fs/operations/AD-FS-user-sign-in-customization.md)
- [Configurer AD FS pour envoyer les revendications d’expiration de mot de passe](../ad-fs/operations/Configure-AD-FS-to-Send-Password-Expiry-Claims.md)
- [Configurer l’authentification basée sur les formulaires intranet pour les appareils qui ne prennent pas en charge WIA](../ad-fs/operations/Configure-intranet-forms-based-authentication-for-devices-that-do-not-support-WIA.md)

## <a name="other"></a>Autre
- [Joindre un espace de travail à partir de n’importe quel appareil en utilisant l’authentification unique et l’authentification de second facteur transparente](../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
- [Gérer les risques avec une authentification multifacteur supplémentaire pour les applications sensibles](../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
- [Gérer les risques avec le contrôle d’accès conditionnel](../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)
- [Configurer un environnement de laboratoire AD FS](../ad-fs/operations/Set-up-an-AD-FS-lab-environment.md)
- [Guide pas à pas : gérer les risques avec des Multi-Factor Authentication supplémentaires pour les applications sensibles](../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
- [Guide pas à pas : gérer les risques avec des Access Control conditionnelles](../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)
- [Procédure pas à pas : Workplace Join avec un appareil Windows](../ad-fs/operations/Walkthrough--Workplace-Join-with-a-Windows-Device.md)
- [Procédure pas à pas : Workplace Join avec un appareil iOS](../ad-fs/operations/Walkthrough--Workplace-Join-with-an-iOS-Device.md)

  


