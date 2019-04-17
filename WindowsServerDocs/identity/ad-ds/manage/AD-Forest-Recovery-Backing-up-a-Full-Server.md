---
title: "Récupération de la forêt ActiveDirectory - sauvegarde complète du serveur."
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adfs
ms.openlocfilehash: b1af97c2eb23d65c2d106906bc0f5bb1f10b23ec
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---backing-up-a-full-server"></a>Récupération de la forêt ActiveDirectory - sauvegarde complète du serveur.  

>S’applique à: Windows Server2016, Windows Server2012 et 2012R2, Windows Server2008 et 2008R2

Une sauvegarde complète du serveur est recommandée de préparer pour une récupération de forêt, car elle peut être restaurée sur un matériel différent ou sur une instance différente du système d’exploitation.  À l’aide de la sauvegarde de Windows Server, vous pouvez effectuer une sauvegarde complète de votre serveur. 

## <a name="windows-server-backup"></a>Sauvegarde de Windows Server
Sauvegarde de Windows Server n’est pas installée par défaut. Dans Windows Server2016 et Windows Server2012R2, vous devez l’installer en suivant les étapes ci-dessous.

>[!NOTE]
>N’oubliez pas que les étapes varient légèrement entre Windows Server2016 et Windows Server2012R2.

Pour obtenir des instructions pour l’installer dans Windows Server2008 et Windows Server2008R2, voir [sauvegarde de l’installation de Windows Server](https://technet.microsoft.com/library/cc771232.aspx).  

### <a name="to-install-windows-server-backup"></a>Pour installer la sauvegarde de Windows Server
1. Ouvrez **le Gestionnaire de serveur** et cliquez sur **ajouter des rôles et fonctionnalités**.
2. Sur le **Assistant Ajouter des rôles et fonctionnalités** cliquez sur **suivant**.
3. Sur le **Type d’Installation** écran, laissez la valeur par défaut **installation basée sur un rôle ou une fonctionnalité** et cliquez sur **suivant**.
4. Sur le **sélection du serveur**, cliquez sur **suivant**.
5. Sur le **des rôles de serveurs** écran, cliquez sur **suivant**.
6. Sur le **fonctionnalités** écran, puis sélectionnez **sauvegarde de Windows Server** et cliquez sur **suivant**<ph x="4">
! [</ph> Installer la sauvegarde](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup2.png)
7. Cliquez sur **installer**.
8. Une fois l’installation terminée, cliquez sur **fermer**.


### <a name="to-perform-a-backup-with-windows-server-backup"></a>Pour effectuer une sauvegarde avec la sauvegarde de Windows Server

1. Ouvrez **le Gestionnaire de serveur**, cliquez sur **outils**, puis cliquez sur **sauvegarde de Windows Server**.
    - Dans Windows Server2008R2 et Windows Server2008, cliquez sur **Démarrer**, pointez sur **outils d’administration**, puis cliquez sur **sauvegarde de Windows Server**. 
![Installer la sauvegarde](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png) 
2. Si vous êtes invité, dans le **contrôle de compte d’utilisateur** boîte de dialogue, fournir des informations d’identification de l’opérateur de sauvegarde, puis cliquez sur **OK**.
3. Cliquez sur **sauvegarde locale**.
4. Sur le **Action** menu, cliquez sur **sauvegarde unique**.
5. Dans l’Assistant sauvegarde unique, sur le **options de sauvegarde**, cliquez sur **différentes options**, puis cliquez sur **suivant**.
![Installer la sauvegarde](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)
6. Sur le **sélectionnez-le configuration de la sauvegarde**, cliquez sur **serveur entier (recommandé)**, puis cliquez sur **suivant**.
7. Sur le **spécifier le type de destination**, cliquez sur **lecteurs locaux** ou **dossier partagé distant**, puis cliquez sur **suivant**.
8. Sur le **sélectionner la Destination de sauvegarde** page, choisissez l’emplacement de sauvegarde.  Si vous avez sélectionné local lecteur choisissez un lecteur local ou si vous avez sélectionné à distance partage un partage réseau.
9. Dans l’écran de confirmation, cliquez sur **sauvegarde**.
![Installer la sauvegarde](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup4.png)
10. Une fois cette opération terminée cliquez sur **fermer**.
11. Fermez la sauvegarde de Windows Server.

>[!NOTE]
>Si vous obtenez une erreur indiquant qu’aucun emplacement de stockage de sauvegarde n’est disponible, vous devez ajouter un nouveau volume ou un partage distant ou exclure l’un des volumes qui a été sélectionnée.
>Si vous recevez un avertissement indiquant que le volume sélectionné est également inclus dans la liste des éléments à sauvegarder, déterminer s’il faut ou non à supprimer, puis cliquez sur **Ok**.

## <a name="using-wbadminexe-to-backup-a-windows-server"></a>À l’aide de Wbadmin.exe pour sauvegarder un serveur windows
Wbadmin.exe est un utilitaire de ligne de commande qui vous permet de sauvegarder et restaurer votre système d’exploitation, des volumes, fichiers, dossiers et applications à partir d’une invite de commandes.

#### <a name="to-perform-a-full-server-backup-using-wbadminexe"></a>Pour effectuer une sauvegarde complète du serveur à l’aide de Wbadmin.exe  
  
1.  Ouvrez une invite de commandes avec élévation de privilèges, tapez la commande suivante et appuyez sur ENTRÉE:  

        wbadmin start backup -backuptarget:<Drive_letter_to store_backup>: -include:<Drive_letter_to_include>:

![Installer la sauvegarde](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup5.png)
## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de forêt ActiveDirectory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt ActiveDirectory - procédures](AD-Forest-Recovery-Procedures.md)
