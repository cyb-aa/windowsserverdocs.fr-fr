---
ms.assetid: 28ecaf5c-9131-406c-b211-a230162e462e
title: "Détermination des prévisions"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0ec953b34475c50e62553a9ba95e4d45d9904bf3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="determining-the-schedule"></a>Détermination des prévisions

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Vous pouvez contrôler la disponibilité de liaison de site en définissant une planification pour les liens de sites. Lorsque la réplication entre deux sites traverse plusieurs liens de sites, l’intersection des planifications de réplication sur tous les liens correspondants détermine la planification de la connexion entre les deux sites.  
  
Pour planifier la planification de lien de site, créez deux planifications qui se chevauchent entre liens de sites qui contiennent des contrôleurs de domaine qui répliquent directement entre eux. Utiliser le calendrier par défaut (100-% disponible) sur ces liens, sauf si vous souhaitez bloquer le trafic de réplication pendant les heures de pointe. Bloquer la réplication, vous donnez la priorité à tout autre trafic, mais vous augmentez aussi la latence de réplication.  
  
Contrôleurs de domaine stockent temps en temps universel coordonné (UTC). Paramètres de durée de la planification d’objet de liaison de site sont conformes à l’heure locale du site et l’ordinateur sur lequel le calendrier est défini. Lorsqu’un contrôleur de domaine contacte un ordinateur qui se trouve dans un autre site et le fuseau horaire, le calendrier sur le contrôleur de domaine affiche le paramètre de temps en fonction de l’heure locale pour le site de l’ordinateur.  
  


