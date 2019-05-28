---
title: Créer un commutateur virtuel pour les ordinateurs virtuels Hyper-V
description: Fournit des instructions sur la création d’un commutateur virtuel à l’aide du Gestionnaire Hyper-V ou Windows PowerShell
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: fdc8063c-47ce-4448-b445-d7ff9894dc17
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 2668f9fa21c8efbad455d82c7e110ff89b729187
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222870"
---
# <a name="create-a-virtual-switch-for-hyper-v-virtual-machines"></a>Créer un commutateur virtuel pour les ordinateurs virtuels Hyper-V

>S'applique à : Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019
  
Un commutateur virtuel permet de machines virtuelles créées sur les ordinateurs hôtes Hyper-V pour communiquer avec d’autres ordinateurs. Vous pouvez créer un commutateur virtuel lorsque vous installez le rôle Hyper-V sur Windows Server. Pour créer des commutateurs virtuels supplémentaires, utilisez le Gestionnaire Hyper-V ou Windows PowerShell. Pour en savoir plus sur les commutateurs virtuels, consultez [commutateur virtuel Hyper-V](../../hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).  
  
Mise en réseau de machine virtuelle peut être un sujet complexe. Et il existe plusieurs nouvelles fonctionnalités de commutateur virtuel que vous pouvez utiliser comme [SET Switch Embedded Teaming ()](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md#switch-embedded-teaming-set). Mais la mise en réseau de base est assez facile à faire. Cette rubrique couvre juste assez pour que vous pouvez créer des machines virtuelles en réseau dans Hyper-V. Pour en savoir plus sur la façon dont vous pouvez configurer votre infrastructure réseau, passez en revue la [réseau](../../../networking/Networking.md) documentation.   
  
## <a name="create-a-virtual-switch-by-using-hyper-v-manager"></a>Créer un commutateur virtuel à l’aide du Gestionnaire Hyper-V  
  
1.  Ouvrez le Gestionnaire Hyper-V, sélectionnez le nom d’ordinateur hôte Hyper-V.  
  
2.  Sélectionnez **Action** > **Gestionnaire de commutateur virtuel**.  
  
    ![Capture d’écran montrant l’option de menu Action > Gestionnaire de commutateur virtuel](../media/Hyper-V-Action-VSwitchManager.png)  
  
3.  Choisissez le type de commutateur virtuel souhaité.  
  
    |Type de connexion|Description|  
    |-------------------|---------------|  
    |Externe|Donne accès aux machines virtuelles à un réseau physique pour communiquer avec les serveurs et clients sur un réseau externe. Aux ordinateurs virtuels sur le même serveur Hyper-V pour communiquer entre eux.|  
    |Interne|Permet la communication entre les machines virtuelles sur le même serveur Hyper-V et entre les machines virtuelles et du système d’exploitation de gestion.|  
    |Private|Autorise uniquement la communication entre les machines virtuelles sur le même serveur Hyper-V. Un réseau privé est isolé de tout le trafic réseau externe sur le serveur Hyper-V. Ce type de réseau est utile lorsque vous devez créer un environnement isolé du réseau, comme un domaine de test isolée.|  
  
4.  Sélectionnez **créer un commutateur virtuel**.  
  
5.  Ajouter un nom pour le commutateur virtuel.  
  
6.  Si vous sélectionnez externe, choisissez la carte réseau (NIC) que vous souhaitez utiliser et autres options décrites dans le tableau suivant.  
  
    ![Capture d’écran montrant les options de réseau externe](../media/Hyper-V-NewVSwitch-ExternalOptions.png)  
  
    |Nom du paramètre|Description|  
    |----------------|---------------|  
    |Autoriser le système d’exploitation de gestion à partager cette carte réseau|Sélectionnez cette option si vous souhaitez permettre à l’hôte Hyper-V partager l’utilisation du commutateur virtuel et la carte réseau ou carte réseau de l’équipe avec la machine virtuelle. Avec cette option est activée, l’hôte peut utiliser les paramètres que vous configurez pour le commutateur virtuel tels que les paramètres de qualité de Service (QoS), paramètres de sécurité ou d’autres fonctionnalités du commutateur virtuel Hyper-V.|  
    |Activer la virtualisation d’e/s de racine unique (SR-IOV)|Sélectionnez cette option uniquement si vous souhaitez autoriser le trafic de machine virtuelle contourner le commutateur de machine virtuelle et accédez directement à la carte physique. Pour plus d’informations, consultez [Single-Root i/o Virtualization](https://technet.microsoft.com/library/dn641211.aspx#Sec4) dans la référence d’accompagnement affiche : Mise en réseau Hyper-V.|  
  
7.  Si vous souhaitez isole le trafic réseau à partir du système d’exploitation de gestion Hyper-V ou d’autres machines virtuelles qui partagent le même commutateur virtuel, sélectionnez **activer l’identification LAN pour système d’exploitation de gestion**. Vous pouvez modifier l’ID de VLAN à n’importe quel nombre ou laissez la valeur par défaut. Il s’agit de numéro d’identification réseau local virtuel que le système d’exploitation de gestion utilisera pour toutes les communications réseau via ce commutateur virtuel.  
  
    ![Capture d’écran montrant les options de l’ID de VLAN](../media/Hyper-V-NewSwitch-VLAN.png)  
  
8.  Cliquez sur **OK**.  
  
9. Cliquez sur **Oui**.  
  
    ![Capture d’écran montrant le message « Modifications en attente risquent de perturber la connectivité réseau »](../media/Hyper-V-NewVSwitch-DisruptNetwork.png)  
  
## <a name="create-a-virtual-switch-by-using-windows-powershell"></a>Créer un commutateur virtuel à l’aide de Windows PowerShell  
  
1.  Sur le Bureau Windows, cliquez sur le bouton Démarrer et tapez une partie du nom **Windows PowerShell**.  
  
2.  Avec le bouton droit de Windows PowerShell et sélectionnez **exécuter en tant qu’administrateur**.  
  
3.  Rechercher des cartes réseau existantes en exécutant la [Get-NetAdapter](https://technet.microsoft.com/library/jj130867.aspx) applet de commande. Prenez note du nom de carte réseau que vous souhaitez utiliser pour le commutateur virtuel.  
  
    ```  
    Get-NetAdapter  
    ```  
  
4.  Créer un commutateur virtuel à l’aide de la [New-VMSwitch](https://technet.microsoft.com/library/hh848455.aspx) applet de commande. Par exemple, pour créer un commutateur virtuel externe nommé ExternalSwitch, à l’aide de la carte réseau ethernet et avec **autoriser le système d’exploitation de gestion à partager cette carte réseau** activé, exécutez la commande suivante.  
  
    ```  
    New-VMSwitch -name ExternalSwitch  -NetAdapterName Ethernet -AllowManagementOS $true  
    ```  
  
    Pour créer un commutateur interne, exécutez la commande suivante.  
  
    ```  
    New-VMSwitch -name InternalSwitch -SwitchType Internal  
    ```  
  
    Pour créer un commutateur privé, exécutez la commande suivante.  
  
    ```  
    New-VMSwitch -name PrivateSwitch -SwitchType Private  
    ```  
  
Pour les scripts Windows PowerShell plus avancées qui couvrent les fonctionnalités de commutateur virtuel nouveaux ou améliorés dans Windows Server 2016, consultez [Remote Direct Memory Access et basculez Embedded Teaming](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  

  
## <a name="next-step"></a>Étape suivante  
[Créer une machine virtuelle avec Hyper-V](Create-a-virtual-machine-in-Hyper-V.md)  
  


