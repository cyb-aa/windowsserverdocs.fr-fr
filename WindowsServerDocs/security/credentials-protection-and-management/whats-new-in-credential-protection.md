---
title: Nouveautés de la protection des informations d’identification
description: Sécurité de Windows Server
ms.prod: windows-server
ms.technology: security-credential-protection
ms.topic: article
ms.assetid: 1b0b5180-f65a-43ac-8ef3-66014116f297
author: gitmichiko
ms.author: michikos
manager: dongill
ms.date: 03/06/2017
ms.openlocfilehash: 35097cee243239735995a00cec7a6fd3936c62a8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857042"
---
# <a name="whats-new-in-credential-protection"></a>Nouveautés de la protection des informations d’identification

## <a name="credential-guard-for-signed-in-user"></a>Credential Guard pour l’utilisateur connecté

À compter de Windows 10, version 1507, Kerberos et NTLM utilisent la sécurité basée sur la virtualisation pour protéger Kerberos & secrets NTLM de la session d’ouverture de session utilisateur connectée. 

À compter de Windows 10, version 1511, le gestionnaire d’informations d’identification utilise la sécurité basée sur la virtualisation pour protéger les informations d’identification enregistrées du type d’informations d’identification de domaine. Les informations d’identification de connexion et les informations d’identification de domaine enregistrées ne seront pas transmises à un hôte distant à l’aide du Bureau à distance. Credential Guard peut être activé sans le verrou UEFI.

À partir de Windows 10, version 1607, le mode utilisateur isolé est inclus dans Hyper-V, de sorte qu’il n’est plus installé séparément pour le déploiement de la protection des informations d’identification.

[En savoir plus sur Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/credential-guard).


## <a name="remote-credential-guard-for-signed-in-user"></a>Remote Credential Guard pour l’utilisateur connecté

À compter de Windows 10, version 1607, Credential protection à distance protège les informations d’identification de l’utilisateur connecté lors de l’utilisation de Bureau à distance en protégeant les secrets Kerberos et NTLM sur le périphérique client. Pour que l’hôte distant évalue les ressources réseau en tant qu’utilisateur, les demandes d’authentification requièrent que l’appareil client utilise les secrets.

À compter de Windows 10, version 1703, Credential Guard à distance protège les informations d’identification de l’utilisateur fournies lors de l’utilisation de Bureau à distance.

[En savoir plus sur Credential Guard à distance](https://technet.microsoft.com/itpro/windows/keep-secure/remote-credential-guard).

## <a name="domain-protections"></a>Protections de domaine

Les protections de domaine nécessitent un domaine Active Directory.

### <a name="domain-joined-device-support-for-authentication-using-public-key"></a>Prise en charge d’appareils joints à un domaine pour l’authentification à l’aide de la clé publique

À compter de Windows 10 version 1507 et de Windows Server 2016, si un appareil joint à un domaine est en mesure d’inscrire sa clé publique liée auprès d’un contrôleur de domaine Windows Server 2016, l’appareil peut s’authentifier avec la clé publique à l’aide de l’authentification PKINIT Kerberos sur un contrôleur de domaine Windows Server 2016.

À partir de Windows Server 2016, les KDC prennent en charge l’authentification à l’aide de la confiance de clé Kerberos.  

En [savoir plus sur la prise en charge des clés publiques pour les appareils joints à un domaine & approbation de clé Kerberos](https://technet.microsoft.com/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication).

### <a name="pkinit-freshness-extension-support"></a>Prise en charge de l’extension d’actualisation de PKINIT

À compter de Windows 10, version 1507 et Windows Server 2016, les clients Kerberos tentent d’effectuer l’extension d’actualisation de PKInit pour les connexions basées sur des clés publiques. 

À partir de Windows Server 2016, les KDC peuvent prendre en charge l’extension d’actualisation de PKInit.  Par défaut, les KDC n’offrent pas l’extension d’actualisation de PKInit. 

[En savoir plus sur la prise en charge de l’extension d’actualisation de PKINIT](https://technet.microsoft.com/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication).

### <a name="rolling-public-key-only-users-ntlm-secrets"></a>Répercussion de la clé publique uniquement des secrets NTLM de l’utilisateur

À partir du niveau fonctionnel de domaine (DFL) de Windows Server 2016, les contrôleurs de domaine peuvent prendre en charge la restauration d’une clé publique uniquement des secrets NTLM de l’utilisateur. Cette fonctionnalité est unavailble dans les DFLs inférieurs.

> [!WARNING] 
> L’ajout d’un contrôleur de domaine à un domaine avec des secrets NTLM enchaînés étant activé avant la mise à jour du contrôleur de domaine avec au moins le 8 novembre, la maintenance de 2016 exécute le risque de panne de contrôleur de domaine. 

Configuration : pour les nouveaux domaines, cette fonctionnalité est activée par défaut. Pour les domaines existants, elle doit être configurée dans le centre d’administration Active Directory : 

1. Dans le centre d’administration Active Directory, cliquez avec le bouton droit sur le domaine dans le volet gauche et sélectionnez **Propriétés**.

    ![Propriétés du domaine](../media/Credentials-Protection-And-Management/domain-properties.png)

2. Sélectionnez **activer le déploiement des secrets NTLM arrivant à expiration pendant l’authentification pour les utilisateurs qui doivent utiliser Microsoft Passport ou une carte à puce pour une ouverture de session interactive**.

    ![Annulation des secrets NTLM arrivant à expiration](../media/Credentials-Protection-And-Management/autoroll-ntlm.png)

3. Cliquez sur **OK**. 

### <a name="allowing-network-ntlm-when-user-is-restricted-to-specific-domain-joined-devices"></a>Autorisation du réseau NTLM quand l’utilisateur est limité à des appareils spécifiques joints à un domaine

À partir du niveau fonctionnel de domaine (DFL) de Windows Server 2016, les contrôleurs de domaine peuvent prendre en charge l’autorisation du réseau NTLM lorsqu’un utilisateur est limité à des appareils spécifiques joints à un domaine. Cette fonctionnalité n’est pas disponible dans les DFLs inférieurs.

Configuration : dans la stratégie d’authentification, cliquez sur **autoriser l’authentification réseau NTLM lorsque l’utilisateur est limité à des appareils sélectionnés**. 

[En savoir plus sur les stratégies d’authentification](https://technet.microsoft.com/windows-server-docs/security/credentials-protection-and-management/authentication-policies-and-authentication-policy-silos).
