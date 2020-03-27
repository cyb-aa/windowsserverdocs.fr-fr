---
title: Augmenter les authentifications simultanées traitées par serveur NPS
description: Cette rubrique fournit des instructions sur la configuration des authentifications simultanées du serveur de stratégie réseau dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 2d9cdada-0625-41c8-8248-a32259b03e47
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 5397a1de41717dfc65ee0b0582c1289c6a53b33a
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316293"
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