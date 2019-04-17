---
title: "Étape7: Effectuer les tâches de post-migration pour la migration de WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d382e3fd-d393-4bd0-883f-db50104a969f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6a61d28f29097bcb6993a471587f4cc1ae0bcc3f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="step-7-perform-post-migration-tasks-for-the-windows-server-essentials-migration"></a>Étape7: Effectuer les tâches de post-migration pour la migration de WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Les tâches suivantes vous aident à terminer la configuration de votre serveur de Destination avec certains de ces paramètres sont le serveur Source. Vous avez peut-être désactivé certains de ces paramètres sur votre serveur Source pendant le processus de migration, afin qu’ils ne sont pas migrés vers le serveur de Destination.  
  
1.  [Supprimer des entrées DNS pour le serveur Source](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
2.  [Partage de métier et d’autres dossiers de données d’application](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
##  <a name="BKMK_DeleteDNSEntries"></a>Supprimer des entrées DNS pour le serveur Source  
 Une fois que le serveur Source est désaffecté, le serveur du Service DNS (Domain Name) peut encore contenir des entrées qui pointent vers le serveur Source. Supprimez ces entrées DNS.  
  
#### <a name="to-delete-dns-entries-that-point-to-the-source-server"></a>Pour supprimer les entrées DNS qui pointent vers le serveur Source  
  
1.  Sur le serveur de Destination, ouvrez **Gestionnaire DNS**.  
  
2.  Dans le Gestionnaire DNS, cliquez sur le nom du serveur, cliquez sur **propriétés**, puis cliquez sur le **redirecteurs** onglet.  
  
3.  Déterminer s’il existe une entrée dans la liste des redirecteurs qui pointe vers le serveur Source. S’il existe, cliquez sur **modifier**, puis supprimez cette entrée dans le **modifier les redirecteurs** fenêtre.  
  
4.  Dans **Gestionnaire DNS**, développez le nom du serveur et puis **Zones de recherche directe**.  
  
5.  Pour chaque Zone de recherche directe, cliquez sur la zone, cliquez sur **propriétés**, puis cliquez sur le **serveurs de noms** onglet.  
  
6.  Sélectionnez une entrée dans le **serveurs de noms** zone de texte qui pointe vers le serveur Source, cliquez sur **supprimer**, puis cliquez sur **OK**.  
  
7.  Répétez les étapes5 et 6 jusqu'à ce que tous les pointeurs vers le serveur Source sont supprimés.  
  
8.  Cliquez sur **OK** pour fermer la **propriétés** fenêtre.  
  
9. Dans **Gestionnaire DNS**, développez **Zones de recherche inversée**.  
  
10. Répétez les étapes6 à 9 pour supprimer toutes les Zones de recherche inversée qui pointent vers le serveur Source.  
  
##  <a name="BKMK_ShareLineOfBusinessAndOtherApplications"></a>Partage de métier et d’autres dossiers de données d’application  
 Vous devez définir les autorisations de dossier partagé et les autorisations NTFS pour line-of-business et d’autres dossiers de données d’application que vous avez copié sur le serveur de Destination. Après avoir défini les autorisations, les dossiers partagés s’affichent dans le tableau de bord sur le **stockage** onglet.  
  
 Si vous utilisez un script d’ouverture de session pour mapper des lecteurs aux dossiers partagés, vous devez mettre à jour le script pour mapper aux lecteurs sur le serveur de Destination.  
  
## <a name="next-steps"></a>Étapes suivantes  
 Vous avez effectué les tâches de post-migration pour la migration de WindowsServerEssentials. Passez maintenant à [étape8: exécuter WindowsServerEssentialsBestPractices Analyzer](Step-8--Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  
  

Pour afficher toutes les étapes, voir [migrer vers WindowsServerEssentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

