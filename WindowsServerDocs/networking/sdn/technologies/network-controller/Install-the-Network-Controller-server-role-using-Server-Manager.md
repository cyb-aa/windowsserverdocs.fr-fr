---
title: Installer le rôle de serveur de contrôleur de réseau à l’aide du Gestionnaire de serveur
description: Cette rubrique fournit des instructions sur la façon d’installer le rôle de serveur de contrôleur de réseau à l’aide du Gestionnaire de serveur dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 3a6e4352-ff62-4290-b8a4-5c83740070fc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 699e2ca5c4ec33099d0ad948523b6f587ad118e4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859060"
---
# <a name="install-the-network-controller-server-role-using-server-manager"></a>Installer le rôle de serveur de contrôleur de réseau à l’aide du Gestionnaire de serveur

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique fournit des instructions sur la façon d’installer le rôle de serveur de contrôleur de réseau à l’aide du Gestionnaire de serveur.

>[!IMPORTANT]
>Ne déployez pas le rôle de serveur de contrôleur de réseau sur des hôtes physiques. Pour déployer un contrôleur de réseau, vous devez installer le rôle de serveur de contrôleur de réseau sur un ordinateur virtuel Hyper-V \(machine virtuelle\) qui est installé sur un ordinateur hôte Hyper-V. Une fois que vous avez installé le contrôleur de réseau sur des machines virtuelles sur trois Hyper différents\-hôtes V, vous devez activer l’Hyper\-hôtes V pour Sdn \(SDN\) en ajoutant les ordinateurs hôtes à l’aide de contrôleur de réseau la commande Windows PowerShell **New-NetworkControllerServer**. En procédant ainsi, vous permettez à l’équilibreur de charge logiciel SDN de la fonction. Pour plus d’informations, consultez [New-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).
  
Après avoir installé le contrôleur de réseau, vous devez utiliser les commandes Windows PowerShell pour la configuration de contrôleur de réseau supplémentaire. Pour plus d’informations, consultez [déployer de contrôleur de réseau à l’aide de Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
### <a name="to-install-network-controller"></a>Pour installer le contrôleur de réseau  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **Gérer**, puis sur **Ajouter des rôles et des fonctionnalités**. L’Assistant Ajout de rôles et fonctionnalités s’ouvre. Cliquez sur **Suivant**.  
  
2.  Dans **sélectionner le Type d’Installation**, conservez le paramètre par défaut et cliquez sur **suivant**.  
  
3.  Dans **sélectionner un serveur de Destination**, choisissez le serveur où vous souhaitez installer le contrôleur de réseau, puis cliquez sur **suivant**.  
  
4.  Dans **sélectionner des rôles de serveur**, dans **rôles**, cliquez sur **contrôleur de réseau**.  
  
    ![Rôle de serveur de contrôleur de réseau](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
5.  Le **ajouter des fonctionnalités qui sont requises pour le contrôleur de réseau** boîte de dialogue s’ouvre. Cliquez sur **ajouter des fonctionnalités**.  
  
    ![Ajouter des fonctionnalités pour le contrôleur de réseau](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_06.jpg)  
  
6.  Dans **sélectionner des rôles de serveur**, cliquez sur **suivant**.  
  
    ![Cliquez sur Suivant](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
7.  Dans **sélectionner des fonctionnalités**, cliquez sur **suivant**.  
  
8.  Dans **contrôleur de réseau** cliquez sur **suivant**.  
  
9. Dans **confirmer les sélections d’installation**, passez en revue vos choix. Installation du contrôleur de réseau nécessite que vous redémarrez l’ordinateur après l’exécution de l’Assistant. Pour cette raison, cliquez sur **redémarrer automatiquement le serveur de destination si nécessaire**. Le **Assistant Ajouter des rôles et fonctionnalités** boîte de dialogue s’ouvre. Cliquez sur **Oui**.  
  
    ![Assistant Ajout de rôles et de fonctionnalités](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_11.jpg)  
  
10. Dans **Confirmer les sélections pour l’installation**, cliquez sur **Installer**.  
  
11. Le rôle de serveur de contrôleur de réseau s’installe sur le serveur de destination et le serveur redémarre.  
  
12. Une fois l’ordinateur redémarré, ouvrez une session l’ordinateur et vérifier l’installation de contrôleur de réseau en consultant le Gestionnaire de serveur.  
  
    ![Gestionnaire de serveur](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/nc_013.jpg)  
  
## <a name="see-also"></a>Voir aussi  
[Contrôleur de réseau](Network-Controller.md)  
  


