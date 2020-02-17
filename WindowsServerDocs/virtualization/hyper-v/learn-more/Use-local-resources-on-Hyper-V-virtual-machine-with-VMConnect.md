---
title: Utiliser des ressources locales sur un ordinateur virtuel Hyper-V avec VMConnect
description: Décrit les exigences pour l’utilisation de ressources locales avec VMConnect
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 18eface5-7518-4c6b-9282-93e2e3e87492
author: KBDAzure
ms.author: kathyDav
ms.date: 12/06/2016
ms.openlocfilehash: 70bf72ec2277679820d985c9f78f10a4ea6e04df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392888"
---
# <a name="use-local-resources-on-hyper-v-virtual-machine-with-vmconnect"></a>Utiliser des ressources locales sur un ordinateur virtuel Hyper-V avec VMConnect

>S'applique à : Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2

Connexion à une machine virtuelle (VMConnect) vous permet d’utiliser les ressources locales d’un ordinateur dans une machine virtuelle, comme un lecteur flash USB amovible ou une imprimante. Le mode de session étendu vous permet également de redimensionner la fenêtre de VMConnect. Cet article vous montre comment configurer l’hôte, puis donner à la machine virtuelle l’accès à une ressource locale.

Le mode de session étendu et Taper le texte du Presse-papiers sont disponibles seulement pour les machines virtuelles qui exécutent des systèmes d’exploitation Windows récents. \(Consultez [Exigences pour l’utilisation des ressources locales](#requirements-for-using-local-resources) ci-dessous.\) 

Pour les machines virtuelles qui exécutent Ubuntu, consultez [Modification de la résolution d’écran d’Ubuntu dans une machine virtuelle Hyper-V](https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/09/19/changing-ubuntu-screen-resolution-in-a-hyper-v-vm/). 
  
## <a name="turn-on-enhanced-session-mode-on-a-hyper-v-host"></a>Activer le mode de session étendu sur un hôte Hyper-V  
Si votre hôte Hyper-V exécute Windows 10 ou Windows 8.1, le mode de session étendu est activé par défaut : vous pouvez donc ignorer cette étape et passer à la section suivante. Cependant, si votre hôte exécute Windows Server 2016 ou Windows Server 2012 R2, faites d’abord ceci. 
  
Activez le mode de session étendu :

1.  Connectez-vous à l'ordinateur qui héberge l'ordinateur virtuel.  
  
2.  Dans le Gestionnaire Hyper-V, sélectionnez le nom d’ordinateur de l’hôte.  
  
    ![Capture d’écran montrant un nom d’ordinateur d’hôte listé sous Gestionnaire Hyper-V dans le volet gauche.](media/Hyper-V-HyperVManager-HostNameSelected.png)  
  
3.  Sélectionnez **Paramètres Hyper-V**.  
  
    ![Capture d’écran montrant l’option Paramètres d’Hyper-V sous Actions dans le volet droit.](media/HyperV-ActionsHyperVSettings.png)  
  
4.  Sous **Serveur**, sélectionnez **Stratégie de mode de session étendu**.  
  
    ![Capture d’écran montrant l’option de stratégie Mode de session étendu sous la section Sécurité.](media/Hyper-V-Settings-ServerEnhancedSessionModePolicy.png)  
  
5.  Cochez la case **Autoriser le mode de session étendu** .  
  
    ![Capture d’écran montrant la case à cocher Autoriser le mode de session étendu pour la stratégie Mode de session étendu.](media/Hyper-V-Settings-EnhancedSessionModePolicyCheckBox.png)  
  
6.  Sous **Utilisateur**, sélectionnez **Mode de session étendu**.  
  
    ![Capture d’écran montrant l’option Mode de session étendu sous la section Utilisateur. ](media/Hyper-V-Settings-UserEnhancedSessionMode.png)  
  
7.  Cochez la case **Autoriser le mode de session étendu** .  
  
8.  Cliquez sur **OK**.  
  
## <a name="choose-a-local-resource"></a>Choisir une ressource locale

Les ressources locales incluent les imprimantes, le Presse-papiers et un lecteur local sur l’ordinateur où vous exécutez VMConnect. Pour plus d’informations, consultez [Exigences pour l’utilisation des ressources locales](#requirements-for-using-local-resources) ci-dessous.  
  
Pour choisir une ressource locale :
  
1.  Ouvrez VMConnect.  
  
2.  Sélectionnez l'ordinateur virtuel auquel vous souhaitez vous connecter.  
  
3.  Cliquez sur **Afficher les options**.  
  
    ![Capture d’écran montrant Afficher les options en bas à gauche de la boîte de dialogue.](media/HyperV-VMConnect-DisplayConfig.png)  
  
4.  Sélectionnez **Ressources locales**.  
  
    ![Capture d’écran montrant l’onglet Ressources locales.](media/HyperV-VMConnect-DisplayConfig-LocalResources.png)  
  
5.  Cliquez sur **Plus**.  
  
    ![Capture d’écran montrant le bouton Plus.](media/HyperV-VMConnect-DisplayConfig-LocalResourcesMore.png)  
  
6.  Sélectionnez le lecteur que vous souhaitez utiliser sur l'ordinateur virtuel, puis cliquez sur **Ok**.  
  
    ![Capture d’écran montrant les ressources locales et les lecteurs que vous pouvez sélectionner.](media/HyperV-VMConnect-Settings-LocalResourcesDrives.png)  
  
7.  Sélectionnez **Enregistrer mes paramètres pour les futures connexions à cet ordinateur virtuel**.  
  
    ![Capture d’écran montrant la case à cocher pour sélectionner cette option.](media/HyperV-VMConnect-SaveSettings.png)  
  
8.  Cliquez sur **Connexion**.  
  
## <a name="edit-vmconnect-settings"></a>Modifier les paramètres VMConnect

Vous pouvez facilement modifier vos paramètres de connexion pour VMConnect en exécutant la commande suivante dans Windows PowerShell ou l'invite de commandes :  
  
`VMConnect.exe <ServerName> <VMName> /edit`  
  
## <a name="requirements-for-using-local-resources"></a>Exigences pour l’utilisation des ressources locales

Pour pouvoir utiliser les ressources locales d’un ordinateur sur une machine virtuelle :  
  
-   Les paramètres **Stratégie de mode de session étendu** et **Mode de session étendu** de l’hôte Hyper-V doivent être activés.  
  
-   L’ordinateur sur lequel vous utilisez VMConnect doit exécuter Windows 10, Windows 8.1, Windows Server 2016 ou Windows Server 2012 R2.  
  
-   Les services Bureau à distance doivent être activés sur la machine virtuelle, qui doit exécuter Windows 10, Windows 8.1, Windows Server 2016 ou Windows Server 2012 R2 comme système d’exploitation invité.  
  
Si l’ordinateur qui exécute VMConnect et la machine virtuelle remplissent les conditions, vous pouvez utiliser les ressources locales suivantes si elles sont disponibles :  
  
-   Configuration de l'affichage  
  
-   Audio
  
-   Imprimantes  
  
-   Presse-papiers pour copier et coller  
  
-   Cartes à puce  
  
-   Périphériques USB  
  
-   Lecteurs  
  
-   Périphériques Plug-and-Play pris en charge  
  
## <a name="why-use-a-computers-local-resources"></a>Pourquoi utiliser les ressources locales d’un ordinateur ?
Vous pouvez utiliser les ressources locales d’un ordinateur pour :  
  
-   résoudre les problèmes d'un ordinateur virtuel sans connexion réseau à l'ordinateur virtuel ;  
  
-   copier et coller des fichiers vers et depuis l'ordinateur virtuel de la même manière que vous copiez et collez du contenu à l'aide d'une connexion Bureau à distance ;  
  
-   établir une connexion à l'ordinateur virtuel à l'aide d'une carte à puce ;  
  
-   imprimer sur une imprimante locale à partir d'un ordinateur virtuel ;  
  
-   tester et dépanner des applications de développeur qui nécessitent une redirection du son et USB sans utiliser de connexion Bureau à distance ;  
  
## <a name="see-also"></a>Voir aussi  
[Se connecter à une machine virtuelle](https://technet.microsoft.com/library/cc742407.aspx)  
[Dois-je créer une machine virtuelle de génération 1 ou 2 dans Hyper-V ?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)



