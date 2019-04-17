---
title: Augmenter les authentifications simultanées traitées par le serveur NPS
description: Cette rubrique fournit des instructions sur la configuration d’authentifications simultanées du serveur de stratégie réseau dans Windows Server2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2d9cdada-0625-41c8-8248-a32259b03e47
ms.author: pashort
author: shortpatti
ms.openlocfilehash: aa70c1a26e2c22d26545e1b46a6151d71a2b4095
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="increase-concurrent-authentications-processed-by-nps"></a>Augmenter les authentifications simultanées traitées par le serveur NPS

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour obtenir des instructions sur la configuration d’authentifications simultanées du serveur de stratégie réseau.

Si vous avez installé le serveur de stratégie réseau \(NPS\) sur un ordinateur autre que d’un contrôleur de domaine et le serveur NPS reçoit un grand nombre de demandes d’authentification par seconde, vous pouvez améliorer les performances de NPS en augmentant le nombre d’authentifications simultanées autorisées entre le serveur NPS et le contrôleur de domaine.

Pour ce faire, vous devez modifier la clé de Registre suivante: 

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Netlogon\Parameters`

Ajoutez une nouvelle valeur nommée **MaxConcurrentApi** et affectez-lui une valeur comprise entre 2 et 5. 

>[!CAUTION]
>Si vous attribuez une valeur à **MaxConcurrentApi** qui est trop élevée, votre serveur NPS peut placer une charge excessive sur votre contrôleur de domaine.

Pour plus d’informations sur la gestion du serveur NPS, voir [gérer un serveur de stratégie réseau](nps-manage-top.md).

Pour plus d’informations sur le serveur NPS, voir [serveur NPS (Network Policy)](nps-top.md).