---
title: Déplacer les paramètres et données de Windows SBS 2011 Essentials vers le serveur de destination pour la migration vers Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 47548994-9fa0-42e0-afa4-c2ccbd063acb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: efc479f4c84baf72b1656afb85eef22d9ca710d8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852462"
---
# <a name="move-windows-sbs-2011-essentials-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Déplacer les paramètres et données de Windows SBS 2011 Essentials vers le serveur de destination pour la migration vers Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Déplacez les paramètres et les données vers le serveur de destination comme suit :  
  

1.  [Copier les données sur le serveur de destination](Move-Windows-SBS-2011-Essentials-to-the-Destination-Server-for-migration.md#BKMK_CopyData)  
  
2.  [Importer des comptes d’utilisateur Active Directory dans le tableau de bord Windows Server Essentials (facultatif)](Move-Windows-SBS-2011-Essentials-to-the-Destination-Server-for-migration.md#BKMK_ImportADaccounts)  
  
3.  [Configurer le réseau](Move-Windows-SBS-2011-Essentials-to-the-Destination-Server-for-migration.md#BKMK_Network)  
  
4.  [Mapper les ordinateurs autorisés aux comptes d’utilisateur](Move-Windows-SBS-2011-Essentials-to-the-Destination-Server-for-migration.md#BKMK_MapPermittedComputers)  
 
##  <a name="copy-data-to-the-destination-server"></a><a name="BKMK_CopyData"></a>Copier les données sur le serveur de destination  
 Avant de copier des données du serveur source vers le serveur de destination, effectuez les tâches suivantes :  
  
-   Passez en revue la liste des dossiers partagés sur le serveur source, y compris les autorisations de chaque dossier. Créez ou personnalisez les dossiers sur le serveur de destination afin qu'ils correspondent à la structure de dossiers que vous migrez depuis le serveur source.  
  
-   Passez en revue la taille de chaque dossier et vérifiez que le serveur de destination dispose de suffisamment d'espace de stockage.  
  
-   Mettez en lecture seule les dossiers partagés sur le serveur source pour tous les utilisateurs afin qu'aucune écriture ne puisse être réalisée sur le lecteur pendant que vous copiez les fichiers vers le serveur de destination.  
  
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Pour copier les données du serveur source vers le serveur de destination  
  
1.  Ouvrez une session sur le serveur de destination en tant qu'administrateur de domaine, puis ouvrez une fenêtre de commande.  
  
2.  À l'invite de commandes, tapez la commande suivante, puis appuyez sur ENTRÉE :  
  
    `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`  
  
     Où :
     - \<SourceServerName\> est le nom du serveur source
     - \<Nomdossiersourcepartagé\> est le nom du dossier partagé sur le serveur source
     - \<NomServeurDestination\> est le nom du serveur de destination,
     - \<Nomdossierdestinationpartagé\> est le dossier partagé sur le serveur de destination vers lequel les données seront copiées.  
        
3.  Répétez l'étape précédente pour chaque dossier partagé que vous migrez depuis le serveur source.  
  
##  <a name="import-active-directory-user-accounts-to-the-windows-server-essentials-dashboard-optional"></a><a name="BKMK_ImportADaccounts"></a>Importer des comptes d’utilisateur Active Directory dans le tableau de bord Windows Server Essentials (facultatif)  
 Par défaut, tous les comptes d’utilisateur créés sur le serveur source sont automatiquement migrés vers le tableau de bord dans Windows Server Essentials. Cependant, la migration automatique d'un compte d'utilisateur Active Directory échoue si toutes les propriétés ne remplissent pas les conditions de la migration. Vous pouvez utiliser l'applet de commande Windows PowerShell suivante pour importer les utilisateurs Active Directory.  
  
#### <a name="to-import-an-active-directory-user-account-to-the-windows-server-essentials-dashboard"></a>Pour importer un compte d’utilisateur Active Directory dans le tableau de bord Windows Server Essentials  
  
1.  Ouvrez une session sur le serveur de destination en tant qu'administrateur de domaine.  
  
2.  Ouvrez Windows PowerShell en tant qu'administrateur.  
  
3.  Exécutez l’applet de commande suivante, où `[AD username]` est le nom du compte d’utilisateur Active Directory que vous souhaitez importer :  
  
     `Import-WssUser  SamAccountName [AD username]`  
  
##  <a name="configure-the-network"></a><a name="BKMK_Network"></a>Configurer le réseau  
  
#### <a name="to-configure-the-network"></a>Pour configurer le réseau  
  
1. Sur le serveur de destination, ouvrez le tableau de bord.  
  
2. Dans la page d'**accueil** du tableau de bord, cliquez sur **CONFIGURATION**, sur **Configurer l'Accès en tout lieu**, puis choisissez l'option **Cliquez pour configurer l'Accès en tout lieu**.  
  
3. Suivez les instructions de l'Assistant pour configurer votre routeur et les noms de domaine.  
  
   Si votre routeur ne prend pas en charge l'infrastructure UPnP, ou si l'infrastructure UPnP est désactivée, une icône d'avertissement jaune peut apparaître en regard du nom du routeur. Vérifiez que les ports suivants sont ouverts et qu'ils sont dirigés vers l'adresse IP du serveur de destination :  
  
-   Port 80 : trafic web HTTP  
  
-   Port 443 : trafic web HTTPS  
  
##  <a name="map-permitted-computers-to-user-accounts"></a><a name="BKMK_MapPermittedComputers"></a>Mapper les ordinateurs autorisés aux comptes d’utilisateur  
 Chaque compte d'utilisateur qui est migré à partir de Windows Small Business Server 2011 Essentials doit être mappé à un ou plusieurs ordinateurs.  
  
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
