---
title: Configuration de carte réseau convergence avec une seule carte réseau
description: Dans cette rubrique, nous vous fournissons les instructions pour configurer la carte d’interface réseau convergé avec une carte réseau unique dans votre hôte Hyper-V.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: eed5c184-fa55-43a8-a879-b1610ebc70ca
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/14/2018
ms.openlocfilehash: 7777278f374984f242e44fd8a8fa94388df88a30
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836470"
---
# <a name="converged-nic-configuration-with-a-single-network-adapter"></a>Configuration de carte réseau convergence avec une seule carte réseau

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette rubrique, nous vous fournissons les instructions pour configurer la carte d’interface réseau convergé avec une carte réseau unique dans votre hôte Hyper-V.

L’exemple de configuration dans cette rubrique décrit deux hôtes Hyper-V, **hôte Hyper-V A**, et **Hyper-V hôte B**. Les deux hôtes ont une seule carte réseau physique (carte réseau) installée, et les cartes réseau sont connectées à un haut de rack \(ToR\) commutateur physique. En outre, les ordinateurs hôtes se trouvent sur le même sous-réseau, qui est 192.168.1.x/24.

![Ordinateurs hôtes Hyper-V](../../media/Converged-NIC/1-single-test-conn.jpg)


## <a name="step-1-test-the-connectivity-between-source-and-destination"></a>Étape 1. Tester la connectivité entre la source et destination

Vérifiez que physique carte d’interface réseau peut se connecter à l’hôte de destination. Ce test démontre la connectivité à l’aide de couche 3 \(L3\) - ou de la couche IP - ainsi que de la couche 2 \(L2\).

1. Afficher les propriétés de la carte réseau.

   ```PowerShell
   Get-NetAdapter
   ```
   
   _**Résultats :**_  

   |Nom|InterfaceDescription|ifIndex|État|MacAddress|LinkSpeed|
   |-----|--------------------|-------|-----|----------|---------|
   |M1|Mellanox ConnectX-3 Pro...| 4| Up (Haut)|7C-FE-90-93-8F-A1|40 Gbits/s|
   ---

2. Afficher les propriétés d’adaptateur supplémentaires, y compris l’adresse IP.

   ```PowerShell
   Get-NetAdapter M1 | fl *
   ```

   _**Résultats :**_

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

## <a name="step-2-ensure-that-source-and-destination-can-communicate"></a>Étape 2. Vérifiez que la source et destination peuvent communiquer

Dans cette étape, nous utilisons le **Test-NetConnection** commande Windows PowerShell, mais si vous pouvez utiliser la **ping** commande si vous préférez. 

>[!TIP]
>Si vous êtes certain que vos hôtes peuvent communiquer entre eux, vous pouvez ignorer cette étape.

1. Vérifiez la communication bidirectionnelle.

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```
   
   _**Résultats :**_

   |Paramètre|Value|
   |---------|-----|
   |ComputerName|192.168.1.5|
   |RemoteAddress|192.168.1.5|
   |InterfaceAlias|M1|
   |SourceAddress|192.168.1.3|
   |PingSucceeded|True|
   |PingReplyDetails \(RTT\)|0 ms|
   ---
   
   Dans certains cas, vous devrez peut-être désactiver le pare-feu Windows avec fonctions avancées de sécurité pour effectuer ce test. Si vous désactivez le pare-feu, n’oubliez pas sécurité et vérifiez que votre configuration répond aux exigences de sécurité de votre organisation.

2. Désactiver tous les profils de pare-feu.

   ```PowerShell
   Set-NetFirewallProfile -All -Enabled False
   ```
    
3. Après avoir désactivé les profils de pare-feu, testez à nouveau la connexion. 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**Résultats :**_

   |Paramètre|Value|
   |---------|-----|
   |ComputerName|192.168.1.5|
   |RemoteAddress|192.168.1.5|
   |InterfaceAlias|Test-40G-1|
   |SourceAddress|192.168.1.3|
   |PingSucceeded|False|
   |PingReplyDetails \(RTT\)|0 ms|
   ---



## <a name="step-3-optional-configure-the-vlan-ids-for-nics-installed-in-your-hyper-v-hosts"></a>Étape 3. (Facultatif) Configurer l’ID de VLAN pour les cartes réseau installées dans vos hôtes Hyper-V

De nombreuses configurations réseau utilisent des réseaux locaux virtuels, et si vous envisagez d’utiliser des réseaux locaux virtuels dans votre réseau, vous devez répéter le test précédent avec les réseaux locaux virtuels configurés. En outre, si vous envisagez d’utiliser RoCE pour les services RDMA vous devez activer les réseaux locaux virtuels.

Pour cette étape, les cartes réseau sont dans **accès** mode. Toutefois, lorsque vous créez un commutateur virtuel Hyper-V \(vSwitch\) plus loin dans ce guide, les propriétés de réseau local virtuel sont appliquées au niveau du port de commutateur virtuel. 

Comme un commutateur peut héberger plusieurs réseaux locaux virtuels, il est nécessaire pour le Top of Rack \(ToR\) commutateur physique pour que le port que l’hôte est connecté à configurée en mode Trunk.

>[!NOTE]
>Pour obtenir des instructions sur la façon de configurer le mode Trunk sur le commutateur, consultez la documentation de votre commutateur ToR.

L’illustration suivante montre deux hôtes Hyper-V, chacun avec une carte réseau physique, et chacun est configuré pour communiquer sur le réseau local virtuel 101.

![Configurer des réseaux locaux virtuels](../../media/Converged-NIC/2-single-configure-vlans.jpg)


>[!IMPORTANT]
>Effectuer cette procédure sur les serveurs locaux et de destination. Si le serveur de destination n’est pas configuré avec le même ID de VLAN que le serveur local, les deux ne peuvent pas communiquer.


1. Configurer l’ID de VLAN pour les cartes réseau installées dans vos hôtes Hyper-V.

   >[!IMPORTANT]
   >N’exécutez pas cette commande si vous êtes connecté à l’hôte à distance via cette interface, car cela entraîne la perte d’accès à l’hôte.
    
   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "101"
   Get-NetAdapterAdvancedProperty -Name M1 | Where-Object {$_.RegistryKeyword -eq "VlanID"} 
   ```

   _**Résultats :**_

   |Nom |DisplayName| DisplayValue| RegistryKeyword |RegistryValue|
   |----|-----------|------------|---------------|-------------|
   |M1|ID du réseau local virtuel|101|VlanID|{101}|
   ---

2. Redémarrez la carte réseau pour appliquer le VLAN ID.

   ```PowerShell
   Restart-NetAdapter -Name "M1"
   ```

3. Vérifiez l’état est **des**.

   ```PowerShell
   Get-NetAdapter -Name "M1"
   ```
   
   _**Résultats :**_

   |Nom|InterfaceDescription|ifIndex| État|MacAddress|LinkSpeed|
   |----|--------------------|-------|------|----------| ---------|
   |M1|Ethernet Pro Mellanox ConnectX-3 Ada...|4|Up (Haut)|7C-FE-90-93-8F-A1|40 Gbits/s|
   ---

   >[!IMPORTANT]
   >Il peut prendre plusieurs secondes pour l’appareil pour redémarrer et sont disponibles sur le réseau. 

4. Vérifier la connectivité.<p>En cas de connectivité, inspecter le commutateur de participation de configuration ou la destination de réseau local virtuel dans le même réseau local virtuel. 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```
    
## <a name="step-4-configure-quality-of-service-qos"></a>Étape 4. Configurer la qualité de Service \(QoS\)

>[!NOTE]
>Vous devez effectuer toutes les étapes de configuration suivantes DCB et QoS sur tous les hôtes qui sont destinées à communiquer entre eux.

1. Installer Data Center Bridging \(DCB\) sur chacun de vos hôtes Hyper-V.

   - **Facultatif** pour les configurations de réseau qui utilisent iWarp pour les services RDMA.
   - **Requis** pour les configurations de réseau qui utilisent RoCE \(n’importe quelle version\) pour les services RDMA.

   ```PowerShell
   Install-WindowsFeature Data-Center-Bridging
   ```

2. Définir les stratégies de QoS à SMB Direct :

   - **Facultatif** pour les configurations de réseau qui utilisent iWarp.
   - **Requis** pour les configurations de réseau qui utilisent RoCE.
   
   Dans l’exemple de commande ci-dessous, la valeur « 3 » est arbitraire. Vous pouvez utiliser n’importe quelle valeur comprise entre 1 et 7, que vous utilisez régulièrement la même valeur tout au long de la configuration des stratégies de QoS.

   ```PowerShell
   New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3
   ```

   _**Résultats :**_

   |Paramètre|Value|
   |---------|-----|
   |Nom |SMB|
   |Propriétaire|Stratégie de groupe \(Machine\)|
   |NetworkProfile|Tous|
   |Priorité|127|
   |JobObject|&nbsp;| 
   |NetDirectPort|445 |
   |PriorityValue|3 |
 ---

3. Pour les déploiements de RoCE, allumez **contrôle de flux de priorité** pour le trafic SMB, qui n’est pas requis pour iWarp.

   ```PowerShell
   Enable-NetQosFlowControl -priority 3
   Get-NetQosFlowControl
   ```

   _**Résultats :**_

   |Priority|Enabled|PolicySet|IfIndex|IfAlias|
   |---------|-----|--------- |-------| -------|
   |0 |False |Globale|&nbsp;|&nbsp;|
   |1 |False |Globale|&nbsp;|&nbsp;|
   |2 |False |Globale|&nbsp;|&nbsp;|
   |3 |True  |Globale|&nbsp;|&nbsp;|
   |4 |False |Globale|&nbsp;|&nbsp;|
   |5 |False |Globale|&nbsp;|&nbsp;|
   |6 |False |Globale|&nbsp;|&nbsp;|
   |7 |False |Globale|&nbsp;|&nbsp;|
   ---

4. Activer la qualité de service pour les adaptateurs de réseau local et de destination.

   - **Inutile** pour les configurations de réseau qui utilisent iWarp.
   - **Requis** pour les configurations de réseau qui utilisent RoCE.

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "M1"
   Get-NetAdapterQos -Name "M1"
   ```

   _**Résultats :**_

   **Nom** : M1  
   **Enabled** : True  

   _**Fonctionnalités :**_   

   |Paramètre|Matériel|Actuel|
   |---------|--------|-------|
   |MacSecBypass|NotSupported|NotSupported|
   |DcbxSupport|Aucune|Aucune|
   |NumTCs(Max/ETS/PFC)|8/8/8|8/8/8|
   ---
 
   _**OperationalTrafficClasses :**_ 

   |TC|TSA|Bande passante|Priorités|
   |----|-----|--------|-------|
   |0| ETS|70%|0-2,4-7|
   |1|ETS|30%|3 |
   ---

   _**OperationalFlowControl :**_  
   
   Priorité 3 activé  

   _**OperationalClassifications :**_  

   |Protocole|Type de port /|Priority|
   |--------|---------|--------|
   |Par défaut|&nbsp;|0|
   |NetDirect| 445|3|
   ---

5. Réserver un pourcentage de la bande passante pour SMB Direct \(RDMA\).

    Dans cet exemple, une réservation de bande passante de 30 % est utilisée. Vous devez sélectionner une valeur qui représente ce que vous attendez que nécessite votre trafic de stockage. 

   ```PowerShell
   New-NetQosTrafficClass "SMB" -Priority 3 -BandwidthPercentage 30 -Algorithm ETS
   ```

   _**Résultats :**_

   |Nom|Algorithm |Bandwidth(%)| Priority |PolicySet |IfIndex |IfAlias |
   |----|---------| ------------ |--------| ---------|------- |------- |
   |SMB | ETS     | 30 |3 |Globale |&nbsp;|&nbsp;|
   ---                                      

6. Afficher les paramètres de réservation de bande passante.  

   ```PowerShell
   Get-NetQosTrafficClass
   ```

   _**Résultats :**_
 
   |Nom|Algorithm |Bandwidth(%)| Priority |PolicySet |IfIndex |IfAlias |
   |----|---------| ------------ |--------| ---------|------- |------- |
   |[Default]|ETS|70 |0-2,4-7| Globale|&nbsp;|&nbsp;| 
   |SMB      |ETS|30 |3 |Globale|&nbsp;|&nbsp;| 
   ---

## <a name="step-5-optional-resolve-the-mellanox-adapter-debugger-conflict"></a>Étape 5. (Facultatif) Résoudre le conflit de débogueur de carte Mellanox 

Par défaut, lorsque vous utilisez une carte Mellanox, le débogueur attaché bloque NetQos, qui est un problème connu. Par conséquent, si vous utilisez un adaptateur de Mellanox et que vous avez l’intention d’attacher un débogueur, utilisez la résolution de commande suivant ce problème. Cette étape n’est pas obligatoire si vous ne souhaitez pas attacher un débogueur ou si vous n’utilisez pas d’une carte Mellanox.

   ```PowerShell    
   Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
   ``` 

## <a name="step-6-verify-the-rdma-configuration-native-host"></a>Étape 6. Vérifier la configuration RDMA (hôte natif)

Vous souhaitez vous assurer que l’infrastructure est correctement configuré avant la création d’un commutateur virtuel et la transition pour RDMA (carte de réseau convergé). 

L’illustration suivante montre l’état actuel des hôtes Hyper-V.

![Test RDMA](../../media/Converged-NIC/4-single-test-rdma.jpg)

1. Vérifiez la configuration RDMA.

   ```PowerShell
   Get-NetAdapterRdma
   ```
   _**Résultats :**_

   |Nom |InterfaceDescription |Enabled|
   |----|--------------------|-------|
   |M1| Carte Ethernet Pro Mellanox ConnectX-3 |True|
   ---

2. Déterminer le **ifIndex** valeur de votre carte cible.<p>Vous utilisez cette valeur dans les étapes suivantes lorsque vous exécutez le script que vous téléchargez.

   ```PowerShell
   Get-NetIPConfiguration -InterfaceAlias "M*" | ft InterfaceAlias,InterfaceIndex,IPv4Address
   ```

   _**Résultats :**_ 

   |InterfaceAlias |InterfaceIndex |IPv4Address|
   |-------------- |-------------- |-----------|
   |M2 |14 |{192.168.1.5}|
   ---

3. Téléchargez le [DiskSpd.exe utilitaire](https://aka.ms/diskspd) et extrayez-le dans C:\TEST\.

4. Téléchargez le [de script powershell Test-RDMA](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1) dans un dossier de test sur votre lecteur local, par exemple, C:\TEST\.

5. Exécutez le **Test-Rdma.ps1** script PowerShell pour passer la valeur ifIndex au script, ainsi que l’adresse IP de la carte à distance sur le même réseau local virtuel.<p>Dans cet exemple, le script transmet la **ifIndex** valeur 14 sur l’adresse de l’adaptateur de réseau distant 192.168.1.5.
   
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
   >Si le trafic RDMA échoue, le cas RoCE en particulier, consultez votre configuration ToR Switch pour les paramètres PFC/ETS appropriés qui doivent correspondre aux paramètres de l’hôte. Reportez-vous à la section de la qualité de service dans ce document pour les valeurs de référence.

## <a name="step-7-remove-the-access-vlan-setting"></a>Étape 7. Supprimez le paramètre VLAN Access

En préparation pour la création d’Hyper-V commutateur, vous devez supprimer les paramètres de réseau local virtuel que vous avez installé précédemment.  

1. Supprimez le paramètre de réseau local virtuel de l’accès à partir de la carte réseau physique pour empêcher la carte réseau à partir du balisage automatique le trafic de sortie avec l’ID de VLAN incorrect.<p>Supprimer ce paramètre empêche également de filtrer le trafic en entrée qui ne correspond pas à l’ID de VLAN ACCESS.
    
   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "0"
   ```    

2. Vérifiez que le **VlanID paramètre** affiche la valeur d’ID de réseau local virtuel en tant que zéro.

   ```PowerShell    
   Get-NetAdapterAdvancedProperty -name m1 | Where-Object {$_.RegistryKeyword -eq 'VlanID'} 
   ```  


## <a name="step-8-create-a-hyper-v-vswitch-on-your-hyper-v-hosts"></a>Étape 8. Créer un commutateur virtuel Hyper-V sur vos ordinateurs hôtes Hyper-V

L’image suivante illustre l’hôte 1 Hyper-V avec un commutateur virtuel.

![Créer un commutateur virtuel Hyper-V](../../media/Converged-NIC/5-single-create-vswitch.jpg)


1. Créer un commutateur virtuel Hyper-V externe dans Hyper-V sur Hyper-V hôte A. <p>Dans cet exemple, le commutateur est nommé VMSTEST. En outre, le paramètre **AllowManagementOS** crée une carte réseau virtuelle hôte qui hérite les adresses MAC et IP de la carte physique.

   ```PowerShell
   New-VMSwitch -Name VMSTEST -NetAdapterName "M1" -AllowManagementOS $true
   ```
   _**Résultats :**_

   |Nom |SwitchType |NetAdapterInterfaceDescription|
   |---- |---------- |------------------------------|
   |VMSTEST |Externe |Carte Ethernet Pro Mellanox ConnectX-3|
   ---

2. Afficher les propriétés de la carte réseau.

   ```PowerShell
   Get-NetAdapter | ft -AutoSize
   ```

   _**Résultats :**_

   |Nom |InterfaceDescription | ifIndex |État |MacAddress |LinkSpeed|
   |---- |-------------------- |-------| ------|----------|---------|
   |vEthernet \(VMSTEST\) |Adaptateur Ethernet virtuel Hyper-V #2|27 |Up (Haut) |E4-1D-2D-07-40-71 |40 Gbits/s|
   ---

3. Gérer la carte réseau virtuelle hôte dans un des deux façons. 

   - **NetAdapter** vue se comporte en fonction de la « vEthernet \(VMSTEST\)« nom. Pas toutes les propriétés de la carte réseau s’affichent dans cette vue.
   - **VMNetworkAdapter** vue supprime le préfixe « vEthernet » et utilise simplement le nom de commutateur vmswitch. (recommandé) 

   ```PowerShell
   Get-VMNetworkAdapter –ManagementOS | ft -AutoSize
   ```

   _**Résultats :**_

   |Nom |IsManagementOs |VMName |SwitchName |MacAddress |État |IPAddresses|
   |----|-------------- |------ |----------|----------|------ |-----------|
   |Commutateur de CORP-externe |True |Commutateur de CORP-externe| 001B785768AA |{Ok} |&nbsp;|
   |VMSTEST |True |VMSTEST | E41D2D074071| {Ok} | &nbsp;| 
   ---

4. Tester la connexion.

   ```Powershell    
   Test-NetConnection 192.168.1.5
   ```

   _**Résultats :**_ 

   ```
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (CORP-External-Switch)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
   ```

5. Attribuer et afficher les paramètres de réseau local virtuel de carte réseau.
    
   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
   Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
   ```    

   _**Résultats :**_

   |VMName |VMNetworkAdapterName |Mode |VlanList|
   |------ |-------------------- |---- |--------|
   |&nbsp;|VMSTEST |Accès |101   |
   ---  
 
6. Tester la connexion.<p>Il peut prendre quelques secondes avant que vous pouvez correctement un test ping sur l’autre carte.  

   ```PowerShell    
   Test-NetConnection 192.168.1.5
   ```

   _**Résultats :**_
     
   ```
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (VMSTEST)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
   ```

## <a name="step-9-test-hyper-v-virtual-switch-rdma-mode-2"></a>Étape 9 : Tester le commutateur virtuel Hyper-V RDMA (Mode 2)

L’image suivante illustre l’état actuel de vos hôtes Hyper-V, y compris le commutateur virtuel sur l’hôte 1 Hyper-V.

![Tester le commutateur virtuel Hyper-V](../../media/Converged-NIC/6-single-test-vswitch-rdma.jpg)


1. Définir la priorité sur la carte réseau virtuelle hôte de balisage.

   ```PowerShell    
   Set-VMNetworkAdapter -ManagementOS -Name "VMSTEST" -IeeePriorityTag on
   Get-VMNetworkAdapter -ManagementOS -Name "VMSTEST" | fl Name,IeeePriorityTag
   ```  
   
   _**Résultats :**_

    Nom : VMSTEST IeeePriorityTag : Activé


2. Afficher la carte réseau d’informations RDMA. 

   ```PowerShell
   Get-NetAdapterRdma
   ```   

   _**Résultats :**_

   |Nom |InterfaceDescription |Enabled |
   |---- |-------------------- |-------|
   |vEthernet \(VMSTEST\)| Adaptateur Ethernet virtuel Hyper-V #2|False|
   ---

   >[!NOTE]
   >Si le paramètre **activé** a la valeur **False**, cela signifie que le RDMA n’est pas activé.
    

3. Afficher les informations de carte réseau.

   ```PowerShell
   Get-NetAdapter
   ```

   _**Résultats :**_   
 
   |Nom |InterfaceDescription |ifIndex |État |MacAddress |LinkSpeed|
   |----|--------------------|-------|------|----------|---------|
   |vEthernet (VMSTEST)|Adaptateur Ethernet virtuel Hyper-V #2|27|Up (Haut)|E4-1D-2D-07-40-71|40 Gbits/s|
   ---


4. Activez RDMA sur la carte réseau virtuelle hôte.

   ```PowerShell
   Enable-NetAdapterRdma -Name "vEthernet (VMSTEST)"
   Get-NetAdapterRdma -Name "vEthernet (VMSTEST)"
   ```

   _**Résultats :**_

   |Nom |InterfaceDescription |Enabled |
   |---- |-------------------- |-------|
   |vEthernet \(VMSTEST\)| Adaptateur Ethernet virtuel Hyper-V #2|True|
   ---

   >[!NOTE]
   >Si le paramètre **activé** a la valeur **True**, cela signifie que le RDMA est activé.

5. Effectuer un test de trafic RDMA.

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

La dernière ligne dans cette sortie, « test de trafic RDMA réussi : Le trafic RDMA a été envoyé à 192.168.1.5, » montre que vous avez correctement configuré convergé la carte réseau sur votre carte.

## <a name="related-topics"></a>Rubriques connexes
- [Configuration des cartes réseau de carte réseau convergé](cnic-datacenter.md)
- [Configuration de commutateur physique pour la carte réseau convergé](cnic-app-switch-config.md)
- [Résolution des problèmes convergé des Configurations de carte réseau](cnic-app-troubleshoot.md)
