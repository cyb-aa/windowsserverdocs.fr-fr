---
title: 'Étape 7 : Effectuer les tâches post-migration pour la migration vers Windows Server Essentials'
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: d382e3fd-d393-4bd0-883f-db50104a969f
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 579be2c36ca01a4b8ab2a34157e13e298e34c48c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852322"
---
# <a name="step-7-perform-post-migration-tasks-for-the-windows-server-essentials-migration"></a>Étape 7 : Effectuer les tâches post-migration pour la migration vers Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Les tâches suivantes vous aident à terminer la configuration de votre serveur de destination avec certains paramètres présents sur le serveur source. Vous avez peut-être désactivé certains paramètres sur votre serveur source pendant le processus de migration. Dans ce cas, ils ne sont pas migrés vers le serveur de destination.  
  
1.  [Supprimer les entrées DNS du serveur source](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
2.  [Partager des dossiers de données métier et d’autres applications](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
##  <a name="delete-dns-entries-for-the-source-server"></a><a name="BKMK_DeleteDNSEntries"></a>Supprimer les entrées DNS du serveur source  
 Une fois le serveur source désaffecté, le serveur DNS (Domain Name Service) peut encore contenir des entrées qui pointent vers le serveur source. Supprimez ces entrées DNS.  
  
#### <a name="to-delete-dns-entries-that-point-to-the-source-server"></a>Pour supprimer des entrées DNS qui pointent vers le serveur source  
  
1.  Sur le serveur de destination, ouvrez le **Gestionnaire DNS**.  
  
2.  Dans le Gestionnaire DNS, cliquez avec le bouton droit sur le nom du serveur, cliquez sur **Propriétés**, puis cliquez sur l'onglet **Redirecteurs**.  
  
3.  Déterminez si l'une des entrées de la liste des redirecteurs pointe vers le serveur source. Si tel est le cas, cliquez sur **Modifier**, puis supprimez cette entrée dans la fenêtre **Modifier les redirecteurs**.  
  
4.  Dans le **Gestionnaire DNS**, développez le nom du serveur, puis **Zones de recherche directes**.  
  
5.  Cliquez avec le bouton droit sur chaque zone de recherche directe, cliquez **Propriétés**, puis cliquez sur l'onglet **Serveurs de noms**.  
  
6.  Dans la zone de texte **Serveurs de noms**, sélectionnez une entrée qui pointe vers le serveur source, cliquez sur **Supprimer**, puis sur **OK**.  
  
7.  Répétez les étapes 5 et 6 pour supprimer tous les pointeurs vers le serveur source.  
  
8.  Cliquez sur **OK** pour fermer la fenêtre **Propriétés**.  
  
9. Dans le **Gestionnaire DNS**, développez **Zones de recherche inversée**.  
  
10. Répétez les étapes 6 à 9 pour supprimer toutes les zones de recherche inversée sur le serveur source.  
  
##  <a name="share-line-of-business-and-other-application-data-folders"></a><a name="BKMK_ShareLineOfBusinessAndOtherApplications"></a>Partager des dossiers de données métier et d’autres applications  
 Vous devez définir les autorisations des dossiers partagés et les autorisations NTFS pour les dossiers des données métier et d'autres applications que vous avez copiés sur le serveur de destination. Une fois les autorisations définies, les dossiers partagés s'affichent dans le tableau de bord sous l'onglet **Stockage**.  
  
 Si vous utilisez un script d'ouverture de session pour mapper des lecteurs aux dossiers partagés, vous devez mettre à jour le script pour effectuer le mappage aux lecteurs sur le serveur de destination.  
  
## <a name="next-steps"></a>Étapes suivantes :  
 Vous avez effectué les tâches de la suite de la migration pour la migration vers Windows Server Essentials. Passez maintenant à [l’étape 8 : exécutez la Best Practices Analyzer Windows Server Essentials](Step-8--Run-the-Windows-Server-Essentials-Best-Practices-Analyzer.md).  
  

Pour afficher toutes les étapes, consultez [migrer vers Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

