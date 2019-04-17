---
title: "Effectuer des tâches de post-migration pour WindowsServerEssentials migration1"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f2d236a4-0d62-4961-9d1f-332054e06f6d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 535a547ded55cb4afc0942259eadf5222a815274
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="perform-post-migration-tasks-for-windows-server-essentials-migration1"></a>Effectuer des tâches de post-migration pour WindowsServerEssentials migration1

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Les tâches suivantes vous aident à terminer la configuration de votre serveur de Destination avec certains de ces paramètres sont le serveur Source. Vous avez peut-être désactivé certains de ces paramètres sur votre serveur Source pendant le processus de migration, afin qu’ils ne sont pas migrés vers le serveur de Destination. Ou bien, ils sont les étapes de configuration facultatives que vous souhaitez effectuer.  
  

-   [Supprimer les entrées DNS du serveur Source](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
-   [Partage de métier et d’autres dossiers de données d’application](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
-   [Résoudre les problèmes de l’ordinateur client après la migration](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_FixClientComputerIssuesAfterMigrating)  
  
-   [Donner au groupe Administrateurs intégré le droit d’ouvrir une session en tant que tâche](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_AdminGroup)  

-   [Supprimer les entrées DNS du serveur Source](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
-   [Partage de métier et d’autres dossiers de données d’application](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
-   [Résoudre les problèmes de l’ordinateur client après la migration](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_FixClientComputerIssuesAfterMigrating)  
  
-   [Donner au groupe Administrateurs intégré le droit d’ouvrir une session en tant que tâche](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_AdminGroup)  

  
##  <a name="BKMK_DeleteDNSEntries"></a>Supprimer les entrées DNS du serveur Source  
 Une fois que le serveur Source est désaffecté, le serveur du Service DNS (Domain Name) peut encore contenir des entrées qui pointent vers le serveur Source. Supprimez ces entrées DNS.  
  
#### <a name="to-delete-dns-entries-that-point-to-the-source-server"></a>Pour supprimer les entrées DNS qui pointent vers le serveur Source  
  
1.  Sur le serveur de Destination, ouvrez **Gestionnaire DNS**.  
  
2.  Dans le Gestionnaire DNS, cliquez sur le nom du serveur, cliquez sur **propriétés**, puis cliquez sur le **redirecteurs** onglet.  
  
3.  Déterminer s’il existe une entrée dans la liste des redirecteurs qui pointe vers le serveur Source. S’il existe, cliquez sur **modifier**, puis supprimez cette entrée dans le **modifier les redirecteurs** fenêtre.  
  
4.  Dans **Gestionnaire DNS**, développez le nom du serveur et puis **Zones de recherche directe**.  
  
5.  Pour chaque Zone de recherche directe, cliquez sur la zone, cliquez sur **propriétés**, puis cliquez sur le **serveurs de noms** onglet.  
  
6.  Cliquez sur une entrée dans le **serveurs de noms** zone qui pointe vers le serveur Source, cliquez sur **supprimer**, puis cliquez sur **OK**.  
  
7.  Répétez les étapes5 et 6 jusqu'à ce que tous les pointeurs vers le serveur Source sont supprimés.  
  
8.  Cliquez sur **OK** pour fermer la **propriétés** fenêtre.  
  
9. Dans le **Gestionnaire DNS** de la console, développez **Zones de recherche inversée**.  
  
10. Répétez les étapes6 à 9 pour supprimer toutes les Zones de recherche inversée qui pointent vers le serveur Source.  
  
##  <a name="BKMK_ShareLineOfBusinessAndOtherApplications"></a>Partage de métier et d’autres dossiers de données d’application  
 Vous devez définir les autorisations de dossier partagé et les autorisations NTFS pour line-of-business et d’autres dossiers de données d’application que vous avez copié sur le serveur de Destination. Après avoir défini les autorisations, les dossiers partagés s’affichent dans le tableau de bord WindowsServerEssentials dans le **stockage** section.  
  
 Si vous utilisez un script d’ouverture de session pour mapper des lecteurs aux dossiers partagés, vous devez mettre à jour le script pour mapper aux lecteurs sur le serveur de Destination.  
  
##  <a name="BKMK_FixClientComputerIssuesAfterMigrating"></a>Résoudre les problèmes de l’ordinateur client après la migration  
 Si vous migrez vers WindowsServerEssentials à partir de WindowsSmallBusinessServer2003 Premium Edition avec MicrosoftInternet Security et Acceleration (ISA) Server installé, les ordinateurs clients sur le réseau ont toujours le Client de pare-feu Microsoft et Internet Explorer est configuré pour utiliser un serveur proxy.  
  
 Cela entraîne des problèmes de connectivité sur le client des ordinateurs, car le serveur proxy n’existe plus. S’il existe un serveur proxy différent configuré, les ordinateurs clients continuent à utiliser le serveur qui exécute Windows SBS2003 pour le serveur proxy. Pour résoudre ce problème, vous devez reconfigurer Internet Explorer à utiliser le nouveau serveur proxy ou de ne pas utiliser un serveur proxy.  
  
#### <a name="to-reconfigure-internet-explorer"></a>Pour reconfigurer Internet Explorer  
  
1.  Dans Internet Explorer, cliquez sur **outils**, puis cliquez sur **Options Internet**.  
  
2.  Cliquez sur le **connexions**, cliquez sur **paramètres LAN**, puis effectuez l’une des opérations suivantes:  
  
    -   Si vous n’utilisez pas un serveur proxy sur votre réseau, désactivez toutes les cases à cocher dans la **les paramètres de réseau local (LAN)** boîte de dialogue.  
  
    -   Si vous souhaitez utiliser un serveur proxy sur votre réseau:  
  
        1.  Dans le **les paramètres de réseau local (LAN)** boîte de dialogue, décochez les cases à cocher dans la **configuration automatique** section.  
  
        2.  Dans le **serveur Proxy** section, vérifiez que les deux cases à cocher sont activées.  
  
        3.  Dans le **adresse**, tapez le nom de domaine complet (FQDN) du serveur proxy.  
  
        4.  Dans le **Port**, tapez **80**.  
  
3.  Cliquez sur **OK** deux fois.  
  
4.  Accédez à un site Web pour vous assurer que les paramètres de connexion sont corrects.  
  
##  <a name="BKMK_AdminGroup"></a>Donner au groupe Administrateurs intégré le droit d’ouvrir une session en tant que tâche  
 Après avoir migré un domaine WindowsSmallBusinessServer2003 existant vers WindowsServerEssentials, vous devez donner au groupe Administrateurs intégré le droit d’ouvrir une session en tant que traitement par lots. Vérifiez que le groupe Administrateurs intégré dispose toujours le droit d’ouvrir une session en tant que tâche sur le serveur de Destination. Les administrateurs ont besoin de ce droit pour lancer une alerte sur le serveur de Destination sans ouvrir de session.  
  
#### <a name="to-give-the-built-in-administrators-group-the-right-to-log-on-as-a-batch-job"></a>Pour donner intégré le droit d’ouvrir une session en tant que tâche du groupe Administrateurs  
  
1.  Sur le serveur de Destination, ouvrez le **gestion des stratégies de groupe** outil d’administration.  
  
2.  Dans le **gestion des stratégies de groupe** arborescence de la Console, développez **forêt:***< serverName\ >*, développez domaines, puis développez votre serveur.  
  
3.  Développez **contrôleurs de domaine**, avec le bouton droit **stratégie des contrôleurs de domaine par défaut**, puis cliquez sur **modifier**.  
  
4.  Dans **éditeur de gestion de stratégie de groupe**, cliquez sur **stratégie des contrôleurs de domaine par défaut***< serverName\ >***stratégie**, puis développez **Configuration ordinateur**.  
  
5.  Développez **stratégies**, développez **paramètres Windows**, puis développez **paramètres de sécurité**.  
  
6.  Dans le **paramètres de sécurité** de l’arborescence, développez **stratégies locales**, puis cliquez sur **attribution des droits utilisateur**.  
  
7.  Dans le volet des résultats, cliquez sur **ouvrir une session en tant que traitement par lots**, puis cliquez sur Propriétés.  
  
8.  Dans le **ouvrir une session en tant que tâche propriétés**, cliquez sur **ajouter un utilisateur ou groupe**.  
  
9. Dans le **ajouter un utilisateur ou groupe** boîte de dialogue, cliquez sur **Parcourir**.  
  
10. Dans le **sélectionner des utilisateurs, ordinateurs ou les groupes** boîte de dialogue, tapez **administrateurs**.  
  
11. Cliquez sur **vérifier les noms** pour vérifier que le groupe Administrateurs intégré s’affiche, puis cliquez sur **OK** trois fois pour enregistrer le paramètre.  
  
## <a name="see-also"></a>Voir aussi  
  

-   [Migrer à partir de Windows SBS 2003](Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
  
-   [Migrer les données de serveur vers Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Migrer à partir de Windows SBS 2003](../migrate/Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
  
-   [Migrer les données de serveur vers Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

