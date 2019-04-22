---
title: Configurer une homologation de réseau virtuel
description: Configuration de l’homologation de réseau virtuel implique la création de deux réseaux virtuels homologués l’obtient.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: 3ef3db879080e3372e7b287dcc55ae052c1fe109
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816390"
---
# <a name="configure-virtual-network-peering"></a>Configurer une homologation de réseau virtuel

>S’applique à : Windows Server

Dans cette procédure, vous utilisez Windows PowerShell pour créer deux réseaux virtuels, chacun avec un sous-réseau. Ensuite, vous configurez l’homologation entre les deux réseaux virtuels pour activer la connectivité entre eux.

- [Étape 1. Créer le premier réseau virtuel](#step-1-create-the-first-virtual-network)

- [Étape 2. Créer le deuxième réseau virtuel](#step-2-create-the-second-virtual-network)

- [Étape 3. Configurer l’homologation du premier réseau virtuel vers le second réseau virtuel](#step-3-configure-peering-from-the-first-virtual-network-to-the-second-virtual-network)

- [Étape 4. Configurer l’homologation à partir du deuxième réseau virtuel au premier réseau virtuel](#step-4-configure-peering-from-the-second-virtual-network-to-the-first-virtual-network)


>[!IMPORTANT]
>Mettez à jour les propriétés de votre environnement.

## <a name="step-1-create-the-first-virtual-network"></a>Étape 1. Créer le premier réseau virtuel

Dans cette étape, vous utilisez Windows PowerShell recherche le réseau logique de fournisseur HNV pour créer le premier réseau virtuel avec un sous-réseau. L’exemple de script suivant crée un réseau virtuel de Contoso avec un sous-réseau.

``` PowerShell
#Find the HNV Provider Logical Network  

$logicalnetworks = Get-NetworkControllerLogicalNetwork -ConnectionUri $uri  
foreach ($ln in $logicalnetworks) {  
   if ($ln.Properties.NetworkVirtualizationEnabled -eq "True") {  
      $HNVProviderLogicalNetwork = $ln  
   }  
}   

#Create the Virtual Subnet  

$vsubnet = new-object Microsoft.Windows.NetworkController.VirtualSubnet  
$vsubnet.ResourceId = "Contoso"  
$vsubnet.Properties = new-object Microsoft.Windows.NetworkController.VirtualSubnetProperties  
$vsubnet.Properties.AddressPrefix = "24.30.1.0/24"
$uri=”https://restserver”  

#Create the Virtual Network  

$vnetproperties = new-object Microsoft.Windows.NetworkController.VirtualNetworkProperties  
$vnetproperties.AddressSpace = new-object Microsoft.Windows.NetworkController.AddressSpace  
$vnetproperties.AddressSpace.AddressPrefixes = @("24.30.1.0/24")  
$vnetproperties.LogicalNetwork = $HNVProviderLogicalNetwork  
$vnetproperties.Subnets = @($vsubnet)  
New-NetworkControllerVirtualNetwork -ResourceId "Contoso_VNet1" -ConnectionUri $uri -Properties $vnetproperties
```

## <a name="step-2-create-the-second-virtual-network"></a>Étape 2. Créer le deuxième réseau virtuel

Dans cette étape, vous créez un deuxième réseau virtuel avec un sous-réseau. L’exemple de script suivant crée un réseau virtuel de la Woodgrove Bank avec un sous-réseau.

``` PowerShell

#Create the Virtual Subnet  

$vsubnet = new-object Microsoft.Windows.NetworkController.VirtualSubnet  
$vsubnet.ResourceId = "Woodgrove"  
$vsubnet.Properties = new-object Microsoft.Windows.NetworkController.VirtualSubnetProperties  
$vsubnet.Properties.AddressPrefix = "24.30.2.0/24"  
$uri=”https://restserver”

#Create the Virtual Network  

$vnetproperties = new-object Microsoft.Windows.NetworkController.VirtualNetworkProperties  
$vnetproperties.AddressSpace = new-object Microsoft.Windows.NetworkController.AddressSpace  
$vnetproperties.AddressSpace.AddressPrefixes = @("24.30.2.0/24")  
$vnetproperties.LogicalNetwork = $HNVProviderLogicalNetwork  
$vnetproperties.Subnets = @($vsubnet)  
New-NetworkControllerVirtualNetwork -ResourceId "Woodgrove_VNet1" -ConnectionUri $uri -Properties $vnetproperties
```

## <a name="step-3-configure-peering-from-the-first-virtual-network-to-the-second-virtual-network"></a>Étape 3. Configurer l’homologation du premier réseau virtuel vers le second réseau virtuel

Dans cette étape, vous configurez l’homologation entre le premier réseau virtuel et le second réseau virtuel que vous avez créé dans les deux étapes précédentes. L’exemple de script suivant établit l’homologation de réseaux virtuels à partir de **Contoso_vnet1** à **Woodgrove_vnet1**.

```PowerShell
$peeringProperties = New-Object Microsoft.Windows.NetworkController.VirtualNetworkPeeringProperties
$vnet2 = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Woodgrove_VNet1"
$peeringProperties.remoteVirtualNetwork = $vnet2

#Indicate whether communication between the two virtual networks
$peeringProperties.allowVirtualnetworkAccess = $true

#Indicates whether forwarded traffic is allowed across the vnets
$peeringProperties.allowForwardedTraffic = $true

#Indicates whether the peer virtual network can access this virtual networks gateway
$peeringProperties.allowGatewayTransit = $false

#Indicates whether this virtual network uses peer virtual networks gateway
$peeringProperties.useRemoteGateways =$false

New-NetworkControllerVirtualNetworkPeering -ConnectionUri $uri -VirtualNetworkId “Contoso_vnet1” -ResourceId “ContosotoWoodgrove” -Properties $peeringProperties

```

>[!IMPORTANT]
>Après avoir créé cette homologation, affiche l’état du réseau virtuel **initiée**.

## <a name="step-4-configure-peering-from-the-second-virtual-network-to-the-first-virtual-network"></a>Étape 4. Configurer l’homologation à partir du deuxième réseau virtuel au premier réseau virtuel

Dans cette étape, vous configurez l’homologation entre le deuxième réseau virtuel et le premier réseau virtuel que vous avez créé aux étapes 1 et 2 ci-dessus. L’exemple de script suivant établit l’homologation de réseaux virtuels à partir de **Woodgrove_vnet1** à **Contoso_vnet1**.

```PowerShell
$peeringProperties = New-Object Microsoft.Windows.NetworkController.VirtualNetworkPeeringProperties 
$vnet2=Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Contoso_VNet1"
$peeringProperties.remoteVirtualNetwork = $vnet2 

# Indicates whether communication between the two virtual networks is allowed 
$peeringProperties.allowVirtualnetworkAccess = $true 

# Indicates whether forwarded traffic will be allowed across the vnets
$peeringProperties.allowForwardedTraffic = $true 

# Indicates whether the peer virtual network can access this virtual network’s gateway
$peeringProperties.allowGatewayTransit = $false 

# Indicates whether this virtual network will use peer virtual network’s gateway
$peeringProperties.useRemoteGateways =$false 

New-NetworkControllerVirtualNetworkPeering -ConnectionUri $uri -VirtualNetworkId “Woodgrove_vnet1” -ResourceId “WoodgrovetoContoso” -Properties $peeringProperties 

```

Après avoir créé cette homologation, l’état d’homologation indique **connecté** pour les deux homologues. À présent, les machines virtuelles dans un réseau virtuel peuvent communiquer avec les machines virtuelles dans le réseau virtuel homologué.

---