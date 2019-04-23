---
title: Déplacer des serveurs de fichiers vers l’unité d’organisation Serveurs de fichiers BranchCache
description: Cette rubrique fait partie de BranchCache déploiement Guide pour Windows Server 2016, qui montre comment déployer BranchCache en mode cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les succursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 56c915ec-edb1-43b0-8ad2-c93841bb566f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 037b354bb6725ac7f91fc323b81bbdf15d03ac15
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885900"
---
# <a name="move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>Déplacer des serveurs de fichiers vers l’unité d’organisation Serveurs de fichiers BranchCache

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour ajouter des serveurs de fichiers BranchCache à une unité d’organisation (UO) dans les Services de domaine Active Directory (AD DS).  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs du domaine** ou à un groupe équivalent.  
  
> [!NOTE]  
> Vous devez créer une unité d'organisation Serveurs de fichiers BranchCache dans la console Utilisateurs et ordinateurs Active Directory avant d'ajouter des comptes d'ordinateur à l'unité d'organisation avec cette procédure. Pour plus d’informations, consultez [créer l’unité d’organisation serveurs de fichiers BranchCache](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md).  
  
### <a name="to-move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>Pour déplacer des serveurs de fichiers pour le BranchCache unité d’organisation serveurs de fichiers  
  
1.  Sur un ordinateur où AD DS est installé, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **Active Directory Users and Computers**. La console Utilisateurs et ordinateurs Active Directory s'ouvre.  
  
2.  Dans la console Utilisateurs et ordinateurs Active Directory, recherchez le compte d'ordinateur pour un serveur de fichiers BranchCache, cliquez pour sélectionner le compte, puis glissez-déplacez le compte d'ordinateur sur l'unité d'organisation Serveurs de fichiers BranchCache que vous avez créée précédemment. Par exemple, si vous avez créé précédemment une unité d’organisation nommée **serveurs de fichiers BranchCache**, faites glisser le compte d’ordinateur sur le **serveurs de fichiers BranchCache** unité d’organisation.  
  
3.  Répétez l’étape précédente pour chaque serveur de fichiers BranchCache dans le domaine que vous souhaitez déplacer vers l’unité d’organisation.  
  


