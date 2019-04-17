---
title: Activer la publication de hachages pour les serveurs de fichiers membres du domaine
description: Cette rubrique fait partie de la BranchCache déploiement Guide pour Windows Server2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé d’optimiser l’utilisation de la bande passante réseau étendu dans les filiales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 11584b73-f9e2-4530-afa5-b8df970e6b24
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4cb3d217115cff3a9b30ee11acb7ba0de6672b24
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="enable-hash-publication-for-non-domain-member-file-servers"></a>Activer la publication de hachages pour les serveurs de fichiers membres du domaine

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette procédure pour configurer la publication de hachages pour BranchCache à l’aide de la stratégie de groupe ordinateur local sur un serveur de fichiers exécutant Windows Server2016 avec la **BranchCache pour fichiers réseau** service de rôle du rôle de serveur Services de fichiers installé.  
  
Cette procédure est conçue pour une utilisation sur un serveur de fichiers membres du domaine. Si vous effectuez cette procédure sur un serveur de fichiers de membre de domaine et que vous configurez également BranchCache à l’aide de la stratégie de groupe du domaine, les paramètres de stratégie de groupe de domaine remplacent les paramètres de stratégie de groupe locaux.  
  
L’appartenance au groupe **administrateurs**, ou équivalent est le minimum requis pour effectuer cette procédure.  
  
> [!NOTE]  
> Si vous avez un ou plusieurs serveurs de fichiers membre de domaine, vous pouvez les ajouter à une unité d’organisation (UO) dans les Services de domaine ActiveDirectory et ensuite utiliser Stratégie de groupe pour configurer la publication de hachages pour tous les serveurs de fichiers en même temps, plutôt que de configurer individuellement chaque serveur de fichiers. Pour plus d’informations, voir [activer la publication de hachages pour les serveurs de fichiers membre de domaine](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).  
  
### <a name="to-enable-hash-publication-for-one-file-server"></a>Pour activer la publication de hachage pour un serveur de fichiers  
  
1.  Ouvrez Windows PowerShell, tapez **mmc**, puis appuyez sur ENTRÉE. La Console MMC (MicrosoftManagement Console) s’ouvre.  
  
2.  Dans la console MMC, sur le **fichier** menu, cliquez sur **ajouter/supprimer un composant logiciel enfichable**. Le **ajouter ou supprimer des composants logiciel enfichables** boîte de dialogue s’ouvre.  
  
3.  Dans **ajouter ou supprimer des composants logiciel enfichables**, dans **des composants logiciels enfichables disponibles**, double-cliquez sur **Éditeur d’objets de stratégie de groupe**. Assistant Stratégie de groupe s’ouvre avec l’objet ordinateur Local sélectionné. Cliquez sur **Terminer**, puis cliquez sur **OK**.  
  
4.  Dans la console MMC Éditeur de stratégie de groupe, développez le chemin d’accès suivant: **stratégie ordinateur Local**, **Configuration ordinateur**, **modèles d’administration**, **réseau**, **serveur Lanman**. Cliquez sur **serveur Lanman**.  
  
5.  Dans le volet d’informations, double-cliquez sur **hachages pour BranchCache**. Le **hachages pour BranchCache** boîte de dialogue s’ouvre.  
  
6.  Dans le **hachages pour BranchCache** boîte de dialogue, cliquez sur **activé**.  
  
7.  Dans **Options**, cliquez sur **autoriser la publication de hachages pour tous les dossiers partagés**, puis cliquez sur une des opérations suivantes:  
  
    1.  Pour activer la publication de hachages pour tous les dossiers partagés sur cet ordinateur, cliquez sur **autoriser la publication de hachages pour tous les dossiers partagés**.  
  
    2.  Pour activer la publication de hachages uniquement pour les dossiers partagés pour lesquels BranchCache est activé, cliquez sur **autoriser la publication de hachages uniquement pour les dossiers partagés sur lesquels BranchCache est activé**.  
  
    3.  Pour désactiver la publication de hachages pour tous les dossiers partagés sur l’ordinateur même si BranchCache est activé sur les partages de fichiers, cliquez sur **interdire les hachages sur tous les dossiers partagés**.  
  
8.  Cliquez sur **OK**.  
  


