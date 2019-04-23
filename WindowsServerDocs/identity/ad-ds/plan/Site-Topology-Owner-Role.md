---
ms.assetid: af76ddbe-83a2-4a62-9989-873e3bb1c772
title: Rôle de propriétaire de topologie de site
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 69443c2fc1af855c7df002e0ac91d43986eff6da
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832110"
---
# <a name="site-topology-owner-role"></a>Rôle de propriétaire de topologie de site

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

L’administrateur qui gère la topologie de site est connu comme le propriétaire de topologie de site. Le propriétaire de topologie de site comprend les conditions du réseau entre les sites et est autorisé à modifier les paramètres dans les Services de domaine Active Directory (AD DS) pour implémenter les modifications apportées à la topologie de site. Modifications apportées à la topologie de site affectent les modifications dans la topologie de réplication. Les responsabilités du propriétaire de topologie de site sont les suivantes :  
  
-   Contrôle des modifications à la topologie de site si la connectivité réseau change.  
  
-   Obtention et la maintenance des informations sur les connexions réseau et les routeurs du groupe réseau. Le propriétaire de topologie de site doit conserver une liste d’adresses de sous-réseau, les masques de sous-réseau et l’emplacement à laquelle chacune appartient. Le propriétaire de topologie de site doit également comprendre les problèmes sur la capacité et la vitesse du réseau qui affectent la topologie de site pour définir efficacement les coûts pour les liens de sites.  
  
-   Déplacement d’objets de serveur Active Directory représentant les contrôleurs de domaine entre les sites si change d’adresse d’un contrôleur de domaine vers un autre sous-réseau dans un autre site, ou si le sous-réseau lui-même est affecté à un autre site. Dans les deux cas, le propriétaire de topologie de site doit déplacer manuellement l’objet de serveur Active Directory du contrôleur de domaine vers le nouveau site.  
  


