---
title: Récupération de la forêt Active Directory-sauvegarde d’un serveur complet
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adds
ms.openlocfilehash: 1579f8e88ea852ddf3f973b51b1b6ceed7c50a00
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824273"
---
# <a name="ad-forest-recovery---backing-up-a-full-server"></a>Récupération de la forêt Active Directory-sauvegarde d’un serveur complet  

>S’applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Une sauvegarde complète du serveur est recommandée pour préparer une récupération de forêt, car elle peut être restaurée sur un matériel différent ou sur une instance de système d’exploitation différente.  À l’aide de Sauvegarde Windows Server vous pouvez effectuer une sauvegarde complète de votre serveur. 

## <a name="windows-server-backup"></a>Sauvegarde Windows Server

Sauvegarde Windows Server n’est pas installé par défaut. Dans Windows Server 2016 et Windows Server 2012 R2, installez-le en suivant les étapes ci-dessous.

>[!NOTE]
>N’oubliez pas que les étapes peuvent varier légèrement entre Windows Server 2016 et Windows Server 2012 R2.

Pour connaître les étapes à suivre pour l’installer dans Windows Server 2008 et Windows Server 2008 R2, consultez [installation de sauvegarde Windows Server](https://technet.microsoft.com/library/cc771232.aspx).  

### <a name="to-install-windows-server-backup"></a>Pour installer Sauvegarde Windows Server

1. Ouvrez **Gestionnaire de serveur** , puis cliquez sur **Ajouter des rôles et des fonctionnalités**.
2. Dans l' **Assistant Ajout de rôles et de fonctionnalités,** cliquez sur **suivant**.
3. Dans l’écran **type d’installation** , laissez l’installation par défaut basée sur un **rôle ou une fonctionnalité** , puis cliquez sur **suivant**.
4. Dans l’écran **sélection du serveur** , cliquez sur **suivant**.
5. Dans l’écran **rôles du serveur** , cliquez sur **suivant**.
6. Dans l’écran **fonctionnalités** , sélectionnez **sauvegarde Windows Server** , puis cliquez sur **suivant**
   ![installer la sauvegarde](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup2.png)
7. Cliquez sur **Installer**.
8. Une fois l’installation terminée, cliquez sur **Fermer**.

### <a name="to-perform-a-backup-with-windows-server-backup"></a>Pour effectuer une sauvegarde avec Sauvegarde Windows Server

1. Ouvrez **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur **sauvegarde Windows Server**.
   - Dans Windows Server 2008 R2 et Windows Server 2008, cliquez sur **Démarrer**, pointez sur **Outils d’administration**, puis cliquez sur **sauvegarde Windows Server**.

   ![Installer la sauvegarde](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png) 

2. Si vous y êtes invité, dans la boîte de dialogue **contrôle de compte d’utilisateur** , fournissez les informations d’identification de l’opérateur de sauvegarde, puis cliquez sur **OK**.
3. Cliquez sur **sauvegarde locale**.
4. Dans le menu **Action**, cliquez sur **Sauvegarde unique**.
5. Dans l’Assistant Sauvegarde unique, sur la page **options de sauvegarde** , cliquez sur **différentes options**, puis cliquez sur **suivant**.

   ![Installer la sauvegarde](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)

6. Dans la page **Sélectionner la configuration** de la sauvegarde, cliquez sur **serveur complet (recommandé)** , puis cliquez sur **suivant**.
7. Dans la page **spécifier le type de destination** , cliquez sur **lecteurs locaux** ou sur **dossier partagé distant**, puis cliquez sur **suivant**.
8. Sur la page **Sélectionner la destination** de la sauvegarde, choisissez l’emplacement de sauvegarde.  Si vous avez sélectionné lecteur local, choisissez un lecteur local ou, si vous avez sélectionné partage distant, choisissez un partage réseau.
9. Dans l’écran confirmation, cliquez sur **sauvegarde**.

   ![Installer la sauvegarde](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup4.png)

10. Une fois cette opération terminée, cliquez sur **Fermer**.
11. Fermez Sauvegarde Windows Server.

>[!NOTE]
>Si vous recevez une erreur indiquant qu’aucun emplacement de stockage de sauvegarde n’est disponible, vous devez soit exclure l’un des volumes qui a été sélectionné, soit ajouter un nouveau volume ou partage distant.
>Si vous recevez un avertissement indiquant que le volume sélectionné est également inclus dans la liste des éléments à sauvegarder, déterminez s’il faut ou non supprimer, puis cliquez sur **OK**.

## <a name="using-wbadminexe-to-backup-a-windows-server"></a>Utilisation de Wbadmin. exe pour sauvegarder un serveur Windows Server

Wbadmin. exe est un utilitaire de ligne de commande qui vous permet de sauvegarder et de restaurer votre système d’exploitation, les volumes, les fichiers, les dossiers et les applications à partir d’une invite de commandes.

### <a name="to-perform-a-full-server-backup-using-wbadminexe"></a>Pour effectuer une sauvegarde complète du serveur à l’aide de Wbadmin. exe
  
- Ouvrez une invite de commandes avec élévation de privilèges, tapez la commande suivante et appuyez sur entrée :  

   ```
   wbadmin start backup -backuptarget:<Drive_letter_to store_backup>: -include:<Drive_letter_to_include>:
   ```

   ![Installer la sauvegarde](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup5.png)

## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt Active Directory : procédures](AD-Forest-Recovery-Procedures.md)
