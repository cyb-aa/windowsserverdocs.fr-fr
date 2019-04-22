---
ms.assetid: 96a6749c-6c9f-4f2f-ad0a-51272d282ace
title: Détermination de l’intervalle
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 57b282102f10a595ce3842ac3a4eb1d289b86d22
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822160"
---
# <a name="determining-the-interval"></a>Détermination de l’intervalle

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous devez définir la propriété d’intervalle de réplication de lien de site pour indiquer la fréquence à laquelle vous souhaitez que la réplication se produisent pendant les heures lorsque la planification autorise la réplication. Par exemple, si la planification autorise la réplication entre les heures de 02:00 et 04:00 heures et l’intervalle de réplication est définie pour les 30 minutes, la réplication peut se produire jusqu'à quatre fois pendant l’heure planifiée. L’intervalle de réplication par défaut est 180 minutes, ou 3 heures. L’intervalle minimum est de 15 minutes.  
  
Prendre en compte les critères suivants pour déterminer la fréquence à laquelle la réplication a lieu au sein de la fenêtre de planification :  
  
-   Un petit intervalle diminue la latence, mais augmente la quantité de trafic de réseau (étendu WAN) étendu.  
  
-   Pour conserver des partitions d’annuaire de domaine à jour, une faible latence est recommandée.  
  
Avec une stratégie de réplication de stockage et transfert, il est difficile de déterminer simplement la durée pendant laquelle une mise à jour de l’annuaire peut prendre être répliqués sur chaque contrôleur de domaine. Pour fournir une estimation prudente de latence maximale, effectuez ces tâches :  
  
-   Créer une table de tous les sites de votre réseau, comme illustré dans l’exemple suivant :  
  
    |Sites|Seattle|Boston|Los Angeles|New York|Région de Washington D.C.|  
    |---------|-----------|----------|---------------|------------|--------------------|  
    |Seattle|0.25|||||  
    |Boston||0.25||||  
    |Los Angeles|||0.25|||  
    |New York||||0.25||  
    |Région de Washington D.C.|||||0.25|  
  
    Une latence le plus défavorable au sein d’un site est estimée à être de 15 minutes.  
  
-   À partir de la planification de la réplication, déterminez la latence de réplication maximale possible sur n’importe quel lien de sites qui connecte les deux sites hub.  
  
    Par exemple, si la réplication a lieu entre Seattle et New York à toutes les trois heures, le délai maximal pour la réplication entre ces sites est trois heures. Si le délai de réplication entre New York et Seattle est le plus long délai planifié parmi tous les sites de concentrateur, la latence maximale entre tous les hubs est trois heures.  
  
-   Pour chaque site concentrateur, créez une table des latences maximales entre le site hub et ses sites satellites.  
  
    Par exemple, si la réplication se produit entre New York et de Washington, D.C., toutes les quatre heures et du délai d’attente de réplication la plus longue entre New York et ses sites satellites, la latence maximale entre New York et ses satellites est quatre heures.  
  
-   Combiner ces latences maximales pour déterminer la latence maximale pour l’ensemble du réseau.  
  
    Par exemple, si la latence maximale entre Seattle et son site satellite à Los Angeles est une journée, la latence de réplication maximale pour cet ensemble de liens (Washington, D.C.-New York-Seattle-Los Angeles) est 31 heures, c'est-à-dire 4 (Washington, D.C.-New York) + 3 (nouveau York-Seattle) + 24 (Seattle-Los Angeles), comme illustré dans le tableau suivant.  
  
    |Sites|Seattle|Boston|Los Angeles|New York|Région de Washington D.C.|  
    |---------|-----------|----------|---------------|------------|--------------------|  
    |Seattle|0.25|4+3|24.00|3.00|4+3|  
    |Boston||0.25|4+3+24|4,00|4,00|  
    |Los Angeles|||0.25|24 + 3|24+3+4|  
    |New York||||0.25|4,00|  
    |Région de Washington D.C.|||||0.25|  
  


