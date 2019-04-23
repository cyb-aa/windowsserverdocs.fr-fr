---
title: Nouveautés de la Protection des informations d’identification
description: Sécurité de Windows Server
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
ms.openlocfilehash: ec41e85949cb61c8130d8765b4786eefe39ebd0b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855590"
---
# <a name="whats-new-in-credential-protection"></a>Nouveautés de la Protection des informations d’identification

## <a name="credential-guard-for-signed-in-user"></a>Credential Guard pour l’utilisateur connecté

À partir de Windows 10, version 1507, Kerberos et NTLM utilisent la sécurité basée sur la virtualisation pour protéger les secrets de Kerberos et NTLM de l’ouverture de session utilisateur connecté. 

À partir de Windows 10, version 1511, Credential Manager utilise la sécurité basée sur la virtualisation pour protéger les informations d’identification enregistrées de type d’informations d’identification de domaine. Signé-informations d’identification et les informations d’identification de domaine enregistré ne seront pas transmises à un hôte distant à l’aide du Bureau à distance. Credential Guard peut être activé sans verrouillage UEFI.

À partir de Windows 10, version 1607, Mode utilisateur isolé est inclus avec Hyper-V afin qu’il n’est plus est installée séparément pour le déploiement de Credential Guard.

[En savoir plus sur Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/credential-guard).


## <a name="remote-credential-guard-for-signed-in-user"></a>De Credential Guard à distance pour l’utilisateur connecté

À partir de Windows 10, version 1607, à distance de Credential Guard protège les informations d’identification de l’utilisateur connecté lorsque vous utilisez Bureau à distance en protégeant les secrets de Kerberos et NTLM sur le périphérique client. Pour l’hôte distant afin d’évaluer les ressources réseau en tant que l’utilisateur, les demandes d’authentification exigeant que l’appareil client à utiliser les secrets.

À partir de Windows 10, version 1703, à distance de Credential Guard protège les informations d’identification de l’utilisateur fourni lors de l’utilisation du Bureau à distance.

[En savoir plus sur la protection des informations d’identification à distance](https://technet.microsoft.com/itpro/windows/keep-secure/remote-credential-guard).

## <a name="domain-protections"></a>Protections de domaine

Protections de domaine nécessitent un domaine Active Directory.

### <a name="domain-joined-device-support-for-authentication-using-public-key"></a>Prise en charge des appareils joints au domaine pour l’authentification à l’aide de la clé publique

Depuis Windows 10 version 1507 et Windows Server 2016, si un appareil joint au domaine est en mesure d’enregistrer sa clé publique lié avec un contrôleur de domaine (DC) Windows Server 2016, puis l’appareil peut s’authentifier avec la clé publique à l’aide de Kerberos PKINIT authentification à un contrôleur de domaine Windows Server 2016.

À compter de Windows Server 2016, KDC prennent en charge l’authentification à l’aide d’approbation de clés Kerberos.  

[En savoir plus sur la prise en charge de la clé publique pour les appareils joints au domaine et approbation de clés Kerberos](https://technet.microsoft.com/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication).

### <a name="pkinit-freshness-extension-support"></a>Prise en charge de PKINIT fraîcheur extension

Depuis Windows 10, version 1507 et Windows Server 2016, les clients Kerberos va tenter de l’extension de fraîcheur PKInit pour la clé publique en connexion complémentaires. 

À compter de Windows Server 2016, KDC peut prendre en charge l’extension de fraîcheur PKInit.  Par défaut, le KDC n’offre pas l’extension de fraîcheur PKInit. 

[En savoir plus sur la prise en charge PKINIT fraîcheur extension](https://technet.microsoft.com/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication).

### <a name="rolling-public-key-only-users-ntlm-secrets"></a>Propagée publique clé uniquement NTLM secrets des utilisateurs

À compter de niveau fonctionnel du domaine Windows Server 2016 (niveau fonctionnel du domaine), contrôleurs de domaine peuvent prendre en charge roulement des secrets NTLM publique clé uniquement d’un utilisateur. Cette fonctionnalité est disponible dans DFLs inférieur.

> [!WARNING] 
> Ajout d’un contrôleur de domaine à un domaine avec propagées secrets NTLM est activées avant que le contrôleur de domaine a été mis à jour au moins 8 novembre 2016 de maintenance s’exécute le risque du contrôleur de domaine se bloque. 

Configuration : Pour de nouveaux domaines, cette fonctionnalité est activée par défaut. Pour des domaines existants, il doit être configuré dans le centre d’administration Active Directory : 

1. À partir du centre d’administration Active Directory, cliquez sur le domaine dans le volet gauche et sélectionnez **propriétés**.

    ![Propriétés de domaine](../media/Credentials-Protection-And-Management/domain-properties.png)
    
2. Sélectionnez **activer le déploiement des secrets NTLM arrivant à expiration lors de l’inscription, pour les utilisateurs qui sont nécessaires pour utiliser Microsoft Passport ou carte à puce pour l’ouverture de session interactive**.

    ![Secrets NTLM Autoroll arrivant à expiration](../media/Credentials-Protection-And-Management/autoroll-ntlm.png)

3. Cliquez sur **OK**. 

### <a name="allowing-network-ntlm-when-user-is-restricted-to-specific-domain-joined-devices"></a>Autorise le réseau NTLM lors de l’utilisateur est limité à des appareils spécifiques à un domaine

À compter de Windows Server 2016 niveau fonctionnel du domaine (DFL), contrôleurs de domaine peut prendre en charge réseau autorisant NTLM lorsqu’un utilisateur est limité à des appareils joints au domaine. Cette fonctionnalité n’est pas disponible dans DFLs inférieur.

Configuration : Dans la stratégie d’authentification, cliquez sur **l’authentification NTLM autoriser réseau lorsque l’utilisateur est limité à des appareils sélectionnés**. 

[En savoir plus sur les stratégies d’authentification](https://technet.microsoft.com/windows-server-docs/security/credentials-protection-and-management/authentication-policies-and-authentication-policy-silos).
