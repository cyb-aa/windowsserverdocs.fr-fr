---
title: Passer en revue les conditions préalables du SGH
ms.prod: windows-server
ms.topic: article
ms.assetid: f4b4d1a8-bf6d-4881-9150-ddeca8b48038
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 0c6256bb5983b3536e633fe0aca45e973b224b54
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856492"
---
# <a name="review-prerequisites-for-the-host-guardian-service"></a>Vérifier les conditions préalables pour le service Guardian hôte

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016


Cette rubrique décrit les conditions préalables du SGH et les étapes initiales à suivre pour préparer le déploiement du SGH.

## <a name="prerequisites"></a>Composants requis 

-   **Matériel**: SGH peut être exécuté sur des machines physiques ou virtuelles, mais les ordinateurs physiques sont recommandés.

    Si vous souhaitez exécuter SGH en tant que cluster physique à trois nœuds (pour la disponibilité), vous devez disposer de trois serveurs physiques. (En tant que meilleure pratique pour le clustering, les trois serveurs doivent avoir un matériel très similaire.)
  
-   **Système d’exploitation**: l’attestation de clé hôte requiert Windows Server 2019 standard ou Datacenter Edition fonctionnant avec l' [attestation v2](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#versioned-attestation-policies). Pour l’attestation basée sur le module de plateforme sécurisée, SGH peut exécuter Windows Server 2019 ou Windows Server 2016, standard ou Datacenter Edition.

-   **Rôles de serveur**: service Guardian hôte et rôles serveur de prise en charge.

-   **Autorisations/privilèges de configuration pour le domaine de l’infrastructure (hôte)** : vous devrez configurer le transfert DNS entre le domaine de l’infrastructure (hôte) et le domaine SGH. 
    
## <a name="upgrading-hgs"></a>Mise à niveau de SGH

Si vous avez déjà déployé SGH et souhaitez mettre à niveau son système d’exploitation, suivez les [instructions de mise à niveau](guarded-fabric-upgrade-to-2019.md) pour mettre à niveau vos serveurs SGH et Hyper-V vers le système d’exploitation le plus récent.

## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Obtenir des certificats pour SGH](guarded-fabric-obtain-certs.md)