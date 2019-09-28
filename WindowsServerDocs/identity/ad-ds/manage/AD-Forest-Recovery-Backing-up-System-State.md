---
title: Récupération de la forêt Active Directory-sauvegarde d’un serveur complet
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 9238cb27-0020-42f7-90d6-fcebf7e3c0bc
ms.technology: identity-adds
ms.openlocfilehash: 14aa7abc19573b76ebc144cb6dea5f510b45e269
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369350"
---
# <a name="ad-forest-recovery---backing-up-the-system-state-data"></a>Récupération de la forêt Active Directory-sauvegarde des données d’État du système  

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Utilisez la procédure suivante pour effectuer une sauvegarde de l’état du système sur un contrôleur de réseau à l’aide de Sauvegarde Windows Server ou de Wbadmin. exe.  

## <a name="to-perform-a-system-state-backup-using-windows-server-backup"></a>Pour effectuer une sauvegarde de l’état du système à l’aide de Sauvegarde Windows Server

1. Ouvrez **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur **sauvegarde Windows Server**.
   - Dans Windows Server 2008 R2 et Windows Server 2008, cliquez sur **Démarrer**, pointez sur **Outils d’administration**, puis cliquez sur **sauvegarde Windows Server**. 

   ![Installer la sauvegarde](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png)

2. Si vous y êtes invité, dans la boîte de dialogue **contrôle de compte d’utilisateur** , fournissez les informations d’identification de l’opérateur de sauvegarde, puis cliquez sur **OK**.
3. Cliquez sur **sauvegarde locale**.
4. Dans le menu **Action**, cliquez sur **Sauvegarde unique**.
5. Dans l’Assistant Sauvegarde unique, sur la page **options de sauvegarde** , cliquez sur **différentes options**, puis cliquez sur **suivant**.

   ![Installer la sauvegarde](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)

6. Dans la page **Sélectionner la configuration** de la sauvegarde, cliquez sur **personnalisé)** , puis cliquez sur **suivant**.
7. Dans l’écran **Sélectionner les éléments à sauvegarder** , cliquez sur **Ajouter des éléments** et sélectionnez État du **système** , puis cliquez sur **OK**.
   - Dans Windows Server 2008 R2 et Windows Server 2008, sélectionnez les volumes à inclure dans la sauvegarde. Si vous activez la case à cocher **activer la récupération du système** , tous les volumes critiques sont sélectionnés. 

   ![Installer la sauvegarde](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup.png)  

8. Dans la page **spécifier le type de destination** , cliquez sur **lecteurs locaux** ou sur **dossier partagé distant**, puis cliquez sur **suivant**.  Si vous effectuez une sauvegarde dans un dossier partagé distant, procédez comme suit :  
   - Tapez le chemin d’accès au dossier partagé.
   - Sous **Access Control**, sélectionnez **ne pas hériter** ou **hériter** pour déterminer l’accès à la sauvegarde, puis cliquez sur **suivant**.  
   - Dans la boîte de dialogue **fournir les informations d’identification de l’utilisateur pour la sauvegarde** , indiquez le nom d’utilisateur et le mot de passe d’un utilisateur qui dispose d’un accès en écriture au dossier partagé, puis cliquez sur **OK**.

9. Pour Windows Server 2008 R2 et Windows Server 2008, dans la page **spécifier une option avancée** , sélectionnez **sauvegarde de copie VSS** , puis cliquez sur **suivant**.
10. Sur la page **Sélectionner la destination** de la sauvegarde, choisissez l’emplacement de sauvegarde.  Si vous avez sélectionné lecteur local, choisissez un lecteur local ou, si vous avez sélectionné partage distant, choisissez un partage réseau.
11. Dans l’écran confirmation, cliquez sur **sauvegarde**.
12. Une fois cette opération terminée, cliquez sur **Fermer**.
13. Fermez Sauvegarde Windows Server.

## <a name="to-perform-a-system-state-backup-using-wbadminexe"></a>Pour effectuer une sauvegarde de l’état du système à l’aide de Wbadmin. exe

Ouvrez une invite de commandes avec élévation de privilèges, tapez la commande suivante et appuyez sur entrée :  
  
   ```
   wbadmin start systemstatebackup -backuptarget:<targetDrive>:
   ```

   ![Installer la sauvegarde](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup2.png)  

## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt Active Directory : procédures](AD-Forest-Recovery-Procedures.md)
