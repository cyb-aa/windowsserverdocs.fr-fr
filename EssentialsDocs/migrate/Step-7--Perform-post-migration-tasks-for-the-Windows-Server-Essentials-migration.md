---
title: 'Étape 7 : Effectuer des tâches de post-migration pour la migration de Windows Server Essentials'
description: Décrit comment utiliser Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819160"
---
# <a name="step-7-perform-post-migration-tasks-for-the-windows-server-essentials-migration"></a>Étape 7 : Effectuer des tâches de post-migration pour la migration de Windows Server Essentials

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Les tâches suivantes vous aident à terminer la configuration de votre serveur de destination avec certains paramètres présents sur le serveur source. Vous avez peut-être désactivé certains paramètres sur votre serveur source pendant le processus de migration. Dans ce cas, ils ne sont pas migrés vers le serveur de destination.  
  
1.  [Supprimer les entrées DNS pour le serveur Source](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
2.  [Partager line-of-business et autres dossiers de données d’application](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
##  <a name="BKMK_DeleteDNSEntries"></a> Supprimer les entrées DNS pour le serveur Source  
 Une fois le serveur source désaffecté, le serveur DNS (Domain Name Service) peut encore contenir des entrées qui pointent vers le serveur source. Supprimez ces entrées DNS.  
  
#### <a name="to-delete-dns-entries-that-point-to-the-source-server"></a>Pour supprimer des entrées DNS qui pointent vers le serveur source  
  
1.  Sur le serveur de destination, ouvrez le **Gestionnaire DNS**.  
  
2.  Dans le Gestionnaire DNS, cliquez avec le bouton droit sur le nom du serveur, cliquez sur **Propriétés**, puis cliquez sur l'onglet **Redirecteurs** .  
  
3.  Déterminez si l'une des entrées de la liste des redirecteurs pointe vers le serveur source. Si tel est le cas, cliquez sur **Modifier**, puis supprimez cette entrée dans la fenêtre **Modifier les redirecteurs** .  
  
4.  Dans le **Gestionnaire DNS**, développez le nom du serveur, puis **Zones de recherche directes**.  
  
5.  Cliquez avec le bouton droit sur chaque zone de recherche directe, cliquez **Propriétés**, puis cliquez sur l'onglet **Serveurs de noms** .  
  
6.  Dans la zone de texte **Serveurs de noms** , sélectionnez une entrée qui pointe vers le serveur source, cliquez sur **Supprimer**, puis sur **OK**.  
  
7.  Répétez les étapes 5 et 6 pour supprimer tous les pointeurs vers le serveur source.  
  
8.  Cliquez sur **OK** pour fermer la fenêtre **Propriétés**.  
  
9. Dans le **Gestionnaire DNS**, développez **Zones de recherche inversée**.  
  
10. Répétez les étapes 6 à 9 pour supprimer toutes les zones de recherche inversée sur le serveur source.  
  
##  <a name="BKMK_ShareLineOfBusinessAndOtherApplications"></a> Partager line-of-business et autres dossiers de données d’application  
 Vous devez définir les autorisations des dossiers partagés et les autorisations NTFS pour les dossiers des données métier et d'autres applications que vous avez copiés sur le serveur de destination. Une fois les autorisations définies, les dossiers partagés s'affichent dans le tableau de bord sous l'onglet **Stockage**.  
  
 Si vous utilisez un script d'ouverture de session pour mapper des lecteurs aux dossiers partagés, vous devez mettre à jour le script pour effectuer le mappage aux lecteurs sur le serveur de destination.  
  
## <a name="next-steps"></a>Étapes suivantes  
 Vous avez effectué les tâches de post-migration pour la migration de Windows Server Essentials. Passez maintenant à [étape 8 : exécuter Windows Server Essentials Best Practices Analyzer](Step-8--Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  
  

Pour afficher toutes les étapes, consultez [migrer vers Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

