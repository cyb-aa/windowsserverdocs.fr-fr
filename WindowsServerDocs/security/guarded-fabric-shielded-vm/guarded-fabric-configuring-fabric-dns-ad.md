---
title: Configurer le DNS de l’infrastructure pour les hôtes service Guardian (AD)
ms.prod: windows-server
ms.topic: article
ms.assetid: 074b6d09-f16e-49bf-b88a-377139d35067
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 5b7deafb49083ad55d5a49d1ad3c5e063e91483f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856822"
---
# <a name="configure-the-fabric-dns-for-guarded-hosts"></a>Configurer le DNS de l’infrastructure pour les hôtes service Guardian

>S’applique à Windows Server 2016


>[!IMPORTANT]
>Le mode AD est déconseillé à partir de Windows Server 2019. Pour les environnements où l’attestation de module de plateforme sécurisée n’est pas possible, configurez l' [attestation de clé hôte](guarded-fabric-initialize-hgs-key-mode.md). L’attestation de clé hôte offre une garantie similaire au mode AD et est plus simple à configurer. 

Un administrateur d’infrastructure doit configurer l’infrastructure que le système DNS prend pour permettre aux hôtes service Guardian de résoudre le cluster SGH. Le cluster SGH doit déjà être [configuré par l’administrateur SGH](/WindowsServerDocs/virtualization/guarded-fabric-shielded-vm/guarded-fabric-setting-up-the-host-guardian-service-hgs.md).



[!INCLUDE [Configure fabric DNS](../../../includes/guarded-fabric-configure-fabric-dns.md)] 


## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Configurer SGH DNS et une approbation à sens unique](guarded-fabric-configure-dns-forwarding-and-trust.md)
