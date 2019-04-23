---
title: Récupération de forêt AD - sauvegarde d’un serveur complet
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adds
ms.openlocfilehash: fec8de8ea1dadb392f6a3bd1c881e8df2266f404
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846520"
---
# <a name="ad-forest-recovery---backing-up-a-full-server"></a>Récupération de forêt AD - sauvegarde d’un serveur complet  

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Une sauvegarde complète du serveur est recommandée pour vous préparer à une récupération de forêt, car elle peut être restaurée à un matériel différent ou une instance différente du système d’exploitation.  À l’aide de la sauvegarde de Windows Server, vous pouvez effectuer une sauvegarde complète de votre serveur. 

## <a name="windows-server-backup"></a>Sauvegarde Windows Server

Sauvegarde de Windows Server n’est pas installée par défaut. Dans Windows Server 2016 et Windows Server 2012 R2, vous devez l’installer en suivant les étapes ci-dessous.

>[!NOTE]
>N’oubliez pas que les étapes peuvent varier légèrement entre Windows Server 2016 et Windows Server 2012 R2.

Pour savoir comment l’installer dans Windows Server 2008 et Windows Server 2008 R2, consultez [sauvegarde de l’installation de Windows Server](https://technet.microsoft.com/library/cc771232.aspx).  

### <a name="to-install-windows-server-backup"></a>Pour installer la sauvegarde de Windows Server

1. Ouvrez **le Gestionnaire de serveur** et cliquez sur **ajouter des rôles et fonctionnalités**.
2. Sur le **Assistant Ajouter des rôles et fonctionnalités** cliquez sur **suivant**.
3. Sur le **Type d’Installation** écran, conservez la valeur par défaut **installation en fonction du rôle ou une fonctionnalité** et cliquez sur **suivant**.
4. Sur le **sélection du serveur** , cliquez sur **suivant**.
5. Sur le **rôles serveur** écran, cliquez sur **suivant**.
6. Sur le **fonctionnalités** s’affiche, sélectionnez **sauvegarde Windows Server** et cliquez sur **suivant**
   ![installer la sauvegarde](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup2.png)
7. Cliquez sur **Installer**.
8. Une fois l’installation est terminée, cliquez sur **fermer**.

### <a name="to-perform-a-backup-with-windows-server-backup"></a>Pour effectuer une sauvegarde avec sauvegarde Windows Server

1. Ouvrez **le Gestionnaire de serveur**, cliquez sur **outils**, puis cliquez sur **sauvegarde Windows Server**.
   - Dans Windows Server 2008 R2 et Windows Server 2008, cliquez sur **Démarrer**, pointez sur **outils d’administration**, puis cliquez sur **sauvegarde Windows Server**.

   ![Installer la sauvegarde](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png) 

2. Si vous êtes invité, dans le **contrôle de compte d’utilisateur** boîte de dialogue, fournissez les informations d’identification de l’opérateur de sauvegarde, puis cliquez sur **OK**.
3. Cliquez sur **sauvegarde locale**.
4. Dans le menu **Action**, cliquez sur **Sauvegarde unique**.
5. Dans l’Assistant sauvegarde unique, sur le **options de sauvegarde** , cliquez sur **différentes options**, puis cliquez sur **suivant**.

   ![Installer la sauvegarde](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)

6. Sur le **sélectionnez configuration de la sauvegarde** , cliquez sur **serveur complet (recommandé)**, puis cliquez sur **suivant**.
7. Sur le **spécifier le type de destination** , cliquez sur **lecteurs locaux** ou **dossier partagé distant**, puis cliquez sur **suivant**.
8. Sur le **sélectionner la Destination de sauvegarde** page, choisissez l’emplacement de sauvegarde.  Si vous avez sélectionné local lecteur choisir un lecteur local ou si vous avez sélectionné à distance partage Choisissez un partage réseau.
9. Dans l’écran de confirmation, cliquez sur **sauvegarde**.

   ![Installer la sauvegarde](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup4.png)

10. Une fois cette opération terminée cliquez sur **fermer**.
11. Fermez la sauvegarde de Windows Server.

>[!NOTE]
>Si vous obtenez une erreur indiquant qu’aucun emplacement de stockage de sauvegarde n’est disponible, vous devez soit exclure un des volumes qui a été sélectionné ou ajouter un nouveau volume ou un partage distant.
>Si vous obtenez un avertissement indiquant que le volume sélectionné est également inclus dans la liste des éléments à sauvegarder, déterminer s’il faut supprimer, cliquez sur **Ok**.

## <a name="using-wbadminexe-to-backup-a-windows-server"></a>À l’aide de Wbadmin.exe pour sauvegarder un serveur windows

WBADMIN.exe est un utilitaire de ligne de commande qui vous permet de sauvegarder et restaurer votre système d’exploitation, les volumes, les fichiers, les dossiers et les applications à partir d’une invite de commandes.

### <a name="to-perform-a-full-server-backup-using-wbadminexe"></a>Pour effectuer une sauvegarde complète du serveur à l’aide de Wbadmin.exe
  
- Ouvrez une invite de commandes avec élévation de privilèges, tapez la commande suivante et appuyez sur ENTRÉE :  

   ```
   wbadmin start backup -backuptarget:<Drive_letter_to store_backup>: -include:<Drive_letter_to_include>:
   ```

   ![Installer la sauvegarde](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup5.png)

## <a name="next-steps"></a>Étapes suivantes

- [Guide de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de forêt AD - procédures](AD-Forest-Recovery-Procedures.md)
