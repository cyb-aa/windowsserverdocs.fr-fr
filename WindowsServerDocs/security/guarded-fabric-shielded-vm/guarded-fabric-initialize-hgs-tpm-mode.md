---
title: Initialiser SGH à l’aide d’une attestation approuvée par le module de plateforme sécurisée
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: c67e15aa011dbff0be5d428c8012a161d9eaea2f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386599"
---
# <a name="initialize-hgs-using-tpm-trusted-attestation"></a>Initialiser SGH à l’aide d’une attestation approuvée par le module de plateforme sécurisée

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Ces étapes varient selon que vous initialisez le SGH dans une nouvelle forêt ou dans une forêt bastion existante :

1. [Initialiser le cluster SGH dans une nouvelle forêt (par défaut)](guarded-fabric-initialize-hgs-tpm-mode-default.md)

   \- Ou -

   [Initialiser le cluster SGH dans une forêt bastion existante](guarded-fabric-initialize-hgs-tpm-mode-bastion.md)

2. [Installer des certificats racines TPM approuvés](guarded-fabric-install-trusted-tpm-root-certificates.md)   
3. [Configurer le DNS de l’infrastructure](guarded-fabric-configuring-fabric-dns.md)

