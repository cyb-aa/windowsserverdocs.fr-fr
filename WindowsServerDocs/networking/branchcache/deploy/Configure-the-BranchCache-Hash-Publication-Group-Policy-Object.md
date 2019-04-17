---
title: Configurer l’objet de stratégie de groupe Publication de hachage BranchCache
description: Cette rubrique fait partie de la BranchCache déploiement Guide pour Windows Server2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé d’optimiser l’utilisation de la bande passante réseau étendu dans les filiales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da74fea7-52b2-4d6d-9d21-19184eedbe3c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6f528c9f0a8a39b286ad7ac4fa728d93c311f779
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-branchcache-hash-publication-group-policy-object"></a>Configurer l’objet de stratégie de groupe Publication de hachage BranchCache

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette procédure pour configurer la publication de hachages BranchCache objet de stratégie de groupe (GPO) afin que tous les serveurs de fichiers que vous avez ajouté à votre unité d’organisation aient le même paramètre de stratégie de publication hachage appliqué.  
  
L’appartenance au groupe **Admins du domaine**, ou équivalent est le minimum requis pour effectuer cette procédure.  
  
> [!NOTE]  
> Avant d’effectuer cette procédure, vous devez créer l’unité de d’organisation serveurs de fichiers BranchCache, déplacer les serveurs de fichiers dans l’unité d’organisation et créer la publication de hachages BranchCache objet stratégie de groupe. Pour plus d’informations, voir [activer la publication de hachages pour les serveurs de fichiers membre de domaine](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).  
  
### <a name="to-configure-the-branchcache-hash-publication-group-policy-object"></a>Pour configurer la publication de hachages BranchCache objet de stratégie de groupe  
  
1.  Exécutez Windows PowerShell en tant qu’administrateur, tapez **mmc**, puis appuyez sur ENTRÉE. La Console MMC (MicrosoftManagement Console) s’ouvre.  
  
2.  Dans la console MMC, sur le **fichier** menu, cliquez sur **ajouter/supprimer un composant logiciel enfichable**. Le **ajouter ou supprimer des composants logiciel enfichables** boîte de dialogue s’ouvre.  
  
3.  Dans **ajouter ou supprimer des composants logiciel enfichables**, dans **des composants logiciels enfichables disponibles**, double-cliquez sur **gestion des stratégies de groupe**, puis cliquez sur **OK**.  
  
4.  Dans la console MMC Gestion de stratégie de groupe, développez le chemin d’accès à la publication de hachages BranchCache GPO que vous avez créé précédemment. Par exemple, si votre forêt s’appelle example.com, votre domaine est nommé example1.com et votre objet de stratégie de groupe est nommé **Publication de hachages BranchCache**, développez le chemin suivant: **gestion des stratégies de groupe**, **forêt: example.com**, **domaines**, **example1.com**, **objets de stratégie de groupe**, **Publication de hachages BranchCache**.  
  
5.  Avec le bouton droit le **Publication de hachages BranchCache** objet stratégie de groupe, puis cliquez sur **modifier**. La console de l’éditeur de gestion de stratégie de groupe s’ouvre.  
  
6.  Dans la console Éditeur de gestion de stratégie de groupe, développez le chemin suivant: **Configuration ordinateur**, **stratégies**, **modèles d’administration**, **réseau**, **serveur Lanman**.  
  
7.  Dans la console Éditeur de gestion de stratégie de groupe, cliquez sur **serveur Lanman**. Dans le volet d’informations, double-cliquez sur **hachages pour BranchCache**. Le **hachages pour BranchCache** boîte de dialogue s’ouvre.  
  
8.  Dans le **hachages pour BranchCache** boîte de dialogue, cliquez sur **activé**.  
  
9. Dans **Options**, cliquez sur **autoriser la publication de hachages pour tous les dossiers partagés**, puis cliquez sur une des opérations suivantes:  
  
    1.  Pour activer la publication de hachages pour tous les dossiers partagés pour tous les fichiers des serveurs que vous avez ajouté à l’unité d’organisation, cliquez sur **autoriser la publication de hachages pour tous les dossiers partagés**.  
  
    2.  Pour activer la publication de hachages uniquement pour les dossiers partagés pour lesquels BranchCache est activé, cliquez sur **autoriser la publication de hachages uniquement pour les dossiers partagés sur lesquels BranchCache est activé**.  
  
    3.  Pour désactiver la publication de hachages pour tous les dossiers partagés sur l’ordinateur même si BranchCache est activé sur les partages de fichiers, cliquez sur **interdire les hachages sur tous les dossiers partagés**.  
  
10. Cliquez sur **OK**.  
  
> [!NOTE]  
> Dans la plupart des cas, vous devez enregistrer la console MMC et actualiser l’affichage pour afficher les modifications de configuration que vous avez apportées.  
  


