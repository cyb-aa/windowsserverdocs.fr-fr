---
title: "Étape4: Déplacer les paramètres et données pour la migration du serveur de Destination pour WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e143df43-e227-4629-a4ab-9f70d9bf6e84
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: db1169a5415039498718a7988c711d068945b4b7
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="step-4-move-settings-and-data-to-the-destination-server-for-windows-server-essentials-migration"></a>Étape4: Déplacer les paramètres et données pour la migration du serveur de Destination pour WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Cette section fournit des informations sur la migration des données et paramètres à partir du serveur Source. Déplacer les paramètres et données vers le serveur de Destination comme suit:  
  
-   [Copier les données sur le serveur de Destination](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_CopyData)  
  
-   [Configurer le réseau](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_Network)  
  
-   [Mapper les ordinateurs autorisés aux comptes d’utilisateurs](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md#BKMK_MapPermittedComputers)  
  
##  <a name="BKMK_CopyData"></a>Copier les données sur le serveur de Destination  
 Avant de copier des données à partir du serveur Source vers le serveur de Destination, effectuez les tâches suivantes:  
  
-   Passez en revue la liste des dossiers partagés sur le serveur Source, y compris les autorisations pour chaque dossier. Créez ou personnalisez les dossiers sur le serveur de Destination pour correspondre à la structure de dossiers que vous migrez à partir du serveur Source.  
  
-   Passez en revue la taille de chaque dossier et assurez-vous que le serveur de Destination dispose de suffisamment d’espace.  
  
-   Mettez les dossiers partagés sur le serveur Source Read-only pour tous les utilisateurs afin qu’aucune écriture ne puisse être réalisée sur le disque dur tout en vous copiez les fichiers vers le serveur de Destination.  
  
-   Le **sauvegarde de l’ordinateur Client** dossier ne peut pas être migré vers le serveur de Destination. Avant la migration du serveur, assurez-vous que tous les ordinateurs clients sont sains. Après la migration du serveur, il est recommandé de configurer et de démarrer les sauvegardes de l’ordinateur client pour vous assurer que les données sont sauvegardées pour tous vos ordinateurs clients importants.  
  
-   Le **sauvegardes de l’historique des fichiers** dossier ne peut pas être directement migré vers le serveur de Destination en raison du dossier sauvegarde et la structure de métadonnées WindowsServerEssentials. Toutefois, il est possible de migrer les **les sauvegardes de l’historique des fichiers** dossier pour un utilisateur spécifique sur un ordinateur spécifique. Pour ce faire, vous devez rechercher la **données** dossier dans le **sauvegardes de l’historique des fichiers** dossier pour cet utilisateur et l’ordinateur, puis copiez ce **données** dossier pour le **sauvegardes de l’historique des fichiers** dossier sur le serveur de Destination.  
  
#### <a name="to-copy-data-from-the-source-server-to-the-destination-server"></a>Pour copier les données à partir du serveur Source vers le serveur de Destination  
  
1.  Connectez-vous au serveur de Destination en tant qu’administrateur de domaine, puis ouvrez une fenêtre d’invite de commandes ou d’une invite de commandes Windows PowerShell.  
  
2.  Si vous utilisez la fenêtre d’invite de commandes, tapez la commande suivante et appuyez sur ENTRÉE:  
  
    `robocopy \\<SourceServerName>\<SharedSourceFolderName> "<PathOfTheDestination>\<SharedDestinationFolderName>" /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`
  
     Où:  
  
    -   \ < SourceServerName\ > est le nom du serveur Source  
  
    -   \ < SharedSourceFolderName\ > est le nom du dossier partagé sur le serveur Source  
  
    -   \ < pathOfTheDestination\ > est le chemin d’accès absolu où vous souhaitez déplacer le dossier  
  
    -   \ < sharedDestinationFolderName\ > est le dossier sur le serveur de Destination sur lequel les données seront copiées  
  
     Par exemple,`robocopy \\sourceserver\MyData "d:\ServerFolders\MyData" /E /B /COPY:DATSOU /LOG:C:\Copyresults.txt`.  
  
3.  Si vous utilisez Windows PowerShell, tapez la commande suivante et appuyez sur ENTRÉE.  
  
     `Add-Wssfolder  Path \ -Name  -KeepPermission`  
  
4.  Répétez ce processus pour chaque dossier partagé que vous effectuez une migration à partir du serveur Source.  
  
##  <a name="BKMK_Network"></a>Configurer le réseau  
  
#### <a name="to-configure-the-network"></a>Pour configurer le réseau  
  
1.  Sur le serveur de Destination, ouvrez le tableau de bord.  
  
2.  Sur le tableau de bord **accueil**, cliquez sur **le programme d’installation**, cliquez sur **configurer l’accès en tout lieu**, puis choisissez le **cliquez pour configurer l’accès en tout lieu** option.  
  
3.  L’ensemble de l’Assistant de l’accès en tout lieu s’affiche. Suivez les instructions de l’Assistant pour configurer votre routeur et les noms de domaine.  
  
 Si votre routeur ne prend pas en charge l’infrastructure UPnP, ou si l’infrastructure UPnP est désactivée, une icône d’avertissement jaune peut apparaître en regard du nom du routeur. Vérifiez que les ports suivants sont ouverts et qu’ils sont dirigés vers l’adresse IP du serveur de Destination:  
  
-   Port 80: Le trafic HTTP Web  
  
-   Port 443: Le trafic Web HTTPS  
  
> [!NOTE]
>  Si vous souhaitez configurer un nom de domaine public sur le serveur de Destination, vous devez libérer le nom de domaine de votre serveur Source pour éviter la concurrence de mise à jour DNS dynamique.  
  
##  <a name="BKMK_MapPermittedComputers"></a>Mapper les ordinateurs autorisés aux comptes d’utilisateurs  
 Chaque compte d’utilisateur qui est migré à partir de versions précédentes de WindowsSmallBusinessServer ou WindowsServerEssentials doit être mappé à un ou plusieurs ordinateurs.  
  
#### <a name="to-map-user-accounts-to-computers"></a>Pour mapper les comptes d’utilisateur aux ordinateurs  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Dans la barre de navigation, cliquez sur **utilisateurs**.  
  
3.  Dans la liste des comptes d’utilisateur, cliquez sur un compte d’utilisateur, puis cliquez sur **permet d’afficher les propriétés du compte**.  
  
4.  Cliquez sur le **accès en tout lieu** onglet, puis cliquez sur **autoriser l’accès Web à distance et l’accès aux applications de services web**.  
  
5.  Cliquez sur **dossiers partagés**, cliquez sur**ordinateurs**, cliquez sur **liens de la page d’accueil**, puis cliquez sur **appliquer**.  
  
6.  Cliquez sur le **accès à l’ordinateur** onglet, puis cliquez sur le nom de l’ordinateur auquel vous souhaitez autoriser l’accès.  
  
7.  Répétez les étapes 3, 4, 5 et 6 pour chaque compte d’utilisateur.  
  
> [!NOTE]
>  Vous n’avez pas besoin de modifier la configuration de l’ordinateur client. Il est configuré automatiquement.  
  
> [!NOTE]
>  Après avoir effectué la migration, si vous rencontrez un problème lorsque vous créez le premier compte d’utilisateur sur le serveur de Destination, supprimer le compte d’utilisateur que vous avez ajouté, puis recréez-le.  
  
## <a name="next-steps"></a>Étapes suivantes  
 Vous avez déplacé les paramètres et données vers le serveur de Destination. Passez maintenant à [étape5: activer la redirection de dossiers sur la migration du serveur de Destination pour WindowsServerEssentials](Step-5--Enable-folder-redirection-on-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  
  

Pour afficher toutes les étapes, voir [migrer vers WindowsServerEssentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

