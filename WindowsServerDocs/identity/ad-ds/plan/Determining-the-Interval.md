---
ms.assetid: 96a6749c-6c9f-4f2f-ad0a-51272d282ace
title: Détermination de l’intervalle
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f39ad2ce2ce84e36d2faff2a07b8310d3600b6c9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822572"
---
# <a name="determining-the-interval"></a>Détermination de l’intervalle

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous devez définir la propriété intervalle de réplication de lien de site pour indiquer la fréquence à laquelle vous souhaitez que la réplication se produise au moment où la planification autorise la réplication. Par exemple, si la planification autorise la réplication entre 02:00 heures et 04:00 heures et que l’intervalle de réplication est défini sur 30 minutes, la réplication peut se produire jusqu’à quatre fois au cours de l’heure planifiée. L’intervalle de réplication par défaut est de 180 minutes, soit 3 heures. L’intervalle minimal est de 15 minutes.  
  
Prenez en compte les critères suivants pour déterminer la fréquence à laquelle la réplication a lieu dans la fenêtre de planification :  
  
-   Un petit intervalle réduit la latence, mais augmente la quantité de trafic de réseau étendu (WAN).  
  
-   Pour tenir à jour les partitions d’annuaire de domaine, une faible latence est recommandée.  
  
Avec une stratégie de réplication de stockage et de transfert, il est difficile de déterminer combien de temps une mise à jour de répertoire peut être nécessaire pour être répliquée sur chaque contrôleur de domaine. Pour fournir une estimation conservatrice de la latence maximale, effectuez les tâches suivantes :  
  
-   Créez un tableau de tous les sites de votre réseau, comme illustré dans l’exemple suivant :  
  
    |Sites|Seattle|Boston|Los Angeles|New York|Washington, D.C.|  
    |---------|-----------|----------|---------------|------------|--------------------|  
    |Seattle|0,25|||||  
    |Boston||0,25||||  
    |Los Angeles|||0,25|||  
    |New York||||0,25||  
    |Washington, D.C.|||||0,25|  
  
    Une latence au pire dans le cas d’un site est estimée à 15 minutes.  
  
-   À partir de la planification de réplication, déterminez la latence de réplication maximale possible sur n’importe quel lien de site qui connecte deux sites hub.  
  
    Par exemple, si la réplication a lieu entre Seattle et New York toutes les trois heures, le délai maximal pour la réplication entre ces sites est de trois heures. Si le délai de réplication entre New York et Seattle est le délai le plus long entre tous les sites hub, la latence maximale entre tous les hubs est de trois heures.  
  
-   Pour chaque site Hub, créez un tableau des latences maximales entre le site Hub et l’un de ses sites satellites.  
  
    Par exemple, si la réplication a lieu entre New York et Washington, D.C., toutes les quatre heures et qu’il s’agit du délai de réplication le plus long entre New York et l’un de ses sites satellites, la latence maximale entre New York et ses satellites est de quatre heures.  
  
-   Combinez ces latences maximales pour déterminer la latence maximale de l’ensemble du réseau.  
  
    Par exemple, si la latence maximale entre Seattle et son site satellite à Los Angeles est d’une journée, la latence de réplication maximale pour cet ensemble de liens (Washington, D.C.-New York-Seattle-Los Angeles) est de 31 heures, c’est-à-dire 4 (Washington, D.C.-New York) + 3 (New York-Seattle) + 24 (Seattle-Los Angeles), comme indiqué dans le tableau suivant.  
  
    |Sites|Seattle|Boston|Los Angeles|New York|Washington, D.C.|  
    |---------|-----------|----------|---------------|------------|--------------------|  
    |Seattle|0,25|4 + 3|24,00|3,00|4 + 3|  
    |Boston||0,25|4 + 3 + 24|4,00|4,00|  
    |Los Angeles|||0,25|plus de 24 + 3|24 + 3 + 4|  
    |New York||||0,25|4,00|  
    |Washington, D.C.|||||0,25|  
  


