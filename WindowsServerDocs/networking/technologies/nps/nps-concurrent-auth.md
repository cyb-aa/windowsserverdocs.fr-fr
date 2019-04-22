---
title: Augmenter les authentifications simultanées traitées par serveur NPS
description: Cette rubrique fournit des instructions sur la configuration d’authentifications simultanées Network Policy Server dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2d9cdada-0625-41c8-8248-a32259b03e47
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fd930e34a4adf6c55812385b691df3e3575a4280
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818260"
---
# <a name="increase-concurrent-authentications-processed-by-nps"></a>Augmenter les authentifications simultanées traitées par serveur NPS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour obtenir des instructions sur la configuration d’authentifications simultanées du serveur NPS.

Si vous avez installé le serveur de stratégie réseau \(NPS\) sur un ordinateur autre qu’un domaine contrôleur et le serveur NPS reçoit un grand nombre de demandes d’authentification par seconde, vous pouvez améliorer les performances de serveur NPS en augmentant le nombre de authentifications simultanées autorisées entre le serveur NPS et le contrôleur de domaine.

Pour ce faire, vous devez modifier la clé de Registre suivante : 

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Netlogon\Parameters`

Ajoutez une nouvelle valeur nommée **MaxConcurrentApi** et à lui affecter une valeur comprise entre 2 et 5. 

>[!CAUTION]
>Si vous affectez une valeur à **MaxConcurrentApi** qui est trop élevée, le serveur NPS peut placer une charge excessive sur votre contrôleur de domaine.

Pour plus d’informations sur la gestion de serveur NPS, consultez [gérer un serveur de stratégie réseau](nps-manage-top.md).

Pour plus d’informations sur le serveur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).