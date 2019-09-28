---
ms.assetid: af76ddbe-83a2-4a62-9989-873e3bb1c772
title: Rôle de propriétaire de topologie de site
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 7b010dcb7a150f4ebcfcc941aa9e5c016795e717
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402497"
---
# <a name="site-topology-owner-role"></a>Rôle de propriétaire de topologie de site

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

L’administrateur qui gère la topologie de site est appelé propriétaire de la topologie de site. Le propriétaire de la topologie de site comprend les conditions du réseau entre les sites et est habilitée à modifier les paramètres de Active Directory Domain Services (AD DS) pour implémenter les modifications apportées à la topologie de site. Les modifications apportées à la topologie de site affectent les modifications apportées à la topologie de réplication. Les responsabilités du propriétaire de la topologie de site sont les suivantes :  
  
-   Contrôle des modifications apportées à la topologie de site en cas de modification de la connectivité réseau.  
  
-   Obtenir et gérer des informations sur les connexions réseau et les routeurs à partir du groupe réseau. Le propriétaire de la topologie de site doit tenir à jour la liste des adresses de sous-réseau, des masques de sous-réseau et de l’emplacement auquel chacun appartient. Le propriétaire de la topologie de site doit également comprendre tous les problèmes liés à la vitesse et à la capacité du réseau qui affectent la topologie de site pour définir efficacement les coûts pour les liens de sites.  
  
-   Déplacement d’objets Active Directory Server représentant des contrôleurs de domaine entre des sites si l’adresse IP d’un contrôleur de domaine change en un autre sous-réseau dans un autre site, ou si le sous-réseau lui-même est attribué à un autre site. Dans les deux cas, le propriétaire de la topologie de site doit déplacer manuellement l’objet Active Directory Server du contrôleur de domaine vers le nouveau site.  
  


