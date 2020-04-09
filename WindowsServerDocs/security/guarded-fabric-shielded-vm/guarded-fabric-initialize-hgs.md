---
title: Initialiser SGH
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 42f76dabbe150f229027909f8b58b843f31ae4e1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856592"
---
# <a name="initialize-the-host-guardian-service-hgs"></a>Initialiser le service Guardian hôte (SGH)

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Lorsque vous initialisez SGH, vous spécifiez le mode utilisé par SGH pour mesurer l’intégrité des hôtes service Guardian. Il existe deux options mutuellement exclusives. Pour obtenir des informations générales sur le mode à choisir, consultez le Guide de planification de l' [infrastructure protégée et de la machine virtuelle protégée pour les hébergeurs](guarded-fabric-planning-for-hosters.md).

Les rubriques suivantes couvrent les étapes de déploiement pour chaque mode :

- [Attestation approuvée par le module de plateforme sécurisée (TPM)](guarded-fabric-initialize-hgs-tpm-mode.md)
- [Attestation de clé hôte (mode clé)](guarded-fabric-initialize-hgs-key-mode.md)
- [Attestation approuvée par l’administrateur (mode AD)](guarded-fabric-initialize-hgs-ad-mode.md)

Vous devez effectuer ces étapes sur un serveur physique.