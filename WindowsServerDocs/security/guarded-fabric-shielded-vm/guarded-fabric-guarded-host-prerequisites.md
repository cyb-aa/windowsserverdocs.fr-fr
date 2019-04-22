---
title: Conditions préalables des hôtes service Guardian
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 5f2c3ec4b2c434ea945d86c4b1593e2e416a5123
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819230"
---
# <a name="prerequisites-for-guarded-hosts"></a>Conditions préalables pour les hôtes service Guardian

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Passez en revue les conditions préalables hôte pour le mode d’attestation que vous avez choisie, puis cliquez sur l’étape suivante pour ajouter des hôtes service Guardian.

## <a name="tpm-trusted-attestation"></a>Attestation approuvée par le module de plateforme sécurisée

Hôtes service Guardian à l’aide du mode de module de plateforme sécurisée doivent respecter les conditions préalables suivantes :

-   **Matériel**: Un ordinateur hôte est requis pour le déploiement initial. Pour tester la migration dynamique d’Hyper-V pour les machines virtuelles protégées, vous devez disposer d’au moins deux hôtes.

    Ordinateurs hôtes doivent avoir :
    
    - IOMMU et deuxième traduction d’adresse niveau (SLAT)
    - TPM 2.0
    - UEFI 2.3.1 ou version ultérieure
    - Configurés pour un démarrage à l’aide d’UEFI (pas le BIOS ou le mode « hérité »)
    - Démarrage sécurisé activé
        
-   **Système d’exploitation**: Windows Server 2016 Datacenter edition ou version ultérieure

    > [!IMPORTANT]
    > Veillez à installer le [dernière mise à jour cumulative](https://support.microsoft.com/help/4000825/windows-10-and-windows-server-2016-update-history).  

-   **Rôle et fonctionnalités**: Rôle Hyper-V et la fonctionnalité de Support de Hyper-V Guardian hôte. La fonctionnalité de Support de Hyper-V Guardian hôte est uniquement disponible sur le centre de données les éditions de Windows Server. 

> [!WARNING]
> La fonctionnalité de Support de Hyper-V Guardian hôte active basés sur la virtualisation de protection de l’intégrité du code qui peut-être être incompatible avec certains appareils. Nous vous recommandons vivement de tester cette configuration dans votre laboratoire avant d’activer cette fonctionnalité. Dans le cas contraire, cela risque d’entraîner des échecs inattendus pouvant aller jusqu'à la perte de données ou un écran bleu d’erreur (également appelé erreur d’arrêt). Pour plus d’informations, consultez [matériel Compatible avec la protection sur la virtualisation Windows Server de l’intégrité du Code](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md).

**Étape suivante :** 
>[!div class="nextstepaction"]
[Capturer les informations de module de plateforme sécurisée](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md)

## <a name="host-key-attestation"></a>Attestation de clé hôte

Les hôtes service Guardian à l’aide de l’attestation de clé hôte doivent respecter les conditions préalables suivantes :

- **Matériel**: N’importe quel serveur capable d’exécuter le début de Hyper-V avec Windows Server 2019
- **Système d’exploitation**: Windows Server 2019 Datacenter edition
- **Rôle et fonctionnalités**: Rôle Hyper-V et la fonctionnalité de Support de Hyper-V Guardian hôte 

L’hôte peut être jointe à un domaine ou un groupe de travail. 

Pour l’attestation de clé hôte, SGH doit être Windows Server 2019 en cours d’exécution et d’exploitation avec l’attestation de v2. Pour plus d’informations, consultez [conditions préalables SGH](guarded-fabric-prepare-for-hgs.md#prerequisites). 

**Étape suivante :** 
>[!div class="nextstepaction"]
[Créer une paire de clés](guarded-fabric-create-host-key.md)

## <a name="admin-trusted-attestation"></a>Attestation approuvée par l’administrateur

>[!IMPORTANT]
>Attestation approuvée par l’administrateur (mode AD) est déconseillée à compter de Windows Server 2019. Pour les environnements où l’attestation TPM n’est pas possible, configurez [héberger l’attestation de clé](#host-key-attestation). L’attestation de clé hôte fournit la garantie similaire au mode d’AD et est plus simple à configurer. 

Hôtes Hyper-V doivent respecter les conditions préalables suivantes pour le mode AD :

-   **Matériel**: N’importe quel serveur capable d’exécuter le début de Hyper-V avec Windows Server 2016. Un ordinateur hôte est requis pour le déploiement initial. Pour tester la migration dynamique d’Hyper-V pour les machines virtuelles protégées, vous avez besoin d’au moins deux hôtes.

-   **Système d’exploitation**: Windows Server 2016 Datacenter edition

    > [!IMPORTANT]
    > Installer le [dernière mise à jour cumulative](https://support.microsoft.com/help/4000825/windows-10-and-windows-server-2016-update-history).

-   **Rôle et fonctionnalités**: Rôle Hyper-V et la fonctionnalité de prise en charge de Hyper-V de Guardian hôte, qui est uniquement disponible dans Windows Server 2016 Datacenter edition. 

> [!WARNING]
> La fonctionnalité de Support de Hyper-V Guardian hôte active basés sur la virtualisation de protection de l’intégrité du code qui peut-être être incompatible avec certains appareils. Nous vous recommandons vivement de tester cette configuration dans votre laboratoire avant d’activer cette fonctionnalité. Dans le cas contraire, cela risque d’entraîner des échecs inattendus pouvant aller jusqu'à la perte de données ou un écran bleu d’erreur (également appelé erreur d’arrêt). Pour plus d’informations, consultez [matériel Compatible avec la protection d’intégrité du Code basé sur la virtualisation Windows Server 2016](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md).

**Étape suivante :** 
>[!div class="nextstepaction"]
[Placer les hôtes service Guardian dans un groupe de sécurité](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md)