---
title: Récupération de forêt AD - sauvegarde d’un serveur complet
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 9238cb27-0020-42f7-90d6-fcebf7e3c0bc
ms.technology: identity-adds
ms.openlocfilehash: a5306960bb2dca3849bdb4fc7304781af3f25335
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815870"
---
# <a name="ad-forest-recovery---backing-up-the-system-state-data"></a>Récupération de forêt AD - sauvegarde des données d’état du système  

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Utilisez la procédure suivante pour effectuer une sauvegarde de l’état système sur un contrôleur de domaine à l’aide de la sauvegarde Windows Server ou wbadmin.exe.  

## <a name="to-perform-a-system-state-backup-using-windows-server-backup"></a>Pour effectuer une sauvegarde de l’état système à l’aide de la sauvegarde Windows Server

1. Ouvrez **le Gestionnaire de serveur**, cliquez sur **outils**, puis cliquez sur **sauvegarde Windows Server**.
   - Dans Windows Server 2008 R2 et Windows Server 2008, cliquez sur **Démarrer**, pointez sur **outils d’administration**, puis cliquez sur **sauvegarde Windows Server**. 

   ![Installer la sauvegarde](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png)

2. Si vous êtes invité, dans le **contrôle de compte d’utilisateur** boîte de dialogue, fournissez les informations d’identification de l’opérateur de sauvegarde, puis cliquez sur **OK**.
3. Cliquez sur **sauvegarde locale**.
4. Dans le menu **Action**, cliquez sur **Sauvegarde unique**.
5. Dans l’Assistant sauvegarde unique, sur le **options de sauvegarde** , cliquez sur **différentes options**, puis cliquez sur **suivant**.

   ![Installer la sauvegarde](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)

6. Dans le **sélectionnez configuration de la sauvegarde** , cliquez sur **personnalisé)**, puis cliquez sur **suivant**.
7. Sur le **sélectionner les éléments à sauvegarder** , cliquez sur **ajouter les éléments** et sélectionnez **état du système** et cliquez sur **Ok**.
   - Dans Windows Server 2008 R2 et Windows Server 2008, sélectionnez les volumes à inclure dans la sauvegarde. Si vous sélectionnez le **activer la récupération de système** case à cocher, tous les volumes critiques sont sélectionnées. 

   ![Installer la sauvegarde](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup.png)  

8. Sur le **spécifier le type de destination** , cliquez sur **lecteurs locaux** ou **dossier partagé distant**, puis cliquez sur **suivant**.  Si vous sauvegardez vos données vers un dossier partagé distant, procédez comme suit :  
   - Tapez le chemin d’accès au dossier partagé.
   - Sous **contrôle d’accès**, sélectionnez **n’héritent pas** ou **hériter** pour déterminer l’accès à la sauvegarde, puis cliquez sur **suivant**.  
   - Dans le **fournissent des informations d’identification utilisateur pour la sauvegarde** boîte de dialogue, fournissez le nom d’utilisateur et le mot de passe pour un utilisateur qui a accès en écriture au dossier partagé, puis cliquez sur **OK**.

9. Pour Windows Server 2008 R2 et Windows Server 2008, sur le **spécifiez option avancée** page, sélectionnez **sauvegarde de copie VSS** puis cliquez sur **suivant**.
10. Sur le **sélectionner la Destination de sauvegarde** page, choisissez l’emplacement de sauvegarde.  Si vous avez sélectionné local lecteur choisir un lecteur local ou si vous avez sélectionné à distance partage Choisissez un partage réseau.
11. Dans l’écran de confirmation, cliquez sur **sauvegarde**.
12. Une fois cette opération terminée cliquez sur **fermer**.
13. Fermez la sauvegarde de Windows Server.

## <a name="to-perform-a-system-state-backup-using-wbadminexe"></a>Pour effectuer une sauvegarde de l’état système à l’aide de Wbadmin.exe

Ouvrez une invite de commandes avec élévation de privilèges, tapez la commande suivante et appuyez sur ENTRÉE :  
  
   ```
   wbadmin start systemstatebackup -backuptarget:<targetDrive>:
   ```

   ![Installer la sauvegarde](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup2.png)  

## <a name="next-steps"></a>Étapes suivantes

- [Guide de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de forêt AD - procédures](AD-Forest-Recovery-Procedures.md)
