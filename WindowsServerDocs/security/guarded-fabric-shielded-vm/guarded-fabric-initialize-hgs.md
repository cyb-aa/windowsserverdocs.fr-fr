---
title: Initialiser SGH
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: c689a1d5f69731db0cb85a884f5af2ee0230e7be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865450"
---
# <a name="initialize-the-host-guardian-service-hgs"></a>Initialiser le Service de Guardian hôte (SGH)

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Lorsque vous initialisez SGH, vous spécifiez le mode que SGH utilisera pour mesurer l’intégrité des hôtes service Guardian. Il existe deux options qui s’excluent mutuellement. Pour plus d’informations générales sur le mode à choisir, consultez [infrastructure service Guardian protégé VM Guide de planification et pour les hébergeurs](guarded-fabric-planning-for-hosters.md).

Les rubriques suivantes couvrent les étapes de déploiement pour chaque mode :

- [Attestation approuvée par le module de plateforme sécurisée (mode de module de plateforme sécurisée)](guarded-fabric-initialize-hgs-tpm-mode.md)
- [Attestation de clé hôte (mode de clé)](guarded-fabric-initialize-hgs-key-mode.md)
- [Attestation approuvée par l’administrateur (mode AD)](guarded-fabric-initialize-hgs-ad-mode.md)

Vous devez effectuer ces étapes sur un serveur physique.