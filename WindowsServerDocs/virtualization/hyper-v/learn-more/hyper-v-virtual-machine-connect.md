---
title: Connexion à une machine virtuelle Hyper-V
description: Décrit Connexion à une machine virtuelle, qui fournit l’accès à distance à une machine virtuelle. Contient des informations détaillées sur la façon d’effectuer des tâches courantes, comme envoyer Ctrl+Alt+Suppr à la machine virtuelle.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: deae35b9-7647-42b8-b6bf-45645a44c9c4
author: kbdazure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 8de3fe607eb9dc0d140fe9f494991cb917b8994f
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80854012"
---
# <a name="hyper-v-virtual-machine-connection"></a>Connexion à une machine virtuelle Hyper-V

>S'applique à : Windows Server 2016, Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2012, Windows 8

\(VMConnect\) de Connexion à une machine virtuelle est un outil qui permet de se connecter à une machine virtuelle pour pouvoir installer ou interagir avec le système d’exploitation invité dans une machine virtuelle. Voici quelques-unes des tâches que vous pouvez effectuer avec VMConnect :  
  
-   Démarrer et arrêter une machine virtuelle  
  
-   Se connecter à une image DVD \(fichier .iso\) ou à un lecteur flash USB  
  
-   Créer un point de contrôle  
  
-   modifier les paramètres d'un ordinateur virtuel  
    
## <a name="tips-for-using-vmconnect"></a>Conseils pour l’utilisation de VMConnect  
Les informations suivantes peuvent vous être utiles pour utiliser VMConnect :  
  
|Pour faire ceci...|Procédez comme suit...|  
|---------------|------------|  
|Envoyer des clics de souris ou une entrée au clavier à la machine virtuelle|Cliquez n’importe où dans la fenêtre de la machine virtuelle. Le pointeur de la souris peut apparaître sous la forme d’un petit point quand vous vous connectez à une machine virtuelle en cours d’exécution.|  
|Retourner des clics de souris ou une entrée au clavier à l’ordinateur physique|Appuyez sur Ctrl\+Alt\+Flèche gauche, puis déplacez le pointeur de la souris en dehors de la fenêtre de la machine virtuelle. Vous pouvez changer cette combinaison de touches de déclenchement de la souris dans les paramètres d’Hyper\-V, dans le Gestionnaire Hyper\-V.|  
|Envoyer la combinaison de touches Ctrl\+Alt\+Suppr à une machine virtuelle|Sélectionnez **Action** > **Ctrl\+Alt\+Suppr** ou utilisez la combinaison de touches Ctrl\+Alt\+Fin.|  
|Passer d’un mode fenêtre à un mode Plein écran complet|Sélectionnez **Afficher** > **Mode Plein écran**. Pour revenir en mode fenêtre, appuyez sur Ctrl\+Alt\+Attn.|  
|Créer un point de contrôle pour capturer l’état actuel de la machine pour la résolution des problèmes|Sélectionnez **Action** > **Point de contrôle** ou utilisez la combinaison de touches Ctrl\+N.|  
|Modifier les paramètres de la machine virtuelle|Sélectionnez **Fichier** > **Paramètres**.|  
|Connectez-vous à une image DVD \(fichier .iso\) ou à une disquette virtuelle \(fichier .vfd\)|Sélectionnez **Média**.<p>Les disquettes virtuelles ne sont pas prises en charge pour les machines virtuelles de génération 2. Pour plus d’informations, consultez [Dois-je créer une machine virtuelle de génération 1 ou 2 dans Hyper-V ?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md).|  
|Utiliser les ressources locales d’un hôte sur une machine virtuelle Hyper\-V comme un lecteur flash USB|Activez le mode de session étendu sur l’hôte Hyper-V, utilisez VMConnect pour vous connecter à la machine virtuelle et, avant de vous connecter, choisissez la ressource locale à utiliser. Pour obtenir les étapes spécifiques, consultez [Utiliser des ressources locales sur une machine virtuelle Hyper\-V avec VMConnect](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md).|  
|Changer les paramètres de VMConnect enregistrés pour une machine virtuelle|Exécutez la commande suivante dans Windows PowerShell ou sur l’invite de commandes :<p>`VMConnect.exe <ServerName> <VMName> /edit`|  
|Empêcher un utilisateur VMConnect de prendre le contrôle d’une session VMConnect d’un autre utilisateur|[Activer le mode de session étendu sur l’hôte Hyper-V](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#turn-on-enhanced-session-mode-on-a-hyper-v-host).<p>Le fait de ne pas activer le mode de session étendu peut poser un risque de sécurité et de confidentialité. Si un utilisateur est connecté et a ouvert une session sur une machine virtuelle via VMConnect, et qu’un autre utilisateur autorisé se connecte à la même machine virtuelle, la session est prise par le deuxième utilisateur, le premier utilisateur perdant la session. Le deuxième utilisateur sera en mesure de voir le Bureau, les documents et les applications du premier utilisateur.|
|Gérer les services ou les composants d’intégration qui permettent à la machine virtuelle de communiquer avec l’hôte Hyper-V| Sur les hôtes Hyper-V qui exécutent Windows 10 ou Windows Server 2016, vous ne pouvez pas gérer les services d’intégration avec VMConnect. Consultez ces rubriques : <br />- [Activer ou désactiver les services d’intégration à partir de l’hôte Hyper-V](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics) <br />- [Activer/désactiver les services d’intégration à partir d’une machine virtuelle Windows](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#manage-integration-services-from-guest-os-windows)<br />- [Activer/désactiver les services d’intégration à partir d’une machine virtuelle Linux](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#manage-integration-services-from-guest-os-linux) <br />- [Conserver les services d’intégration à jour pour la machine virtuelle](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/managing_ics#integration-service-maintenance)  <br />Pour les hôtes qui exécutent Windows Server 2012 ou Windows Server 2012 R2, consultez [Services d’intégration](https://technet.microsoft.com/library/dn798297(v=ws.11).aspx).|
|Redimensionner la fenêtre de VMConnect|Vous pouvez changer la taille de la fenêtre de VMConnect pour les machines virtuelles de génération 2 qui exécutent un système d’exploitation Windows. Pour cela, il peut être nécessaire d’activer le mode de session étendu sur l’hôte Hyper-V. Pour plus d’informations, consultez [Activer le mode de session étendu sur l’hôte Hyper-V](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md#turn-on-enhanced-session-mode-on-a-hyper-v-host). Pour les machines virtuelles qui exécutent Ubuntu, consultez [Modification de la résolution d’écran d’Ubuntu dans une machine virtuelle Hyper-V](https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/09/19/changing-ubuntu-screen-resolution-in-a-hyper-v-vm/).|


## <a name="keyboard-shortcuts"></a>Raccourcis clavier  
Par défaut, l’entrée au clavier et les clics de souris sont envoyés à la machine virtuelle. Par conséquent, il peut être nécessaire d’appuyer sur Ctrl+Alt+Flèche gauche avant d’utiliser les touches de raccourci suivantes. 

|Combinaison de touches|Description|  
|-------------------|---------------|  
|Ctrl\+Alt\+Flèche gauche|Relâchement de la souris|  
|Ctrl\+Alt\+Fin|Équivalent de Ctrl\+Alt\+Suppr dans la machine virtuelle|  
|Ctrl\+Alt\+Attn|Repasser du mode Plein écran au mode Fenêtre|  
|Ctrl\+O|Ouvre les paramètres pour la machine virtuelle|  
|Ctrl\+S|Démarre la machine virtuelle|  
|Ctrl\+N|Créer un point de contrôle|  
|Ctrl\+E|Revenir à un point de contrôle|  
|Ctrl\+C|Faire une capture d’écran|  

## <a name="see-also"></a>Voir aussi  
-   [Utiliser des ressources locales sur une machine virtuelle Hyper-V avec VMConnect](Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)  
-   [Hyper-V sur Windows Server 2016](../Hyper-V-on-Windows-Server.md)  
-   [Hyper-V sur Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/windows_welcome)  
  
  
