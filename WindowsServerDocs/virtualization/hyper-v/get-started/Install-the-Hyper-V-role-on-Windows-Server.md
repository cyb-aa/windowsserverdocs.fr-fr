---
title: Installer le rôle Hyper-V sur Windows Server
description: Fournit des instructions pour installer Hyper-V à l’aide du Gestionnaire de serveur ou de Windows PowerShell
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 8e871317-09d2-4314-a6ec-ced12b7aee89
author: KBDAzure
ms.author: kathydav
ms.date: 12/02/2016
ms.openlocfilehash: a4167065c11cf87ba761ed65884b88c5413dfdf8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870430"
---
# <a name="install-the-hyper-v-role-on-windows-server"></a>Installer le rôle Hyper-V sur Windows Server

>S'applique à : Windows Server 2016, Windows Server 2019
  
Pour créer et exécuter des machines virtuelles, installer le rôle Hyper-V sur Windows Server à l’aide du Gestionnaire de serveur ou le **Install-WindowsFeature** applet de commande dans Windows PowerShell. Pour Windows 10, consultez [installer Hyper-V sur Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v).

Pour en savoir plus sur Hyper-V, consultez le [vue d’ensemble de la technologie Hyper-V](..\Hyper-V-Technology-Overview.md). Pour essayer Windows Server 2019, vous pouvez télécharger et installer une copie d’évaluation. Consultez le [centre d’évaluation](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019).

Avant d’installer Windows Server ou ajouter le rôle Hyper-V, assurez-vous que :
- Matériel de votre ordinateur est compatible. Pour plus d’informations, consultez [System Configuration requise pour Windows Server](../../../get-started/System-Requirements.md) et [configuration système requise pour Hyper-V sur Windows Server](../System-requirements-for-Hyper-V-on-Windows.md).
- Vous ne souhaitez pas utiliser des applications de la virtualisation par des tiers qui s’appuient sur les mêmes fonctionnalités de processeur qui nécessite de Hyper-V. Exemples : VMWare Workstation et VirtualBox. Vous pouvez installer Hyper-V sans désinstaller ces autres applications. Toutefois, si vous essayez de les utiliser pour gérer des machines virtuelles lors de l’exécution de l’hyperviseur Hyper-V, les machines virtuelles ne peut pas démarrer ou peuvent s’exécuter unreliably. Pour plus d’informations et des instructions sur la désactivation de l’hyperviseur Hyper-V si vous devez utiliser une de ces applications, consultez [des applications de virtualisation ne fonctionnent pas avec Hyper-V, Device Guard et Credential Guard](https://support.microsoft.com/help/3204980/virtualization-applications-do-not-work-together-with-hyper-v-device-g).

Si vous souhaitez installer uniquement les outils de gestion, telles que le Gestionnaire Hyper-V, consultez [gérer à distance des ordinateurs hôtes Hyper-V avec le Gestionnaire Hyper-V](..\Manage\Remotely-manage-Hyper-V-hosts.md).
  
## <a name="BKMK_SERV"></a>Installer Hyper-V à l’aide du Gestionnaire de serveur  
  
1. Dans **Gestionnaire de serveur**, dans le menu **Gérer**, cliquez sur **Ajouter des rôles et fonctionnalités**.  
  
2. Dans la page **Avant de commencer** , vérifiez que votre serveur de destination et environnement réseau sont préparés pour le rôle et la fonctionnalité que vous voulez installer. Cliquez sur **Suivant**.  
  
3. Dans la page **Sélectionner le type d’installation**, cliquez sur **Installation basée sur un rôle ou une fonctionnalité**, puis sur **Suivant**.  
  
4. Dans la page **Sélectionner le serveur de destination**, sélectionnez un serveur dans le pool de serveurs, puis cliquez sur **Suivant**.  
  
5. Dans la page **Sélectionner des rôles de serveurs**, sélectionnez **Hyper-V**.  
  
6. Pour ajouter les outils avec lesquels vous créez et gérez des ordinateurs virtuels, cliquez sur **Ajouter des fonctionnalités**. Dans la page Fonctionnalités, cliquez sur **Suivant**.  
  
7. Dans les pages **Créer des commutateurs virtuels**, **Migration d’ordinateur virtuel** et **Emplacements par défaut**, sélectionnez les options appropriées.  
  
8. Dans la page **Confirmer les sélections d’installation**, sélectionnez **Redémarrer automatiquement le serveur de destination, si nécessaire**, puis cliquez sur **Installer**.  
  
9. Lors de l’installation terminée, vérifiez que Hyper-V a été installé correctement. Ouvrez le **tous les serveurs** page dans le Gestionnaire de serveur et sélectionnez un serveur sur lequel vous avez installé Hyper-V. Vérifier le **des rôles et fonctionnalités** vignette sur la page pour le serveur sélectionné.  
  
## <a name="BKMK_PWRSH"></a>Installer Hyper-V à l’aide de l’applet de commande Install-WindowsFeature  
  
1. Sur le Bureau Windows, cliquez sur le bouton Démarrer et tapez une partie du nom **Windows PowerShell**.  
  
2. Avec le bouton droit de Windows PowerShell et sélectionnez **exécuter en tant qu’administrateur**.  
  
3. Pour installer Hyper-V sur un serveur auquel vous êtes connecté à distance, exécutez la commande suivante et remplacez `<computer_name>` avec le nom du serveur.  
  
    ```powershell
    Install-WindowsFeature -Name Hyper-V -ComputerName <computer_name> -IncludeManagementTools -Restart  
    ```  
  
    Si vous êtes connecté localement sur le serveur, exécutez la commande sans `-ComputerName <computer_name>`.  
  
4. Après le redémarrage du serveur, vous pouvez voir que le rôle Hyper-V est installé et voir quels sont les autres rôles et des fonctionnalités installés en exécutant la commande suivante :  
  
    ```powershell
    Get-WindowsFeature -ComputerName <computer_name>  
    ```  
  
    Si vous êtes connecté localement sur le serveur, exécutez la commande sans `-ComputerName <computer_name>`.  
  
> [!NOTE]  
> Si vous installez ce rôle sur un serveur qui exécute l’option d’installation Server Core de Windows Server 2016 et que vous utilisez le paramètre `-IncludeManagementTools`, uniquement le Hyper-V Module pour Windows PowerShell est installé. Vous pouvez utiliser l’outil de gestion de l’interface graphique utilisateur, le Gestionnaire Hyper-V, sur un autre ordinateur pour gérer à distance un ordinateur hôte Hyper-V qui s’exécute sur une installation Server Core. Pour obtenir des instructions sur la connexion à distance, consultez [gérer à distance des ordinateurs hôtes Hyper-V avec le Gestionnaire Hyper-V](..\Manage\Remotely-manage-Hyper-V-hosts.md).  
  
## <a name="see-also"></a>Voir aussi  
  
- [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/Microsoft.Windows.ServerManager.Migration/Install-WindowsFeature)  
