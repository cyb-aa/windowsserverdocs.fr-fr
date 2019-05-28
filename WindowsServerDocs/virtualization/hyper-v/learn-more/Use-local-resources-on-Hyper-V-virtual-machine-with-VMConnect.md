---
title: Utiliser des ressources locales sur un ordinateur virtuel Hyper-V avec VMConnect
description: Décrit la configuration requise pour l’utilisation des ressources locales avec VMConnect
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 18eface5-7518-4c6b-9282-93e2e3e87492
author: KBDAzure
ms.author: kathyDav
ms.date: 12/06/2016
ms.openlocfilehash: a7e465313c68ee793715aba045cc56a2ca5fd1de
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222839"
---
# <a name="use-local-resources-on-hyper-v-virtual-machine-with-vmconnect"></a>Utiliser des ressources locales sur un ordinateur virtuel Hyper-V avec VMConnect

>S'applique à : Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2

Connexion à une Machine virtuelle (VMConnect) vous permet d’utiliser des ressources locales d’un ordinateur dans une machine virtuelle, comme un amovible lecteur flash USB ou une imprimante. Mode de session étendu vous permet également de redimensionner la fenêtre VMConnect. Cet article vous montre comment configurer l’hôte et ensuite donner l’accès de machine virtuelle à une ressource locale.

Mode de session étendu et tapez le texte du Presse-papiers sont disponibles uniquement pour les machines virtuelles qui exécutent les systèmes d’exploitation Windows récents. \(Consultez [conditions d’utilisation des ressources locales](#requirements-for-using-local-resources), ci-dessous.\) 

Pour les machines virtuelles qui s’exécutent Ubuntu, consultez [modification Ubuntu résolution de l’écran dans une machine virtuelle Hyper-V](https://blogs.msdn.microsoft.com/virtual_pc_guy/2014/09/19/changing-ubuntu-screen-resolution-in-a-hyper-v-vm/). 
  
## <a name="turn-on-enhanced-session-mode-on-a-hyper-v-host"></a>Activer le mode de session étendu sur un ordinateur hôte Hyper-V  
Si votre hôte Hyper-V exécute Windows 10 ou Windows 8.1, mode de session étendu est activée par défaut, vous pouvez donc ignorer cette étape et passer à la section suivante. Mais si votre hôte exécute Windows Server 2016 ou Windows Server 2012 R2, à faire en premier. 
  
Activer le mode de session étendu :

1.  Connectez-vous à l'ordinateur qui héberge l'ordinateur virtuel.  
  
2.  Dans le Gestionnaire Hyper-V, sélectionnez le nom de l’ordinateur de l’hôte.  
  
    ![Capture d’écran montrant un hôte de nom de l’ordinateur est répertorié sous le Gestionnaire Hyper-V dans le volet gauche.](media/Hyper-V-HyperVManager-HostNameSelected.png)  
  
3.  Sélectionnez **Paramètres Hyper-V**.  
  
    ![Capture d’écran montrant l’option des paramètres Hyper-V sous Actions dans le volet droit.](media/HyperV-ActionsHyperVSettings.png)  
  
4.  Sous **Serveur**, sélectionnez **Stratégie de mode de session étendu**.  
  
    ![Capture d’écran montrant l’option de stratégie de mode de session étendu dans la section sécurité.](media/Hyper-V-Settings-ServerEnhancedSessionModePolicy.png)  
  
5.  Cochez la case **Autoriser le mode de session étendu** .  
  
    ![Capture d’écran montrant l’autoriser amélioré la case à cocher du mode de session pour la stratégie de mode de session étendu.](media/Hyper-V-Settings-EnhancedSessionModePolicyCheckBox.png)  
  
6.  Sous **Utilisateur**, sélectionnez **Mode de session étendu**.  
  
    ![Capture d’écran montrant l’option de mode de session étendu dans la section de l’utilisateur. ](media/Hyper-V-Settings-UserEnhancedSessionMode.png)  
  
7.  Cochez la case **Autoriser le mode de session étendu** .  
  
8.  Cliquez sur **OK**.  
  
## <a name="choose-a-local-resource"></a>Choisissez une ressource locale

Les ressources locales incluent des imprimantes, le Presse-papiers et un lecteur local sur l’ordinateur où vous exécutez VMConnect. Pour plus d’informations, consultez [conditions d’utilisation des ressources locales](#requirements-for-using-local-resources), ci-dessous.  
  
Pour choisir une ressource locale :
  
1.  Ouvrez VMConnect.  
  
2.  Sélectionnez l'ordinateur virtuel auquel vous souhaitez vous connecter.  
  
3.  Cliquez sur **Afficher les options**.  
  
    ![Capture d’écran qui appelle afficher les options dans la coin inférieur gauche de la boîte de dialogue.](media/HyperV-VMConnect-DisplayConfig.png)  
  
4.  Sélectionnez **Ressources locales**.  
  
    ![Capture d’écran qui appelle l’onglet ressources locales.](media/HyperV-VMConnect-DisplayConfig-LocalResources.png)  
  
5.  Cliquez sur **Plus**.  
  
    ![Capture d’écran qui appelle le bouton plus.](media/HyperV-VMConnect-DisplayConfig-LocalResourcesMore.png)  
  
6.  Sélectionnez le lecteur que vous souhaitez utiliser sur l'ordinateur virtuel, puis cliquez sur **Ok**.  
  
    ![Capture d’écran qui affiche les ressources locales et les lecteurs que vous pouvez sélectionner.](media/HyperV-VMConnect-Settings-LocalResourcesDrives.png)  
  
7.  Sélectionnez **Enregistrer mes paramètres pour les futures connexions à cet ordinateur virtuel**.  
  
    ![Capture d’écran qui appelle la case à cocher pour sélectionner cette option.](media/HyperV-VMConnect-SaveSettings.png)  
  
8.  Cliquer sur **Se connecter**.  
  
## <a name="edit-vmconnect-settings"></a>Modifier les paramètres VMConnect

Vous pouvez facilement modifier vos paramètres de connexion pour VMConnect en exécutant la commande suivante dans Windows PowerShell ou l'invite de commandes :  
  
`VMConnect.exe <ServerName> <VMName> /edit`  
  
## <a name="requirements-for-using-local-resources"></a>Configuration requise pour utiliser les ressources locales

Pour être en mesure d’utiliser des ressources locales d’un ordinateur sur une machine virtuelle :  
  
-   L’hôte Hyper-V doit être **stratégie de mode de session étendu** et **mode de session étendu** paramètres activés.  
  
-   L’ordinateur sur lequel vous utilisez VMConnect doit exécuter Windows 10, Windows 8.1, Windows Server 2016 ou Windows Server 2012 R2.  
  
-   La machine virtuelle doit avoir des Services Bureau à distance activé et exécutez Windows 10, Windows 8.1, Windows Server 2016 ou Windows Server 2012 R2 comme système d’exploitation invité.  
  
Si l’ordinateur exécutant VMConnect et la machine virtuelle à la fois la configuration requise, vous pouvez utiliser toutes les ressources locales suivantes si elles sont disponibles :  
  
-   Configuration de l'affichage  
  
-   Audio
  
-   Imprimantes  
  
-   Presse-papiers pour copier et coller  
  
-   Cartes à puce  
  
-   Périphériques USB  
  
-   Lecteurs  
  
-   Périphériques Plug-and-Play pris en charge  
  
## <a name="why-use-a-computers-local-resources"></a>Pourquoi utiliser des ressources locales d’un ordinateur ?
Il se peut que vous souhaitiez utiliser les ressources locales d’un ordinateur à :  
  
-   résoudre les problèmes d'un ordinateur virtuel sans connexion réseau à l'ordinateur virtuel ;  
  
-   copier et coller des fichiers vers et depuis l'ordinateur virtuel de la même manière que vous copiez et collez du contenu à l'aide d'une connexion Bureau à distance ;  
  
-   établir une connexion à l'ordinateur virtuel à l'aide d'une carte à puce ;  
  
-   imprimer sur une imprimante locale à partir d'un ordinateur virtuel ;  
  
-   tester et dépanner des applications de développeur qui nécessitent une redirection du son et USB sans utiliser de connexion Bureau à distance ;  
  
## <a name="see-also"></a>Voir aussi  
[Se connecter à une Machine virtuelle](https://technet.microsoft.com/library/cc742407.aspx)  
[Dois-je créer une machine virtuelle de génération 1 ou 2 dans Hyper-V ?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)



