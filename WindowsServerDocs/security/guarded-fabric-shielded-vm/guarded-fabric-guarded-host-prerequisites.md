---
title: Conditions préalables pour l’hôte service Guardian
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 8a9273eef906130b11b98148cf1e84f7e18812b0
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79322491"
---
# <a name="prerequisites-for-guarded-hosts"></a>Conditions préalables pour les hôtes service Guardian

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Passez en revue les conditions préalables de l’ordinateur hôte pour le mode d’attestation que vous avez choisi, puis cliquez sur l’étape suivante pour ajouter des hôtes service Guardian.

## <a name="tpm-trusted-attestation"></a>Attestation approuvée par le module de plateforme sécurisée

Les hôtes service Guardian utilisant le mode TPM doivent remplir les conditions préalables suivantes :

-   **Matériel**: un ordinateur hôte est requis pour le déploiement initial. Pour tester la migration dynamique Hyper-V pour les machines virtuelles dotées d’une protection maximale, vous devez disposer d’au moins deux ordinateurs hôtes.

    Les hôtes doivent avoir :
    
    - IOMMU et traduction d’adresses de second niveau (SLAT)
    - TPM 2.0
    - UEFI 2.3.1 ou version ultérieure
    - Configuré pour démarrer à l’aide d’UEFI (et non du BIOS ou en mode « hérité »)
    - Démarrage sécurisé activé
        
-   **Système d’exploitation**: Windows Server 2016 Datacenter Edition ou version ultérieure

    > [!IMPORTANT]
    > Veillez à installer la [dernière mise à jour cumulative](https://support.microsoft.com/help/4000825/windows-10-and-windows-server-2016-update-history).  

-   **Rôle et fonctionnalités**: rôle Hyper-v et fonctionnalité de prise en charge d’Hyper-v Guardian hôte. La fonctionnalité de prise en charge d’Hyper-V Guardian hôte est uniquement disponible dans les éditions Datacenter de Windows Server. 

> [!WARNING]
> La fonctionnalité de prise en charge d’Hyper-V Guardian hôte permet une protection basée sur la virtualisation de l’intégrité du code qui peut être incompatible avec certains appareils. Nous vous recommandons vivement de tester cette configuration dans votre laboratoire avant d’activer cette fonctionnalité. Dans le cas contraire, cela risque d’entraîner des échecs inattendus pouvant aller jusqu'à la perte de données ou un écran bleu d’erreur (également appelé erreur d’arrêt). Pour plus d’informations, consultez [compatibilité matérielle avec la protection de l’intégrité du code basée sur la virtualisation Windows Server](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md).

**Étape suivante :** 
> [!div class="nextstepaction"]
> [Capturer les informations du module de plateforme sécurisée](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md)

## <a name="host-key-attestation"></a>Attestation de clé hôte

Les hôtes service Guardian utilisant l’attestation de clé d’hôte doivent remplir les conditions préalables suivantes :

- **Matériel**: tout serveur susceptible d’exécuter Hyper-V à compter de Windows Server 2019
- **Système d’exploitation**: Windows Server 2019 Datacenter Edition
- **Rôle et fonctionnalités**: rôle Hyper-v et fonctionnalité de prise en charge d’Hyper-v Guardian hôte 

L’hôte peut être joint à un domaine ou à un groupe de travail. 

Pour l’attestation de clé hôte, SGH doit exécuter Windows Server 2019 et fonctionner avec l’attestation v2. Pour plus d’informations, consultez [conditions préalables du SGH](guarded-fabric-prepare-for-hgs.md#prerequisites). 

**Étape suivante :** 
> [!div class="nextstepaction"]
> [Créer une paire de clés](guarded-fabric-create-host-key.md)

## <a name="admin-trusted-attestation"></a>Attestation approuvée par l’administrateur

>[!IMPORTANT]
>L’attestation approuvée par l’administrateur (mode AD) est déconseillée à compter de Windows Server 2019. Pour les environnements où l’attestation de module de plateforme sécurisée n’est pas possible, configurez l' [attestation de clé hôte](#host-key-attestation). L’attestation de clé hôte offre une garantie similaire au mode AD et est plus simple à configurer. 

Les ordinateurs hôtes Hyper-V doivent remplir les conditions préalables suivantes pour le mode AD :

-   **Matériel**: tout serveur susceptible d’exécuter Hyper-V à compter de Windows Server 2016. Un hôte est requis pour le déploiement initial. Pour tester la migration dynamique Hyper-V pour les machines virtuelles dotées d’une protection maximale, vous devez disposer d’au moins deux ordinateurs hôtes.

-   **Système d’exploitation**: Windows Server 2016 Datacenter Edition

    > [!IMPORTANT]
    > Installez la [dernière mise à jour cumulative](https://support.microsoft.com/help/4000825/windows-10-and-windows-server-2016-update-history).

-   **Rôle et fonctionnalités**: rôle Hyper-v et la fonctionnalité de prise en charge d’Hyper-v Guardian hôte, disponible uniquement dans Windows Server 2016 Datacenter Edition. 

> [!WARNING]
> La fonctionnalité de prise en charge d’Hyper-V Guardian hôte permet une protection basée sur la virtualisation de l’intégrité du code qui peut être incompatible avec certains appareils. Nous vous recommandons vivement de tester cette configuration dans votre laboratoire avant d’activer cette fonctionnalité. Dans le cas contraire, cela risque d’entraîner des échecs inattendus pouvant aller jusqu'à la perte de données ou un écran bleu d’erreur (également appelé erreur d’arrêt). Pour plus d’informations, consultez [matériel compatible avec la protection de l’intégrité du code basée sur la virtualisation Windows Server 2016](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md).

**Étape suivante :** 
> [!div class="nextstepaction"]
> [Placer des hôtes service Guardian dans un groupe de sécurité](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md)