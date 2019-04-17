---
title: Configuration des cartes réseau convergé avec une seule carte réseau
description: Cette rubrique fournit des instructions sur la procédure de déploiement convergé de cartes réseau avec une seule carte réseau dans Windows Server2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: eed5c184-fa55-43a8-a879-b1610ebc70ca
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d6663a966026afb301a4bad90a9573d16fc82875
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="converged-nic-configuration-with-a-single-network-adapter"></a>Configuration des cartes réseau convergé avec une seule carte réseau

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Les sections suivantes fournissent des instructions pour la configuration de la carte d’interface réseau convergé avec une seule carte réseau dans votre hôte Hyper-V.

L’exemple de configuration dans ce guide illustre deux hôtes Hyper-V, **un hôte Hyper-V**, et **hôte Hyper-V B**.

## <a name="test-connectivity-between-source-and-destination"></a>Tester la connectivité entre la Source et de Destination

Cette section fournit les étapes requises pour tester la connectivité entre la source et de destination hôtes Hyper-V.

L’illustration suivante représente deux hôtes Hyper-V, **un hôte Hyper-V** et **hôte Hyper-V B**. 

Une seule carte réseau physique (pNIC) installée sur les deux serveurs, mais les cartes réseau sont connectés à un commutateur top of rack \(ToR\) physique. En outre, les serveurs se trouvent sur le même sous-réseau, qui est 192.168.1.x/24.

![Ordinateurs hôtes Hyper-V](../../media/Converged-NIC/1-single-test-conn.jpg)


### <a name="test-nic-connectivity-to-the-hyper-v-virtual-switch"></a>Tester la connectivité des cartes réseau au commutateur virtuel Hyper\-V

À l’aide de cette étape, vous pouvez vous assurer que la carte réseau physique pour lequel vous allez créer ultérieurement un commutateur virtuel Hyper\-V, permettre se connecter à l’hôte de destination. 

Ce test illustre la connectivité à l’aide de couche 3 \(L3\) - ou la couche IP - ainsi \(L2\) couche 2.

Vous pouvez utiliser la commande Windows PowerShell suivante pour obtenir les propriétés de la carte réseau.

    Get-NetAdapter
     
Voici les résultats de l’exemple de cette commande.

|Nom|InterfaceDescription|ifIndex|État|Adresse MAC|Vitesse de la connexion|
|-----|--------------------|-------|-----|----------|---------|
|
|M1|Mellanox ConnectX-3 Pro...| 4| À distance|7C-FE-90-93-8F-A1|40Gbits/s|

Vous pouvez utiliser une des commandes suivantes pour obtenir les propriétés de la carte supplémentaires, notamment l’adresse IP.

    Get-NetAdapter M1 | fl *

Voici les résultats exemple modifié de cette commande.
    
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
    

### <a name="ensure-that-source-and-destination-can-communicate"></a>Assurez-vous que la Source et Destination peuvent communiquer

Vous pouvez utiliser cette étape pour vérifier la communication bidirectionnelle \ (ping à partir de la source à destination et vice versa sur les deux systèmes d’exploitation\).  Dans l’exemple suivant, le **Test-NetConnection** commande Windows PowerShell est utilisé, mais si vous préférez que vous pouvez utiliser le **ping** commande. 

>[!NOTE]
>Si vous êtes certain que vos ordinateurs hôtes peuvent communiquer entre eux, vous pouvez ignorer cette étape.

    Test-NetConnection 192.168.1.5

Voici les résultats de l’exemple de cette commande.

|Paramètre|Valeur|
|---------|-----|
|
|Nom_Ordinateur|192.168.1.5|
|RemoteAddress|192.168.1.5|
|InterfaceAlias|M1|
|Adresse source|192.168.1.3|
|PingSucceeded|True|
|PingReplyDetails \(RTT\)|0ms|

Dans certains cas, vous devrez peut-être désactiver le pare-feu Windows avec fonctions avancées de sécurité pour effectuer ce test. Si vous désactivez le pare-feu, n’oubliez pas sécurité et vérifiez que votre configuration répond aux exigences de sécurité de votre organisation.

La commande suivante vous permet de désactiver tous les profils de pare-feu.

    Set-NetFirewallProfile -All -Enabled False
    
Une fois que vous désactivez le pare-feu, vous pouvez utiliser la commande suivante pour tester la connexion.

    Test-NetConnection 192.168.1.5

Voici les résultats de l’exemple de cette commande.

|Paramètre|Valeur|
|---------|-----|
|
|Nom_Ordinateur|192.168.1.5|
|RemoteAddress|192.168.1.5|
|InterfaceAlias|Test-40G-1|
|Adresse source|192.168.1.3|
|PingSucceeded|False (faux)|
|PingReplyDetails \(RTT\)|0ms|

## <a name="configure-vlans-optional"></a>Configurer les réseaux locaux virtuels \(Optional\)

De nombreuses configurations réseau faire utiliser des réseaux locaux virtuels. Si vous envisagez d’utiliser des réseaux locaux virtuels dans votre réseau, vous devez répéter le test précédent avec les réseaux locaux virtuels configurés. \ (Si vous envisagez d’utiliser RoCE pour les services RDMA, vous devez activer VLAN. \)

Pour cette étape, les cartes réseau sont dans **accès** mode. Toutefois lorsque vous créez un commutateur virtuel Hyper-V de \(vSwitch\) plus loin dans ce guide, les propriétés de réseau local virtuel sont appliquées au niveau du port de commutateur virtuel. 

Dans la mesure où un commutateur peut héberger plusieurs réseaux locaux virtuels, il est nécessaire pour le commutateur physique de \(ToR\) Top of Rack pour que le port que l’ordinateur hôte est connecté à configuré en mode Trunk.

>[!NOTE]
>Pour obtenir des instructions sur la façon de configurer le mode Trunk sur le commutateur, consultez la documentation de votre commutateur ToR.

L’illustration suivante représente deux hôtes Hyper-V, chacun avec une carte réseau physique, et chacun est configuré pour communiquer sur le réseau local virtuel 101.

![Configurer les réseaux locaux virtuels](../../media/Converged-NIC/2-single-configure-vlans.jpg)

### <a name="configure-the-vlan-id"></a>Configurer l’ID de VLAN

Vous pouvez utiliser cette étape pour configurer les ID de réseau local virtuel pour les cartes réseau installées dans vos ordinateurs hôtes Hyper-V.

#### <a name="configure-nic-m1"></a>Configurer les cartes réseau M1

Avec les commandes suivantes, configurez l’ID de VLAN pour la première carte réseau, M1, puis afficher la configuration obtenue.

>[!IMPORTANT]
>N’exécutez pas cette commande si vous êtes connecté à l’ordinateur hôte à distance via cette interface, car cela entraînerait une perte de l’accès à l’ordinateur hôte.
    
    Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "101"
    Get-NetAdapterAdvancedProperty -Name M1 | Where-Object {$_.RegistryKeyword -eq "VlanID"} 
    
Voici les résultats de l’exemple de cette commande.

|Nom |DisplayName| Valeur d’affichage| RegistryKeyword |RegistryValue|
|----|-----------|------------|---------------|-------------|
|
|M1|ID DE VLAN|101|VlanID|{101}|


Assurez-vous que l’ID de VLAN prend effet indépendamment de l’implémentation de la carte réseau à l’aide de la commande suivante pour redémarrer la carte réseau.

    Restart-NetAdapter -Name "M1"

Vous pouvez utiliser la commande suivante pour vous assurer que l’état de la carte réseau est **des** avant de continuer.

    Get-NetAdapter -Name "M1"

Voici les résultats de l’exemple de cette commande.

|Nom|InterfaceDescription|ifIndex| État|Adresse MAC|Vitesse de la connexion|
|----|--------------------|-------|------|----------| ---------|
|
|M1|Ethernet Pro Mellanox ConnectX-3 Ada...|4|À distance|7C-FE-90-93-8F-A1|40Gbits/s|

Assurez-vous que vous effectuez cette opération sur les serveurs locaux et de destination. Si le serveur de destination n’est pas configuré avec le même ID de réseau local virtuel que le serveur local, les deux ne peut pas communiquer.

### <a name="verify-connectivity"></a>Vérifiez la connectivité

Vous pouvez utiliser cette section pour vérifier la connectivité une fois que les cartes réseau sont redémarrés. Vous pouvez confirmer la connectivité après avoir appliqué la balise VLAN pour les deux cartes. En cas d’échec de la connectivité, vous pouvez examiner la participation de configuration ou de destination de réseau local virtuel dans le même réseau local virtuel du commutateur. 

>[!IMPORTANT]
>Après avoir effectué les étapes décrites dans la section précédente, il peut prendre plusieurs secondes pour le périphérique pour redémarrer et sont disponibles sur le réseau.

#### <a name="verify-connectivity-for-nic-test-40g-1"></a>Vérifiez la connectivité des cartes réseau Test-40G-1

Pour vérifier la connectivité pour la première carte réseau, vous pouvez exécuter la commande suivante.

    Test-NetConnection 192.168.1.5
    
## <a name="configure-data-center-bridging-dcb"></a>Configurer Data Center Bridging \(DCB\)

L’étape suivante consiste à configurer \(QoS\) DCB et de qualité de Service, ce qui nécessite que tout d’abord installer la fonctionnalité de Windows Server2016 DCB.

>[!NOTE]
>Vous devez effectuer toutes les étapes de configuration suivantes DCB et de qualité de service sur tous les serveurs qui sont destinés à communiquer entre eux.

### <a name="install-data-center-bridging-dcb"></a>Installer Data Center Bridging \(DCB\)

Vous pouvez utiliser cette étape pour installer et activer DCB. 

>[!IMPORTANT]
>- Installation et configuration DCB sont **facultatif** pour les configurations réseau qui utilisent iWarp pour les services RDMA.
>- Installation et configuration DCB sont **requis** pour les configurations réseau qui utilisent RoCE \(any version\) pour les services RDMA.


Vous pouvez utiliser la commande suivante pour installer DCB sur chacun de vos ordinateurs hôtes Hyper-V. 

    Install-WindowsFeature Data-Center-Bridging

### <a name="set-the-qos-policies-for-smb-direct"></a>Définir les stratégies de QoS pour SMB Direct 

Vous pouvez utiliser la commande suivante pour configurer des stratégies de QoS pour SMB Direct.

>[!IMPORTANT]
>- Cette étape est facultative pour les configurations réseau qui utilisent iWarp.
>- Cette étape est nécessaire pour les configurations réseau qui utilisent RoCE.
>- Dans l’exemple de commande ci-dessous, la valeur «3» est arbitraire. Vous pouvez utiliser n’importe quelle valeur comprise entre 1 et 7 tant que vous utilisez régulièrement la même valeur dans l’ensemble de la configuration des stratégies de qualité de service.

    New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3

Voici les résultats de l’exemple de cette commande.

|Paramètre|Valeur|
|---------|-----|
|
|Nom |SMB|
|Propriétaire|\(Machine\) stratégie de groupe|
|NetworkProfile|Tous|
|Priorité|127|
|CreateJobObject|&nbsp;| 
|NetDirectPort|445
|PriorityValue|3

### <a name="for-roce-deployments-turn-on-priority-flow-control-for-smb-traffic"></a>Pour le RoCE déploiements activer le contrôle de flux de priorité pour le trafic SMB 

Si vous utilisez RoCE pour vos services RDMA, vous pouvez utiliser les commandes suivantes pour activer le contrôle de flux SMB et afficher les résultats. Contrôle de flux de priorité est obligatoire pour le RoCE, mais n’est pas nécessaire lorsque vous utilisez iWarp.

    Enable-NetQosFlowControl -priority 3
    Get-NetQosFlowControl

Voici les résultats de l’exemple de la **Get-NetQosFlowControl** commande.

|Priorité|Activé|PolicySet|IfIndex|IfAlias|
|---------|-----|--------- |-------| -------|
|
|0 |False (faux) |Global|&nbsp;|&nbsp;|
|1 |False (faux) |Global|&nbsp;|&nbsp;|
|2 |False (faux) |Global|&nbsp;|&nbsp;|
|3 |True  |Global|&nbsp;|&nbsp;|
|4 |False (faux) |Global|&nbsp;|&nbsp;|
|5 |False (faux) |Global|&nbsp;|&nbsp;|
|6 |False (faux) |Global|&nbsp;|&nbsp;|
|7 |False (faux) |Global|&nbsp;|&nbsp;|

### <a name="enable-qos-for-the-local-and-destination-network-adapters"></a>Activer la qualité de service pour les cartes réseau local et de destination
Avec cette étape, vous pouvez activer DCB sur les cartes réseau spécifique.

>[!IMPORTANT]
>-  Cette étape n’est pas nécessaire pour les configurations réseau qui utilisent iWarp.
>-  Cette étape est nécessaire pour les configurations réseau qui utilisent RoCE.




#### <a name="enable-qos-for-nic-m1"></a>Activer la qualité de service pour les cartes réseau M1

Vous pouvez utiliser les commandes suivantes pour activer la qualité de service et afficher les résultats de votre configuration.

    Enable-NetAdapterQos -InterfaceAlias "M1"
    Get-NetAdapterQos -Name "M1"

Voici les résultats de l’exemple de cette commande.

**Nom**: M1 **activé**: True **fonctionnalités**:   

|Paramètre|Matériel|En cours|
|---------|--------|-------|
|
|MacSecBypass|NotSupported|NotSupported|
|DcbxSupport|None|None|
|NumTCs(Max/ETS/PFC)|8/8/8|8/8/8|
 
**OperationalTrafficClasses**: 

|TC|TSA|Bande passante|Priorités|
|----|-----|--------|-------|
|
|0| ETS|70%|0-2,4-7|
|1|ETS|30%|3

**OperationalFlowControl**: priorité 3 activé **OperationalClassifications**:

|Protocole|Port/Type|Priorité|
|--------|---------|--------|
|
|Par défaut|&nbsp;|0|
|NetDirect| 445|3|

### <a name="reserve-a-percentage-of-the-bandwidth-for-smb-direct-rdma"></a>Réserver un pourcentage de la bande passante pour SMB Direct \(RDMA\)

Vous pouvez utiliser la commande suivante pour réserver un pourcentage de la bande passante pour SMB Direct.  

Dans cet exemple, une réservation de bande passante de 30% est utilisée. Vous devez sélectionner une valeur qui représente ce que vous attendez que nécessite votre trafic de stockage. La valeur de la **- bandwidthpercentage** paramètre doit être un multiple de 10%.

    New-NetQosTrafficClass "SMB" -Priority 3 -BandwidthPercentage 30 -Algorithm ETS

Voici les résultats de l’exemple de cette commande.

|Nom|Algorithme |Bandwidth(%)| Priorité |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|SMB | ETS     | 30 |3 |Global |&nbsp;|&nbsp;|                                      
 
Vous pouvez utiliser la commande suivante pour afficher des informations sur la réservation de bande passante.

    Get-NetQosTrafficClass

Voici les résultats de l’exemple de cette commande.
 
|Nom|Algorithme |Bandwidth(%)| Priorité |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|[Par défaut]|ETS|70 |0-2,4-7| Global|&nbsp;|&nbsp;| 
|SMB      |ETS|30 |3 |Global|&nbsp;|&nbsp;| 

## <a name="remove-debugger-conflict-mellanox-adapter-only"></a>Supprimer le débogueur conflit \(Mellanox adapter only\)

Si vous utilisez un adaptateur de Mellanox, vous devez effectuer cette étape pour configurer le débogueur. Par défaut, lorsqu’une carte Mellanox est utilisée, le débogueur attaché bloque NetQos. Vous pouvez utiliser la commande suivante pour remplacer le débogueur.

    
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
    

## <a name="test-rdma-native-host"></a>Test RDMA (hôte natif)

Vous pouvez utiliser cette étape pour vous assurer que l’infrastructure est correctement configuré avant la création d’un commutateur virtuel et la transition vers \(Converged NIC\) RDMA.

L’illustration suivante représente l’état actuel des ordinateurs hôtes Hyper-V.

![Test RDMA](../../media/Converged-NIC/4-single-test-rdma.jpg)

Pour vérifier la configuration RDMA, vous pouvez exécuter la commande suivante.

    Get-NetAdapterRdma

Voici les résultats de l’exemple de cette commande.

|Nom |InterfaceDescription |Activé|
|----|--------------------|-------|
|
|M1| Carte Ethernet Pro Mellanox ConnectX-3 |True|

### <a name="download-diskspdexe-and-a-powershell-script"></a>Téléchargement de DiskSpd.exe et un Script PowerShell

Pour continuer, vous devez d’abord télécharger les éléments suivants.

- Télécharger l’utilitaire de DiskSpd.exe et extraire l’utilitaire dans C:\TEST\ [Diskspd utilitaire: un stockage test outil robuste (remplacement SQLIO)](https://gallery.technet.microsoft.com/DiskSpd-a-robust-storage-6cd2f223)

- Télécharger le script powershell Test-RDMA à C:\TEST\[https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

### <a name="determine-the-ifindex-value-of-your-target-adapter"></a>Déterminer la valeur ifIndex de votre carte cible

Vous pouvez utiliser la commande suivante pour détecter la valeur ifIndex de la carte cible. Vous pouvez utiliser cette valeur dans les étapes suivantes lorsque vous exécutez le script que vous avez téléchargés.

    Get-NetIPConfiguration -InterfaceAlias "M*" | ft InterfaceAlias,InterfaceIndex,IPv4Address

Voici les résultats de l’exemple de cette commande.

|InterfaceAlias |InterfaceIndex |IPv4Address|
|-------------- |-------------- |-----------|
|
|M2 |14 |{192.168.1.5}|

### <a name="run-the-powershell-script"></a>Exécutez le script PowerShell

Lorsque vous exécutez le script Windows PowerShell de .ps1 Test-Rdma, vous pouvez transmettre la valeur ifIndex le script, ainsi que l’adresse IP de la carte à distance sur le même réseau local virtuel.

Vous pouvez utiliser la commande suivante pour exécuter le script avec une ifIndex 14 sur la carte réseau 192.168.1.5.
    
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
    

>[!NOTE]
>Si le trafic RDMA échoue, le cas RoCE plus précisément, consultez votre configuration ToR Switch pour les paramètres PFC/ETS appropriées qui doivent correspondre aux paramètres de l’ordinateur hôte. Reportez-vous à la section de la qualité de service dans ce document pour les valeurs de référence.

## <a name="remove-the-access-vlan-setting"></a>Supprimer le paramètre d’accès VLAN

En vue de créer le commutateur Hyper-V, vous devez supprimer les paramètres de réseau local virtuel que vous avez installé précédemment.  Vous pouvez utiliser la commande suivante pour supprimer le paramètre d’accès de réseau local virtuel à partir de la carte physique. Cette action empêche le balisage automatique le trafic sortant avec l’ID de VLAN incorrecte de la carte réseau et il empêche également de filtrage du trafic d’entrée qui ne correspond pas à l’ID de réseau local virtuel de l’accès.

    
    Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "0"
    

Vous pouvez utiliser la commande suivante pour confirmer le paramètre VlanID et afficher les résultats, qui indiquent que la valeur d’ID de réseau local virtuel est égal à zéro.

    
    Get-NetAdapterAdvancedProperty -name m1 | Where-Object {$_.RegistryKeyword -eq 'VlanID'} 
    


## <a name="create-a-hyper-v-virtual-switch"></a>Créer un commutateur virtuel Hyper-V

Vous pouvez utiliser cette section pour créer un \(vSwitch\) commutateur virtuel Hyper-V sur vos ordinateurs hôtes Hyper-V.

L’illustration suivante représente l’hôte 1 Hyper-V avec un commutateur virtuel.

![Créer un commutateur virtuel Hyper-V](../../media/Converged-NIC/5-single-create-vswitch.jpg)


### <a name="create-an-external-hyper-v-virtual-switch"></a>Créer un commutateur virtuel Hyper-V externe

Vous pouvez utiliser cette section pour créer un commutateur virtuel externe dans Hyper-V sur l’hôte Hyper-V A.

Vous pouvez utiliser la commande suivante pour créer un commutateur nommé VMSTEST.

>[!NOTE]
>Le paramètre **AllowManagementOS** dans la commande suivante crée une carte réseau virtuelle hôte qui hérite de l’adresse MAC et l’adresse IP de la carte physique.

    New-VMSwitch -Name VMSTEST -NetAdapterName "M1" -AllowManagementOS $true

Voici les résultats de l’exemple de cette commande.

|Nom |SwitchType |NetAdapterInterfaceDescription|
|---- |---------- |------------------------------|
|VMSTEST |Externe |Carte Ethernet Pro Mellanox ConnectX-3|

Vous pouvez utiliser la commande suivante pour afficher les propriétés de la carte réseau.

    Get-NetAdapter | ft -AutoSize

Voici les résultats de l’exemple de cette commande.

|Nom |InterfaceDescription | ifIndex |État |Adresse MAC |Vitesse de la connexion|
|---- |-------------------- |-------| ------|----------|---------|
|
|vEthernet \(VMSTEST\) |Carte Ethernet virtuelle Hyper-V #2|27 |À distance |E4-1D-2D-07-40-71 |40Gbits/s|


Vous pouvez gérer une carte réseau virtuelle hôte de deux manières. Est une méthode le **NetAdapter** vue, qui fonctionne en fonction de la «vEthernet \(VMSTEST\) «nom.

L’autre méthode est la **VMNetworkAdapter** mode, qui supprime le préfixe «vEthernet» et utilise simplement le nom du commutateur vmswitch.

Le **VMNetworkAdapter** affichage présente certaines propriétés de la carte réseau qui ne sont pas affichées avec le **NetAdapter** commande.

Vous pouvez utiliser la commande suivante pour afficher les résultats de la **VMNetworkAdapter** méthode.

    Get-VMNetworkAdapter –ManagementOS | ft -AutoSize

Voici les résultats de l’exemple de cette commande.

|Nom |IsManagementOs |VMName |SwitchName |Adresse MAC |État |AdressesIP|
|----|-------------- |------ |----------|----------|------ |-----------|
|
|Commutateur de CORP-externe |True |Commutateur de CORP-externe| 001B785768AA |{Ok} |&nbsp;|
|VMSTEST |True |VMSTEST | E41D2D074071| {Ok} | &nbsp;| 

### <a name="test-the-connection"></a>Tester la connexion

Vous pouvez utiliser la commande suivante pour tester la connexion et afficher les résultats.
    
    Test-NetConnection 192.168.1.5

    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (CORP-External-Switch)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    
Vous pouvez utiliser les exemples de commandes suivantes pour attribuer et afficher les paramètres de réseau local virtuel de carte réseau.
    
    Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
    Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
    

|VMName |VMNetworkAdapterName |Mode |VlanList|
|------ |-------------------- |---- |--------|
|
|&nbsp;|VMSTEST |Accès |101     
 
### <a name="test-the-connection"></a>Tester la connexion

N’oubliez pas que la modification peut prendre quelques secondes avant que l’autre carte ping.

Vous pouvez utiliser la commande suivante pour tester la connexion et afficher les résultats.
    
    Test-NetConnection 192.168.1.5
     
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (VMSTEST)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    

## <a name="test-hyper-v-virtual-switch-rdma-mode-2"></a>Tester le commutateur virtuel Hyper-V RDMA (Mode 2)

L’illustration suivante représente l’état actuel de vos ordinateurs hôtes Hyper-V, y compris le commutateur virtuel sur l’hôte 1 Hyper-V.

![Testez le commutateur virtuel Hyper-V](../../media/Converged-NIC/6-single-test-vswitch-rdma.jpg)


### <a name="set-priority-tagging-on-the-host-vnic"></a>Définir la priorité de marquage sur la carte réseau virtuelle hôte

Vous pouvez utiliser les exemples de commandes suivantes pour définir la priorité de marquage sur la carte réseau virtuelle hôte et afficher les résultats de vos actions.
    
    Set-VMNetworkAdapter -ManagementOS -Name "VMSTEST" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "VMSTEST" | fl Name,IeeePriorityTag
    
    Name: VMSTEST
    IeeePriorityTag : On
    
Vous pouvez utiliser la commande suivante pour afficher la carte réseau informations RDMA. Dans les résultats, lorsque le paramètre **activé** a la valeur **False**, cela signifie que le RDMA n’est pas activé.
    
    Get-NetAdapterRdma
    
|Nom |InterfaceDescription |Activé |
|---- |-------------------- |-------|
|
|vEthernet \(VMSTEST\)| Carte Ethernet virtuelle Hyper-V #2|False (faux)|

### <a name="enable-rdma-on-the-host-vnic"></a>Activez RDMA sur la carte réseau virtuelle hôte

Vous pouvez utiliser les exemples de commandes suivants pour afficher les propriétés de la carte réseau, activer RDMA pour la carte, puis afficher la carte réseau informations RDMA.
    
    Get-NetAdapter
    
|Nom |InterfaceDescription |ifIndex |État |Adresse MAC |Vitesse de la connexion|
|----|--------------------|-------|------|----------|---------|
|
|vEthernet (VMSTEST)|Carte Ethernet virtuelle Hyper-V #2|27|À distance|E4-1D-2D-07-40-71|40Gbits/s|

    Enable-NetAdapterRdma -Name "vEthernet (VMSTEST)"
    Get-NetAdapterRdma -Name "vEthernet (VMSTEST)"

Dans les résultats suivants, lorsque le paramètre **activé** a la valeur **True**, cela signifie que le RDMA est activé.

|Nom |InterfaceDescription |Activé |
|---- |-------------------- |-------|
|
|vEthernet \(VMSTEST\)| Carte Ethernet virtuelle Hyper-V #2|True|


    
    Get-NetAdapter 
    

|Nom |InterfaceDescription |ifIndex |État |Adresse MAC |Vitesse de la connexion|
|----|--------------------|-------|------|----------|---------|
|
|vEthernet (VMSTEST)|Carte Ethernet virtuelle Hyper-V #2|27|À distance|E4-1D-2D-07-40-71|40Gbits/s|

### <a name="perform-rdma-traffic-test-by-using-the-script"></a>Effectuer un test de trafic RDMA en utilisant le script

Vous pouvez utiliser la commande suivante pour exécuter le script que vous avez téléchargé et afficher les résultats.
    
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
    
La dernière ligne dans cette sortie, «test de trafic RDMA réussite: trafic RDMA a été envoyé à 192.168.1.5, «montre que vous avez correctement configuré NIC convergée sur votre carte.

## <a name="all-topics-in-this-guide"></a>Toutes les rubriques de ce guide

Ce guide contient les rubriques suivantes.

- [Configuration des cartes réseau convergé avec une seule carte réseau](cnic-single.md)
- [Configuration des cartes réseau associées de cartes réseau convergé](cnic-datacenter.md)
- [Configuration du commutateur physique pour les cartes réseau convergé](cnic-app-switch-config.md)
- [Résolution des problèmes convergent des Configurations de cartes réseau](cnic-app-troubleshoot.md)
