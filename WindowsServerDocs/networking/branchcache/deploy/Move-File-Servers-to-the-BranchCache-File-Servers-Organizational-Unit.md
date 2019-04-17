---
title: Déplacer les serveurs de fichiers vers l’unité d’organisation serveurs de fichiers BranchCache
description: Cette rubrique fait partie de la BranchCache déploiement Guide pour Windows Server2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé d’optimiser l’utilisation de la bande passante réseau étendu dans les filiales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 56c915ec-edb1-43b0-8ad2-c93841bb566f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 09afb4936545cb1f5bb14573261008ff18badd4d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>Déplacer les serveurs de fichiers vers l’unité d’organisation serveurs de fichiers BranchCache

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette procédure pour ajouter des serveurs de fichiers BranchCache à une unité d’organisation (UO) dans les Services de domaine Active Directory (AD DS).  
  
L’appartenance au groupe **Admins du domaine**, ou équivalent est le minimum requis pour effectuer cette procédure.  
  
> [!NOTE]  
> Avant d’ajouter des comptes d’ordinateur à l’unité d’organisation avec cette procédure, vous devez créer une unité d’organisation de serveurs de fichiers BranchCache dans la console Active Directory Users and Computers. Pour plus d’informations, voir [créer l’unité d’organisation de BranchCache fichier serveurs](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md).  
  
### <a name="to-move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>Pour déplacer des serveurs de fichiers BranchCache unité d’organisation serveurs de fichiers  
  
1.  Sur un ordinateur sur lequel les services ADDS est installé, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **ActiveDirectory Users and Computers**. La console ActiveDirectory Users and Computers s’ouvre.  
  
2.  Dans la console Active Directory Users and Computers, recherchez le compte d’ordinateur pour un serveur de fichiers BranchCache, cliquez pour sélectionner le compte et puis faites glisser le compte d’ordinateur sur les serveurs de fichiers BranchCache unité d’organisation que vous avez créé précédemment. Par exemple, si vous avez créé précédemment une unité d’organisation nommée **serveurs de fichiers BranchCache**, faites glisser le compte d’ordinateur sur le **serveurs de fichiers BranchCache** unité d’organisation.  
  
3.  Répétez l’étape précédente pour chaque serveur de fichiers BranchCache dans le domaine que vous souhaitez déplacer vers l’unité d’organisation.  
  


