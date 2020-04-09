---
ms.assetid: de054ac2-a386-43ec-a537-c0de21549741
title: Définition des propriétés des liens de sites
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f448a29384b9ae9409981a67c515dab3640899dd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821862"
---
# <a name="setting-site-link-properties"></a>Définition des propriétés des liens de sites

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La réplication intersite a lieu en fonction des propriétés des objets de connexion. Lorsque le vérificateur de cohérence des connaissances (KCC) crée des objets de connexion, il dérive la planification de réplication des propriétés des objets de lien de sites. Chaque objet lien de site représente la connexion de réseau étendu (WAN) entre deux sites ou plus.  
  
La définition des propriétés de l’objet lien de site comprend les étapes suivantes :  
  
-   Détermination du coût associé à ce chemin de réplication. Le KCC utilise le coût pour déterminer l’itinéraire le moins coûteux pour la réplication entre deux sites qui répliquent la même partition d’annuaire.  
  
-   Détermination de la planification qui définit les heures pendant lesquelles la réplication intersite peut se produire.  
  
-   Détermination de l’intervalle de réplication qui définit la fréquence à laquelle la réplication doit se produire au moment où la réplication est autorisée, comme défini dans la planification.  
  
## <a name="in-this-guide"></a>Dans ce guide  
  
-   [Détermination du coût](../../ad-ds/plan/Determining-the-Cost.md)  
  
-   [Détermination de la planification](../../ad-ds/plan/Determining-the-Schedule.md)  
  
-   [Détermination de l’intervalle](../../ad-ds/plan/Determining-the-Interval.md)  
  


