---
title: "Récupération de la forêt ActiveDirectory - sauvegarde complète du serveur."
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 9238cb27-0020-42f7-90d6-fcebf7e3c0bc
ms.technology: identity-adfs
ms.openlocfilehash: a86d61536f8b426e1a5258c661d4e53da63d4162
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---backing-up-the-system-state-data"></a>Récupération de la forêt ActiveDirectory - sauvegarde des données d’état du système  

>S’applique à: Windows Server2016, Windows Server2012 et 2012R2, Windows Server2008 et 2008R2
 
Utilisez la procédure suivante pour effectuer une sauvegarde de l’état système sur un contrôleur de domaine à l’aide de la sauvegarde de Windows Server ou wbadmin.exe.  
  
## <a name="to-perform-a-system-state-backup-using-windows-server-backup"></a>Pour effectuer une sauvegarde de l’état système à l’aide de la sauvegarde de Windows Server  
1. Ouvrez **le Gestionnaire de serveur**, cliquez sur **outils**, puis cliquez sur **sauvegarde de Windows Server**.
    - Dans Windows Server2008R2 et Windows Server2008, cliquez sur **Démarrer**, pointez sur **outils d’administration**, puis cliquez sur **sauvegarde de Windows Server**. 
![Installer la sauvegarde](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png) 
2. Si vous êtes invité, dans le **contrôle de compte d’utilisateur** boîte de dialogue, fournir des informations d’identification de l’opérateur de sauvegarde, puis cliquez sur **OK**.
3. Cliquez sur **sauvegarde locale**.
4. Sur le **Action** menu, cliquez sur **sauvegarde unique**.
5. Dans l’Assistant sauvegarde unique, sur le **options de sauvegarde**, cliquez sur **différentes options**, puis cliquez sur **suivant**.
![Installer la sauvegarde](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)
6. Dans le **sélectionnez-le configuration de la sauvegarde**, cliquez sur **personnalisée)**, puis cliquez sur **suivant**.
7. Sur le **sélectionner les éléments à sauvegarder**, cliquez sur **ajouter les éléments** et sélectionnez **état du système** et cliquez sur **Ok**.
    - Dans Windows Server2008R2 et Windows Server2008, sélectionnez les volumes à inclure dans la sauvegarde. Si vous sélectionnez le **activer la récupération du système** case à cocher, tous les volumes critiques sont sélectionnés. 
![Installer la sauvegarde](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup.png)  
8. Sur le **spécifier le type de destination**, cliquez sur **lecteurs locaux** ou **dossier partagé distant**, puis cliquez sur **suivant**.  Si vous sauvegardez sur un dossier partagé distant, procédez comme suit:  
  
 1.  Tapez le chemin d’accès au dossier partagé.  
 2.  Sous **le contrôle d’accès**, sélectionnez **n’héritent pas des** ou **hériter** pour déterminer l’accès à la sauvegarde, puis cliquez sur **suivant**.  
 3.  Dans le **fournissent des informations d’identification utilisateur pour la sauvegarde** boîte de dialogue, fournissez le nom d’utilisateur et mot de passe pour un utilisateur qui a accès en écriture au dossier partagé, puis cliquez sur **OK**.
9. Pour Windows Server2008R2 et Windows Server2008, sur le **spécifier option avancée** page, sélectionnez **sauvegarde de copie VSS** puis cliquez sur **suivant**.
10. Sur le **sélectionner la Destination de sauvegarde** page, choisissez l’emplacement de sauvegarde.  Si vous avez sélectionné local lecteur choisissez un lecteur local ou si vous avez sélectionné à distance partage un partage réseau.
11. Dans l’écran de confirmation, cliquez sur **sauvegarde**.
12. Une fois cette opération terminée cliquez sur **fermer**.
13. Fermez la sauvegarde de Windows Server.

  
## <a name="to-perform-a-system-state-backup-using-wbadminexe"></a>Pour effectuer une sauvegarde de l’état système à l’aide de Wbadmin.exe  
  
1.  Ouvrez une invite de commandes avec élévation de privilèges, tapez la commande suivante et appuyez sur ENTRÉE:  
  
    ```  
    wbadmin start systemstatebackup -backuptarget:<targetDrive>:
    ```  
![Installer la sauvegarde](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup2.png)  

## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de forêt ActiveDirectory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt ActiveDirectory - procédures](AD-Forest-Recovery-Procedures.md)
