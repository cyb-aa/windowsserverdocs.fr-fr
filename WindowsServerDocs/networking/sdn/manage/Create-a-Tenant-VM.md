---
title: Créer une machine virtuelle et établir la connexion à un réseau virtuel locataire ou un réseau local virtuel
description: Dans cette rubrique, nous vous montrons comment créer une machine virtuelle cliente et la connecter à un réseau virtuel que vous avez créé avec la virtualisation de réseau Hyper-V ou à un réseau local virtuel (VLAN).
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c62f533-1815-4f08-96b1-dc271f5a2b36
ms.author: lizross
author: eross-msft
ms.date: 08/24/2018
ms.openlocfilehash: ef588cfc93216f13490ef3196ec0990b9e7f48d3
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309794"
---
# <a name="create-a-vm-and-connect-to-a-tenant-virtual-network-or-vlan"></a>Créer une machine virtuelle et établir la connexion à un réseau virtuel locataire ou un réseau local virtuel

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette rubrique, vous allez créer une machine virtuelle cliente et la connecter à un réseau virtuel que vous avez créé avec la virtualisation de réseau Hyper-V ou à un réseau local virtuel (VLAN). Vous pouvez utiliser les applets de commande du contrôleur de réseau Windows PowerShell pour vous connecter à un réseau virtuel ou à NetworkControllerRESTWrappers pour vous connecter à un réseau local virtuel.

Utilisez les processus décrits dans cette rubrique pour déployer des appliances virtuelles. En quelques étapes supplémentaires, vous pouvez configurer des appliances pour traiter ou inspecter des paquets de données qui circulent vers ou à partir d’autres machines virtuelles sur le réseau virtuel.

Les sections de cette rubrique incluent des exemples de commandes Windows PowerShell qui contiennent des exemples de valeurs pour de nombreux paramètres. Veillez à remplacer les valeurs d’exemple dans ces commandes par des valeurs appropriées pour votre déploiement avant d’exécuter ces commandes. 


## <a name="prerequisites"></a>Composants requis

1. Cartes réseau d’ordinateurs virtuels créées avec des adresses MAC statiques pendant la durée de vie de la machine virtuelle.<p>Si l’adresse MAC change au cours de la durée de vie de la machine virtuelle, le contrôleur de réseau ne peut pas configurer la stratégie nécessaire pour la carte réseau. Si vous ne configurez pas la stratégie pour le réseau, cela empêche la carte réseau de traiter le trafic réseau, et toutes les communications avec le réseau échouent.  

2. Si la machine virtuelle nécessite un accès réseau au démarrage, ne démarrez pas la machine virtuelle avant d’avoir défini l’ID d’interface sur le port de carte réseau d’ordinateur virtuel. Si vous démarrez la machine virtuelle avant de définir l’ID d’interface et que l’interface réseau n’existe pas, la machine virtuelle ne peut pas communiquer sur le réseau dans le contrôleur de réseau et toutes les stratégies sont appliquées.

3. Si vous avez besoin de listes de contrôle d’accès (ACL) personnalisées pour cette interface réseau, créez-la maintenant à l’aide des instructions de la rubrique [utiliser des listes de Access Control pour gérer le flux de trafic réseau du centre de](../../sdn/manage/Use-Access-Control-Lists--ACLs--to-Manage-Datacenter-Network-Traffic-Flow.md) données

Vérifiez que vous avez déjà créé un réseau virtuel avant d’utiliser cet exemple de commande. Pour plus d’informations, consultez [créer, supprimer ou mettre à jour des réseaux virtuels locataires](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create%2c-delete%2c-or-update-tenant-virtual-networks).

## <a name="create-a-vm-and-connect-to-a-virtual-network-by-using-the-windows-powershell-network-controller-cmdlets"></a>Créer une machine virtuelle et se connecter à un réseau virtuel à l’aide des applets de commande du contrôleur de réseau Windows PowerShell


1. Créez une machine virtuelle avec une carte réseau d’ordinateur virtuel avec une adresse MAC statique. 

   ```PowerShell    
   New-VM -Generation 2 -Name "MyVM" -Path "C:\VMs\MyVM" -MemoryStartupBytes 4GB -VHDPath "c:\VMs\MyVM\Virtual Hard Disks\WindowsServer2016.vhdx" -SwitchName "SDNvSwitch" 
    
   Set-VM -Name "MyVM" -ProcessorCount 4
    
   Set-VMNetworkAdapter -VMName "MyVM" -StaticMacAddress "00-11-22-33-44-55" 
   ```

2. Récupérez le réseau virtuel qui contient le sous-réseau auquel vous souhaitez connecter la carte réseau.

   ```Powershell 
   $vnet = get-networkcontrollervirtualnetwork -connectionuri $uri -ResourceId “Contoso_WebTier”
   ```

3. Créez un objet d’interface réseau dans le contrôleur de réseau.

   >[!TIP]
   >Dans cette étape, vous utilisez l’ACL personnalisée.

   ```PowerShell
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
   ```

4. Récupérez l’InstanceId de l’interface réseau à partir du contrôleur de réseau.

   ```PowerShell 
    $nic = Get-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId "MyVM-Ethernet1"
   ```

5. Définissez l’ID d’interface sur le port de carte réseau d’ordinateur virtuel Hyper-V.

   >[!NOTE]
   >Vous devez exécuter ces commandes sur l’hôte Hyper-V sur lequel la machine virtuelle est installée.

   ```PowerShell 
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
   ```

6. Démarrez la machine virtuelle.

   ```PowerShell
    Get-VM -Name “MyVM” | Start-VM 
   ```

Vous venez de créer une machine virtuelle, de connecter la machine virtuelle à un réseau virtuel locataire et de démarrer la machine virtuelle afin qu’elle puisse traiter les charges de travail des locataires.

## <a name="create-a-vm-and-connect-to-a-vlan-by-using-networkcontrollerrestwrappers"></a>Créer une machine virtuelle et se connecter à un réseau local virtuel à l’aide de NetworkControllerRESTWrappers


1. Créez la machine virtuelle et affectez une adresse MAC statique à la machine virtuelle.

   ```PowerShell
   New-VM -Generation 2 -Name "MyVM" -Path "C:\VMs\MyVM" -MemoryStartupBytes 4GB -VHDPath "c:\VMs\MyVM\Virtual Hard Disks\WindowsServer2016.vhdx" -SwitchName "SDNvSwitch" 

   Set-VM -Name "MyVM" -ProcessorCount 4

   Set-VMNetworkAdapter -VMName "MyVM" -StaticMacAddress "00-11-22-33-44-55" 
   ```

2. Définissez l’ID de réseau local virtuel sur la carte réseau de machine virtuelle.

   ```PowerShell
   Set-VMNetworkAdapterIsolation –VMName “MyVM” -AllowUntaggedTraffic $true -IsolationMode VLAN -DefaultIsolationId 123
   ```

3. Récupérez le sous-réseau de réseau logique et créez l’interface réseau. 

   ```PowerShell
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
   ```

4. Définissez l’InstanceId sur le port Hyper-V.

   ```PowerShell  
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
   ```

5. Démarrez la machine virtuelle.

   ```PowerShell
   Get-VM -Name “MyVM” | Start-VM 
   ```

Vous venez de créer une machine virtuelle, de connecter la machine virtuelle à un réseau local virtuel et de démarrer la machine virtuelle afin qu’elle puisse traiter les charges de travail des locataires.

  

