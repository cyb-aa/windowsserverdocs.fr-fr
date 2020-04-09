---
title: Effectuer des tâches de postconnexion pour Windows Server Essentials migration1
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: f2d236a4-0d62-4961-9d1f-332054e06f6d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b5093772e22fc95a19e800db5c83dec261e7b63a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852423"
---
# <a name="perform-post-migration-tasks-for-windows-server-essentials-migration1"></a>Effectuer des tâches de postconnexion pour Windows Server Essentials migration1

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Les tâches suivantes vous aident à terminer la configuration de votre serveur de destination avec certains paramètres présents sur le serveur source. Vous avez peut-être désactivé certains paramètres sur votre serveur source pendant le processus de migration. Dans ce cas, ils ne sont pas migrés vers le serveur de destination. Ou ils représentent des étapes de configuration facultatives que vous souhaitez peut-être effectuer.  
  

-   [Supprimer les entrées DNS du serveur source](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
-   [Partager des dossiers de données métier et d’autres applications](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
-   [Résoudre les problèmes des ordinateurs clients après la migration](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_FixClientComputerIssuesAfterMigrating)  
  
-   [Accordez au groupe Administrateurs intégré le droit d’ouvrir une session en tant que tâche.](Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_AdminGroup)  

-   [Supprimer les entrées DNS du serveur source](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_DeleteDNSEntries)  
  
-   [Partager des dossiers de données métier et d’autres applications](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_ShareLineOfBusinessAndOtherApplications)  
  
-   [Résoudre les problèmes des ordinateurs clients après la migration](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_FixClientComputerIssuesAfterMigrating)  
  
-   [Accordez au groupe Administrateurs intégré le droit d’ouvrir une session en tant que tâche.](../migrate/Perform-post-migration-tasks-for-Windows-Server-Essentials-migration.md#BKMK_AdminGroup)  

  
##  <a name="delete-dns-entries-of-the-source-server"></a><a name="BKMK_DeleteDNSEntries"></a>Supprimer les entrées DNS du serveur source  
 Une fois le serveur source désaffecté, le serveur DNS (Domain Name Service) peut encore contenir des entrées qui pointent vers le serveur source. Supprimez ces entrées DNS.  
  
#### <a name="to-delete-dns-entries-that-point-to-the-source-server"></a>Pour supprimer des entrées DNS qui pointent vers le serveur source  
  
1.  Sur le serveur de destination, ouvrez le **Gestionnaire DNS**.  
  
2.  Dans le Gestionnaire DNS, cliquez avec le bouton droit sur le nom du serveur, cliquez sur **Propriétés**, puis cliquez sur l'onglet **Redirecteurs**.  
  
3.  Déterminez si l'une des entrées de la liste des redirecteurs pointe vers le serveur source. Si tel est le cas, cliquez sur **Modifier**, puis supprimez cette entrée dans la fenêtre **Modifier les redirecteurs**.  
  
4.  Dans le **Gestionnaire DNS**, développez le nom du serveur, puis **Zones de recherche directes**.  
  
5.  Cliquez avec le bouton droit sur chaque zone de recherche directe, cliquez **Propriétés**, puis cliquez sur l'onglet **Serveurs de noms**.  
  
6.  Dans la zone **Serveurs de noms**, cliquez sur une entrée qui pointe vers le serveur source, sur **Supprimer**, puis sur **OK**.  
  
7.  Répétez les étapes 5 et 6 pour supprimer tous les pointeurs vers le serveur source.  
  
8.  Cliquez sur **OK** pour fermer la fenêtre **Propriétés**.  
  
9. Dans la console **Gestionnaire DNS**, développez **Zones de recherche inversée**.  
  
10. Répétez les étapes 6 à 9 pour supprimer toutes les zones de recherche inversée sur le serveur source.  
  
##  <a name="share-line-of-business-and-other-application-data-folders"></a><a name="BKMK_ShareLineOfBusinessAndOtherApplications"></a>Partager des dossiers de données métier et d’autres applications  
 Vous devez définir les autorisations des dossiers partagés et les autorisations NTFS pour les dossiers des données métier et d'autres applications que vous avez copiés sur le serveur de destination. Une fois les autorisations définies, les dossiers partagés s’affichent dans le tableau de bord Windows Server Essentials dans la section **stockage** .  
  
 Si vous utilisez un script d'ouverture de session pour mapper des lecteurs aux dossiers partagés, vous devez mettre à jour le script pour effectuer le mappage aux lecteurs sur le serveur de destination.  
  
##  <a name="fix-client-computer-issues-after-migrating"></a><a name="BKMK_FixClientComputerIssuesAfterMigrating"></a>Résoudre les problèmes des ordinateurs clients après la migration  
 Si vous migrez vers Windows Server Essentials à partir de Windows Small Business Server 2003 Premium Edition avec Microsoft Internet Security and Acceleration (ISA) Server installé, les ordinateurs clients sur le réseau disposent toujours du client de pare-feu Microsoft et d’Internet Explorateur configuré pour utiliser un serveur proxy.  
  
 Cela entraîne des problèmes de connectivité sur les ordinateurs clients, car le serveur proxy n'existe plus. Si un autre serveur proxy est configuré, les ordinateurs clients continuent d’utiliser le serveur exécutant Windows SBS 2003 pour le serveur proxy. Pour résoudre ce problème, vous devez reconfigurer Internet Explorer pour qu'il n'utilise pas de serveur proxy ou pour qu'il utilise le nouveau serveur proxy.  
  
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
  
##  <a name="give-the-built-in-administrators-group-the-right-to-log-on-as-a-batch-job"></a><a name="BKMK_AdminGroup"></a>Accordez au groupe Administrateurs intégré le droit d’ouvrir une session en tant que tâche.  
 Après la migration d’un domaine Windows Small Business Server 2003 existant vers Windows Server Essentials, vous devez accorder au groupe Administrateurs intégré le droit d’ouvrir une session en tant que tâche. Vérifiez que le groupe Administrateurs intégré dispose toujours du droit d'ouvrir une session en tant que tâche sur le serveur de destination. Les administrateurs ont besoin de ce droit pour lancer une alerte sur le serveur de destination sans ouvrir de session.  
  
#### <a name="to-give-the-built-in-administrators-group-the-right-to-log-on-as-a-batch-job"></a>Pour donner au groupe Administrateurs intégré le droit d'ouvrir une session en tant que tâche  
  
1. Sur le serveur de destination, ouvrez l'outil d'administration **Gestion des stratégies de groupe**.  
  
2. Dans l’arborescence de la console de **gestion stratégie de groupe** , développez **forêt :** *< NomServeur\>* , domaines, puis votre serveur.  
  
3. Développez **Contrôleurs de domaine**, cliquez avec le bouton droit sur **Stratégie Contrôleurs de domaine par défaut**, puis cliquez sur **Modifier**.  
  
4. Dans **éditeur de gestion des stratégies de groupe**, cliquez **sur stratégie des contrôleurs de domaine par défaut**<em>< nom</em> de la **stratégie de**\>ServerName, puis développez Configuration de l' **ordinateur**.  
  
5. Développez **Stratégies**, **Paramètres Windows**, puis **Paramètres de sécurité**.  
  
6. Dans l'arborescence **Paramètres de sécurité**, développez **Stratégies locales**, puis cliquez sur **Attribution des droits utilisateur**.  
  
7. Dans le volet de résultats, cliquez avec le bouton droit sur **Ouvrir une session en tant que tâche**, puis cliquez sur Propriétés.  
  
8. Dans la page **Propriétés**, cliquez sur **Ajouter un utilisateur ou un groupe**.  
  
9. Dans la boîte de dialogue **Ajouter un utilisateur ou un groupe**, cliquez sur **Parcourir**.  
  
10. Dans la boîte de dialogue **Sélectionner les utilisateurs, les ordinateurs ou les groupes**, tapez **Administrators**.  
  
11. Cliquez sur **Vérifier les noms** pour vérifier que le groupe Administrateurs intégré s'affiche, puis cliquez sur **OK** trois fois pour enregistrer le paramètre.  
  
## <a name="see-also"></a>Voir aussi  
  

-   [Migrer à partir de Windows SBS 2003](Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
  
-   [Migrer les données du serveur vers Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Migrer à partir de Windows SBS 2003](../migrate/Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
  
-   [Migrer les données du serveur vers Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

