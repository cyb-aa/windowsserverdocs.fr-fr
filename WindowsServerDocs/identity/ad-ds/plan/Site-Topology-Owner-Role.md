---
ms.assetid: af76ddbe-83a2-4a62-9989-873e3bb1c772
title: "Rôle de propriétaire de topologie de site"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7c98d313cac28ab07380791a384a87bffacfbda2
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="site-topology-owner-role"></a>Rôle de propriétaire de topologie de site

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

L’administrateur qui gère la topologie de site est appelé le propriétaire de topologie de site. Le propriétaire de topologie de site comprend les conditions du réseau entre les sites et est autorisé à modifier les paramètres dans les Services de domaine ActiveDirectory (ADDS) pour implémenter les modifications apportées à la topologie de site. Modifications apportées à la topologie de site affectent les modifications apportées à la topologie de réplication. Responsabilités du propriétaire de topologie de site sont les suivantes:  
  
-   Contrôle des modifications à la topologie de site si la connectivité réseau change.  
  
-   Obtention et la maintenance des informations sur les connexions réseau et les routeurs du groupe réseau. Le propriétaire de topologie de site doit conserver une liste des adresses de sous-réseau, masques de sous-réseau et l’emplacement auquel chaque appartient. Le propriétaire de topologie de site doit également comprendre les problèmes de vitesse du réseau et la capacité qui affectent la topologie de site pour définir efficacement les coûts de liens de sites.  
  
-   Déplacement des objets de serveur ActiveDirectory représentant les contrôleurs de domaine entre les sites adresse IP du contrôleur de domaine une modification à un sous-réseau différent dans un autre site ou si le sous-réseau lui-même est attribué à un autre site. Dans les deux cas, le propriétaire de topologie de site doit déplacer manuellement l’objet de serveur ActiveDirectory du contrôleur de domaine vers le nouveau site.  
  


