---
ms.assetid: de054ac2-a386-43ec-a537-c0de21549741
title: Définition des propriétés des liens de sites
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4fa9a1fa8d2a463fe5f361a5a27ee2b9e3edc0f6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870360"
---
# <a name="setting-site-link-properties"></a>Définition des propriétés des liens de sites

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La réplication intersite se produit en fonction des propriétés des objets de connexion. Lorsque le vérificateur de cohérence des connaissances (KCC) crée des objets de connexion, il dérive de la planification de réplication à partir des propriétés des objets de lien de site. Chaque objet de lien de site représente la connexion réseau (étendu WAN) entre deux ou plusieurs sites.  
  
Définition des propriétés d’objet de lien de site comprend les étapes suivantes :  
  
-   Détermination du coût associé à ce chemin d’accès de la réplication. Le KCC utilise coût pour déterminer l’itinéraire le moins coûteux pour la réplication entre deux sites qui répliquent la partition d’annuaire.  
  
-   Détermination de la planification qui définit les heures pendant lesquelles la réplication intersite peut se produire.  
  
-   Détermination de l’intervalle de réplication qui définit la fréquence à laquelle la réplication doit se faire pendant les heures lorsque la réplication est autorisée, comme défini dans la planification.  
  
## <a name="in-this-guide"></a>Dans ce guide  
  
-   [Détermination du coût](../../ad-ds/plan/Determining-the-Cost.md)  
  
-   [Détermination de la planification](../../ad-ds/plan/Determining-the-Schedule.md)  
  
-   [Détermination de l’intervalle](../../ad-ds/plan/Determining-the-Interval.md)  
  


