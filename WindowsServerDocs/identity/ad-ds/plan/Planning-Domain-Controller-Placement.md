---
ms.assetid: 692bd2af-deee-44cf-9af9-f364677e267f
title: Planification de l’emplacement des contrôleurs de domaine
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ff0cba67454080db7cca4b012ae0a2d5cb40e412
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408759"
---
# <a name="planning-domain-controller-placement"></a>Planification de l’emplacement des contrôleurs de domaine

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Une fois que vous avez rassemblé toutes les informations réseau qui seront utilisées pour concevoir la topologie de votre site, planifiez l’emplacement où vous souhaitez placer les contrôleurs de domaine, y compris les contrôleurs de domaine racine de la forêt, les contrôleurs de domaine régionaux, les détenteurs du rôle de maître d’opérations et serveurs de catalogue global.  
  
Dans Windows Server 2008, vous pouvez également tirer parti des contrôleurs de domaine en lecture seule (RODC). Un RODC est un nouveau type de contrôleur de domaine qui héberge des partitions en lecture seule de la base de données Active Directory. À l’exception des mots de passe de compte, un RODC contient tous les objets et attributs Active Directory qu’un contrôleur de domaine accessible en écriture détient. Toutefois, les modifications ne peuvent pas être apportées à la base de données stockée sur le contrôleur de domaine en lecture seule. Les modifications doivent être apportées sur un contrôleur de domaine accessible en écriture, puis répliquées sur le RODC.  
  
Un contrôleur de domaine en lecture seule est conçu principalement pour être déployé dans des environnements distants ou des succursales, qui ont généralement un nombre limité d’utilisateurs, une sécurité physique médiocre, une bande passante réseau relativement médiocre à un site Hub et un personnel ayant une connaissance limitée des informations. technologie. Le déploiement de RODC produit une sécurité améliorée et un accès plus efficace aux ressources réseau. Pour plus d’informations sur les fonctionnalités RODC, consultez AD DS : Contrôleurs de domaine en lecture seule ([https://go.microsoft.com/fwlink/?LinkID=106616](https://go.microsoft.com/fwlink/?LinkID=106616)). Pour plus d’informations sur le déploiement d’un contrôleur de domaine en lecture seule, consultez le guide pas à pas pour les contrôleurs de domaine en lecture seule ([https://go.microsoft.com/fwlink/?LinkID=92728](https://go.microsoft.com/fwlink/?LinkID=92728)).  
  
> [!NOTE]  
> Ce guide n’explique pas comment déterminer le nombre approprié de contrôleurs de domaine et la configuration matérielle requise du contrôleur de domaine pour chaque domaine représenté dans chaque site.  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Planification de l’emplacement des contrôleurs de domaine racines de forêt](../../ad-ds/plan/Planning-Forest-Root-Domain-Controller-Placement.md)  
  
-   [Planification de l’emplacement des contrôleurs de domaine régionaux](../../ad-ds/plan/Planning-Regional-Domain-Controller-Placement.md)  
  
-   [Planification de l’emplacement des serveurs de catalogues globaux](../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md)  
  
-   [Planification de l’emplacement des rôles de maître d’opérations](../../ad-ds/plan/Planning-Operations-Master-Role-Placement.md)  
  


