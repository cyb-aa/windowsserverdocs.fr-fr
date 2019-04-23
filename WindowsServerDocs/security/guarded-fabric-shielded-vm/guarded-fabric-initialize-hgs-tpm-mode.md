---
title: Initialiser SGH à l’aide de l’attestation approuvée par le module de plateforme sécurisée
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 672054462437b615236dfc4f00c8b20c946f11a7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882330"
---
# <a name="initialize-hgs-using-tpm-trusted-attestation"></a>Initialiser SGH à l’aide de l’attestation approuvée par le module de plateforme sécurisée

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Ces étapes varient selon que vous initialisez SGH dans une nouvelle forêt ou d’une forêt bastion existante :

1. [Initialiser le cluster SGH dans une nouvelle forêt (par défaut)](guarded-fabric-initialize-hgs-tpm-mode-default.md)

   - Ou -

   [Initialiser le cluster SGH dans une forêt bastion existante](guarded-fabric-initialize-hgs-tpm-mode-bastion.md)

2. [Installer des certificats de racine de confiance TPM](guarded-fabric-install-trusted-tpm-root-certificates.md)   
3. [Configuration de l’infrastructure DNS](guarded-fabric-configuring-fabric-dns.md)

