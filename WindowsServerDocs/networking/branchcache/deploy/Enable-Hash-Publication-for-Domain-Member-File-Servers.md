---
title: Activer la publication de hachages pour les serveurs de fichiers membre de domaine
description: Cette rubrique fait partie de la BranchCache déploiement Guide pour Windows Server2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé d’optimiser l’utilisation de la bande passante réseau étendu dans les filiales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: a3f1f7c4-d9b2-43e6-8bfa-fac707bbd4d3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 318879eae82d37f68acbc18cdb21ae5290f6d02b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="enable-hash-publication-for-domain-member-file-servers"></a>Activer la publication de hachages pour les serveurs de fichiers membre de domaine

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Lorsque vous utilisez des Services de domaine ActiveDirectory (ADDS), vous pouvez utiliser la stratégie de groupe du domaine pour activer la publication de hachages BranchCache pour plusieurs serveurs de fichiers. Pour cela, vous devez créer une unité d’organisation (UO), d’ajouter des serveurs de fichiers à l’unité d’organisation, créer une publication de hachages BranchCache objet de stratégie de groupe (GPO), puis configurez l’objet de stratégie de groupe.  
  
Consultez les rubriques suivantes pour activer la publication de hachages pour plusieurs serveurs de fichiers.  
  
-   [Créer l’unité d’organisation serveurs de fichiers BranchCache](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [Déplacer les serveurs de fichiers vers l’unité d’organisation serveurs de fichiers BranchCache](../../branchcache/deploy/Move-File-Servers-to-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [Créer l’objet de stratégie de groupe Publication de hachage BranchCache](../../branchcache/deploy/Create-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  
-   [Configurer l’objet de stratégie de groupe Publication de hachage BranchCache](../../branchcache/deploy/Configure-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  


