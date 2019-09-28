---
title: Connexion à un ordinateur virtuel Hyper-V
description: Décrit la connexion à un ordinateur virtuel, qui fournit l’accès à distance à un ordinateur virtuel. Contient des détails sur la façon d’effectuer des tâches courantes, comme envoyer Ctrl-Alt-Suppr à la machine virtuelle.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: fba83d22d9e5d9f31a5809781aa04943cc4cd3af
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364154"
---
# <a name="hyper-v-virtual-machine-connection"></a>Connexion à un ordinateur virtuel Hyper-V

>S'applique à : Windows Server 2016, Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2012, Windows 8

La vmconnect \(\) de connexion à un ordinateur virtuel est un outil que vous utilisez pour vous connecter à une machine virtuelle afin de pouvoir installer ou interagir avec le système d’exploitation invité sur une machine virtuelle. Voici quelques-unes des tâches que vous pouvez effectuer à l’aide de VMConnect :  
  
-   Démarrer et arrêter un ordinateur virtuel  
  
-   Connectez-vous à un \(fichier\) . ISO image de DVD ou à un disque mémoire flash USB  
  
-   Créer un point de contrôle  
  
-   modifier les paramètres d'un ordinateur virtuel  
    
## <a name="tips-for-using-vmconnect"></a>Conseils pour l’utilisation de VMConnect  
Vous pouvez trouver les informations suivantes utiles pour l’utilisation de VMConnect :  
  
|Pour effectuer cette opération...|Procédez comme suit...|  
|---------------|------------|  
|Envoyer des clics de souris ou une entrée au clavier sur l’ordinateur virtuel|Cliquez n’importe où dans la fenêtre de la machine virtuelle. Le pointeur de la souris peut apparaître sous la forme d’un petit point lorsque vous vous connectez à une machine virtuelle en cours d’exécution.|  
|Retourner des clics de souris ou une entrée au clavier de l’ordinateur physique|Appuyez sur\+Ctrl\+ALT gauche, puis déplacez le pointeur de la souris en dehors de la fenêtre de l’ordinateur virtuel. Cette combinaison de touches de déclenchement de la souris peut être\-modifiée dans les paramètres\-Hyper v dans le Gestionnaire Hyper-v.|  
|Envoyer la\+combinaison\+de touches Ctrl Alt Suppr à une machine virtuelle|Sélectionnez **action** > \+\+**CTRLALTSUPPR\+ou utilisez la combinaison de touches Ctrl Alt fin.\+**|  
|Passer d’un mode fenêtre à un mode\-plein écran|Sélectionnez mode**plein écran**. >  Pour revenir en mode fenêtre, appuyez sur CTRL\+Alt\+arrêter.|  
|Créer un point de contrôle pour capturer l’état actuel de l’ordinateur pour la résolution des problèmes|Sélectionnez > **point de contrôle de** l’action ou utilisez\+la combinaison de touches Ctrl N.|  
|Modifier les paramètres de la machine virtuelle|Sélectionnez**paramètres**du **fichier** > .|  
|Se connecter à un fichier \(.\) ISO d’image de DVD \(ou à un fichier. VFD de disquette virtuelle\)|Sélectionnez **média**.<br /><br />Les disquettes virtuelles ne sont pas prises en charge pour les ordinateurs virtuels de génération 2. Pour plus d’informations, consultez [dois-je créer une machine virtuelle de génération 1 ou 2 dans Hyper-V ?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md).|  
|Utiliser les ressources locales d’un ordinateur hôte\-sur une machine virtuelle Hyper-V comme un disque mémoire flash USB|Activez le mode de session étendu sur l’hôte Hyper-V, utilisez VMConnect pour vous connecter à la machine virtuelle, et avant de vous connecter, choisissez la ressource locale que vous souhaitez utiliser. Pour connaître les étapes spécifiques, consultez [utiliser des ressources locales\-sur une machine virtuelle Hyper-V avec vmconnect](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md).|  
|Modifier les paramètres VMConnect enregistrés pour un ordinateur virtuel|Exécutez la commande suivante dans Windows PowerShell ou l’invite de commandes :<br /><br />`VMConnect.exe <ServerName> <VMName> \/edit`|  
|Empêcher un utilisateur VMConnect de prendre le contrôle d’une session VMConnect d’un autre utilisateur|[Activez le mode de session étendu sur l’hôte Hyper-V](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#turn-on-enhanced-session-mode-on-a-hyper-v-host).<br /><br />Le fait de ne pas activer le mode de session étendu peut poser un risque de sécurité et de confidentialité. Si un utilisateur est connecté et connecté à un ordinateur virtuel via VMConnect et qu’un autre utilisateur autorisé se connecte à la même machine virtuelle, la session est prise par le deuxième utilisateur et le premier utilisateur perd la session. Le deuxième utilisateur sera en mesure d’afficher le bureau, les documents et les applications du premier utilisateur.|
|Gérer les services d’intégration ou les composants qui permettent à la machine virtuelle de communiquer avec l’hôte Hyper-V| Sur les ordinateurs hôtes Hyper-V qui exécutent Windows 10 ou Windows Server 2016, vous ne pouvez pas gérer les services d’intégration avec VMConnect. Consultez les rubriques suivantes : <br />- [Activer/désactiver Integration Services à partir de l’hôte Hyper-V](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics) <br />- [Activer/désactiver Integration Services à partir d’une machine virtuelle Windows](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#manage-integration-services-from-guest-os-windows)<br />- [Activer/désactiver Integration Services à partir d’une machine virtuelle Linux](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#manage-integration-services-from-guest-os-linux) <br />- [Conserver les services d’intégration mis à jour pour la machine virtuelle](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#integration-service-maintenance)  <br />Pour les ordinateurs hôtes qui exécutent Windows Server 2012 ou Windows Server 2012 R2, consultez [Integration Services](https://technet.microsoft.com/library/dn798297(v=ws.11).aspx).|
|Redimensionner la fenêtre VMConnect|Vous pouvez modifier la taille de la fenêtre VMConnect pour les ordinateurs virtuels de génération 2 qui exécutent un système d’exploitation Windows. Pour ce faire, vous devrez peut-être activer le mode de session étendu sur l’hôte Hyper-V. Pour plus d’informations, consultez [activer le mode de session étendu sur l’hôte Hyper-V](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#turn-on-enhanced-session-mode-on-a-hyper-v-host). Pour les machines virtuelles qui exécutent Ubuntu, consultez [modification de la résolution d’écran Ubuntu sur une machine virtuelle Hyper-V](https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/09/19/changing-ubuntu-screen-resolution-in-a-hyper-v-vm/).|


## <a name="keyboard-shortcuts"></a>Raccourcis clavier  
Par défaut, l’entrée au clavier et les clics de souris sont envoyés à la machine virtuelle. Par conséquent, vous devrez peut-être appuyer sur CTRL + ALT + flèche gauche avant d’utiliser les touches de raccourci suivantes. 

|Combinaison de touches|Description|  
|-------------------|---------------|  
|Ctrl\+Alt\+gauche|Version de la souris|  
|CTRL\+ALT\+FIN|Équivalent de Ctrl\+Alt\+SUPPR dans l’ordinateur virtuel|  
|CTRL\+ALT\+ARRÊTER|Passer du mode\-plein écran au mode fenêtre|  
|CTRL\+O|Ouvre les paramètres de la machine virtuelle.|  
|CTRL\+S|Démarre la machine virtuelle|  
|CTRL\+N|Créer un point de contrôle|  
|CTRL\+E|Revenir à un point de contrôle|  
|CTRL\+C|Effectuer une capture d’écran|  

## <a name="see-also"></a>Voir aussi  
-   [Utiliser des ressources locales sur un ordinateur virtuel Hyper-V avec VMConnect](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)  
-   [Hyper-V sur Windows Server 2016](../Hyper-V-on-Windows-Server.md)  
-   [Hyper-V sur Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/windows_welcome)  
  
  
