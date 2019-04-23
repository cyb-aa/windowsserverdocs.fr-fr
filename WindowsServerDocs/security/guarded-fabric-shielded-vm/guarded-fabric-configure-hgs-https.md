---
title: Configurer le service SGH pour les communications Https
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 83529a5bdb4547b9881bb307a8a4cd526552d02c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829000"
---
# <a name="configure-hgs-for-https-communications"></a>Configurer le service SGH pour les communications HTTPS

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Par défaut, lorsque vous initialisez un serveur SGH il configurera les sites web IIS pour les communications HTTP uniquement.
Tous les documents sensibles transmises vers et depuis SGH sont toujours chiffrées à l’aide du chiffrement au niveau du message, cependant, si vous le souhaitez un niveau plus élevé de sécurité vous pouvez également activer HTTPS en configurant SGH avec un certificat SSL.

[!INCLUDE [Configure HTTPS](../../../includes/configure-hgs-for-https.md)] 

