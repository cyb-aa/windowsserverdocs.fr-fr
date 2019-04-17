---
title: "Nouveautés de la Protection des informations d’identification"
description: "Sécurité de Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b0b5180-f65a-43ac-8ef3-66014116f297
author: gitmichiko
ms.author: michikos
manager: dongill
ms.date: 03/06/2017
ms.openlocfilehash: 0556c606b987a69eae663b0196467f532d5a307a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="whats-new-in-credential-protection"></a>Nouveautés de la Protection des informations d’identification

## <a name="credential-guard-for-signed-in-user"></a>Protection des informations d’identification pour l’utilisateur connecté

À compter de Windows 10, version 1507, Kerberos et NTLM utilisent la sécurité basée sur la virtualisation de protéger les secrets Kerberos et NTLM de l’ouverture de session utilisateur connecté. 

À partir de Windows 10, version 1511, Credential Manager utilise la sécurité basée sur la virtualisation pour protéger les informations d’identification enregistrées du type d’informations d’identification de domaine. Signé informations d’identification et les informations d’identification de domaine enregistré ne seront pas passées à un hôte distant à l’aide des services Bureau à distance. Credential Guard peut être activé sans verrouillage UEFI.

À compter de Windows 10, version 1607, Mode utilisateur isolé est inclus avec Hyper-V afin qu’il n’est plus est installée séparément pour le déploiement de Credential Guard.

[En savoir plus sur Credential Guard](https://technet.microsoft.com/en-us/itpro/windows/keep-secure/credential-guard).


## <a name="remote-credential-guard-for-signed-in-user"></a>Credential Guard à distance pour l’utilisateur connecté

Credential Guard à distance à partir de Windows 10, version 1607, protège les informations d’identification de l’utilisateur connecté lors de l’utilisation des services Bureau à distance en protégeant les secrets Kerberos et NTLM sur le périphérique client. Pour l’hôte distant afin d’évaluer les ressources réseau en tant que l’utilisateur, les demandes d’authentification nécessitent l’appareil client à utiliser les secrets.

Credential Guard à distance à partir de Windows 10, version 1703, protège les informations d’identification d’utilisateur fourni lors de l’utilisation des services Bureau à distance.

[En savoir plus sur la protection des informations d’identification à distance](https://technet.microsoft.com/en-us/itpro/windows/keep-secure/remote-credential-guard).

## <a name="domain-protections"></a>Protections de domaine

Protections de domaine nécessitent un domaine Active Directory.

### <a name="domain-joined-device-support-for-authentication-using-public-key"></a>Prise en charge des appareils joints au domaine pour l’authentification à l’aide de la clé publique

Compter de Windows 10 version 1507 et Windows Server 2016, si un appareil joint au domaine est en mesure d’enregistrer sa clé publique lié avec un contrôleur de domaine Windows Server 2016 (DC), puis l’appareil peut s’authentifier avec la clé publique à l’aide de l’authentification Kerberos PKINIT à un contrôleur de domaine Windows Server 2016.

À compter de Windows Server 2016, KDC prennent en charge l’authentification par approbation de clés Kerberos.  

[En savoir plus sur la prise en charge de la clé publique pour les appareils joints au domaine et approbation de clés Kerberos](https://technet.microsoft.com/en-us/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication).

### <a name="pkinit-freshness-extension-support"></a>Prise en charge de rafraîchissement du PKINIT extension

À partir de Windows 10, version 1507 et Windows Server 2016, les clients Kerberos tente de l’extension de rafraîchissement du PKInit de clé publique en fonction connexion complémentaires. 

À compter de Windows Server 2016, KDC peut prendre en charge l’extension de rafraîchissement du PKInit.  Par défaut, le KDC n’offre pas l’extension de rafraîchissement du PKInit. 

[En savoir plus sur la prise en charge des extensions de rafraîchissement du PKINIT](https://technet.microsoft.com/en-us/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication).

### <a name="rolling-public-key-only-users-ntlm-secrets"></a>Propagée secrets NTLM publique clé uniquement de l’utilisateur

À compter de niveau fonctionnel du domaine Windows Server 2016 (DFL), les contrôleurs de domaine peuvent prendre en charge propagée secrets NTLM publique clé uniquement d’un utilisateur. Cette fonctionnalité est disponible dans DFLs inférieurs.

> [!WARNING] 
> Ajout d’un contrôleur de domaine à un domaine avec propagée secrets NTLM activés avant que le contrôleur de domaine a été mis à jour au moins 8 novembre 2016 maintenance s’exécute le risque du blocage du contrôleur de domaine. 

Configuration: Pour les nouveaux domaines, cette fonctionnalité est activée par défaut. Pour des domaines existants, il doit être configuré dans le centre d’administration Active Directory: 

1. À partir du centre d’administration Active Directory, cliquez sur le domaine dans le volet gauche, puis sélectionnez **propriétés**.

    ![Propriétés du domaine](../media/Credentials-Protection-And-Management/domain-properties.png)
    
2. Sélectionnez **activer rouler arrivant à expiration secrets NTLM lors de l’inscription, pour les utilisateurs sont tenus d’utiliser Microsoft Passport ou carte à puce pour l’ouverture de session interactive**.

    ![Secrets NTLM Autoroll arrivant à expiration](../media/Credentials-Protection-And-Management/autoroll-ntlm.png)

3. Cliquez sur **OK**. 

### <a name="allowing-network-ntlm-when-user-is-restricted-to-specific-domain-joined-devices"></a>Autoriser le réseau NTLM lorsque l’utilisateur est limité à des appareils spécifiques joints au domaine

À compter de Windows Server 2016 niveau fonctionnel du domaine (DFL), les contrôleurs de domaine peut prendre en charge réseau permettant NTLM lorsqu’un utilisateur est limité à des appareils joints au domaine. Cette fonctionnalité n’est pas disponible dans DFLs inférieurs.

Configuration: Dans la stratégie d’authentification, cliquez sur **sélectionné l’authentification NTLM autoriser réseau, lorsque l’utilisateur est limité à des périphériques**. 

[En savoir plus sur les stratégies d’authentification](https://technet.microsoft.com/en-us/windows-server-docs/security/credentials-protection-and-management/authentication-policies-and-authentication-policy-silos).
