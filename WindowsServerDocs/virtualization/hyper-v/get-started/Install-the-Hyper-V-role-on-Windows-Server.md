---
title: Installer le rôle Hyper-V sur Windows Server
description: Fournit des instructions pour l’installation d’Hyper-V à l’aide de Gestionnaire de serveur ou Windows PowerShell
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: get-started-article
ms.assetid: 8e871317-09d2-4314-a6ec-ced12b7aee89
author: kbdazure
ms.author: kathydav
ms.date: 12/02/2016
ms.openlocfilehash: d4d8f2343f0935ea7185890319a3e33564750572
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860822"
---
# <a name="install-the-hyper-v-role-on-windows-server"></a>Installer le rôle Hyper-V sur Windows Server

>S’applique à : Windows Server 2016, Windows Server 2019
  
Pour créer et exécuter des ordinateurs virtuels, installez le rôle Hyper-V sur Windows Server à l’aide de Gestionnaire de serveur ou de l’applet de commande **install-WindowsFeature** dans Windows PowerShell. Pour Windows 10, consultez [installer Hyper-V sur Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v).

Pour en savoir plus sur Hyper-V, consultez la [vue d’ensemble de la technologie Hyper-v](../Hyper-V-Technology-Overview.md). Pour tester Windows Server 2019, vous pouvez télécharger et installer une copie d’évaluation. Consultez le [Centre d’évaluation](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019).

Avant d’installer Windows Server ou d’ajouter le rôle Hyper-V, vérifiez les éléments suivants :
- Le matériel de votre ordinateur est compatible. Pour plus d’informations, consultez [Configuration système requise pour Windows Server](../../../get-started/System-Requirements.md) et [Configuration système requise pour Hyper-V sur Windows Server](../System-requirements-for-Hyper-V-on-Windows.md).
- Vous n’envisagez pas d’utiliser des applications de virtualisation tierces qui reposent sur les mêmes fonctionnalités de processeur que celles requises par Hyper-V. Exemples : VMWare Workstation et VirtualBox. Vous pouvez installer Hyper-V sans désinstaller ces autres applications. Toutefois, si vous essayez de les utiliser pour gérer des ordinateurs virtuels lorsque l’hyperviseur Hyper-V est en cours d’exécution, il se peut que les ordinateurs virtuels ne démarrent pas ou ne s’exécutent pas de manière fiable. Pour plus d’informations et pour obtenir des instructions sur la désactivation de l’hyperviseur Hyper-V si vous devez utiliser l’une de ces applications, consultez les [applications de virtualisation ne fonctionnent pas avec Hyper-v, Device Guard et Credential Guard](https://support.microsoft.com/help/3204980/virtualization-applications-do-not-work-together-with-hyper-v-device-g).

Si vous souhaitez installer uniquement les outils de gestion, tels que le Gestionnaire Hyper-V, consultez [gérer à distance les ordinateurs hôtes Hyper-v à l’aide du Gestionnaire Hyper-v](../Manage/Remotely-manage-Hyper-V-hosts.md).
  
## <a name="install-hyper-v-by-using-server-manager"></a>Installer Hyper-V à l’aide de Gestionnaire de serveur  
  
1. Dans **Gestionnaire de serveur**, dans le menu **Gérer**, cliquez sur **Ajouter des rôles et fonctionnalités**.  
  
2. Dans la page **Avant de commencer**, vérifiez que votre serveur de destination et environnement réseau sont préparés pour le rôle et la fonctionnalité que vous voulez installer. Cliquez sur **Suivant**.  
  
3. Dans la page **Sélectionner le type d’installation**, cliquez sur **Installation basée sur un rôle ou une fonctionnalité**, puis sur **Suivant**.  
  
4. Dans la page **Sélectionner le serveur de destination**, sélectionnez un serveur dans le pool de serveurs, puis cliquez sur **Suivant**.  
  
5. Dans la page **Sélectionner des rôles de serveurs**, sélectionnez **Hyper-V**.  
  
6. Pour ajouter les outils avec lesquels vous créez et gérez des ordinateurs virtuels, cliquez sur **Ajouter des fonctionnalités**. Dans la page Fonctionnalités, cliquez sur **Suivant**.  
  
7. Dans les pages **Créer des commutateurs virtuels**, **Migration d’ordinateur virtuel** et **Emplacements par défaut**, sélectionnez les options appropriées.  
  
8. Dans la page **Confirmer les sélections d’installation**, sélectionnez **Redémarrer automatiquement le serveur de destination, si nécessaire**, puis cliquez sur **Installer**.  
  
9. Une fois l’installation terminée, vérifiez que Hyper-V est correctement installé. Ouvrez la page **tous les serveurs** dans Gestionnaire de serveur et sélectionnez un serveur sur lequel vous avez installé Hyper-V. Vérifiez la vignette **rôles et fonctionnalités** sur la page du serveur sélectionné.  
  
## <a name="install-hyper-v-by-using-the-install-windowsfeature-cmdlet"></a>Installer Hyper-V à l’aide de l’applet de commande Install-WindowsFeature  
  
1. Sur le Bureau Windows, cliquez sur le bouton Démarrer et tapez une partie du nom **Windows PowerShell**.  
  
2. Cliquez avec le bouton droit sur Windows PowerShell, puis sélectionnez **exécuter en tant qu’administrateur**.  
  
3. Pour installer Hyper-V sur un serveur auquel vous êtes connecté à distance, exécutez la commande suivante et remplacez `<computer_name>` par le nom du serveur.  
  
    ```powershell
    Install-WindowsFeature -Name Hyper-V -ComputerName <computer_name> -IncludeManagementTools -Restart  
    ```  
  
    Si vous êtes connecté localement au serveur, exécutez la commande sans `-ComputerName <computer_name>`.  
  
4. Une fois le serveur redémarré, vous pouvez voir que le rôle Hyper-V est installé et voir les autres rôles et fonctionnalités installés en exécutant la commande suivante :  
  
    ```powershell
    Get-WindowsFeature -ComputerName <computer_name>  
    ```  
  
    Si vous êtes connecté localement au serveur, exécutez la commande sans `-ComputerName <computer_name>`.  
  
> [!NOTE]  
> Si vous installez ce rôle sur un serveur qui exécute l’option d’installation minimale de Windows Server 2016 et que vous utilisez le paramètre `-IncludeManagementTools`, seul le module Hyper-V pour Windows PowerShell est installé. Vous pouvez utiliser l’outil de gestion GUI, le Gestionnaire Hyper-V, sur un autre ordinateur pour gérer à distance un ordinateur hôte Hyper-V qui s’exécute sur une installation Server Core. Pour obtenir des instructions sur la connexion à distance, consultez [gérer à distance les ordinateurs hôtes Hyper-v avec le Gestionnaire Hyper-v](../Manage/Remotely-manage-Hyper-V-hosts.md).  
  
## <a name="see-also"></a>Voir aussi  
  
- [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/Microsoft.Windows.ServerManager.Migration/Install-WindowsFeature)  
