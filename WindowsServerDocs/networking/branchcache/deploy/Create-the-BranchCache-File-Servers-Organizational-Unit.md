---
title: Créer l’unité d’organisation serveurs de fichiers BranchCache
description: Cette rubrique fait partie de la BranchCache déploiement Guide pour Windows Server2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé d’optimiser l’utilisation de la bande passante réseau étendu dans les filiales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 2cda192f-6b45-4e6c-88d9-70ca179ddb94
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a92cb3110e4aecb1ef09a45ed14173305722c655
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="create-the-branchcache-file-servers-organizational-unit"></a>Créer l’unité d’organisation serveurs de fichiers BranchCache

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette procédure pour créer une unité d’organisation (UO) dans les Services de domaine ActiveDirectory (ADDS) pour les serveurs de fichiers BranchCache.  
  
L’appartenance au groupe **Admins du domaine**, ou équivalent est le minimum requis pour effectuer cette procédure.  
  
### <a name="to-create-the-branchcache-file-servers-organizational-unit"></a>Pour créer unité d’organisation serveurs de fichiers BranchCache  
  
1.  Sur un ordinateur sur lequel les services ADDS est installé, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **ActiveDirectory Users and Computers**. La console ActiveDirectory Users and Computers s’ouvre.  
  
2.  Dans la console ActiveDirectory Users and Computers, cliquez sur le domaine auquel vous souhaitez ajouter une unité d’organisation. Par exemple, si votre domaine est nommé example.com, avec le bouton droit sur **example.com**. Pointez sur **New**, puis cliquez sur **unité d’organisation**. Le **nouvel objet - unité d’organisation** boîte de dialogue s’ouvre.  
  
3.  Dans le **nouvel objet - unité d’organisation** la boîte de dialogue dans **nom**, tapez un nom pour la nouvelle unité d’organisation. Par exemple, si vous souhaitez nommer les serveurs de fichiers BranchCache de l’unité d’organisation, tapez **serveurs de fichiers BranchCache**, puis cliquez sur **OK**.  
  


