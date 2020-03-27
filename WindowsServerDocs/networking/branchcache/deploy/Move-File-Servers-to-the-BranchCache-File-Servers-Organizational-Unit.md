---
title: Déplacer des serveurs de fichiers vers l’unité d’organisation Serveurs de fichiers BranchCache
description: Cette rubrique fait partie du Guide de déploiement BranchCache pour Windows Server 2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les filiales.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 56c915ec-edb1-43b0-8ad2-c93841bb566f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 045764f9ed7cb7eef5a996748e293b48ec6018fb
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319203"
---
# <a name="move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>Déplacer des serveurs de fichiers vers l’unité d’organisation Serveurs de fichiers BranchCache

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour ajouter des serveurs de fichiers BranchCache à une unité d’organisation (UO) dans Active Directory Domain Services (AD DS).  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs du domaine** ou à un groupe équivalent.  
  
> [!NOTE]  
> Vous devez créer une unité d'organisation Serveurs de fichiers BranchCache dans la console Utilisateurs et ordinateurs Active Directory avant d'ajouter des comptes d'ordinateur à l'unité d'organisation avec cette procédure. Pour plus d’informations, consultez [créer l’unité d’organisation serveurs de fichiers BranchCache](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md).  
  
### <a name="to-move-file-servers-to-the-branchcache-file-servers-organizational-unit"></a>Pour déplacer des serveurs de fichiers vers l’unité d’organisation serveurs de fichiers BranchCache  
  
1.  Sur un ordinateur sur lequel AD DS est installé, dans Gestionnaire de serveur, cliquez sur **Outils**, puis sur **Active Directory utilisateurs et ordinateurs**. La console Utilisateurs et ordinateurs Active Directory s'ouvre.  
  
2.  Dans la console Utilisateurs et ordinateurs Active Directory, recherchez le compte d'ordinateur pour un serveur de fichiers BranchCache, cliquez pour sélectionner le compte, puis glissez-déplacez le compte d'ordinateur sur l'unité d'organisation Serveurs de fichiers BranchCache que vous avez créée précédemment. Par exemple, si vous avez créé précédemment une UO nommée **serveurs de fichiers BranchCache**, glissez-déplacez le compte d’ordinateur sur l’unité d’organisation **serveurs de fichiers BranchCache** .  
  
3.  Répétez l’étape précédente pour chaque serveur de fichiers BranchCache dans le domaine que vous souhaitez déplacer vers l’unité d’organisation.  
  


