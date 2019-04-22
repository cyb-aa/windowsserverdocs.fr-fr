---
title: Effectuer des tâches de post-migration pour migration1 de Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821020"
---
# <a name="perform-post-migration-tasks-for-windows-server-essentials-migration1"></a>Effectuer des tâches de post-migration pour migration1 de Windows Server Essentials

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Les tâches suivantes vous aident à terminer la configuration de votre serveur de destination avec certains paramètres présents sur le serveur source. Vous avez peut-être désactivé certains paramètres sur votre serveur source pendant le processus de migration. Dans ce cas, ils ne sont pas migrés vers le serveur de destination. Ou ils représentent des étapes de configuration facultatives que vous souhaitez peut-être effectuer.  
  

-   [Supprimer les entrées DNS du serveur Source](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
-   [Partager line-of-business et autres dossiers de données d’application](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
-   [Résoudre les problèmes de l’ordinateur client après la migration](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_FixClientComputerIssuesAfterMigrating)  
  
-   [Donnez au groupe Administrateurs intégré le droit Ouvrir une session en tant que traitement par lots](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_AdminGroup)  

-   [Supprimer les entrées DNS du serveur Source](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
-   [Partager line-of-business et autres dossiers de données d’application](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
-   [Résoudre les problèmes de l’ordinateur client après la migration](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_FixClientComputerIssuesAfterMigrating)  
  
-   [Donnez au groupe Administrateurs intégré le droit Ouvrir une session en tant que traitement par lots](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_AdminGroup)  

  
##  <a name="BKMK_DeleteDNSEntries"></a> Supprimer les entrées DNS du serveur Source  
 Une fois le serveur source désaffecté, le serveur DNS (Domain Name Service) peut encore contenir des entrées qui pointent vers le serveur source. Supprimez ces entrées DNS.  
  
#### <a name="to-delete-dns-entries-that-point-to-the-source-server"></a>Pour supprimer des entrées DNS qui pointent vers le serveur source  
  
1.  Sur le serveur de destination, ouvrez le **Gestionnaire DNS**.  
  
2.  Dans le Gestionnaire DNS, cliquez avec le bouton droit sur le nom du serveur, cliquez sur **Propriétés**, puis cliquez sur l'onglet **Redirecteurs** .  
  
3.  Déterminez si l'une des entrées de la liste des redirecteurs pointe vers le serveur source. Si tel est le cas, cliquez sur **Modifier**, puis supprimez cette entrée dans la fenêtre **Modifier les redirecteurs** .  
  
4.  Dans le **Gestionnaire DNS**, développez le nom du serveur, puis **Zones de recherche directes**.  
  
5.  Cliquez avec le bouton droit sur chaque zone de recherche directe, cliquez **Propriétés**, puis cliquez sur l'onglet **Serveurs de noms** .  
  
6.  Dans la zone **Serveurs de noms**, cliquez sur une entrée qui pointe vers le serveur source, sur **Supprimer**, puis sur **OK**.  
  
7.  Répétez les étapes 5 et 6 pour supprimer tous les pointeurs vers le serveur source.  
  
8.  Cliquez sur **OK** pour fermer la fenêtre **Propriétés**.  
  
9. Dans la console **Gestionnaire DNS**, développez **Zones de recherche inversée**.  
  
10. Répétez les étapes 6 à 9 pour supprimer toutes les zones de recherche inversée sur le serveur source.  
  
##  <a name="BKMK_ShareLineOfBusinessAndOtherApplications"></a> Partager line-of-business et autres dossiers de données d’application  
 Vous devez définir les autorisations des dossiers partagés et les autorisations NTFS pour les dossiers des données métier et d'autres applications que vous avez copiés sur le serveur de destination. Après avoir défini les autorisations, les dossiers partagés sont affichés dans le tableau de bord Windows Server Essentials dans le **stockage** section.  
  
 Si vous utilisez un script d'ouverture de session pour mapper des lecteurs aux dossiers partagés, vous devez mettre à jour le script pour effectuer le mappage aux lecteurs sur le serveur de destination.  
  
##  <a name="BKMK_FixClientComputerIssuesAfterMigrating"></a> Résoudre les problèmes de l’ordinateur client après la migration  
 Si vous migrez vers Windows Server Essentials à partir de Windows Small Business Server 2003 Premium Edition avec Microsoft Internet Security et Acceleration (ISA) Server installé, les ordinateurs clients sur le réseau ont toujours le Client de pare-feu Microsoft et Internet Explorer configuré pour utiliser un serveur proxy.  
  
 Cela entraîne des problèmes de connectivité sur les ordinateurs clients, car le serveur proxy n'existe plus. S’il existe un serveur proxy différent configuré, les ordinateurs clients continuent à utiliser le serveur qui exécute Windows SBS 2003 pour le serveur proxy. Pour résoudre ce problème, vous devez reconfigurer Internet Explorer pour qu'il n'utilise pas de serveur proxy ou pour qu'il utilise le nouveau serveur proxy.  
  
#### <a name="to-reconfigure-internet-explorer"></a>Pour reconfigurer Internet Explorer  
  
1.  Dans Internet Explorer, cliquez sur **Outils**, puis sur **Options Internet**.  
  
2.  Cliquez sur l'onglet **Connexions**, sur **Paramètres réseau**, puis effectuez une des opérations suivantes :  
  
    -   Si vous n'utilisez pas de serveur proxy sur votre réseau, décochez toutes les cases de la boîte de dialogue **Paramètres du réseau local**.  
  
    -   Si vous souhaitez utiliser un serveur proxy sur votre réseau :  
  
        1.  Dans la boîte de dialogue **Paramètres du réseau local**, décochez les cases de la section **Configuration automatique**.  
  
        2.  Dans la section **Serveur proxy**, vérifiez que les deux cases sont cochées.  
  
        3.  Dans la zone **Adresse**, tapez le nom de domaine complet du serveur proxy.  
  
        4.  Dans la zone **Port**, tapez **80**.  
  
3.  Cliquez deux fois sur **OK**.  
  
4.  Accédez à un site web pour vérifier que les paramètres de connexion sont corrects.  
  
##  <a name="BKMK_AdminGroup"></a> Donnez au groupe Administrateurs intégré le droit Ouvrir une session en tant que traitement par lots  
 Après avoir migré un domaine Windows Small Business Server 2003 existant vers Windows Server Essentials, vous devez donner au groupe Administrateurs intégré le droit pour ouvrir une session en tant que traitement par lots. Vérifiez que le groupe Administrateurs intégré dispose toujours du droit d'ouvrir une session en tant que tâche sur le serveur de destination. Les administrateurs ont besoin de ce droit pour lancer une alerte sur le serveur de destination sans ouvrir de session.  
  
#### <a name="to-give-the-built-in-administrators-group-the-right-to-log-on-as-a-batch-job"></a>Pour donner au groupe Administrateurs intégré le droit d'ouvrir une session en tant que tâche  
  
1.  Sur le serveur de destination, ouvrez l'outil d'administration **Gestion des stratégies de groupe**.  
  
2.  Dans le **Group Policy Management** arborescence de la Console, développez **forêt :** *< nom_serveur\>*, développez domaines, puis développez votre serveur.  
  
3.  Développez **Contrôleurs de domaine**, cliquez avec le bouton droit sur **Stratégie Contrôleurs de domaine par défaut**, puis cliquez sur **Modifier**.  
  
4.  Dans **éditeur de gestion de stratégie de groupe**, cliquez sur **stratégie des contrôleurs de domaine par défaut ***< nom_serveur\>*** stratégie**, puis développez  **Configuration de l’ordinateur**.  
  
5.  Développez **Stratégies**, **Paramètres Windows**, puis **Paramètres de sécurité**.  
  
6.  Dans l'arborescence **Paramètres de sécurité**, développez **Stratégies locales**, puis cliquez sur **Attribution des droits utilisateur**.  
  
7.  Dans le volet de résultats, cliquez avec le bouton droit sur **Ouvrir une session en tant que tâche**, puis cliquez sur Propriétés.  
  
8.  Dans la page **Propriétés**, cliquez sur **Ajouter un utilisateur ou un groupe**.  
  
9. Dans la boîte de dialogue **Ajouter un utilisateur ou un groupe**, cliquez sur **Parcourir**.  
  
10. Dans la boîte de dialogue **Sélectionner les utilisateurs, les ordinateurs ou les groupes**, tapez **Administrators**.  
  
11. Cliquez sur **Vérifier les noms** pour vérifier que le groupe Administrateurs intégré s'affiche, puis cliquez sur **OK** trois fois pour enregistrer le paramètre.  
  
## <a name="see-also"></a>Voir aussi  
  

-   [Migrer à partir de Windows SBS 2003](Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
  
-   [Migrer des données de serveur vers Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Migrer à partir de Windows SBS 2003](../migrate/Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
  
-   [Migrer des données de serveur vers Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

