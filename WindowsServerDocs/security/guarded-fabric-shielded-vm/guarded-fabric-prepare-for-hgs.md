---
title: Passez en revue les conditions préalables SGH
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: f4b4d1a8-bf6d-4881-9150-ddeca8b48038
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: dddf694aaceab93bd102456dbe86df17a001cb01
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879880"
---
# <a name="review-prerequisites-for-the-host-guardian-service"></a>Passez en revue les conditions préalables pour le Service Guardian hôte

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016


Cette rubrique traite des conditions préalables SGH et les premières étapes pour préparer le déploiement de SGH.

## <a name="prerequisites"></a>Prérequis 

-   **Matériel**: SGH peut être exécuté sur des ordinateurs physiques ou virtuels, mais les machines physiques sont recommandées.

    Si vous souhaitez exécuter SGH comme un cluster à trois nœuds physique (pour la disponibilité), vous devez disposer de trois serveurs physiques. (Recommandé pour le clustering, les trois serveurs doivent avoir un matériel très similaire).
  
-   **Système d’exploitation**: L’attestation de clé hôte requiert Windows Server 2019 Standard ou Datacenter edition fonctionne avec [v2 attestation](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#versioned-attestation-policies). Attestation de module de plateforme sécurisée, SGH peut exécuter Windows Server 2019 ou Windows Server 2016, Standard ou Datacenter edition.

-   **Rôles de serveur**: Service Guardian hôte et prenant en charge les rôles de serveur.

-   **Configuration des autorisations/des privilèges pour le domaine de fabric (hôte)**: Vous devez configurer la redirection DNS entre le domaine de fabric (hôte) et le service SGH. 
    
## <a name="upgrading-hgs"></a>La mise à niveau SGH

Si vous avez déjà déployé le service SGH et que vous souhaitez mettre à niveau son système d’exploitation, suivez la [des conseils de mise à niveau](guarded-fabric-upgrade-to-2019.md) mise à niveau vos serveurs SGH et Hyper-V vers le dernier système d’exploitation.

## <a name="next-step"></a>Étape suivante

>[!div class="nextstepaction"]
[Obtenir des certificats pour SGH](guarded-fabric-obtain-certs.md)