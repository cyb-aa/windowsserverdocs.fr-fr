---
title: Initialiser SGH à l’aide d’une attestation approuvée par l’administrateur
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 491754e8bcaad4524084604b78c7c6ed0fdee295
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402359"
---
# <a name="initialize-hgs-using-admin-trusted-attestation"></a>Initialiser SGH à l’aide d’une attestation approuvée par l’administrateur

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

>[!IMPORTANT]
>L’attestation approuvée par l’administrateur (mode AD) est déconseillée à compter de Windows Server 2019. Pour les environnements où l’attestation de module de plateforme sécurisée n’est pas possible, configurez l' [attestation de clé hôte](guarded-fabric-initialize-hgs-key-mode.md). L’attestation de clé hôte offre une garantie similaire au mode AD et est plus simple à configurer. 


Ces étapes varient selon que vous initialisez le SGH dans une nouvelle forêt ou dans une forêt bastion existante :

1. [Initialiser le cluster SGH dans une nouvelle forêt (par défaut)](guarded-fabric-initialize-hgs-ad-mode-default.md)

   \- Ou -

   [Initialiser le cluster SGH dans une forêt bastion existante](guarded-fabric-initialize-hgs-ad-mode-bastion.md)

2. [Configurer le transfert DNS dans le domaine de l’infrastructure](guarded-fabric-configuring-fabric-dns.md)

3. [Configurer le transfert DNS et une approbation unidirectionnelle dans le domaine SGH](guarded-fabric-configure-dns-forwarding-and-trust.md)



