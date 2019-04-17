---
title: Créer l’objet de stratégie de groupe Publication de hachage BranchCache
description: Cette rubrique fait partie de la BranchCache déploiement Guide pour Windows Server2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé d’optimiser l’utilisation de la bande passante réseau étendu dans les filiales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: c3d33bed-83ef-4eb8-acf9-0719ecb4a931
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4510d13c806adc2db46dfccce02a5a1b2814a2c4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="create-the-branchcache-hash-publication-group-policy-object"></a>Créer l’objet de stratégie de groupe Publication de hachage BranchCache

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette procédure pour créer la publication de hachages BranchCache objet de stratégie de groupe (GPO).  
  
L’appartenance au groupe **Admins du domaine**, ou équivalent est le minimum requis pour effectuer cette procédure.  
  
> [!NOTE]  
> Avant d’effectuer cette procédure, vous devez créer l’unité d’organisation de serveurs de fichier de BranchCache et déplacer les serveurs de fichiers dans l’unité d’organisation. Pour plus d’informations, voir [activer la publication de hachages pour les serveurs de fichiers membre de domaine](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).  
  
### <a name="to-create-the-branchcache-hash-publication-group-policy-object"></a>Pour créer la publication de hachages BranchCache objet de stratégie de groupe  
  
1.  Ouvrez Windows PowerShell, tapez **mmc**, puis appuyez sur ENTRÉE. La Console MMC (MicrosoftManagement Console) s’ouvre.  
  
2.  Dans la console MMC, sur le **fichier** menu, cliquez sur **ajouter/supprimer un composant logiciel enfichable**. Le **ajouter ou supprimer des composants logiciel enfichables** boîte de dialogue s’ouvre.  
  
3.  Dans **ajouter ou supprimer des composants logiciel enfichables**, dans **des composants logiciels enfichables disponibles**, double-cliquez sur **gestion des stratégies de groupe**, puis cliquez sur **OK**.  
  
4.  Dans la console MMC Gestion de stratégie de groupe, développez le chemin d’accès aux serveurs de fichiers BranchCache unité d’organisation que vous avez créé précédemment. Par exemple, si votre forêt s’appelle example.com, votre domaine est nommé example1.com et nommée votre unité d’organisation serveurs de fichiers BranchCache, développez le chemin suivant: **gestion des stratégies de groupe**, **forêt: example.com**, **domaines**, **example1.com**, **serveurs de fichiers BranchCache**.  
  
5.  Avec le bouton droit **serveurs de fichiers BranchCache**, puis cliquez sur **créer un objet GPO dans ce domaine et le lier ici**. Le **nouvel objet GPO** boîte de dialogue s’ouvre. Dans **nom**, tapez un nom pour le nouvel objet GPO. Par exemple, si vous souhaitez nommer l’objet Publication de hachages BranchCache, tapez **Publication de hachages BranchCache**. Cliquez sur **OK**.  
  


