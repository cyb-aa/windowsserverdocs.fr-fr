---
title: Installer le rôle de serveur du contrôleur de réseau à l’aide de Gestionnaire de serveur
description: Cette rubrique fournit des instructions sur l’installation du rôle de serveur de contrôleur de réseau à l’aide de Gestionnaire de serveur dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 3a6e4352-ff62-4290-b8a4-5c83740070fc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8b656bbd823a10f1e36d1757bb53c4565d4e828c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405832"
---
# <a name="install-the-network-controller-server-role-using-server-manager"></a>Installer le rôle de serveur du contrôleur de réseau à l’aide de Gestionnaire de serveur

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Cette rubrique fournit des instructions sur l’installation du rôle de serveur de contrôleur de réseau à l’aide de Gestionnaire de serveur.

>[!IMPORTANT]
>Ne déployez pas le rôle de serveur de contrôleur de réseau sur les hôtes physiques. Pour déployer le contrôleur de réseau, vous devez installer le rôle serveur contrôleur de réseau sur une machine virtuelle Hyper-V \(VM @ no__t-1 qui est installée sur un ordinateur hôte Hyper-V. Une fois que vous avez installé le contrôleur de réseau sur les machines virtuelles sur trois ordinateurs hôtes Hyper @ no__t-0V différents, vous devez activer les hôtes Hyper @ no__t-1V pour la mise en réseau définie par logiciel \(SDN @ no__t-3 en ajoutant les ordinateurs hôtes au contrôleur de réseau à l’aide de Windows PowerShell commande **New-NetworkControllerServer**. En procédant ainsi, vous activez le Load Balancer logiciel SDN pour fonctionner. Pour plus d’informations, consultez [New-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).
  
Une fois que vous avez installé le contrôleur de réseau, vous devez utiliser les commandes Windows PowerShell pour une configuration de contrôleur de réseau supplémentaire. Pour plus d’informations, consultez [déployer un contrôleur de réseau à l’aide de Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
### <a name="to-install-network-controller"></a>Pour installer le contrôleur de réseau  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **Gérer**, puis sur **Ajouter des rôles et des fonctionnalités**. L’Assistant Ajout de rôles et de fonctionnalités s’ouvre. Cliquez sur **Suivant**.  
  
2.  Dans **Sélectionner le type d’installation**, conservez le paramètre par défaut et cliquez sur **suivant**.  
  
3.  Dans **Sélectionner le serveur de destination**, choisissez le serveur sur lequel vous souhaitez installer le contrôleur de réseau, puis cliquez sur **suivant**.  
  
4.  Dans **Sélectionner des rôles de serveurs**, dans **rôles**, cliquez sur **contrôleur de réseau**.  
  
    ![Rôle de serveur du contrôleur de réseau](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
5.  La boîte de dialogue **Ajouter des fonctionnalités requises pour le contrôleur de réseau** s’ouvre. Cliquez sur **Ajouter des fonctionnalités**.  
  
    ![Ajouter des fonctionnalités pour le contrôleur de réseau](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_06.jpg)  
  
6.  Dans **Sélectionner des rôles de serveurs**, cliquez sur **suivant**.  
  
    ![Cliquez sur suivant](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
7.  Dans **Sélectionner des fonctionnalités**, cliquez sur **suivant**.  
  
8.  Dans **contrôleur de réseau** , cliquez sur **suivant**.  
  
9. Dans **confirmer les sélections d’installation**, passez en revue vos choix. L’installation du contrôleur de réseau requiert le redémarrage de l’ordinateur après l’exécution de l’Assistant. Pour cette raison, cliquez sur **redémarrer automatiquement le serveur de destination, si nécessaire**. La boîte de dialogue **Assistant Ajout de rôles et de fonctionnalités** s’ouvre. Cliquez sur **Oui**.  
  
    ![Assistant Ajout de rôles et de fonctionnalités](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_11.jpg)  
  
10. Dans **Confirmer les sélections pour l’installation**, cliquez sur **Installer**.  
  
11. Le rôle serveur contrôleur de réseau est installé sur le serveur de destination, puis le serveur redémarre.  
  
12. Une fois l’ordinateur redémarré, connectez-vous à l’ordinateur et vérifiez l’installation du contrôleur de réseau en affichant Gestionnaire de serveur.  
  
    ![Gestionnaire de serveur](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/nc_013.jpg)  
  
## <a name="see-also"></a>Voir aussi  
[Contrôleur de réseau](Network-Controller.md)  
  


