---
title: "Déplacer les paramètres et les données pour la migration du serveur de Destination pour Windows Server Essentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2b882e87-347a-4010-b7fd-9599d61198dd
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 97a9f7ec7a9710b66236d8eca05dea2432df04ba
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="move-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Déplacer les paramètres et les données pour la migration du serveur de Destination pour Windows Server Essentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Déplacer les paramètres et données vers le serveur de Destination comme suit:  
  

1.  [Copier les données sur le serveur de Destination](Move-Windows-SBS-2008-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_CopyData)  
  
2.  [Configurer le réseau](Move-Windows-SBS-2008-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_Network)  
  
3.  [Mapper les ordinateurs autorisés aux comptes d’utilisateurs](Move-Windows-SBS-2008-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MapPermittedComputers)  
 
##  <a name="BKMK_CopyData"></a>Copier les données sur le serveur de Destination  
 Avant de copier des données à partir du serveur Source vers le serveur de Destination, effectuez les tâches suivantes:  
  
-   Passez en revue la liste des dossiers partagés sur le serveur Source, y compris les autorisations pour chaque dossier. Créez ou personnalisez les dossiers sur le serveur de Destination pour correspondre à la structure de dossiers que vous migrez à partir du serveur Source.  
  
-   Passez en revue la taille de chaque dossier et assurez-vous que le serveur de Destination dispose de suffisamment d’espace.  
  
-   Vérifiez les dossiers partagés sur le serveur Source en lecture seule pour tous les utilisateurs donc aucune écriture ne puisse être réalisée sur le lecteur pendant que vous copiez les fichiers vers le serveur de Destination.  
  
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Pour copier les données à partir du serveur Source vers le serveur de Destination  
  
1.  Ouvrez une session sur le serveur de Destination en tant qu’administrateur de domaine, puis ouvrez une fenêtre de commande.  
  
2.  À l’invite de commandes, tapez la commande suivante et appuyez sur ENTRÉE:  
  
    `robocopy \\<SourceServerName> \<SharedSourceFolderName> \\<DestinationServerName> \<SharedDestinationFolderName> /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`  
  
     Où:
     - \ < SourceServerName\ > est le nom du serveur Source
     - \ < SharedSourceFolderName\ > est le nom du dossier partagé sur le serveur Source
     - \ < DestinationServerName\ > est le nom du serveur de Destination,
     - \ < SharedDestinationFolderName\ > est le dossier partagé sur le serveur de Destination vers lequel les données seront copiées.  
  
3.  Répétez l’étape précédente pour chaque dossier partagé que vous effectuez une migration à partir du serveur Source.  
  
##  <a name="BKMK_Network"></a>Configurer le réseau  
 Après avoir déplacé le rôle DHCP vers le routeur, configurez les paramètres réseau sur le serveur de Destination.  
  
#### <a name="to-configure-the-network"></a>Pour configurer le réseau  
  
1.  Sur le serveur de Destination, ouvrez le tableau de bord.  
  
2.  Sur le tableau de bord **accueil** , cliquez sur **le programme d’installation**, cliquez sur **configurer l’accès en tout lieu**, puis choisissez le **cliquez pour configurer l’accès en tout lieu** option.  
  
3.  Suivez les instructions de l’Assistant pour configurer votre routeur et les noms de domaine.  
  
 Si votre routeur ne prend pas en charge l’infrastructure UPnP, ou si l’infrastructure UPnP est désactivée, une icône d’avertissement jaune peut apparaître en regard du nom du routeur. Vérifiez que les ports suivants sont ouverts et qu’ils sont dirigés vers l’adresse IP du serveur de Destination:  
  
-   Port 80: Le trafic HTTP Web  
  
-   Port 443: Le trafic Web HTTPS  
  
##  <a name="BKMK_MapPermittedComputers"></a>Mapper les ordinateurs autorisés aux comptes d’utilisateurs  
 Chaque compte d’utilisateur qui est migré à partir du serveur Source doit être mappé à un ou plusieurs ordinateurs.  
  
#### <a name="to-map-user-accounts-to-computers"></a>Pour mapper les comptes d’utilisateur aux ordinateurs  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **utilisateurs**.  
  
3.  Dans la liste des comptes d’utilisateur, cliquez sur un compte d’utilisateur, puis cliquez sur **permet d’afficher les propriétés du compte**.  
  
4.  Cliquez sur le **accès en tout lieu** onglet, puis cliquez sur **autoriser l’accès Web à distance et l’accès aux applications de services web. **.  
  
5.  Sélectionnez **dossiers partagés**, sélectionnez **ordinateurs**, sélectionnez **liens de la page d’accueil**, puis cliquez sur **appliquer**.  
  
6.  Cliquez sur le **accès à l’ordinateur** onglet, puis cliquez sur le nom de l’ordinateur auquel vous souhaitez autoriser l’accès.  
  
7.  Répétez les étapes 3, 4, 5 et 6 pour chaque compte d’utilisateur.  
  
> [!NOTE]
>  Vous n’avez pas besoin de modifier la configuration de l’ordinateur client. Il est configuré automatiquement.  
  
> [!NOTE]
>  Après avoir effectué la migration, si vous rencontrez un problème lorsque vous créez le premier compte d’utilisateur sur le serveur de Destination, supprimer le compte d’utilisateur que vous avez ajouté, puis recréez-le.
