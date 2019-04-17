---
title: "Résoudre les problèmes de l’historique des fichiers dans WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed062945-27e9-4572-b1bb-6c8cf1b9c2f4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 34442565b54b089064c1fa19317a24f591e44fda
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="troubleshoot-file-history-in-windows-server-essentials"></a>Résoudre les problèmes de l’historique des fichiers dans WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials 
  
## <a name="troubleshoot-issues-with-user-file-history-backups"></a>Résoudre les problèmes avec les sauvegardes de l’historique des fichiers utilisateur  
 Les problèmes suivants peuvent se produire lors de la gestion des sauvegardes de l’historique des fichiers pour un utilisateur ou un ordinateur qui a été ajouté à un serveur exécutant WindowsServerEssentials.  
  
### <a name="file-history-data-is-not-automatically-deleted"></a>Données de l’historique des fichiers ne sont pas automatiquement supprimées.  
 Les données de l’historique des fichiers ne soient pas supprimées automatiquement si:  
  
-   Lorsque vous supprimez un compte d’utilisateur, vous choisissez de ne pas supprimer le compte d’utilisateur s des données de l’historique des fichiers et choisissez de supprimer les données manuellement.  
  
-   Lorsque vous essayez de supprimer les données de l’historique des fichiers, les données de l’historique des fichiers sont en cours d’utilisation par un autre processus.  
  
 Pour résoudre ce problème, vous devez supprimer manuellement l’historique des fichiers à l’aide de la procédure suivante:  
  
####  <a name="BKMK_manuallyDelete"></a>Pour supprimer manuellement les sauvegardes de l’historique des fichiers pour un utilisateur ou un ordinateur  
  
1.  Ouvrez une session en tant qu’administrateur le serveur.  
  
2.  Exécuter l’Explorateur de fichiers en tant qu’administrateur.  
  
3.  Accédez au dossier sauvegardes de l’historique des fichiers. L’emplacement par défaut est C:\ServerFolders\File l’historique des sauvegardes.  
  
4.  Supprimer le dossier partagé qui stocke la sauvegarde de l’historique des fichiers:  
  
    -   Pour supprimer l’historique des fichiers pour un utilisateur, supprimez le dossier enfant de sauvegarde de l’historique des fichiers qui possède le nom d’utilisateur s.  
  
    -   Pour supprimer l’historique des fichiers pour un ordinateur, supprimez le dossier enfant de sauvegarde de l’historique des fichiers dont le nom de l’ordinateur. Par exemple, si une utilisatrice a retiré < myComputer01\ > une fois qu’elle a commencé à travailler sur son nouvel ordinateur portable, < myComputer02\ >, vous supprimez C:\ServerFolders\File historique Backups\\ < myAccount\ > \\ < myComputer01\ > après avoir vérifié avec l’utilisateur qu’elle a transféré tous les fichiers et dossiers à son nouvel ordinateur portable et n’a pas besoin de l’historique des fichiers à l’avenir.  
  
### <a name="cannot-apply-file-history-setting-to-a-new-user"></a>Impossible d’appliquer le paramètre historique des fichiers à un nouvel utilisateur  
 Si vous ajoutez un nouvel utilisateur dont le nom est identique au nom d’utilisateur de l’utilisateur qui a été supprimé de WindowsServerEssentials, la configuration de l’historique des fichiers pour le nouvel utilisateur peut échouer en raison d’un conflit de noms lorsque WindowsServerEssentials tente de créer un dossier pour stocker l’historique des fichiers du nouvel utilisateur. Pour résoudre ce problème, vous pouvez renommer le dossier de l’historique des fichiers de l’utilisateur supprimé.  
  
##### <a name="to-locate-user-file-history-on-the-server"></a>Pour localiser l’historique des fichiers utilisateur sur le serveur  
  
1.  Ouvrez une session en tant qu’administrateur le serveur.  
  
2.  Dans le tableau de bord WindowsServerEssentials, cliquez sur **stockage**.  
  
3.  Sur le **dossiers du serveur** onglet, notez l’emplacement du dossier sauvegardes de l’historique des fichiers. L’emplacement par défaut est %SystemDrive%\ServerFolders\File Backups\\ historique.  
  
##### <a name="to-resolve-file-history-issues-for-a-new-user-with-a-name-conflict"></a>Pour résoudre les problèmes de l’historique des fichiers pour un nouvel utilisateur avec un conflit de noms  
  
1.  Ouvrez une session en tant qu’administrateur le serveur.  
  
2.  Exécuter l’Explorateur de fichiers en tant qu’administrateur.  
  
3.  Accédez au dossier sauvegardes de l’historique des fichiers. L’emplacement par défaut est C:\ServerFolders\File l’historique des sauvegardes.  
  
     Le dossier sauvegardes de l’historique des fichiers comporte un sous-dossier pour chaque compte d’utilisateur qui a été ajouté à WindowsServerEssentials. Par exemple, l’historique des fichiers de l’utilisateur John Smith est stocké dans le historique des fichiers\johnsmith.  
  
4.  Renommez le sous-dossier de l’utilisateur que vous avez supprimé, par exemple, **<*nom d’utilisateur*> _supprimé **. Si vous n’avez plus besoin l’historique des fichiers de l’utilisateur, vous pouvez supprimer le dossier.  
  

5.  Vous pouvez maintenant ajouter le nouvel utilisateur. Pour obtenir des instructions, voir Ajouter un compte d’utilisateur? dans [gérer les comptes d’utilisateur](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
### <a name="a-user-account-was-removed-but-the-users-file-history-remains"></a>Un compte d’utilisateur a été supprimé, mais le restent de l’historique des fichiers de l’utilisateur  
 Dans certains cas, l’administrateur réseau peut choisir de supprimer un utilisateur ou un ordinateur à partir du serveur, mais de conserver l’historique des fichiers sauvegarde pour une utilisation ultérieure. Lorsque vous n’avez plus besoin l’historique des fichiers, supprimez le dossier sauvegardes de l’historique des fichiers de l’utilisateur ou l’ordinateur à partir de dossiers partagés sur le serveur. Pour ce faire, voir [supprimer manuellement les sauvegardes de l’historique des fichiers pour un utilisateur ou un ordinateur](Troubleshoot-File-History-in-Windows-Server-Essentials.md#BKMK_manuallyDelete).  

5.  Vous pouvez maintenant ajouter le nouvel utilisateur. Pour obtenir des instructions, voir Ajouter un compte d’utilisateur? dans [gérer les comptes d’utilisateur](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
### <a name="a-user-account-was-removed-but-the-users-file-history-remains"></a>Un compte d’utilisateur a été supprimé, mais le restent de l’historique des fichiers de l’utilisateur  
 Dans certains cas, l’administrateur réseau peut choisir de supprimer un utilisateur ou un ordinateur à partir du serveur, mais de conserver l’historique des fichiers sauvegarde pour une utilisation ultérieure. Lorsque vous n’avez plus besoin l’historique des fichiers, supprimez le dossier sauvegardes de l’historique des fichiers de l’utilisateur ou l’ordinateur à partir de dossiers partagés sur le serveur. Pour ce faire, voir [supprimer manuellement les sauvegardes de l’historique des fichiers pour un utilisateur ou un ordinateur](../support/Troubleshoot-File-History-in-Windows-Server-Essentials.md#BKMK_manuallyDelete).  

  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer la sauvegarde du Client](../manage/Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md)  
  

-   [Prise en charge Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [Prise en charge Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

