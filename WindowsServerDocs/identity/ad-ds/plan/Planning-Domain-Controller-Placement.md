---
ms.assetid: 692bd2af-deee-44cf-9af9-f364677e267f
title: "Planification du Placement des contrôleurs de domaine"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c6d9ca064699f95390e5b95863a6871ea2dc9d6e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="planning-domain-controller-placement"></a>Planification du Placement des contrôleurs de domaine

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Une fois que vous avez rassemblé toutes les informations de réseau qui seront utilisées pour concevoir votre topologie de site, où vous souhaitez placer les contrôleurs de domaine, y compris les contrôleurs de domaine racine de forêt, les contrôleurs de domaine régional, envisagez de maître d’opérations possesseurs de rôles et les serveurs de catalogue global.  
  
Dans Windows Server2008, vous pouvez également tirer parti des contrôleurs de domaine en lecture seule (RODC). Un RODC est un nouveau type de contrôleur de domaine qui héberge les partitions en lecture seule de la base de données ActiveDirectory. À l’exception des mots de passe de compte, un RODC contient tous les objets ActiveDirectory et les attributs qui contient un contrôleur de domaine accessible en écriture. Toutefois, ne peuvent pas être modifiés à la base de données qui est stocké sur le RODC. Modifications doivent être effectuées sur un contrôleur de domaine accessible en écriture et puis répliquées sur le RODC.  
  
Un RODC est principalement conçu pour être déployé dans à distance ou les environnements de succursales, qui ont généralement relativement peu des utilisateurs, une sécurité physique insuffisante, la bande passante réseau est relativement mauvaise à un site concentrateur et le personnel limité connaissance de technologies de l’information (TI). Déploiement RODC entraîne une sécurité améliorée et plus efficace accès aux ressources réseau. Pour plus d’informations sur les fonctionnalités RODC, voir les services ADDS: Read-Only contrôleurs de domaine ([https://go.microsoft.com/fwlink/?LinkID=106616](https://go.microsoft.com/fwlink/?LinkID=106616)). Pour plus d’informations sur le déploiement d’un RODC, voir la Step-by-Step Guide for Read-Only contrôleurs de domaine ([https://go.microsoft.com/fwlink/?LinkID=92728](https://go.microsoft.com/fwlink/?LinkID=92728)).  
  
> [!NOTE]  
> Ce guide n’explique pas comment vous déterminez le nombre approprié de contrôleurs de domaine et la domaine contrôleur configuration matérielle requise pour chaque domaine qui est représenté dans chaque site.  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Planification d’emplacement de contrôleur de domaine de racine de forêt](../../ad-ds/plan/Planning-Forest-Root-Domain-Controller-Placement.md)  
  
-   [Planification du Placement des contrôleurs de domaine régional](../../ad-ds/plan/Planning-Regional-Domain-Controller-Placement.md)  
  
-   [Planification du Placement de serveur de catalogue Global](../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md)  
  
-   [Planification de l’emplacement du rôle de maître des opérations](../../ad-ds/plan/Planning-Operations-Master-Role-Placement.md)  
  


