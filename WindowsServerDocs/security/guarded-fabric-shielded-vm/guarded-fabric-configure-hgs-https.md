---
title: Configurer SGH pour les communications HTTPs
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: de57a4026a33561760ad36fd78d732352b3aa340
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856842"
---
# <a name="configure-hgs-for-https-communications"></a>Configurer SGH pour les communications HTTPs

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Par défaut, lorsque vous initialisez le serveur SGH, il configure les sites Web IIS pour les communications HTTP uniquement.
Toutes les ressources sensibles transmises à et à partir du SGH sont toujours chiffrées à l’aide du chiffrement au niveau du message. Toutefois, si vous souhaitez un niveau de sécurité plus élevé, vous pouvez également activer le protocole HTTPs en configurant le code SGH avec un certificat SSL.

[!INCLUDE [Configure HTTPS](../../../includes/configure-hgs-for-https.md)] 

