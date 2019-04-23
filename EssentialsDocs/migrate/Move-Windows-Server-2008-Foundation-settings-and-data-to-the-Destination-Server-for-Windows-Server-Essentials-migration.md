---
title: Déplacer les paramètres et données de Windows Server 2008 Foundation vers le serveur de destination pour la migration vers Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ff7d040-ebd1-421c-80db-765deacedd4c
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 80e70ba954d981985caa5329379661d978456c7f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867480"
---
# <a name="move-windows-server-2008-foundation-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Déplacer les paramètres et données de Windows Server 2008 Foundation vers le serveur de destination pour la migration vers Windows Server Essentials

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Déplacez les paramètres et les données vers le serveur de destination comme suit : 

1.  [Copier des données vers le serveur de Destination (facultatif)](Move-Windows-Server-2008-Foundation-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_CopyData)  
  
2.  [Importer des comptes d’utilisateur Active Directory à bord Windows Server Essentials (facultatif)](Move-Windows-Server-2008-Foundation-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_ImportADaccounts)  
  
3.  [Déplacer le rôle de serveur DHCP à partir du serveur Source vers le routeur](Move-Windows-Server-2008-Foundation-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MoveDHCP)  
  
4.  [Configurer le réseau](Move-Windows-Server-2008-Foundation-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_Network)  
  
5.  [Mapper les ordinateurs autorisés aux comptes d’utilisateurs](Move-Windows-Server-2008-Foundation-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MapPermittedComputers)  
  
##  <a name="BKMK_CopyData"></a> Copier des données vers le serveur de Destination (facultatif)  
 Avant de copier des données du serveur source vers le serveur de destination, effectuez les tâches suivantes :  
  
-   Passez en revue la liste des dossiers partagés sur le serveur source, y compris les autorisations de chaque dossier. Créez ou personnalisez les dossiers sur le serveur de destination afin qu'ils correspondent à la structure de dossiers que vous migrez depuis le serveur source.  
  
-   Passez en revue la taille de chaque dossier et vérifiez que le serveur de destination dispose de suffisamment d'espace de stockage.  
  
-   Mettez en lecture seule les dossiers partagés sur le serveur source pour tous les utilisateurs afin qu'aucune écriture ne puisse être réalisée sur le lecteur pendant que vous copiez les fichiers vers le serveur de destination.  
  
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Pour copier les données du serveur source vers le serveur de destination  
  
1.  Ouvrez une session sur le serveur de destination en tant qu'administrateur de domaine, puis ouvrez une fenêtre de commande.  
  
2.  À l'invite de commandes, tapez la commande suivante et appuyez sur Entrée :  
  
    `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`  
  
     Où :
     - \<Nom_serveur_source\> est le nom du serveur Source
     - \<Nomdossiersourcepartagé\> est le nom du dossier partagé sur le serveur Source
     - \<NomServeurDestination\> est le nom du serveur de Destination,
     - \<Nomdossierdestinationpartagé\> est le dossier partagé sur le serveur de Destination vers lequel les données seront copiées.  
  
3.  Répétez l'étape précédente pour chaque dossier partagé que vous migrez depuis le serveur source.  
  
##  <a name="BKMK_ImportADaccounts"></a> Importer des comptes d’utilisateur Active Directory à bord Windows Server Essentials (facultatif)  
 Par défaut, tous les comptes d’utilisateur créés sur le serveur Source migrent automatiquement vers le tableau de bord dans Windows Server Essentials. Cependant, la migration automatique d'un compte d'utilisateur Active Directory échoue si toutes les propriétés ne remplissent pas les conditions de la migration. Vous pouvez utiliser l'applet de commande Windows PowerShell suivante pour importer les utilisateurs Active Directory.  
  
#### <a name="to-import-an-active-directory-user-account-to-the-windows-server-essentials-dashboard"></a>Pour importer un compte d’utilisateur Active Directory dans le tableau de bord Windows Server Essentials  
  
1.  Ouvrez une session sur le serveur de destination en tant qu'administrateur de domaine.  
  
2.  Ouvrez Windows PowerShell en tant qu'administrateur.  
  
3.  Exécutez l’applet de commande suivante, où `[AD username]` est le nom du compte d’utilisateur Active Directory que vous souhaitez importer :  
  
     `Import-WssUser  SamAccountName [AD username]`  
  
##  <a name="BKMK_MoveDHCP"></a> Déplacer le rôle de serveur DHCP à partir du serveur Source vers le routeur  
 Si votre serveur source exécute le rôle DHCP, procédez comme suit pour déplacer le rôle DHCP vers le routeur.  
  
#### <a name="to-move-the-dhcp-role-from-the-source-server-to-the-router"></a>Pour déplacer le rôle DHCP du serveur source vers le routeur  
  
1.  Désactivez le service DHCP sur le serveur source en procédant comme suit :  
  
    1.  Sur le serveur source, cliquez sur **Démarrer**, sur **Outils d'administration**, puis sur **Services**.  
  
    2.  Dans la liste des services en cours d'exécution, cliquez avec le bouton droit sur **Serveur DHCP**, puis cliquez sur **Propriétés**.  
  
    3.  Pour **Type de démarrage**, sélectionnez **Désactivé**.  
  
    4.  Arrêtez le service.  
  
2.  Activez le rôle DHCP sur votre routeur.  
  
    1.  Suivez les instructions fournies dans la documentation de votre routeur pour activer le rôle DHCP sur le routeur.  
  
    2.  Afin de garantir que les adresses IP émises par le serveur source restent les mêmes, suivez les instructions fournies dans la documentation de votre routeur pour faire en sorte que la plage DHCP sur le routeur soit identique à la plage DHCP sur le serveur source.  
  
        > [!IMPORTANT]
        >  Si vous n'avez pas configuré d'adresse IP statique ou de réservations DHCP sur le routeur du serveur de destination et si la plage DHCP n'est pas la même que sur le serveur source, il est possible que le routeur émette une nouvelle adresse IP pour le serveur de destination. Dans ce cas, réinitialisez les règles de réacheminement de port du routeur afin de mettre en place un réacheminement vers la nouvelle adresse IP du serveur de destination.  
  
##  <a name="BKMK_Network"></a> Configurer le réseau  
 Après avoir déplacé le rôle DHCP vers le routeur, configurez les paramètres réseau sur le serveur de destination.  
  
#### <a name="to-configure-the-network"></a>Pour configurer le réseau  
  
1.  Sur le serveur de destination, ouvrez le tableau de bord.  
  
2.  Dans la page d'**accueil** du tableau de bord, cliquez sur **CONFIGURATION**, sur **Configurer l'Accès en tout lieu**, puis choisissez l'option **Cliquez pour configurer l'Accès en tout lieu**.  
  
3.  Suivez les instructions de l'Assistant pour configurer votre routeur et les noms de domaine.  
  
 Si votre routeur ne prend pas en charge l'infrastructure UPnP, ou si l'infrastructure UPnP est désactivée, une icône d'avertissement jaune peut apparaître en regard du nom du routeur. Vérifiez que les ports suivants sont ouverts et qu'ils sont dirigés vers l'adresse IP du serveur de destination :  
  
-   Port 80 : Trafic Web HTTP  
  
-   Port 443 : Trafic Web HTTPS  
  
##  <a name="BKMK_MapPermittedComputers"></a> Mapper les ordinateurs autorisés aux comptes d’utilisateurs  
 Dans Windows Server Essentials, un utilisateur doit être explicitement affecté à un ordinateur pour qu’il s’affiche dans l’accès Web à distance. Chaque compte d'utilisateur qui est migré à partir de Windows Server 2008 Foundation doit être mappé à un ou plusieurs ordinateurs.  
  
#### <a name="to-map-user-accounts-to-computers"></a>Pour mapper les comptes d'utilisateur aux ordinateurs  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **Utilisateurs**.  
  
3.  Dans la liste des comptes d'utilisateur, cliquez avec le bouton droit sur un compte d'utilisateur, puis cliquez sur **Afficher les propriétés du compte**.  
  
4.  Cliquez sur l'onglet **Accès en tout lieu**, puis sur **Autoriser l'accès web à distance et l'accès aux applications de services web**.  
  
5.  Sélectionnez **Dossiers partagés**, **Ordinateurs**, puis **Liens vers la page d'accueil** et cliquez sur **Appliquer**.  
  
6.  Cliquez sur l'onglet **Accès à l'ordinateur**, puis sur le nom de l'ordinateur auquel vous souhaitez autoriser l'accès.  
  
7.  Répétez les étapes 3, 4, 5 et 6 pour chaque compte d'utilisateur.  
  
> [!NOTE]
>  Vous n'avez pas besoin de modifier la configuration de l'ordinateur client. Il est configuré automatiquement.  
  
> [!NOTE]
>  Après avoir effectué la migration, si vous rencontrez un problème quand vous créez le premier compte d'utilisateur sur le serveur de destination, supprimez le compte d'utilisateur que vous avez ajouté, puis recréez-le.
