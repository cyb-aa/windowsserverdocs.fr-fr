---
title: Configurer l’infrastructure DNS pour les hôtes service Guardian (AD)
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 074b6d09-f16e-49bf-b88a-377139d35067
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 8a2ddc0f2ab495b2500d4bfe48f3ee83c333c769
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842750"
---
# <a name="configure-the-fabric-dns-for-guarded-hosts"></a>Configuration de l’infrastructure DNS pour les hôtes service Guardian

>S'applique à : Windows Server 2016


>[!IMPORTANT]
>Mode AD est déconseillé à compter de Windows Server 2019. Pour les environnements où l’attestation TPM n’est pas possible, configurez [héberger l’attestation de clé](guarded-fabric-initialize-hgs-key-mode.md). L’attestation de clé hôte fournit la garantie similaire au mode d’AD et est plus simple à configurer. 

Un administrateur d’infrastructure a besoin configurer l’infrastructure que DNS nécessaire pour autoriser des hôtes service Guardian résoudre le cluster SGH. Le SGH cluster doit déjà être [est configuré par l’administrateur SGH](/WindowsServerDocs/virtualization/guarded-fabric-shielded-vm/guarded-fabric-setting-up-the-host-guardian-service-hgs.md).



[!INCLUDE [Configure fabric DNS](../../../includes/guarded-fabric-configure-fabric-dns.md)] 


## <a name="next-step"></a>Étape suivante

>[!div class="nextstepaction"]
[Configurer HGS DNS et une approbation à sens unique](guarded-fabric-configure-dns-forwarding-and-trust.md)
