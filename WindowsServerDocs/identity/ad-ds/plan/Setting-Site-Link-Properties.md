---
ms.assetid: de054ac2-a386-43ec-a537-c0de21549741
title: "Définition des propriétés de lien de Site"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 495ed006ecac5458877191a14060c5fd4b746d96
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="setting-site-link-properties"></a>Définition des propriétés de lien de Site

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

La réplication intersite a lieu en fonction des propriétés des objets de connexion. Lorsque le vérificateur de cohérence des connaissances (KCC) crée des objets de connexion, il dérive de la planification de réplication à partir des propriétés des objets de lien de site. Chaque objet de lien de site représente la connexion réseau (étendu WAN) entre deux ou plusieurs sites.  
  
Définition des propriétés d’objet de lien de site comprend les étapes suivantes:  
  
-   Détermination du coût associé à ce chemin d’accès de la réplication. Le processus KCC utilise coût pour déterminer l’itinéraire le moins coûteux pour la réplication entre deux sites qui répliquent la partition d’annuaire.  
  
-   Détermination des prévisions qui définit les heures pendant lesquelles la réplication intersite peut se produire.  
  
-   Détermination de l’intervalle de réplication qui définit la fréquence à laquelle la réplication doit se faire pendant les heures lorsque la réplication est autorisée, comme défini dans le calendrier.  
  
## <a name="in-this-guide"></a>Dans ce guide  
  
-   [Détermination du coût](../../ad-ds/plan/Determining-the-Cost.md)  
  
-   [Détermination des prévisions](../../ad-ds/plan/Determining-the-Schedule.md)  
  
-   [Détermination de l’intervalle](../../ad-ds/plan/Determining-the-Interval.md)  
  


