---
title: Configurer SGH pour les communications https
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: f0cbf6a6dc1970499758a6a48bfaadb95c464ec1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403677"
---
# <a name="configure-hgs-for-https-communications"></a>Configurer SGH pour les communications HTTPs

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Par défaut, lorsque vous initialisez le serveur SGH, il configure les sites Web IIS pour les communications HTTP uniquement.
Toutes les ressources sensibles transmises à et à partir du SGH sont toujours chiffrées à l’aide du chiffrement au niveau du message. Toutefois, si vous souhaitez un niveau de sécurité plus élevé, vous pouvez également activer le protocole HTTPs en configurant le code SGH avec un certificat SSL.

[!INCLUDE [Configure HTTPS](../../../includes/configure-hgs-for-https.md)] 

