---
title: Augmenter les authentifications simultanées traitées par serveur NPS
description: Cette rubrique fournit des instructions sur la configuration des authentifications simultanées du serveur de stratégie réseau dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 2d9cdada-0625-41c8-8248-a32259b03e47
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 31dec30ba3c843b78974daa7a1e4ed6893b1ebbd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396428"
---
# <a name="increase-concurrent-authentications-processed-by-nps"></a>Augmenter les authentifications simultanées traitées par serveur NPS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour obtenir des instructions sur la configuration des authentifications simultanées du serveur de stratégie réseau.

Si vous avez installé le serveur de stratégie réseau \(\) NPS sur un ordinateur autre qu’un contrôleur de domaine et que le serveur NPS reçoit un grand nombre de demandes d’authentification par seconde, vous pouvez améliorer les performances de NPS en renforçant le nombre d’authentifications simultanées autorisées entre le serveur NPS et le contrôleur de domaine.

Pour ce faire, vous devez modifier la clé de Registre suivante : 

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Netlogon\Parameters`

Ajoutez une nouvelle valeur nommée **MaxConcurrentApi** et affectez-lui une valeur comprise entre 2 et 5. 

>[!CAUTION]
>Si vous attribuez une valeur à **MaxConcurrentApi** trop élevée, votre serveur NPS risque de placer une charge excessive sur votre contrôleur de domaine.

Pour plus d’informations sur la gestion de NPS, consultez [gérer un serveur de stratégie réseau](nps-manage-top.md).

Pour plus d’informations sur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).