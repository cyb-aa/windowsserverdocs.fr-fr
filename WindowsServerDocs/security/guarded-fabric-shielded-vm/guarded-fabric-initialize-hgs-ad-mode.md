---
title: Initialiser à l’aide de l’attestation approuvée par l’administrateur de service SGH
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 664404cb72981e162bca016df14847e684d987c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821830"
---
# <a name="initialize-hgs-using-admin-trusted-attestation"></a>Initialiser à l’aide de l’attestation approuvée par l’administrateur de service SGH

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

>[!IMPORTANT]
>Attestation approuvée par l’administrateur (mode AD) est déconseillée à compter de Windows Server 2019. Pour les environnements où l’attestation TPM n’est pas possible, configurez [héberger l’attestation de clé](guarded-fabric-initialize-hgs-key-mode.md). L’attestation de clé hôte fournit la garantie similaire au mode d’AD et est plus simple à configurer. 


Ces étapes varient selon que vous initialisez SGH dans une nouvelle forêt ou d’une forêt bastion existante :

1. [Initialiser le cluster SGH dans une nouvelle forêt (par défaut)](guarded-fabric-initialize-hgs-ad-mode-default.md)

   - Ou -

   [Initialiser le cluster SGH dans une forêt bastion existante](guarded-fabric-initialize-hgs-ad-mode-bastion.md)

2. [Configurer la redirection DNS dans le domaine de fabric](guarded-fabric-configuring-fabric-dns.md)

3. [Configurer la redirection DNS et une approbation à sens unique dans le domaine SGH](guarded-fabric-configure-dns-forwarding-and-trust.md)



