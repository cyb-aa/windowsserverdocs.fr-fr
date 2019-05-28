---
title: Connexion de l’ordinateur virtuel Hyper-V
description: Décrit la connexion de Machine virtuelle, qui fournit l’accès à distance à une machine virtuelle. Inclut des détails sur la façon d’effectuer des tâches courantes, comme envoyer Ctrl-Alt-Suppr à la machine virtuelle.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: deae35b9-7647-42b8-b6bf-45645a44c9c4
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: a3c0fd18ded0621c550546a2f0108b573cc67767
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222512"
---
# <a name="hyper-v-virtual-machine-connection"></a>Connexion de l’ordinateur virtuel Hyper-V

>S'applique à : Windows Server 2016, Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2012, Windows 8

Connexion de Machine virtuelle \(VMConnect\) est un outil qui vous permet de se connecter à une machine virtuelle afin que vous pouvez installer ou interagir avec le système d’exploitation invité dans un ordinateur virtuel. Certaines des tâches que vous pouvez effectuer à l’aide de VMConnect sont les suivantes :  
  
-   Démarrer et arrêter une machine virtuelle  
  
-   Se connecter à une image DVD \(fichier .iso\) ou un lecteur flash USB  
  
-   Créer un point de contrôle  
  
-   modifier les paramètres d'un ordinateur virtuel  
    
## <a name="tips-for-using-vmconnect"></a>Conseils pour l’utilisation de VMConnect  
Vous pouvez trouver les informations suivantes utiles pour l’utilisation de VMConnect :  
  
|Pour ce faire...|Procédez comme suit...|  
|---------------|------------|  
|Envoyer des clics de souris ou l’entrée au clavier pour la machine virtuelle|Cliquez n’importe où dans la fenêtre de la machine virtuelle. Le pointeur de la souris peut apparaître comme un petit point lorsque vous vous connectez à une machine virtuelle en cours d’exécution.|  
|Retourner les clics de souris ou du clavier à l’ordinateur physique|Appuyez sur CTRL\+ALT\+gauche flèche et que vous déplacez le pointeur de souris en dehors de la fenêtre de la machine virtuelle. Cette combinaison de touches de mise en production de la souris peut être modifiée dans Hyper\-paramètres Hyper V\-V Manager.|  
|Envoyer CTRL\+ALT\+combinaison de touches de suppression à une machine virtuelle|Sélectionnez **Action** > **Ctrl\+Alt\+supprimer** ou utilisez la combinaison de touches CTRL\+ALT\+fin.|  
|Passer d’un mode de fenêtre à un intégral\-mode plein écran|Sélectionnez **vue** > **Mode plein écran**. Pour revenir au mode de fenêtre, appuyez sur CTRL\+ALT\+arrêter.|  
|Créer un point de contrôle pour capturer l’état actuel de l’ordinateur pour la résolution des problèmes|Sélectionnez **Action** > **point de contrôle** ou utilisez la combinaison de touches CTRL\+N.|  
|Modifier les paramètres de la machine virtuelle|Sélectionnez **fichier** > **paramètres**.|  
|Se connecter à une image DVD \(fichier .iso\) ou une disquette virtuelle \(fichier .vfd\)|Sélectionnez **Media**.<br /><br />Disquettes virtuelles ne sont pas pris en charge pour les machines virtuelles de génération 2. Pour plus d’informations, consultez [dois-je créer une machine virtuelle de génération 1 ou 2 dans Hyper-V ?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md).|  
|Utiliser les ressources locales d’un ordinateur hôte Hyper\-V virtual machine comme un lecteur flash USB|Activer le mode de session étendu sur l’ordinateur hôte Hyper-V, utilisez VMConnect pour vous connecter à la machine virtuelle et avant de vous connectez, choisissez la ressource locale que vous souhaitez utiliser. Pour les étapes spécifiques, consultez [utiliser les ressources locales sur Hyper\-ordinateur virtuel V avec VMConnect](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md).|  
|Modifier l’enregistrement des paramètres de VMConnect pour une machine virtuelle|Dans Windows PowerShell ou l’invite de commandes, exécutez la commande suivante :<br /><br />`VMConnect.exe <ServerName> <VMName> \/edit`|  
|Empêcher un utilisateur de VMConnect de prise en charge la session de VMConnect d’un autre utilisateur|[Activer le mode de session étendu sur l’hôte Hyper-V](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#turn-on-enhanced-session-mode-on-a-hyper-v-host).<br /><br />Ne pas apportant des améliorations activé le mode de session peuvent poser un risque de sécurité et de confidentialité. Si un utilisateur est connecté et ouvert une session sur une machine virtuelle via VMConnect et un autre utilisateur autorisé se connecte à la même machine virtuelle, la session sera prise par le second utilisateur et le premier utilisateur n’auront plus la session. Le deuxième utilisateur sera en mesure d’afficher le bureau du premier utilisateur, des documents et des applications.|
|Gérer les services d’intégration ou les composants qui permettent à la machine virtuelle communiquer avec l’hôte Hyper-V| Sur les hôtes Hyper-V qui exécutent Windows 10 ou Windows Server 2016, vous ne pouvez pas gérer d’integration services avec VMConnect. Consultez les rubriques suivantes : <br />- [Activer / désactiver les services d’intégration de l’hôte Hyper-V](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics) <br />- [Activer / désactiver les services d’intégration à partir d’une machine virtuelle de Windows](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#manage-integration-services-from-guest-os-windows)<br />- [Activer / désactiver les services d’intégration à partir d’une machine virtuelle Linux](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#manage-integration-services-from-guest-os-linux) <br />- [Conserver les services d’intégration mis à jour de la machine virtuelle](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#integration-service-maintenance)  <br />Pour les ordinateurs hôtes qui exécutent Windows Server 2012 ou Windows Server 2012 R2, consultez [Integration Services](https://technet.microsoft.com/library/dn798297(v=ws.11).aspx).|
|Redimensionnez la fenêtre VMConnect|Vous pouvez modifier la taille de la fenêtre VMConnect pour les machines virtuelles de génération 2 qui exécutent un système d’exploitation de Windows. Pour ce faire, vous devrez activer le mode de session étendu sur l’ordinateur hôte Hyper-V. Pour plus d’informations, consultez [activer le mode de session étendu sur l’hôte Hyper-V](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#turn-on-enhanced-session-mode-on-a-hyper-v-host). Pour les machines virtuelles qui s’exécutent Ubuntu, consultez [modification Ubuntu résolution de l’écran dans une machine virtuelle Hyper-V](https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/09/19/changing-ubuntu-screen-resolution-in-a-hyper-v-vm/).|


## <a name="keyboard-shortcuts"></a>Raccourcis clavier  
Par défaut, les clavier d’entrée et clics de souris sont envoyées à la machine virtuelle. Vous devrez peut-être appuyer sur CTRL + ALT + flèche gauche flèche avant d’utiliser les touches de raccourci suivantes. 

|Combinaison de touches|Description|  
|-------------------|---------------|  
|CTRL\+ALT\+flèche gauche|Version de la souris|  
|CTRL\+ALT\+FIN|Équivalent de CTRL\+ALT\+supprimer dans la machine virtuelle|  
|CTRL\+ALT\+ARRÊTER|Passer d’intégral\-mode plein écran au mode fenêtré|  
|CTRL\+O|Ouvre les paramètres de la machine virtuelle|  
|CTRL\+S|Démarre la machine virtuelle|  
|CTRL\+N|Créer un point de contrôle|  
|CTRL\+E|Revenir à un point de contrôle|  
|CTRL\+C|Effectuez une capture d’écran|  

## <a name="see-also"></a>Voir aussi  
-   [Utiliser les ressources locales sur l’ordinateur virtuel Hyper-V avec VMConnect](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)  
-   [Hyper-V sur Windows Server 2016](../Hyper-V-on-Windows-Server.md)  
-   [Hyper-V sur Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/windows_welcome)  
  
  
