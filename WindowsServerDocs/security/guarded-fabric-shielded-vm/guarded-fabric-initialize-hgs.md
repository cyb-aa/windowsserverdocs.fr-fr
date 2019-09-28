---
title: Initialiser SGH
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 07c40b1da829239dda5210dde0dabe485f659ae0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403591"
---
# <a name="initialize-the-host-guardian-service-hgs"></a>Initialiser le service Guardian hôte (SGH)

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Lorsque vous initialisez SGH, vous spécifiez le mode utilisé par SGH pour mesurer l’intégrité des hôtes service Guardian. Il existe deux options mutuellement exclusives. Pour obtenir des informations générales sur le mode à choisir, consultez le Guide de planification de l' [infrastructure protégée et de la machine virtuelle protégée pour les hébergeurs](guarded-fabric-planning-for-hosters.md).

Les rubriques suivantes couvrent les étapes de déploiement pour chaque mode :

- [Attestation approuvée par le module de plateforme sécurisée (TPM)](guarded-fabric-initialize-hgs-tpm-mode.md)
- [Attestation de clé hôte (mode clé)](guarded-fabric-initialize-hgs-key-mode.md)
- [Attestation approuvée par l’administrateur (mode AD)](guarded-fabric-initialize-hgs-ad-mode.md)

Vous devez effectuer ces étapes sur un serveur physique.