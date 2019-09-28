---
title: Configuration de carte réseau convergée avec une seule carte réseau
description: Dans cette rubrique, nous vous fournissons les instructions permettant de configurer une carte réseau convergée avec une seule carte réseau dans votre hôte Hyper-V.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: eed5c184-fa55-43a8-a879-b1610ebc70ca
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/14/2018
ms.openlocfilehash: 2ad7592fd9faf1e92893e6271daabdad907d3aaa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405801"
---
# <a name="converged-nic-configuration-with-a-single-network-adapter"></a>Configuration de carte réseau convergée avec une seule carte réseau

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Dans cette rubrique, nous vous fournissons les instructions permettant de configurer une carte réseau convergée avec une seule carte réseau dans votre hôte Hyper-V.

L’exemple de configuration de cette rubrique décrit deux hôtes Hyper-V, l' **hôte Hyper-v A**et l' **hôte B Hyper-v**. Les deux ordinateurs hôtes ont une seule carte réseau physique (pNIC) installée, et les cartes réseau sont connectées à un commutateur physique \(ToR @ no__t-1. En outre, les hôtes se trouvent sur le même sous-réseau, à savoir 192.168.1. x/24.

![Ordinateurs hôtes Hyper-V](../../media/Converged-NIC/1-single-test-conn.jpg)


## <a name="step-1-test-the-connectivity-between-source-and-destination"></a>Étape 1. Tester la connectivité entre la source et la destination

Vérifiez que la carte réseau physique peut se connecter à l’ordinateur hôte de destination. Ce test illustre la connectivité à l’aide de la couche 3 \(L3 @ no__t-1-ou de la couche IP, ainsi que de la couche 2 \(L2 @ no__t-3.

1. Affichez les propriétés de la carte réseau.

   ```PowerShell
   Get-NetAdapter
   ```

   _**About**_  


   | Nom |    InterfaceDescription     | IfIndex | Statut |    macAddress     | LinkSpeed |
   |------|-----------------------------|---------|--------|-------------------|-----------|
   |  M1  | Mellanox ConnectX-3 Pro... |    4    |   Up (Haut)   | 7C-FE-90-93-8F-A1 |  40 Gbits/s  |

   ---

2. Affichez les propriétés de l’adaptateur supplémentaires, y compris l’adresse IP.

   ```PowerShell
   Get-NetAdapter M1 | fl *
   ```

   _**About**_

   ```PowerShell   
    MacAddress   : 7C-FE-90-93-8F-A1
    Status   : Up
    LinkSpeed: 40 Gbps
    MediaType: 802.3
    PhysicalMediaType: 802.3
    AdminStatus  : Up
    MediaConnectionState : Connected
    DriverInformation: Driver Date 2016-08-28 Version 5.25.12665.0 NDIS 6.60
    DriverFileName   : mlx4eth63.sys
    NdisVersion  : 6.60
    ifOperStatus : Up
    ifAlias  : M1
    InterfaceAlias   : M1
    ifIndex  : 4
    ifDesc   : Mellanox ConnectX-3 Pro Ethernet Adapter
    ifName   : ethernet_32773
    DriverVersion: 5.25.12665.0
    LinkLayerAddress : 7C-FE-90-93-8F-A1
    Caption  :
    Description  :
    ElementName  :
    InstanceID   : {39B58B4C-8833-4ED2-A2FD-E105E7146D43}
    CommunicationStatus  :
    DetailedStatus   :
    HealthState  :
    InstallDate  :
    Name : M1
    OperatingStatus  :
    OperationalStatus:
    PrimaryStatus:
    StatusDescriptions   :
    AvailableRequestedStates :
    EnabledDefault   : 2
    EnabledState : 5
    OtherEnabledState:
    RequestedState   : 12
    TimeOfLastStateChange:
    TransitioningToState : 12
    AdditionalAvailability   :
    Availability :
    CreationClassName: MSFT_NetAdapter
   ``` 

## <a name="step-2-ensure-that-source-and-destination-can-communicate"></a>Étape 2. S’assurer que la source et la destination peuvent communiquer

Dans cette étape, nous utilisons la commande **test-NetConnection** Windows PowerShell, mais si vous le souhaitez, vous pouvez utiliser la commande **ping** . 

>[!TIP]
>Si vous êtes certain que vos hôtes peuvent communiquer entre eux, vous pouvez ignorer cette étape.

1. Vérifiez la communication bidirectionnelle.

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**About**_


   |        Paramètre         |    Value    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      RemoteAddress       | 192.168.1.5 |
   |      InterfaceAlias      |     M1      |
   |      SourceAddress       | 192.168.1.3 |
   |      PingSucceeded       |    True     |
   | RTT \(PingReplyDetails\) |    0 ms     |

   ---

   Dans certains cas, vous devrez peut-être désactiver le pare-feu Windows avec fonctions avancées de sécurité pour effectuer ce test. Si vous désactivez le pare-feu, gardez à l’esprit la sécurité et assurez-vous que votre configuration répond aux exigences de sécurité de votre organisation.

2. Désactivez tous les profils de pare-feu.

   ```PowerShell
   Set-NetFirewallProfile -All -Enabled False
   ```

3. Après avoir désactivé les profils de pare-feu, testez à nouveau la connexion. 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**About**_


   |        Paramètre         |    Value    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      RemoteAddress       | 192.168.1.5 |
   |      InterfaceAlias      | Test-40G-1  |
   |      SourceAddress       | 192.168.1.3 |
   |      PingSucceeded       |    False    |
   | RTT \(PingReplyDetails\) |    0 ms     |

   ---



## <a name="step-3-optional-configure-the-vlan-ids-for-nics-installed-in-your-hyper-v-hosts"></a>Étape 3. Facultatif Configuration des ID de réseau local virtuel pour les cartes réseau installées dans vos ordinateurs hôtes Hyper-V

De nombreuses configurations réseau utilisent des réseaux locaux virtuels. Si vous envisagez d’utiliser des réseaux locaux virtuels dans votre réseau, vous devez répéter le test précédent avec des réseaux locaux virtuels configurés. En outre, si vous envisagez d’utiliser RoCE pour les services RDMA, vous devez activer les réseaux locaux virtuels.

Pour cette étape, les cartes réseau sont en mode d' **accès** . Toutefois, lorsque vous créez un commutateur \(virtuel Hyper-V vswitch\) plus loin dans ce guide, les propriétés du réseau local virtuel sont appliquées au niveau du port vswitch. 

Étant donné qu’un commutateur peut héberger plusieurs réseaux locaux virtuels, il est nécessaire que \(le\) commutateur physique du rack soit connecté au port auquel l’ordinateur hôte est connecté en mode Trunk.

>[!NOTE]
>Consultez la documentation de votre commutateur pour obtenir des instructions sur la configuration du mode Trunk sur le commutateur.

L’illustration suivante montre deux hôtes Hyper-V, chacun avec une carte réseau physique et chacun configuré pour communiquer sur le réseau local virtuel 101.

![Configurer des réseaux locaux virtuels](../../media/Converged-NIC/2-single-configure-vlans.jpg)


>[!IMPORTANT]
>Effectuez cette exécution sur les serveurs locaux et de destination. Si le serveur de destination n’est pas configuré avec le même ID de réseau local virtuel que le serveur local, les deux ne peuvent pas communiquer.


1. Configurez l’ID de réseau local virtuel pour les cartes réseau installées dans vos ordinateurs hôtes Hyper-V.

   >[!IMPORTANT]
   >N’exécutez pas cette commande si vous êtes connecté à l’hôte à distance via cette interface, car cela entraîne une perte d’accès à l’hôte.

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "101"
   Get-NetAdapterAdvancedProperty -Name M1 | Where-Object {$_.RegistryKeyword -eq "VlanID"} 
   ```

   _**About**_


   | Nom | DisplayName | DisplayValue | RegistryKeyword | RegistryValue |
   |------|-------------|--------------|-----------------|---------------|
   |  M1  |   ID du réseau local virtuel   |     101      |     VlanID      |     {101}     |

   ---

2. Redémarrez la carte réseau pour appliquer l’ID de réseau local virtuel.

   ```PowerShell
   Restart-NetAdapter -Name "M1"
   ```

3. Assurez-vous que l’État est **up**.

   ```PowerShell
   Get-NetAdapter -Name "M1"
   ```

   _**About**_


   | Nom |          InterfaceDescription           | IfIndex | Statut |    macAddress     | LinkSpeed |
   |------|-----------------------------------------|---------|--------|-------------------|-----------|
   |  M1  | Mellanox ConnectX-3 Pro Ethernet Ada... |    4    |   Up (Haut)   | 7C-FE-90-93-8F-A1 |  40 Gbits/s  |

   ---

   >[!IMPORTANT]
   >Le redémarrage de l’appareil peut prendre plusieurs secondes et être disponible sur le réseau. 

4. Vérifiez la connectivité.<p>Si la connectivité échoue, inspectez la configuration du réseau local virtuel du commutateur ou la participation à la destination dans le même réseau local virtuel. 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

## <a name="step-4-configure-quality-of-service-qos"></a>Étape 4. Configurer la qualité de \(service (QoS)\)

>[!NOTE]
>Vous devez effectuer toutes les étapes de configuration DCB et QoS suivantes sur tous les ordinateurs hôtes qui sont destinés à communiquer entre eux.

1. Installez Data Center \(Bridging\) DCB sur chacun de vos ordinateurs hôtes Hyper-V.

   - **Facultatif** pour les configurations réseau qui utilisent iWarp pour les services RDMA.
   - **Requis** pour les configurations réseau qui utilisent RoCE \(n’importe\) quelle version pour les services RDMA.

   ```PowerShell
   Install-WindowsFeature Data-Center-Bridging
   ```

2. Définir les stratégies QoS pour SMB-direct :

   - **Facultatif** pour les configurations réseau qui utilisent iWarp.
   - **Requis** pour les configurations réseau qui utilisent RoCE.

   Dans l’exemple de commande ci-dessous, la valeur « 3 » est arbitraire. Vous pouvez utiliser n’importe quelle valeur comprise entre 1 et 7 tant que vous utilisez régulièrement la même valeur tout au long de la configuration des stratégies QoS.

   ```PowerShell
   New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3
   ```

   _**About**_


   |   Paramètre    |          Value           |
   |----------------|--------------------------|
   |      Nom      |           SMB            |
   |     Propriétaire      | Ordinateur \(stratégie de groupe\) |
   | NetworkProfile |           Tous            |
   |   Priorité   |           127            |
   |   JobObject    |          &nbsp;          |
   | NetDirectPort  |           445            |
   | PriorityValue  |            3             |

   ---

3. Pour les déploiements RoCE, activez le **contrôle de flux de priorité** pour le trafic SMB, ce qui n’est pas requis pour iWarp.

   ```PowerShell
   Enable-NetQosFlowControl -priority 3
   Get-NetQosFlowControl
   ```

   _**About**_


   | Priority | Enabled | PolicySet | IfIndex | IfAlias |
   |----------|---------|-----------|---------|---------|
   |    0     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    1     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    2     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    3     |  True   |  Global   | &nbsp;  | &nbsp;  |
   |    4     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    5     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    6\.     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    7     |  False  |  Global   | &nbsp;  | &nbsp;  |

   ---

4. Activez la qualité de service pour les cartes réseau locales et de destination.

   - **Non nécessaire** pour les configurations réseau qui utilisent iWarp.
   - **Requis** pour les configurations réseau qui utilisent RoCE.

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "M1"
   Get-NetAdapterQos -Name "M1"
   ```

   _**About**_

   **Nom** : M1  
   **Enabled** : True  

   _**Possibilités**_   


   |      Paramètre      |   Matériel   |   Actuelle    |
   |---------------------|--------------|--------------|
   |    MacSecBypass     | NotSupported | NotSupported |
   |     DcbxSupport     |     Aucune     |     Aucune     |
   | NumTCs (max/ETS/PFC) |    8/8/8     |    8/8/8     |

   ---

   _**OperationalTrafficClasses:**_ 


   | CT | AUTORITÉ TSA | La | Priorité |
   |----|-----|-----------|------------|
   | 0  | STE |    70%    |  0-2, 4-7   |
   | 1  | STE |    30    |     3      |

   ---

   _**OperationalFlowControl:**_  

   Priorité 3 activée  

   _**OperationalClassifications:**_  


   | Protocol  | Port/type | Priority |
   |-----------|-----------|----------|
   |  Par défaut  |  &nbsp;   |    0     |
   | NetDirect |    445    |    3     |

   ---

5. Réservez un pourcentage de la bande passante pour SMB direct \(RDMA @ no__t-1.

    Dans cet exemple, une réservation de bande passante de 30% est utilisée. Vous devez sélectionner une valeur qui représente ce dont votre trafic de stockage a besoin. 

   ```PowerShell
   New-NetQosTrafficClass "SMB" -Priority 3 -BandwidthPercentage 30 -Algorithm ETS
   ```

   _**About**_


   | Nom | Algorithm | Bande passante (%) | Priority | PolicySet | IfIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | SMB  |    STE    |      30      |    3     |  Global   | &nbsp;  | &nbsp;  |

   ---                                      

6. Affichez les paramètres de réservation de bande passante.  

   ```PowerShell
   Get-NetQosTrafficClass
   ```

   _**About**_


   |   Nom    | Algorithm | Bande passante (%) | Priority | PolicySet | IfIndex | IfAlias |
   |-----------|-----------|--------------|----------|-----------|---------|---------|
   | Valeurs |    STE    |      70      | 0-2, 4-7  |  Global   | &nbsp;  | &nbsp;  |
   |    SMB    |    STE    |      30      |    3     |  Global   | &nbsp;  | &nbsp;  |

   ---

## <a name="step-5-optional-resolve-the-mellanox-adapter-debugger-conflict"></a>Étape 5. Facultatif Résoudre le conflit du débogueur de l’adaptateur Mellanox 

Par défaut, lors de l’utilisation d’un adaptateur Mellanox, le débogueur attaché bloque NetQos, qui est un problème connu. Par conséquent, si vous utilisez un adaptateur Mellanox et que vous envisagez d’attacher un débogueur, utilisez la commande suivante pour résoudre ce problème. Cette étape n’est pas requise si vous n’envisagez pas d’attacher un débogueur ou si vous n’utilisez pas d’adaptateur Mellanox.

   ```PowerShell    
   Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
   ``` 

## <a name="step-6-verify-the-rdma-configuration-native-host"></a>Étape 6. Vérifier la configuration RDMA (hôte natif)

Vous voulez vous assurer que l’infrastructure est correctement configurée avant de créer un vSwitch et de passer à RDMA (carte réseau convergée). 

L’illustration suivante montre l’état actuel des ordinateurs hôtes Hyper-V.

![Tester RDMA](../../media/Converged-NIC/4-single-test-rdma.jpg)

1. Vérifiez la configuration RDMA.

   ```PowerShell
   Get-NetAdapterRdma
   ```
   _**About**_


   | Nom |           InterfaceDescription           | Enabled |
   |------|------------------------------------------|---------|
   |  M1  | Adaptateur Ethernet Mellanox ConnectX-3 Pro |  True   |

   ---

2. Déterminez la valeur **ifIndex** de votre adaptateur cible.<p>Vous utilisez cette valeur dans les étapes suivantes lorsque vous exécutez le script que vous téléchargez.

   ```PowerShell
   Get-NetIPConfiguration -InterfaceAlias "M*" | ft InterfaceAlias,InterfaceIndex,IPv4Address
   ```

   _**About**_ 


   | InterfaceAlias | InterfaceIndex |  IPv4Address  |
   |----------------|----------------|---------------|
   |       M2       |       14       | 192.168.1.5 |

   ---

3. Téléchargez l' [utilitaire DiskSpd. exe](https://aka.ms/diskspd) et extrayez-le dans C:\test\.

4. Téléchargez le [script PowerShell test-RDMA](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1) dans un dossier de test sur votre disque local, par exemple, C:\test @ no__t-1

5. Exécutez le script PowerShell **test-RDMA. ps1** pour transmettre la valeur ifIndex au script, ainsi que l’adresse IP de la carte à distance sur le même réseau local virtuel.<p>Dans cet exemple, le script transmet la valeur **ifIndex** de 14 à l’adresse IP de la carte réseau distante 192.168.1.5.

   ```PowerShell
    C:\TEST\Test-RDMA.PS1 -IfIndex 14 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\

    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter M2 is a physical adapter
    VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
    VERBOSE: QoS/DCB/PFC configuration is correct.
    VERBOSE: RDMA configuration is correct.
    VERBOSE: Checking if remote IP address, 192.168.1.5, is reachable.
    VERBOSE: Remote IP 192.168.1.5 is reachable.
    VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
    VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 662979201 RDMA bytes written per second
    VERBOSE: 37561021 RDMA bytes sent per second
    VERBOSE: 1023098948 RDMA bytes written per second
    VERBOSE: 8901349 RDMA bytes sent per second
    VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
    VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.1.5
   ```

   >[!NOTE]
   >Si le trafic RDMA échoue, pour le cas RoCE, consultez la configuration de votre commutateur pour obtenir les paramètres PFC/ETS appropriés qui doivent correspondre aux paramètres de l’ordinateur hôte. Reportez-vous à la section QoS dans ce document pour obtenir les valeurs de référence.

## <a name="step-7-remove-the-access-vlan-setting"></a>Étape 7. Supprimer le paramètre d’accès VLAN

En vue de la création du commutateur Hyper-V, vous devez supprimer les paramètres de réseau local virtuel que vous avez installés ci-dessus.  

1. Supprimez le paramètre VLAN d’accès de la carte réseau physique pour empêcher la carte réseau de baliser automatiquement le trafic de sortie avec l’ID de réseau local virtuel incorrect.<p>La suppression de ce paramètre l’empêche également de filtrer le trafic d’entrée qui ne correspond pas à l’ID de réseau local virtuel d’accès.

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "0"
   ```    

2. Vérifiez que le **paramètre VlanID** affiche la valeur de l’ID de réseau local virtuel sur zéro.

   ```PowerShell    
   Get-NetAdapterAdvancedProperty -name m1 | Where-Object {$_.RegistryKeyword -eq 'VlanID'} 
   ```  


## <a name="step-8-create-a-hyper-v-vswitch-on-your-hyper-v-hosts"></a>Étape 8. Créer un vSwitch Hyper-V sur vos ordinateurs hôtes Hyper-V

L’image suivante représente l’hôte 1 Hyper-V avec un vSwitch.

![Créer un commutateur virtuel Hyper-V](../../media/Converged-NIC/5-single-create-vswitch.jpg)


1. Créer un vSwitch Hyper-V externe dans Hyper-v sur l’hôte Hyper-V A. <p>Dans cet exemple, le commutateur est nommé VMSTEST. En outre, le paramètre **AllowManagementOS** crée un carte réseau virtuelle hôte qui hérite des adresses Mac et IP de la carte réseau physique.

   ```PowerShell
   New-VMSwitch -Name VMSTEST -NetAdapterName "M1" -AllowManagementOS $true
   ```
   _**About**_


   |  Nom   | SwitchType |      Paramètre netadapterinterfacedescription      |
   |---------|------------|------------------------------------------|
   | VMSTEST |  Externe  | Adaptateur Ethernet Mellanox ConnectX-3 Pro |

   ---

2. Affichez les propriétés de la carte réseau.

   ```PowerShell
   Get-NetAdapter | ft -AutoSize
   ```

   _**About**_


   |         Nom          |        InterfaceDescription         | IfIndex | Statut |    macAddress     | LinkSpeed |
   |-----------------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet \(VMSTEST @ no__t-1 | Carte Ethernet virtuelle Hyper-V #2 |   27    |   Up (Haut)   | E4-1D-2D-07-40-71 |  40 Gbits/s  |

   ---

3. Gérez le carte réseau virtuelle de l’ordinateur hôte de l’une des deux manières suivantes : 

   - La vue **NetAdapter** fonctionne en fonction du nom « vEthernet \(VMSTEST @ no__t-2 ». Toutes les propriétés de la carte réseau ne s’affichent pas dans cette vue.
   - La vue **VMNetworkAdapter** supprime le préfixe « vEthernet » et utilise simplement le nom vmswitch. (recommandé) 

   ```PowerShell
   Get-VMNetworkAdapter –ManagementOS | ft -AutoSize
   ```

   _**About**_


   |         Nom         | IsManagementOs |        VMName        |  SwitchName  | macAddress | Statut | AdressesIP |
   |----------------------|----------------|----------------------|--------------|------------|--------|-------------|
   | CORP-External-Switch |      True      | CORP-External-Switch | 001B785768AA |    OK    | &nbsp; |             |
   |       VMSTEST        |      True      |       VMSTEST        | E41D2D074071 |    OK    | &nbsp; |             |

   ---

4. Testez la connexion.

   ```Powershell    
   Test-NetConnection 192.168.1.5
   ```

   _**About**_ 

   ```
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (CORP-External-Switch)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
   ```

5. Attribuer et afficher les paramètres de réseau local virtuel de la carte réseau.

   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
   Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
   ```    

   _**About**_


   | VMName | VMNetworkAdapterName |  Mode  | VlanList |
   |--------|----------------------|--------|----------|
   | &nbsp; |       VMSTEST        | Accès |   101    |

   ---  

6. Testez la connexion.<p>L’exécution de la commande ping de l’autre adaptateur peut prendre quelques secondes.  

   ```PowerShell    
   Test-NetConnection 192.168.1.5
   ```

   _**About**_

   ```
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (VMSTEST)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
   ```

## <a name="step-9-test-hyper-v-virtual-switch-rdma-mode-2"></a>Étape 9 : Tester le commutateur virtuel Hyper-V RDMA (mode 2)

L’illustration suivante représente l’état actuel de vos ordinateurs hôtes Hyper-V, y compris le vSwitch sur l’hôte 1 Hyper-V.

![Tester le commutateur virtuel Hyper-V](../../media/Converged-NIC/6-single-test-vswitch-rdma.jpg)


1. Définissez le balisage de priorité sur le carte réseau virtuelle hôte.

   ```PowerShell    
   Set-VMNetworkAdapter -ManagementOS -Name "VMSTEST" -IeeePriorityTag on
   Get-VMNetworkAdapter -ManagementOS -Name "VMSTEST" | fl Name,IeeePriorityTag
   ```  

   _**About**_

    Nom : VMSTEST IeeePriorityTag : Activé


2. Affichez les informations RDMA de la carte réseau. 

   ```PowerShell
   Get-NetAdapterRdma
   ```   

   _**About**_


   |         Nom          |        InterfaceDescription         | Enabled |
   |-----------------------|-------------------------------------|---------|
   | vEthernet \(VMSTEST @ no__t-1 | Carte Ethernet virtuelle Hyper-V #2 |  False  |

   ---

   >[!NOTE]
   >Si le paramètre **Enabled** a la valeur **false**, cela signifie que RDMA n’est pas activé.


3. Affichez les informations de la carte réseau.

   ```PowerShell
   Get-NetAdapter
   ```

   _**About**_   


   |        Nom         |        InterfaceDescription         | IfIndex | Statut |    macAddress     | LinkSpeed |
   |---------------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet (VMSTEST) | Carte Ethernet virtuelle Hyper-V #2 |   27    |   Up (Haut)   | E4-1D-2D-07-40-71 |  40 Gbits/s  |

   ---


4. Activez RDMA sur l’ordinateur hôte carte réseau virtuelle.

   ```PowerShell
   Enable-NetAdapterRdma -Name "vEthernet (VMSTEST)"
   Get-NetAdapterRdma -Name "vEthernet (VMSTEST)"
   ```

   _**About**_


   |         Nom          |        InterfaceDescription         | Enabled |
   |-----------------------|-------------------------------------|---------|
   | vEthernet \(VMSTEST @ no__t-1 | Carte Ethernet virtuelle Hyper-V #2 |  True   |

   ---

   >[!NOTE]
   >Si le paramètre **Enabled** a la valeur **true**, cela signifie que RDMA est activé.

5. Effectuez un test de trafic RDMA.

   ```PowerShell    
    C:\TEST\Test-RDMA.PS1 -IfIndex 27 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter vEthernet (VMSTEST) is a virtual adapter
    VERBOSE: Retrieving vSwitch bound to the virtual adapter
    VERBOSE: Found vSwitch: VMSTEST
    VERBOSE: Found the following physical adapter(s) bound to vSwitch: TEST-40G-1
    VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
    VERBOSE: QoS/DCB/PFC configuration is correct.
    VERBOSE: RDMA configuration is correct.
    VERBOSE: Checking if remote IP address, 192.168.1.5, is reachable.
    VERBOSE: Remote IP 192.168.1.5 is reachable.
    VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
    VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 9162492 RDMA bytes sent per second
    VERBOSE: 938797258 RDMA bytes written per second
    VERBOSE: 34621865 RDMA bytes sent per second
    VERBOSE: 933572610 RDMA bytes written per second
    VERBOSE: 35035861 RDMA bytes sent per second
    VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
    VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.1.5
   ```

La dernière ligne de cette sortie, «test du trafic RDMA réussie : Le trafic RDMA a été envoyé à 192.168.1.5, «indique que vous avez correctement configuré la carte réseau convergée sur votre carte.

## <a name="related-topics"></a>Rubriques connexes
- [Configuration de carte réseau associée à une carte réseau convergée](cnic-datacenter.md)
- [Configuration du commutateur physique pour la carte réseau convergée](cnic-app-switch-config.md)
- [Résolution des problèmes de configuration de cartes réseau convergées](cnic-app-troubleshoot.md)
