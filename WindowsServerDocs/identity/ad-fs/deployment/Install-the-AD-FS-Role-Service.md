---
ms.assetid: c28a1b8b-5bec-4eed-8c95-a1a29cfc957c
title: Installer le service de rôle AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 46231015ffb91f7c6a1f15615e0eede3fc948be2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408352"
---
# <a name="install-the-ad-fs-role-service"></a>Installer le service de rôle AD FS

Vous pouvez utiliser la procédure suivante pour installer le service de rôle AD FS sur un ordinateur qui exécute Windows Server 2012 R2 afin qu’il devienne le premier serveur de Fédération dans une batterie de serveurs de Fédération ou un serveur de Fédération dans une batterie de serveurs de fédération existante.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **administrateurs**ou à un groupe équivalent sur l’ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-install-the-ad-fs-server-role-via-the-add-roles-and-features-wizard"></a>Pour installer le rôle serveur AD FS via l’Assistant Ajout de rôles et de fonctionnalités  
  
1.  Ouvrez le Gestionnaire de serveur. Pour ouvrir Gestionnaire de serveur, cliquez sur **Gestionnaire de serveur** dans l’écran d' **accueil** , ou **Gestionnaire de serveur** dans la barre des tâches sur le bureau. Sous l’onglet **Démarrage rapide** de la vignette **Bienvenue** dans la page **Tableau de bord**, cliquez sur **Ajouter des rôles et des fonctionnalités**. Vous pouvez également cliquer sur **Ajouter des rôles et fonctionnalités** dans le menu **Gérer** .  
  
2.  Dans la page **Avant de commencer** , cliquez sur **Suivant**.  
  
3.  Dans la page **Sélectionner le type d’installation** , cliquez sur **rôle @ no__t-2Based ou sur fonctionnalité @ no__t-3based installation**, puis cliquez sur **suivant**.  
  
4.  Dans la page **Sélectionner le serveur de destination** , cliquez sur **Sélectionner un serveur du pool de serveurs**, vérifiez que l'ordinateur cible est sélectionné, puis cliquez sur **Suivant**.  
  
5.  Dans la page **Sélectionner des rôles de serveurs** , cliquez sur **Services AD FS (Active Directory Federation Services)** , puis cliquez sur **Suivant**.  
  
6.  Dans la page **Sélectionner les fonctionnalités** , cliquez sur **Suivant**. La configuration requise est présélectionnée pour vous. Vous n’avez pas besoin de sélectionner d’autres fonctionnalités.  
  
7.  Dans la page **Active Directory service FS (Federation Service) \(AD FS @ no__t-2** , cliquez sur **suivant**.  
  
8.  Après avoir vérifié les informations de la page **confirmer les sélections d’installation** , cliquez sur **installer**.  
  
9. Dans la page **Progression de l'installation** , vérifiez que tout a été correctement installé, puis cliquez sur **Fermer**.  
  
### <a name="to-install-the-ad-fs-server-role-via-windows-powershell"></a>Pour installer le rôle serveur AD FS via Windows PowerShell  
  
1.  Sur l’ordinateur que vous souhaitez configurer en tant que serveur de Fédération, ouvrez la fenêtre de commande Windows PowerShell, puis exécutez la commande suivante `Install-windowsfeature adfs-federation –IncludeManagementTools`:.  
  
## <a name="see-also"></a>Voir aussi 

[Déploiement d’AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guide de déploiement de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Déploiement d’une batterie de serveurs de fédération](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

