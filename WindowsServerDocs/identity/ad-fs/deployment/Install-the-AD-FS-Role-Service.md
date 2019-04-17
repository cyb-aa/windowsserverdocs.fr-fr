---
ms.assetid: c28a1b8b-5bec-4eed-8c95-a1a29cfc957c
title: "Installer le Service de rôle FS AD"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9851134d1ad73092ee44c34c99bc2d873d20ca07
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="install-the-ad-fs-role-service"></a>Installer le Service de rôle FS AD

>S’applique à: Windows Server2016, Windows Server2012R2

Vous pouvez utiliser la procédure suivante pour installer le service de rôle ADFS sur un ordinateur qui exécute Windows Server2012R2 à devenir le premier serveur de fédération dans une batterie de serveurs de fédération ou un serveur de fédération dans une batterie de serveurs de fédération existante.  
  
L’appartenance au groupe **administrateurs**, ou équivalent, sur l’ordinateur local est le minimum requis pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-install-the-ad-fs-server-role-via-the-add-roles-and-features-wizard"></a>Pour installer le rôle de serveur ADFS via l’Assistant Ajout de rôles et fonctionnalités  
  
1.  Ouvrez le Gestionnaire de serveur. Pour ouvrir le Gestionnaire de serveur, cliquez sur **le Gestionnaire de serveur** sur le **Démarrer** écran, ou **le Gestionnaire de serveur** dans la barre des tâches sur le bureau. Dans le **Quick Start** onglet de la **Bienvenue** vignette sur le **tableau de bord**, cliquez sur **ajouter des rôles et fonctionnalités**. Vous pouvez également cliquer sur **Ajout de rôles et fonctionnalités** sur le **gérer** menu.  
  
2.  Sur le **avant de commencer** , cliquez sur **suivant**.  
  
3.  Sur le **sélectionner le type d’installation**, cliquez sur **installation basée sur un rôle ou une fonctionnalité**, puis cliquez sur **suivant**.  
  
4.  Sur le **serveur de destination sélectionnez**, cliquez sur **sélectionner un serveur du pool de serveurs**, vérifiez que l’ordinateur cible est sélectionné, puis cliquez sur **suivant**.  
  
5.  Sur le **sélectionner des rôles de serveur**, cliquez sur **ActiveDirectory Federation Services**, puis cliquez sur **suivant**.  
  
6.  Sur le **sélectionner des fonctionnalités** , cliquez sur **suivant**. Les composants requis sont déjà présélectionnés. Vous n’êtes pas obligé de sélectionner d’autres fonctionnalités.  
  
7.  Sur le **ActiveDirectory Federation Services \(ADFS\)**, cliquez sur **suivant**.  
  
8.  Après avoir vérifié les informations sur la **confirmer les sélections d’installation**, cliquez sur **installer**.  
  
9. Sur le **progression de l’Installation** page, vérifiez que tous les éléments installés correctement, puis cliquez sur **fermer**.  
  
### <a name="to-install-the-ad-fs-server-role-via-windows-powershell"></a>Pour installer le rôle de serveur ADFS via Windows PowerShell  
  
1.  Sur l’ordinateur que vous souhaitez configurer en tant qu’un serveur de fédération, ouvrez la fenêtre de commande Windows PowerShell, puis exécutez la commande suivante:`Install-windowsfeature adfs-federation –IncludeManagementTools`.  
  
## <a name="see-also"></a>Voir aussi 

[Déploiement d’ADFS](../../ad-fs/AD-FS-Deployment.md)  

[Guide de déploiement de Windows Server2012R2 ADFS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Déploiement d’une batterie de serveurs de fédération](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

