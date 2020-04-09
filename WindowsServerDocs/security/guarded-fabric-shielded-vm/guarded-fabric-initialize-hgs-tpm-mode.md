---
title: Initialiser SGH à l’aide d’une attestation approuvée par le module de plateforme sécurisée
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: b8ceebe63a586ec95b502dfea12f99d174549448
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856612"
---
# <a name="initialize-hgs-using-tpm-trusted-attestation"></a>Initialiser SGH à l’aide d’une attestation approuvée par le module de plateforme sécurisée

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Ces étapes varient selon que vous initialisez le SGH dans une nouvelle forêt ou dans une forêt bastion existante :

1. [Initialiser le cluster SGH dans une nouvelle forêt (par défaut)](guarded-fabric-initialize-hgs-tpm-mode-default.md)

   \- Ou -

   [Initialiser le cluster SGH dans une forêt bastion existante](guarded-fabric-initialize-hgs-tpm-mode-bastion.md)

2. [Installer des certificats racines TPM approuvés](guarded-fabric-install-trusted-tpm-root-certificates.md)   
3. [Configurer le DNS de l’infrastructure](guarded-fabric-configuring-fabric-dns.md)

