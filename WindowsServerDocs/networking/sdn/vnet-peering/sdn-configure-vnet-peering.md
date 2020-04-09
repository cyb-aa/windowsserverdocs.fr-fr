---
title: Configurer une homologation de réseau virtuel
description: La configuration de l’homologation de réseaux virtuels implique la création de deux réseaux virtuels qui sont homologués.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/08/2018
ms.openlocfilehash: ede13fd47c32b2d75ec71ad7c7bf7eb50c269c82
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853562"
---
# <a name="configure-virtual-network-peering"></a>Configurer une homologation de réseau virtuel

>S’applique à : Windows Server

Dans cette procédure, vous utilisez Windows PowerShell pour créer deux réseaux virtuels, chacun avec un sous-réseau. Ensuite, vous configurez l’homologation entre les deux réseaux virtuels pour activer la connectivité entre eux.

- [Étape 1. Créer le premier réseau virtuel](#step-1-create-the-first-virtual-network)

- [Étape 2. Créer le deuxième réseau virtuel](#step-2-create-the-second-virtual-network)

- [Étape 3. Configurer l’homologation du premier réseau virtuel vers le second réseau virtuel](#step-3-configure-peering-from-the-first-virtual-network-to-the-second-virtual-network)

- [Étape 4. Configuration de l’homologation à partir du deuxième réseau virtuel sur le premier réseau virtuel](#step-4-configure-peering-from-the-second-virtual-network-to-the-first-virtual-network)


>[!IMPORTANT]
>N’oubliez pas de mettre à jour les propriétés de votre environnement.

## <a name="step-1-create-the-first-virtual-network"></a>Étape 1. Créer le premier réseau virtuel

Dans cette étape, vous utilisez Windows PowerShell pour rechercher le réseau logique du fournisseur HNV afin de créer le premier réseau virtuel avec un sous-réseau. L’exemple de script suivant crée le réseau virtuel de contoso avec un sous-réseau.

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

Au cours de cette étape, vous allez créer un deuxième réseau virtuel avec un sous-réseau. L’exemple de script suivant crée un réseau virtuel de Woodgrove avec un sous-réseau.

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

## <a name="step-3-configure-peering-from-the-first-virtual-network-to-the-second-virtual-network"></a>Étape 3. Configurer l’homologation du premier réseau virtuel vers le second réseau virtuel

Dans cette étape, vous configurez l’homologation entre le premier réseau virtuel et le deuxième réseau virtuel que vous avez créé au cours des deux étapes précédentes. L’exemple de script suivant établit l’homologation de réseaux virtuels à partir de **Contoso_vnet1** vers **Woodgrove_vnet1**.

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
>Une fois cette homologation créée, l’état du réseau virtuel indique **initié**.

## <a name="step-4-configure-peering-from-the-second-virtual-network-to-the-first-virtual-network"></a>Étape 4. Configuration de l’homologation à partir du deuxième réseau virtuel sur le premier réseau virtuel

Dans cette étape, vous configurez l’homologation entre le deuxième réseau virtuel et le premier réseau virtuel que vous avez créé aux étapes 1 et 2 ci-dessus. L’exemple de script suivant établit l’homologation de réseaux virtuels à partir de **Woodgrove_vnet1** vers **Contoso_vnet1**.

```PowerShell
$peeringProperties = New-Object Microsoft.Windows.NetworkController.VirtualNetworkPeeringProperties 
$vnet2=Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Contoso_VNet1"
$peeringProperties.remoteVirtualNetwork = $vnet2 

# Indicates whether communication between the two virtual networks is allowed 
$peeringProperties.allowVirtualnetworkAccess = $true 

# Indicates whether forwarded traffic will be allowed across the vnets
$peeringProperties.allowForwardedTraffic = $true 

# Indicates whether the peer virtual network can access this virtual network's gateway
$peeringProperties.allowGatewayTransit = $false 

# Indicates whether this virtual network will use peer virtual network's gateway
$peeringProperties.useRemoteGateways =$false 

New-NetworkControllerVirtualNetworkPeering -ConnectionUri $uri -VirtualNetworkId “Woodgrove_vnet1” -ResourceId “WoodgrovetoContoso” -Properties $peeringProperties 

```

Après avoir créé cette homologation, l’état d’homologation de réseau virtuel s’affiche **connecté** pour les deux homologues. Désormais, les machines virtuelles d’un réseau virtuel peuvent communiquer avec des machines virtuelles dans le réseau virtuel homologué.

---