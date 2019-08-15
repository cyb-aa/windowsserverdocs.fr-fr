---
title: Résoudre les problèmes d’historique des fichiers dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
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
ms.openlocfilehash: 99467cb5be7a71ce8b080223e8a89db4d9b6eabd
ms.sourcegitcommit: d83933c6a2e180b747c2db910392117569348901
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68807890"
---
# <a name="troubleshoot-file-history-in-windows-server-essentials"></a>Résoudre les problèmes d’historique des fichiers dans Windows Server Essentials

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials 
  
## <a name="troubleshoot-issues-with-user-file-history-backups"></a>Résoudre les problèmes avec les sauvegardes de l’historique des fichiers utilisateur  
 Les problèmes suivants peuvent se produire lors de la gestion des sauvegardes de l’historique des fichiers pour un utilisateur ou un ordinateur qui a été ajouté à un serveur exécutant Windows Server Essentials.  
  
### <a name="file-history-data-is-not-automatically-deleted"></a>Les données de l’historique des fichiers ne sont pas automatiquement supprimées  
 Il se peut que les données de l’historique des fichiers ne soient pas supprimées automatiquement dans les cas suivants :  
  
- Lorsque vous supprimez un compte d’utilisateur, vous choisissez de ne pas supprimer les données de l’historique des fichiers du compte d’utilisateur et d’opter pour la suppression manuelle des données.  
  
- Lorsque vous essayez de supprimer les données de l’historique des fichiers, ces dernières sont en cours d’utilisation par un autre processus.  
  
  Pour résoudre ce problème, vous devez supprimer manuellement l’historique des fichiers à l’aide de la procédure suivante :  
  
####  <a name="BKMK_manuallyDelete"></a>Pour supprimer manuellement les sauvegardes de l’historique des fichiers pour un utilisateur ou un ordinateur  
  
1.  Connectez-vous au serveur en tant qu’administrateur.  
  
2.  Exécutez l’Explorateur de fichiers en tant qu’administrateur.  
  
3.  Accédez au dossier Sauvegardes de l’Historique des fichiers. L’emplacement par défaut est C:\ServerFolders\Sauvegardes de l’Historique des fichiers.  
  
4.  Supprimez le dossier partagé qui stocke la sauvegarde de l’historique des fichiers :  
  
    -   Pour supprimer l’historique des fichiers d’un utilisateur, supprimez le dossier enfant de sauvegarde de l’historique des fichiers qui porte le nom de l’utilisateur.  
  
    -   Pour supprimer l’historique des fichiers d’un ordinateur, supprimez le dossier enfant de sauvegarde de l’historique des fichiers portant le nom de l’ordinateur. Par exemple, si un utilisateur a mis hors\> service < MyComputer01 après avoir commencé à travailler sur son nouvel\>ordinateur portable, < MyComputer02, vous\\supprimez les\> sauvegardes de l’historique c:\serverfolders\sauvegardes < mon\\ dossier < MyComputer01\> après avoir vérifié auprès de l’utilisateur qu’il a transféré tous les fichiers et dossiers vers son nouvel ordinateur portable et n’a pas besoin de l’historique des fichiers à l’avenir.  
  
### <a name="cannot-apply-file-history-setting-to-a-new-user"></a>Impossible d’appliquer le paramètre de l’historique des fichiers à un nouvel utilisateur  
 Si vous ajoutez un nouvel utilisateur dont le nom est identique au nom d’un utilisateur qui a été supprimé de Windows Server Essentials, la configuration de l’historique des fichiers du nouvel utilisateur peut échouer en raison d’un conflit de noms lorsque Windows Server Essentials tente de créer un dossier pour stocker l’historique des fichiers du nouvel utilisateur. Pour résoudre ce problème, vous pouvez renommer le dossier de l’historique des fichiers de l’utilisateur supprimé.  
  
##### <a name="to-locate-user-file-history-on-the-server"></a>Pour localiser l’historique des fichiers utilisateur sur le serveur  
  
1.  Connectez-vous au serveur en tant qu’administrateur.  
  
2.  Dans le tableau de bord Windows Server Essentials, cliquez sur **Stockage**.  
  
3.  Dans l’onglet **Dossiers serveur** , notez l’emplacement du dossier Sauvegardes de l’Historique des fichiers. L’emplacement par défaut est%SystemDrive%\ServerFolders\File l’historique\\des sauvegardes.  
  
##### <a name="to-resolve-file-history-issues-for-a-new-user-with-a-name-conflict"></a>Pour résoudre les problèmes de l’historique des fichiers d’un nouvel utilisateur avec un conflit de noms  
  
1.  Connectez-vous au serveur en tant qu’administrateur.  
  
2.  Exécutez l’Explorateur de fichiers en tant qu’administrateur.  
  
3.  Accédez au dossier Sauvegardes de l’Historique des fichiers. L’emplacement par défaut est C:\ServerFolders\Sauvegardes de l’Historique des fichiers.  
  
     Le dossier Sauvegardes de l’Historique des fichiers comporte un sous-dossier pour chaque compte d’utilisateur qui a été ajouté à Windows Server Essentials. Par exemple, l’historique des fichiers de l’utilisateur John Smith est stocké dans le sous-fichier Sauvegardes de l’Historique des fichiers\JohnSmith.  
  
4.  Renommez le sous-dossier de l’utilisateur que vous avez supprimé, par exemple,  **< *nom d’utilisateur*> _deleted**. Si vous n’avez plus besoin de l’historique des fichiers de l’utilisateur, vous pouvez supprimer le dossier.  
  

5.  Vous pouvez maintenant ajouter le nouvel utilisateur. Pour obtenir des instructions, consultez Ajouter un compte d’utilisateur. dans [gérer les comptes d’utilisateur](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
### <a name="a-user-account-was-removed-but-the-users-file-history-remains"></a>Un compte d’utilisateur a été supprimé, mais l’historique des fichiers de l’utilisateur a été conservé  
 Dans certains cas, l’administrateur réseau peut choisir de supprimer un utilisateur ou un ordinateur du serveur, mais de conserver la sauvegarde de l’historique des fichiers pour une utilisation ultérieure. Lorsque vous n’avez plus besoin de l’historique des fichiers, supprimez le dossier Sauvegardes de l’Historique des fichiers de l’utilisateur ou de l’ordinateur des dossiers partagés sur le serveur. Pour ce faire, consultez [Pour supprimer manuellement les sauvegardes de l’historique des fichiers pour un utilisateur ou un ordinateur](Troubleshoot-File-History-in-Windows-Server-Essentials.md#BKMK_manuallyDelete).  

5. Vous pouvez maintenant ajouter le nouvel utilisateur. Pour obtenir des instructions, consultez Ajouter un compte d’utilisateur. dans [gérer les comptes d’utilisateur](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
### <a name="a-user-account-was-removed-but-the-users-file-history-remains"></a>Un compte d’utilisateur a été supprimé, mais l’historique des fichiers de l’utilisateur a été conservé  
 Dans certains cas, l’administrateur réseau peut choisir de supprimer un utilisateur ou un ordinateur du serveur, mais de conserver la sauvegarde de l’historique des fichiers pour une utilisation ultérieure. Lorsque vous n’avez plus besoin de l’historique des fichiers, supprimez le dossier Sauvegardes de l’Historique des fichiers de l’utilisateur ou de l’ordinateur des dossiers partagés sur le serveur. Pour ce faire, consultez [Pour supprimer manuellement les sauvegardes de l’historique des fichiers pour un utilisateur ou un ordinateur](../support/Troubleshoot-File-History-in-Windows-Server-Essentials.md#BKMK_manuallyDelete).  

  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer la sauvegarde des clients](../manage/Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md)  
  

-   [Prendre en charge Windows Server Essentials](Support-Windows-Server-Essentials.md)

-   [Prendre en charge Windows Server Essentials](../support/Support-Windows-Server-Essentials.md)

