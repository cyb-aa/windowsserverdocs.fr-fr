---
title: Créer un commutateur virtuel pour les machines virtuelles Hyper-V
description: Fournit des instructions sur la création d’un commutateur virtuel à l’aide du Gestionnaire Hyper-V ou de Windows PowerShell.
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
ms.openlocfilehash: 3c0ba19183dd68a86d995293f663accf10e91df9
ms.sourcegitcommit: 0467b8e69de66e3184a42440dd55cccca584ba95
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69546387"
---
# <a name="create-a-virtual-switch-for-hyper-v-virtual-machines"></a>Créer un commutateur virtuel pour les machines virtuelles Hyper-V

>S'applique à : Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019
  
Un commutateur virtuel permet aux ordinateurs virtuels créés sur des hôtes Hyper-V de communiquer avec d’autres ordinateurs. Vous pouvez créer un commutateur virtuel lorsque vous installez pour la première fois le rôle Hyper-V sur Windows Server. Pour créer des commutateurs virtuels supplémentaires, utilisez le Gestionnaire Hyper-V ou Windows PowerShell. Pour en savoir plus sur les commutateurs virtuels, consultez [commutateur virtuel Hyper-V](../../hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).  
  
La mise en réseau des machines virtuelles peut être un sujet complexe. Et il existe plusieurs nouvelles fonctionnalités de commutateur virtuel que vous pouvez utiliser comme [switch Embedded Teaming (Set)](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md#switch-embedded-teaming-set). Mais la mise en réseau de base est relativement simple. Cette rubrique couvre juste assez pour vous permettre de créer des machines virtuelles en réseau dans Hyper-V. Pour en savoir plus sur la façon dont vous pouvez configurer votre infrastructure réseau, consultez la documentation [réseau](../../../networking/Networking.md) .   
  
## <a name="create-a-virtual-switch-by-using-hyper-v-manager"></a>Créer un commutateur virtuel à l’aide du Gestionnaire Hyper-V  
  
1.  Ouvrez le Gestionnaire Hyper-V, puis sélectionnez le nom de l’ordinateur hôte Hyper-V.  
  
2.  Sélectionnez **action** > **Gestionnaire de commutateur virtuel**.  
  
    ![Capture d’écran qui montre l’option de menu Action > Gestionnaire de commutateur virtuel](../media/Hyper-V-Action-VSwitchManager.png)  
  
3.  Choisissez le type de commutateur virtuel de votre choix.  
  
    |Type de connexion|Description|  
    |-------------------|---------------|  
    |Ressource externe|Permet aux machines virtuelles d’accéder à un réseau physique pour communiquer avec les serveurs et les clients sur un réseau externe. Permet aux ordinateurs virtuels sur le même serveur Hyper-V de communiquer entre eux.|  
    |Interne|Autorise la communication entre les ordinateurs virtuels sur le même serveur Hyper-V et entre les machines virtuelles et le système d’exploitation hôte de gestion.|  
    |Private|Autorise uniquement la communication entre les ordinateurs virtuels sur le même serveur Hyper-V. Un réseau privé est isolé de tout le trafic réseau externe sur le serveur Hyper-V. Ce type de réseau est utile lorsque vous devez créer un environnement de mise en réseau isolé, comme un domaine de test isolé.|  
  
4.  Sélectionnez **créer un commutateur virtuel**.  
  
5.  Ajoutez un nom pour le commutateur virtuel.  
  
6.  Si vous sélectionnez externe, choisissez la carte réseau (NIC) que vous souhaitez utiliser et toute autre option décrite dans le tableau suivant.  
  
    ![Capture d’écran montrant les options de réseau externe](../media/Hyper-V-NewVSwitch-ExternalOptions.png)  
  
    |Nom du paramètre|Description|  
    |----------------|---------------|  
    |Autoriser le système d’exploitation de gestion à partager cette carte réseau|Sélectionnez cette option si vous souhaitez autoriser l’hôte Hyper-V à partager l’utilisation du commutateur virtuel et de la carte réseau ou de l’Association de cartes réseau avec l’ordinateur virtuel. Lorsque cette option est activée, l’ordinateur hôte peut utiliser l’un des paramètres que vous configurez pour le commutateur virtuel, tels que les paramètres de qualité de service (QoS), les paramètres de sécurité ou d’autres fonctionnalités du commutateur virtuel Hyper-V.|  
    |Activer la virtualisation d’e/s d’une racine unique (SR-IOV)|Sélectionnez cette option uniquement si vous souhaitez autoriser le trafic des ordinateurs virtuels à contourner le commutateur d’ordinateur virtuel et à accéder directement à la carte réseau physique. Pour plus d’informations, consultez [virtualisation d’e/s d’une racine unique](https://technet.microsoft.com/library/dn641211.aspx#Sec4) dans le Guide de référence de l’affiche: Mise en réseau Hyper-V.|  
  
7.  Si vous souhaitez isoler le trafic réseau du système d’exploitation de l’hôte Hyper-V de gestion ou d’autres ordinateurs virtuels qui partagent le même commutateur virtuel, sélectionnez **activer l’identification du réseau local virtuel pour le système d’exploitation de gestion**. Vous pouvez remplacer l’ID de réseau local virtuel par n’importe quel nombre ou conserver la valeur par défaut. Il s’agit du numéro d’identification du réseau local virtuel qui sera utilisé par le système d’exploitation de gestion pour toutes les communications réseau via ce commutateur virtuel.  
  
    ![Capture d’écran montrant les options de l’ID de réseau local virtuel](../media/Hyper-V-NewSwitch-VLAN.png)  
  
8.  Cliquez sur **OK**.  
  
9. Cliquez sur **Oui**.  
  
    ![Capture d’écran montrant le message «les modifications en attente peuvent perturber la connectivité réseau»](../media/Hyper-V-NewVSwitch-DisruptNetwork.png)  
  
## <a name="create-a-virtual-switch-by-using-windows-powershell"></a>Créer un commutateur virtuel à l’aide de Windows PowerShell  
  
1.  Sur le Bureau Windows, cliquez sur le bouton Démarrer et tapez une partie du nom **Windows PowerShell**.  
  
2.  Cliquez avec le bouton droit sur Windows PowerShell, puis sélectionnez **exécuter en tant qu’administrateur**.  
  
3.  Recherchez des cartes réseau existantes en exécutant l’applet de commande [obtenir-NetAdapter](https://technet.microsoft.com/library/jj130867.aspx) . Prenez note du nom de la carte réseau que vous souhaitez utiliser pour le commutateur virtuel.  
  
    ```  
    Get-NetAdapter  
    ```  
  
4.  Créez un commutateur virtuel à l’aide de l’applet [de commande New-VMSwitch](https://technet.microsoft.com/library/hh848455.aspx) . Par exemple, pour créer un commutateur virtuel externe nommé ExternalSwitch, à l’aide de la carte réseau Ethernet et avec **autoriser le système d’exploitation de gestion à partager cette carte réseau** activée, exécutez la commande suivante.  
  
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
  
Pour obtenir des scripts Windows PowerShell plus avancés qui couvrent les fonctionnalités améliorées ou nouvelles des commutateurs virtuels dans Windows Server 2016, consultez [accès direct à la mémoire à distance et basculement d’association incorporée](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  

  
## <a name="next-step"></a>Étape suivante  
[Créer une machine virtuelle avec Hyper-V](Create-a-virtual-machine-in-Hyper-V.md)  
  


