---
title: Installer le rôle de serveur de contrôleur de réseau à l’aide du Gestionnaire de serveur
description: Cette rubrique fournit des instructions sur la façon d’installer le rôle de serveur de contrôleur de réseau à l’aide du Gestionnaire de serveur dans Windows Server2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 3a6e4352-ff62-4290-b8a4-5c83740070fc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 15cb1ef3bad7038cc97784504807b44b4920def6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-network-controller-server-role-using-server-manager"></a>Installer le rôle de serveur de contrôleur de réseau à l’aide du Gestionnaire de serveur

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique fournit des instructions sur la façon d’installer le rôle de serveur de contrôleur de réseau à l’aide du Gestionnaire de serveur.

>[!IMPORTANT]
>Ne déployez pas le rôle de serveur de contrôleur de réseau sur des hôtes physiques. Pour déployer un contrôleur de réseau, vous devez installer le rôle de serveur de contrôleur de réseau sur un ordinateur virtuel Hyper-V \(VM\) qui est installé sur un ordinateur hôte Hyper-V. Une fois que vous avez installé le contrôleur de réseau sur les ordinateurs virtuels sur trois différents hôtes Hyper\-V, vous devez activer les ordinateurs hôtes Hyper\-V pour \(SDN\) logiciel défini de mise en réseau en ajoutant les ordinateurs hôtes à un contrôleur de réseau à l’aide de la commande Windows PowerShell **New-NetworkControllerServer**. En procédant ainsi, vous activez l’équilibrage de charge logicielle SDN de la fonction. Pour plus d’informations, voir [New-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).
  
Après avoir installé le contrôleur de réseau, vous devez utiliser les commandes Windows PowerShell pour la configuration de contrôleur de réseau supplémentaire. Pour plus d’informations, voir [déployer le contrôleur de réseau à l’aide de Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
### <a name="to-install-network-controller"></a>Pour installer le contrôleur de réseau  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **gérer**, puis cliquez sur **Ajout de rôles et fonctionnalités**. L’Assistant Ajout de rôles et fonctionnalités s’ouvre. Cliquez sur **suivant**.  
  
2.  Dans **sélectionner le Type d’Installation**, conservez la valeur par défaut et cliquez sur **suivant**.  
  
3.  Dans **sélectionner le serveur de Destination**, choisissez le serveur où vous souhaitez installer le contrôleur de réseau, puis cliquez sur **suivant**.  
  
4.  Dans **sélectionner des rôles de serveur**, dans **rôles**, cliquez sur **contrôleur de réseau**.  
  
    ![Rôle de serveur de contrôleur de réseau](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
5.  Le **ajouter des fonctionnalités qui sont requises pour le contrôleur de réseau** boîte de dialogue s’ouvre. Cliquez sur **ajouter des fonctionnalités**.  
  
    ![Ajouter des fonctionnalités de contrôleur de réseau](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_06.jpg)  
  
6.  Dans **sélectionner des rôles de serveur**, cliquez sur **suivant**.  
  
    ![Cliquez sur Suivant](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
7.  Dans **sélectionner des fonctionnalités**, cliquez sur **suivant**.  
  
8.  Dans **contrôleur de réseau** cliquez sur **suivant**.  
  
9. Dans **confirmer les sélections d’installation**, passez en revue vos choix. Installation du contrôleur de réseau nécessite que vous redémarrez l’ordinateur après l’exécution de l’Assistant. Pour cette raison, cliquez sur **redémarrer automatiquement le serveur de destination si nécessaire**. Le **Assistant Ajouter des rôles et fonctionnalités** boîte de dialogue s’ouvre. Cliquez sur **Oui**.  
  
    ![Assistant Ajout de rôles et fonctionnalités](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_11.jpg)  
  
10. Dans **confirmer les sélections d’installation**, cliquez sur **installer**.  
  
11. Le rôle de serveur de contrôleur de réseau installe sur le serveur de destination et le serveur redémarre.  
  
12. Après le redémarrage de l’ordinateur, ouvrez une session sur l’ordinateur et vérifier l’installation du contrôleur de réseau en consultant le Gestionnaire de serveur.  
  
    ![Gestionnaire de serveur](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/nc_013.jpg)  
  
## <a name="see-also"></a>Voir aussi  
[Contrôleur de réseau](Network-Controller.md)  
  


