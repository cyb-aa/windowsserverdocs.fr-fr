---
ms.assetid: 692bd2af-deee-44cf-9af9-f364677e267f
title: Planification de l’emplacement des contrôleurs de domaine
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 68406d4973dd585bf0a98562c987c1b1512c095c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883360"
---
# <a name="planning-domain-controller-placement"></a>Planification de l’emplacement des contrôleurs de domaine

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Une fois que vous avez rassemblé toutes les informations de réseau qui seront utilisées pour concevoir votre topologie de site, envisagez où vous souhaitez placer les contrôleurs de domaine, y compris les contrôleurs de domaine racine de forêt, les contrôleurs de domaine régional, les détenteurs du rôle de maître d’opérations, et serveurs de catalogue global.  
  
Dans Windows Server 2008, vous pouvez également tirer parti des contrôleurs de domaine en lecture seule (RODC). Un RODC est un nouveau type de contrôleur de domaine qui héberge les partitions en lecture seule de la base de données Active Directory. À l’exception des mots de passe de compte, un RODC contient tous les objets Active Directory et les attributs qui contient un contrôleur de domaine accessible en écriture. Toutefois, les modifications ne peuvent pas être apportées à la base de données qui est stocké sur le RODC. Modification doit être effectuée sur un contrôleur de domaine accessible en écriture et puis répliquées vers le RODC.  
  
Un RODC est principalement conçu pour être déployé dans à distance ou les environnements de succursale, qui ont généralement relativement peu les utilisateurs, une sécurité physique insuffisante, la bande passante réseau relativement faibles à un site hub et le personnel avec une connaissance limitée d’informations technologie (IT). Déploiement RODC entraîne une sécurité améliorée et un accès plus efficace aux ressources réseau. Pour plus d’informations sur les fonctionnalités RODC, consultez les services AD DS : Contrôleurs de domaine en lecture seule ([https://go.microsoft.com/fwlink/?LinkID=106616](https://go.microsoft.com/fwlink/?LinkID=106616)). Pour plus d’informations sur le déploiement d’un RODC, consultez le Guide pas à pas pour les contrôleurs de domaine en lecture seule ([https://go.microsoft.com/fwlink/?LinkID=92728](https://go.microsoft.com/fwlink/?LinkID=92728)).  
  
> [!NOTE]  
> Ce guide n’explique pas comment déterminer le nombre approprié de contrôleurs de domaine et de la configuration matérielle requise par le contrôleur domaine pour chaque domaine qui est représenté dans chaque site.  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Planification du Placement des contrôleurs de domaine racine forêt](../../ad-ds/plan/Planning-Forest-Root-Domain-Controller-Placement.md)  
  
-   [Planification du Placement des contrôleurs de domaine régional](../../ad-ds/plan/Planning-Regional-Domain-Controller-Placement.md)  
  
-   [Planification du Placement de serveur de catalogue Global](../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md)  
  
-   [Planification de l’emplacement du rôle de maître des opérations](../../ad-ds/plan/Planning-Operations-Master-Role-Placement.md)  
  


