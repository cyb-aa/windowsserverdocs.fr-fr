---
title: Initialiser SGH à l’aide d’une attestation approuvée par l’administrateur
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: b7c0b88071a28953ddda8abb57a805ef119511e0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856672"
---
# <a name="initialize-hgs-using-admin-trusted-attestation"></a>Initialiser SGH à l’aide d’une attestation approuvée par l’administrateur

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

>[!IMPORTANT]
>L’attestation approuvée par l’administrateur (mode AD) est déconseillée à compter de Windows Server 2019. Pour les environnements où l’attestation de module de plateforme sécurisée n’est pas possible, configurez l' [attestation de clé hôte](guarded-fabric-initialize-hgs-key-mode.md). L’attestation de clé hôte offre une garantie similaire au mode AD et est plus simple à configurer. 


Ces étapes varient selon que vous initialisez le SGH dans une nouvelle forêt ou dans une forêt bastion existante :

1. [Initialiser le cluster SGH dans une nouvelle forêt (par défaut)](guarded-fabric-initialize-hgs-ad-mode-default.md)

   \- Ou -

   [Initialiser le cluster SGH dans une forêt bastion existante](guarded-fabric-initialize-hgs-ad-mode-bastion.md)

2. [Configurer le transfert DNS dans le domaine de l’infrastructure](guarded-fabric-configuring-fabric-dns.md)

3. [Configurer le transfert DNS et une approbation unidirectionnelle dans le domaine SGH](guarded-fabric-configure-dns-forwarding-and-trust.md)



