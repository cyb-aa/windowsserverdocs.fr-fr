---
ms.assetid: 96a6749c-6c9f-4f2f-ad0a-51272d282ace
title: "Détermination de l’intervalle"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d5eb824b3b15c8b0734b2df23b79649778b28e9d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="determining-the-interval"></a>Détermination de l’intervalle

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Vous devez définir la propriété d’intervalle de réplication de lien de site pour indiquer la fréquence à laquelle vous souhaitez que la réplication se produise pendant les heures lorsque la planification autorise la réplication. Par exemple, si la planification autorise la réplication entre 02:00heures 04:00heures et l’intervalle de réplication est définie pendant 30minutes, la réplication peut se produire jusqu'à quatre fois au cours de l’heure planifiée. L’intervalle de réplication par défaut est de 180minutes, ou 3heures. L’intervalle minimal est de 15minutes.  
  
Tenez compte des critères suivants pour déterminer la fréquence à laquelle la réplication a lieu au sein de la fenêtre de calendrier:  
  
-   Un petit intervalle diminue la latence, mais augmente la quantité de trafic étendu (WAN).  
  
-   Pour maintenir les partitions d’annuaire de domaine, à faible latence est préférée.  
  
Avec une stratégie de réplication de stockage et de transfert, il est difficile de déterminer simplement la durée pendant laquelle une mise à jour de l’annuaire peut prendre être répliqués sur chaque contrôleur de domaine. Pour fournir une estimation de la latence maximale conservateur, effectuer ces tâches:  
  
-   Créer un tableau de tous les sites sur votre réseau, comme illustré dans l’exemple suivant:  
  
    |Sites|Seattle|Boston|Los Angeles|New York|État de Washington, continu|  
    |---------|-----------|----------|---------------|------------|--------------------|  
    |Seattle|0.25|||||  
    |Boston||0.25||||  
    |Los Angeles|||0.25|||  
    |New York||||0.25||  
    |État de Washington, continu|||||0.25|  
  
    Une latence pire au sein d’un site est estimée à 15minutes.  
  
-   À partir de la planification de réplication, déterminez la latence de réplication maximale possible sur n’importe quel lien de sites qui relie les deux sites hub.  
  
    Par exemple, si la réplication a lieu entre Seattle et New York toutes les trois heures, le délai maximal pour la réplication entre ces sites est trois heures. Si le délai de réplication entre New York et Seattle est le plus long délai planifié parmi tous les sites hub, la latence maximale entre tous les concentrateurs est trois heures.  
  
-   Pour chaque site concentrateur, créez une table des latences maximales entre le site hub et ses sites satellite.  
  
    Par exemple, si la réplication a lieu entre New York et Washington, continu, toutes les quatre heures, et il s’agit du délai de réplication plus long entre New York et ses sites satellite, la latence maximale entre New York et ses satellites est de quatre heures.  
  
-   Combiner ces temps de latence maximales pour déterminer la latence maximale pour l’ensemble du réseau.  
  
    Par exemple, si la latence maximale entre Seattle et son site satellite à Los Angeles est un jour, la latence de réplication maximale pour cet ensemble de liens (Washington, continu-New York-Seattle-Los Angeles) est de 31heures, autrement dit, 4 (Washington, continu-New York) + 3 (New York-Seattle) + 24 (Seattle-Los Angeles), comme indiqué dans le tableau suivant.  
  
    |Sites|Seattle|Boston|Los Angeles|New York|État de Washington, continu|  
    |---------|-----------|----------|---------------|------------|--------------------|  
    |Seattle|0.25|4+3|24.00|3.00|4+3|  
    |Boston||0.25|4+3+24|4.00|4.00|  
    |Los Angeles|||0.25|24 + 3|24+3+4|  
    |New York||||0.25|4.00|  
    |État de Washington, continu|||||0.25|  
  


