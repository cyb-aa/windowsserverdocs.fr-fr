---
ms.assetid: 28ecaf5c-9131-406c-b211-a230162e462e
title: Détermination des prévisions
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: d5c1c7c77304317bab2e80223dd745c88bc79efd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822542"
---
# <a name="determining-the-schedule"></a>Détermination des prévisions

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez contrôler la disponibilité des liaisons de sites en définissant une planification pour les liens de sites. Lorsque la réplication entre deux sites parcourt plusieurs liens de sites, l’intersection des planifications de réplication sur tous les liens pertinents détermine la planification de la connexion entre les deux sites.  
  
Pour planifier la définition de la planification de lien de sites, créez deux planifications qui se chevauchent entre les liens de sites qui contiennent des contrôleurs de domaine qui se répliquent directement les uns avec les autres. Utilisez la planification par défaut (100% disponible) sur ces liens, sauf si vous souhaitez bloquer le trafic de réplication pendant les heures de pointe. En bloquant la réplication, vous attribuez la priorité à un autre trafic, mais vous augmentez également la latence de la réplication.  
  
Les contrôleurs de domaine stockent l’heure en temps universel coordonné (UTC). Les paramètres de temps dans les planifications d’objet de lien de sites sont conformes à l’heure locale du site et de l’ordinateur sur lequel la planification est définie. Lorsqu’un contrôleur de domaine contacte un ordinateur qui se trouve dans un autre site et un autre fuseau horaire, la planification sur le contrôleur de domaine affiche le paramètre d’heure en fonction de l’heure locale du site de l’ordinateur.  
  


