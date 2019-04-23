---
title: Créer l’unité d’organisation Serveurs de fichiers BranchCache
description: Cette rubrique fait partie de BranchCache déploiement Guide pour Windows Server 2016, qui montre comment déployer BranchCache en mode cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les succursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 2cda192f-6b45-4e6c-88d9-70ca179ddb94
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b7b26ec5808f5b11141e81dc5e738c83c94ef6b8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874080"
---
# <a name="create-the-branchcache-file-servers-organizational-unit"></a>Créer l’unité d’organisation Serveurs de fichiers BranchCache

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour créer une unité d’organisation (UO) dans les Services de domaine Active Directory (AD DS) pour les serveurs de fichiers BranchCache.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs du domaine** ou à un groupe équivalent.  
  
### <a name="to-create-the-branchcache-file-servers-organizational-unit"></a>Pour créer l’unité d’organisation serveurs de fichiers BranchCache  
  
1.  Sur un ordinateur où AD DS est installé, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **Active Directory Users and Computers**. La console Utilisateurs et ordinateurs Active Directory s'ouvre.  
  
2.  Dans la console Active Directory Users and Computers, cliquez sur le domaine auquel vous souhaitez ajouter une unité d’organisation. Par exemple, si votre domaine s'appelle example.com, cliquez avec le bouton droit sur **example.com**. Pointez sur **Nouveau**, puis cliquez sur **Unité d’organisation**. Le **nouvel objet - unité d’organisation** boîte de dialogue s’ouvre.  
  
3.  Dans le **nouvel objet - unité d’organisation** boîte de dialogue **nom**, tapez un nom pour la nouvelle unité d’organisation. Par exemple, si vous souhaitez nommer l'unité d'organisation Serveurs de fichiers BranchCache, tapez **Serveurs de fichiers BranchCache**, puis cliquez sur **OK**.  
  


