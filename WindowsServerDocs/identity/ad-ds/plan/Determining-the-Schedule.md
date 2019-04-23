---
ms.assetid: 28ecaf5c-9131-406c-b211-a230162e462e
title: Détermination des prévisions
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: dee63ce0fb687b2b722ce64614c54388fc544433
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839000"
---
# <a name="determining-the-schedule"></a>Détermination des prévisions

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez contrôler la disponibilité de lien de site en définissant une planification pour les liens de site. Lors de la réplication entre deux sites traverse plusieurs liens de sites, l’intersection des planifications de réplication sur tous les liens pertinents détermine la planification de la connexion entre les deux sites.  
  
Pour planifier la configuration de la planification de lien de site, créez deux planifications qui se chevauchent entre liens de sites qui contiennent des contrôleurs de domaine qui répliquent directement entre eux. Utiliser la planification par défaut (100 % disponibles) sur ces liens, sauf si vous souhaitez bloquer le trafic de réplication pendant les heures de pointe. Bloquer la réplication, vous donnez la priorité à tout autre trafic, mais vous augmentez également la latence de réplication.  
  
Contrôleurs de domaine stockent l’heure en temps universel coordonné (UTC). Paramètres d’heure dans la planification d’objet de liaison de site est conforme à l’heure locale du site et de l’ordinateur sur lequel le calendrier est défini. Lorsqu’un contrôleur de domaine contacte un ordinateur qui se trouve dans un autre site et le fuseau horaire, la planification sur le contrôleur de domaine affiche le paramètre de temps en fonction de l’heure locale pour le site de l’ordinateur.  
  


