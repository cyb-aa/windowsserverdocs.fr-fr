---
title: Créer un ordinateur virtuel et vous connecter à un client virtuel réseau ou réseau local virtuel
description: Cette rubrique fait partie du guide logiciel défini de mise en réseau sur la façon de gérer les charges de travail clientes et des réseaux virtuels dans Windows Server2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c62f533-1815-4f08-96b1-dc271f5a2b36
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 001eb3efa073e4ffbdfad41f88949303159a7274
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-vm-and-connect-to-a-tenant-virtual-network-or-vlan"></a>Créer un ordinateur virtuel et vous connecter à un client virtuel réseau ou réseau local virtuel

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour créer un client virtuel \(VM\) de l’ordinateur et de connecter l’ordinateur virtuel à un réseau virtuel que vous avez créé avec la virtualisation de réseau Hyper-V ou à un \(VLAN\) réseau Local virtuel. 

Cette rubrique contient les sections suivantes.

- [Créer un ordinateur virtuel et vous connecter à un réseau virtuel en utilisant les applets de commande du contrôleur de réseau de Windows PowerShell](#bkmk_vn)
- [Créer un ordinateur virtuel et vous connecter à un réseau local virtuel à l’aide de NetworkControllerRESTWrappers](#bkmk_vlan)

## <a name="requirements"></a>Configuration requise
Avant d’effectuer les procédures décrites dans les sections suivantes, notez les exigences suivantes.

1. Vous devez créer des cartes réseau de machine virtuelle avec un support statique le contrôle d’accès que \(mac\) adresses afin que l’adresse MAC de l’ordinateur virtuel ne change pas pendant la durée de vie de machine virtuelle. 
>[!NOTE]
>Si l’adresse MAC de l’ordinateur virtuel est modifiée au cours de la durée de vie de machine virtuelle, le contrôleur de réseau ne peut pas configurer la stratégie nécessaire pour la carte réseau. Si la stratégie pour la carte réseau n’est pas configurée, la carte réseau ne peut pas traiter le trafic réseau, et toutes les communications avec le réseau échoue. 

2. Si l’ordinateur virtuel requiert un accès réseau au démarrage, il est important que vous ne démarrez pas l’ordinateur virtuel qu’après la dernière étape de configuration - configuration de l’ID d’Interface sur l’ordinateur virtuel port de carte réseau. Si vous démarrez l’ordinateur virtuel avant d’effectuer cette étape, l’ordinateur virtuel ne peut pas communiquer sur le réseau jusqu'à ce que l’interface réseau est créé dans le contrôleur de réseau et le contrôleur a appliqué à toutes les stratégies applicables - stratégie de réseau virtuel, \(ACLs\) et la qualité de service \(QoS\) des listes de contrôle d’accès.

Vous pouvez également utiliser les processus décrits dans cette rubrique pour le déploiement d’équipements virtuels. En quelques étapes supplémentaires, vous pouvez configurer des appareils pour traiter ou inspecter les paquets de données de flux vers ou à partir d’autres ordinateurs virtuels sur le réseau virtuel.

>[!IMPORTANT]
>Les sections suivantes incluent des exemples de commandes Windows PowerShell qui contiennent des exemples de valeurs pour un grand nombre de paramètres. Assurez-vous que vous remplacez les exemples de valeurs de ces commandes par les valeurs appropriées pour votre déploiement avant d’exécuter ces commandes.  

## <a name="bkmk_vn"></a>Créer un ordinateur virtuel et vous connecter à un réseau virtuel en utilisant les applets de commande du contrôleur de réseau de Windows PowerShell

Cette section comprend les rubriques suivantes.

1.  [Créer un ordinateur virtuel avec une carte réseau virtuelle qui possède une adresse MAC statique](#bkmk_create)
2.  [Obtenir le réseau virtuel qui contient le sous-réseau auquel vous souhaitez connecter la carte réseau](#bkmk_getvn)
3.  [Créer un objet d’interface réseau dans le contrôleur de réseau](#bkmk_object)
4.  [Obtenir l’ID d’instance de l’interface réseau à partir du contrôleur de réseau](#bkmk_getinstance)
5.  [Définir l’ID d’Interface sur l’ordinateur virtuel Hyper-V port de carte réseau](#bkmk_setinstance)
6.  [Démarrer l’ordinateur virtuel](#bkmk_start)

### <a name="bkmk_create"></a>Créer un ordinateur virtuel avec une carte réseau virtuelle qui possède une adresse MAC statique

Pour créer un ordinateur virtuel avec une carte réseau qui a une adresse MAC statique, utilisez la commande suivante.

    
    New-VM -Generation 2 -Name "MyVM" -Path "C:\VMs\MyVM" -MemoryStartupBytes 4GB -VHDPath "c:\VMs\MyVM\Virtual Hard Disks\WindowsServer2016.vhdx" -SwitchName "SDNvSwitch" 
    
    Set-VM -Name "MyVM" -ProcessorCount 4
    
    Set-VMNetworkAdapter -VMName "MyVM" -StaticMacAddress "00-11-22-33-44-55" 
    

### <a name="bkmk_getvn"></a>Obtenir le réseau virtuel qui contient le sous-réseau auquel vous souhaitez connecter la carte réseau
Assurez-vous que vous avez déjà créé un réseau virtuel avant d’utiliser cet exemple de commande. Pour plus d’informations, voir [créer, supprimer ou mise à jour des réseaux virtuels locataires](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create%2c-delete%2c-or-update-tenant-virtual-networks).

Pour obtenir le réseau virtuel, utilisez la commande suivante.

    
    $vnet = get-networkcontrollervirtualnetwork -connectionuri $uri -ResourceId “Contoso_WebTier”
    

>[!NOTE]
>Si vous avez besoin d’ACL personnalisé pour cette interface réseau, puis créer la liste ACL désormais à l’aide des instructions dans la rubrique [utilisation Access Control Lists (ACL) pour gérer les centres de données réseau le flux de trafic](../../sdn/manage/Use-Access-Control-Lists--ACLs--to-Manage-Datacenter-Network-Traffic-Flow.md)

### <a name="bkmk_object"></a>Créer un objet d’interface réseau dans le contrôleur de réseau

Pour créer un objet d’interface réseau dans le contrôleur de réseau, utilisez la commande suivante.

>[!NOTE]
>Si vous avez créé une liste ACL personnalisée après l’étape précédente, vous pouvez l’utiliser.

    
    $vmnicproperties = new-object Microsoft.Windows.NetworkController.NetworkInterfaceProperties
    $vmnicproperties.PrivateMacAddress = "001122334455" 
    $vmnicproperties.PrivateMacAllocationMethod = "Static" 
    $vmnicproperties.IsPrimary = $true 

    $vmnicproperties.DnsSettings = new-object Microsoft.Windows.NetworkController.NetworkInterfaceDnsSettings
    $vmnicproperties.DnsSettings.DnsServers = @("24.30.1.11", "24.30.1.12")
    
    $ipconfiguration = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfiguration
    $ipconfiguration.resourceid = "MyVM_IP1"
    $ipconfiguration.properties = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfigurationProperties
    $ipconfiguration.properties.PrivateIPAddress = “24.30.1.101”
    $ipconfiguration.properties.PrivateIPAllocationMethod = "Static"
    
    $ipconfiguration.properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $ipconfiguration.properties.subnet.ResourceRef = $vnet.Properties.Subnets[0].ResourceRef
    
    $vmnicproperties.IpConfigurations = @($ipconfiguration)
    New-NetworkControllerNetworkInterface –ResourceID “MyVM_Ethernet1” –Properties $vmnicproperties –ConnectionUri $uri
    

### <a name="bkmk_getinstance"></a>Obtenir l’ID d’instance de l’interface réseau à partir du contrôleur de réseau
Pour obtenir l’ID d’instance de l’interface réseau à partir du contrôleur de réseau, utilisez la commande suivante.

    
    $nic = Get-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId "MyVM-Ethernet1"
    

### <a name="bkmk_setinstance"></a>Définir l’ID d’Interface sur l’ordinateur virtuel Hyper-V port de carte réseau
Pour définir l’ID d’Interface sur l’ordinateur virtuel Hyper-V de port de carte réseau, utilisez la commande suivante.

>[!NOTE]
>Vous devez exécuter ces commandes sur l’ordinateur hôte Hyper-V sur lequel l’ordinateur virtuel est installé.

    
    #Do not change the hardcoded IDs in this section, because they are fixed values and must not change.
    
    $FeatureId = "9940cd46-8b06-43bb-b9d5-93d50381fd56"
    
    $vmNics = Get-VMNetworkAdapter -VMName “MyVM”
    
    $CurrentFeature = Get-VMSwitchExtensionPortFeature -FeatureId $FeatureId -VMNetworkAdapter $vmNics
    
    if ($CurrentFeature -eq $null)
    {
    $Feature = Get-VMSystemSwitchExtensionPortFeature -FeatureId $FeatureId
    
    $Feature.SettingData.ProfileId = "{$($nic.InstanceId)}"
    $Feature.SettingData.NetCfgInstanceId = "{56785678-a0e5-4a26-bc9b-c0cba27311a3}"
    $Feature.SettingData.CdnLabelString = "TestCdn"
    $Feature.SettingData.CdnLabelId = 1111
    $Feature.SettingData.ProfileName = "Testprofile"
    $Feature.SettingData.VendorId = "{1FA41B39-B444-4E43-B35A-E1F7985FD548}"
    $Feature.SettingData.VendorName = "NetworkController"
    $Feature.SettingData.ProfileData = 1
    
    Add-VMSwitchExtensionPortFeature -VMSwitchExtensionFeature  $Feature -VMNetworkAdapter $vmNics
    }
    else
    {
    $CurrentFeature.SettingData.ProfileId = "{$($nic.InstanceId)}"
    $CurrentFeature.SettingData.ProfileData = 1
    
    Set-VMSwitchExtensionPortFeature -VMSwitchExtensionFeature $CurrentFeature  -VMNetworkAdapter $vmNic
    }
    

### <a name="bkmk_start"></a>Démarrer l’ordinateur virtuel
Pour démarrer l’ordinateur virtuel, utilisez la commande suivante.

    
    Get-VM -Name “MyVM” | Start-VM 
    
Vous disposez désormais correctement créé un ordinateur virtuel, connecté l’ordinateur virtuel à un réseau virtuel locataire et démarré l’ordinateur virtuel afin qu’il puisse traiter des charges de travail clientes.

## <a name="bkmk_vlan"></a>Créer un ordinateur virtuel et vous connecter à un réseau local virtuel à l’aide de NetworkControllerRESTWrappers

Cette section comprend les rubriques suivantes.

1. [Créer l’ordinateur virtuel et attribuer une adresse MAC statique](#bkmk_mac)
2. [Définir l’ID de VLAN sur la carte réseau d’ordinateur virtuel](#bkmk_vid)
3. [Obtenir le sous-réseau de réseau logique et créer l’interface réseau](#bkmk_subnet)
4. [Définir l’ID d’instance sur le port Hyper-V](#bkmk_instance)
5. [Démarrer l’ordinateur virtuel](#bkmk_startvlan)


###<a name="bkmk_mac"></a>Créer l’ordinateur virtuel et attribuer une adresse MAC statique
Pour créer un ordinateur virtuel et attribuer un support statique \(MAC\) adresse de contrôle d’accès à l’ordinateur virtuel, vous pouvez utiliser les exemples de commandes suivants.

    New-VM -Generation 2 -Name "MyVM" -Path "C:\VMs\MyVM" -MemoryStartupBytes 4GB -VHDPath "c:\VMs\MyVM\Virtual Hard Disks\WindowsServer2016.vhdx" -SwitchName "SDNvSwitch" 

    Set-VM -Name "MyVM" -ProcessorCount 4

    Set-VMNetworkAdapter -VMName "MyVM" -StaticMacAddress "00-11-22-33-44-55" 

###<a name="bkmk_vid"></a>Définir l’ID de VLAN sur la carte réseau d’ordinateur virtuel
Pour définir l’ID de VLAN sur la carte réseau, vous pouvez utiliser la commande suivante.


    Set-VMNetworkAdapterIsolation –VMName “MyVM” -AllowUntaggedTraffic $true -IsolationMode VLAN -DefaultIsolationId 123


###<a name="bkmk_subnet"></a>Obtenir le sous-réseau de réseau logique et créer l’interface réseau

Pour obtenir le sous-réseau de réseau logique et créer l’interface réseau à l’aide du sous-réseau de réseau logique, vous pouvez utiliser les exemples de commandes suivants.


    $logicalnet = get-networkcontrollerLogicalNetwork -connectionuri $uri -ResourceId "00000000-2222-1111-9999-000000000002"

    $vmnicproperties = new-object Microsoft.Windows.NetworkController.NetworkInterfaceProperties
    $vmnicproperties.PrivateMacAddress = "00-1D-C8-B7-01-02"
    $vmnicproperties.PrivateMacAllocationMethod = "Static"
    $vmnicproperties.IsPrimary = $true 
    
    $vmnicproperties.DnsSettings = new-object Microsoft.Windows.NetworkController.NetworkInterfaceDnsSettings
    $vmnicproperties.DnsSettings.DnsServers = $logicalnet.Properties.Subnets[0].DNSServers

    $ipconfiguration = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfiguration
    $ipconfiguration.resourceid = "MyVM_Ip1"
    $ipconfiguration.properties = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfigurationProperties
    $ipconfiguration.properties.PrivateIPAddress = “10.127.132.177”
    $ipconfiguration.properties.PrivateIPAllocationMethod = "Static"

    $ipconfiguration.properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $ipconfiguration.properties.subnet.ResourceRef = $logicalnet.Properties.Subnets[0].ResourceRef

    $vmnicproperties.IpConfigurations = @($ipconfiguration)
    $vnic = New-NetworkControllerNetworkInterface –ResourceID “MyVM_Ethernet1” –Properties $vmnicproperties –ConnectionUri $uri

    $vnic.InstanceId
    

###<a name="bkmk_instance"></a>Définir l’ID d’instance sur le port Hyper-V
Pour définir l’ID d’instance sur le port Hyper-V, vous pouvez utiliser les exemples de commandes suivants sur l’ordinateur hôte Hyper-V sur lequel se trouve l’ordinateur virtuel.

  
    #The hardcoded Ids in this section are fixed values and must not change.
    $FeatureId = "9940cd46-8b06-43bb-b9d5-93d50381fd56"

    $vmNics = Get-VMNetworkAdapter -VMName “MyVM”

    $CurrentFeature = Get-VMSwitchExtensionPortFeature -FeatureId $FeatureId -VMNetworkAdapter $vmNic
        
    if ($CurrentFeature -eq $null)
    {
        $Feature = Get-VMSystemSwitchExtensionFeature -FeatureId $FeatureId
        
        $Feature.SettingData.ProfileId = "{$InstanceId}"
        $Feature.SettingData.NetCfgInstanceId = "{56785678-a0e5-4a26-bc9b-c0cba27311a3}"
        $Feature.SettingData.CdnLabelString = "TestCdn"
        $Feature.SettingData.CdnLabelId = 1111
        $Feature.SettingData.ProfileName = "Testprofile"
        $Feature.SettingData.VendorId = "{1FA41B39-B444-4E43-B35A-E1F7985FD548}"
        $Feature.SettingData.VendorName = "NetworkController"
        $Feature.SettingData.ProfileData = 1
                
        Add-VMSwitchExtensionFeature -VMSwitchExtensionFeature  $Feature -VMNetworkAdapter $vmNic
    }        
    else
    {
        $CurrentFeature.SettingData.ProfileId = "{$InstanceId}"
        $CurrentFeature.SettingData.ProfileData = 1

        Set-VMSwitchExtensionPortFeature -VMSwitchExtensionFeature $CurrentFeature  -VMNetworkAdapter $vmNic
    }


###<a name="bkmk_startvlan"></a>Démarrer l’ordinateur virtuel
Pour démarrer l’ordinateur virtuel, vous pouvez utiliser la commande suivante.


    Get-VM -Name “MyVM” | Start-VM 

Vous disposez désormais correctement créé un ordinateur virtuel, connecté l’ordinateur virtuel à un réseau local virtuel et démarré l’ordinateur virtuel afin qu’il puisse traiter des charges de travail clientes.

  

