---
title: Créer l’objet de stratégie de groupe de la publication de hachages BranchCache
description: Cette rubrique fait partie de BranchCache déploiement Guide pour Windows Server 2016, qui montre comment déployer BranchCache en mode cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les succursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: c3d33bed-83ef-4eb8-acf9-0719ecb4a931
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 15e89b961d20b6f14065392553e413374358062d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865430"
---
# <a name="create-the-branchcache-hash-publication-group-policy-object"></a>Créer l’objet de stratégie de groupe de la publication de hachages BranchCache

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour créer l’objet de stratégie de groupe (GPO) de la publication de hachages BranchCache.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs du domaine** ou à un groupe équivalent.  
  
> [!NOTE]  
> Avant d'exécuter cette procédure, vous devez créer l'unité d'organisation Serveurs de fichiers BranchCache et déplacer les serveurs de fichiers dans l'unité d'organisation. Pour plus d’informations, consultez [activer la Publication de hachages pour les serveurs de fichiers de membre de domaine](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).  
  
### <a name="to-create-the-branchcache-hash-publication-group-policy-object"></a>Pour créer l’objet stratégie de groupe de la publication de hachages BranchCache  
  
1.  Ouvrez Windows PowerShell, saisissez **mmc**, puis appuyez sur ENTRÉE. La console MMC s'affiche.  
  
2.  Dans la console MMC, dans le menu **Fichier**, cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**. La boîte de dialogue **Ajouter ou supprimer des composants logiciels enfichables** s'ouvre.  
  
3.  Dans **Ajouter ou supprimer des composants logiciels enfichables**, dans **Composants logiciels enfichables disponibles**, double-cliquez sur **Gestion des stratégies de groupe**, puis cliquez sur **OK**.  
  
4.  Dans la console MMC de gestion de la stratégie groupe, développez le chemin d'accès à l'unité d'organisation Serveurs de fichiers BranchCache que vous avez créée précédemment. Par exemple, si votre forêt s’appelle example.com, votre domaine s’appelle example1.com, et votre unité d’organisation est nommée de serveurs de fichiers BranchCache, développez le chemin suivant : **Gestion de stratégie de groupe**, **forêt : example.com**, **domaines**, **exemple1.com**, **serveurs de fichiers BranchCache**.  
  
5.  Avec le bouton droit **serveurs de fichiers BranchCache**, puis cliquez sur **créer un objet GPO dans ce domaine et le lier ici**. La boîte de dialogue **Nouvel objet GPO** s'ouvre. Dans **nom**, tapez un nom pour le nouvel objet GPO. Par exemple, si vous souhaitez nommer l'objet Publication de hachages BranchCache, tapez **Publication de hachages BranchCache**. Cliquez sur **OK**.  
  


