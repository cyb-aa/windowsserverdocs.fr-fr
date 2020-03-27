---
title: Créer, supprimer ou mettre à jour un réseau virtuel locataire
description: Dans cette rubrique, vous allez apprendre à créer, supprimer et mettre à jour des réseaux virtuels de virtualisation de réseau Hyper-V après avoir déployé SDN (Software Defined Networking). La virtualisation de réseau Hyper-V vous permet d’isoler les réseaux locataires afin que chaque réseau client soit une entité distincte. Chaque entité n’a aucune possibilité de connexion croisée, sauf si vous configurez des charges de travail d’accès public.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a820826-e829-4ef2-9a20-f74235f8c25b
ms.author: lizross
author: eross-msft
ms.date: 08/24/2018
ms.openlocfilehash: f85f593ec3dca33c5b35fb065c7d84ed12ea9af2
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309826"
---
# <a name="create-delete-or-update-tenant-virtual-networks"></a>Créer, supprimer ou mettre à jour des réseaux virtuels locataires

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette rubrique, vous allez apprendre à créer, supprimer et mettre à jour des réseaux virtuels de virtualisation de réseau Hyper-V après avoir déployé SDN (Software Defined Networking). La virtualisation de réseau Hyper-V vous permet d’isoler les réseaux locataires afin que chaque réseau client soit une entité distincte. Chaque entité n’a aucune possibilité de connexion croisée, sauf si vous configurez des charges de travail d’accès public.   
  
## <a name="create-a-new-virtual-network"></a>Créer un nouveau réseau virtuel  
La création d’un réseau virtuel pour un locataire le place dans un domaine de routage unique sur l’hôte Hyper-V. Sous chaque réseau virtuel, il existe au moins un sous-réseau virtuel. Les sous-réseaux virtuels sont définis par un préfixe IP et font référence à une liste de contrôle d’accès précédemment définie.  

Les étapes de création d’un réseau virtuel sont les suivantes :

1. Identifiez les préfixes d’adresses IP à partir desquels vous souhaitez créer les sous-réseaux virtuels.   
2. Identifiez le réseau du fournisseur logique sur lequel le trafic du locataire est acheminé par tunnel.   
3. Créez au moins un sous-réseau virtuel pour chaque préfixe d’adresse IP que vous avez identifié à l’étape 1. 
4. Facultatif Ajoutez les listes de contrôle d’accès précédemment créées aux sous-réseaux virtuels ou ajoutez la connectivité de passerelle pour les locataires. 

Le tableau suivant contient des exemples d’ID de sous-réseau et de préfixes pour deux locataires fictifs. Fabrikam dispose de deux sous-réseaux virtuels, tandis que le locataire Contoso a trois sous-réseaux virtuels.  
 
  
Nom du client  |ID de sous-réseau virtuel  |Préfixe de sous-réseau virtuel    
---------|---------|---------  
Fabrikam    |5001         |24.30.1.0/24           
Fabrikam     |5002         | 24.30.2.0/20          
Contoso    |6001         |  24.30.1.0/24         
Contoso    | 6002        |  24.30.2.0/24         
Contoso     | 6003        | 24.30.3.0/24          
  
L’exemple de script suivant utilise des commandes Windows PowerShell exportées à partir du module **NetworkController** pour créer un réseau virtuel et un sous-réseau de Contoso :   
  
```Powershell  
import-module networkcontroller  
$URI = "https://ncrest.contoso.local"  
  
#Find the HNV Provider Logical Network  
  
$logicalnetworks = Get-NetworkControllerLogicalNetwork -ConnectionUri $uri  
foreach ($ln in $logicalnetworks) {  
   if ($ln.Properties.NetworkVirtualizationEnabled -eq "True") {  
      $HNVProviderLogicalNetwork = $ln  
   }  
}   
  
#Find the Access Control List to user per virtual subnet  
  
$acllist = Get-NetworkControllerAccessControlList -ConnectionUri $uri -ResourceId "AllowAll"  
  
#Create the Virtual Subnet  
  
$vsubnet = new-object Microsoft.Windows.NetworkController.VirtualSubnet  
$vsubnet.ResourceId = "Contoso_WebTier"  
$vsubnet.Properties = new-object Microsoft.Windows.NetworkController.VirtualSubnetProperties  
$vsubnet.Properties.AccessControlList = $acllist  
$vsubnet.Properties.AddressPrefix = "24.30.1.0/24"  
  
#Create the Virtual Network  
  
$vnetproperties = new-object Microsoft.Windows.NetworkController.VirtualNetworkProperties  
$vnetproperties.AddressSpace = new-object Microsoft.Windows.NetworkController.AddressSpace  
$vnetproperties.AddressSpace.AddressPrefixes = @("24.30.1.0/24")  
$vnetproperties.LogicalNetwork = $HNVProviderLogicalNetwork  
$vnetproperties.Subnets = @($vsubnet)  
New-NetworkControllerVirtualNetwork -ResourceId "Contoso_VNet1" -ConnectionUri $uri -Properties $vnetproperties  
  
```  
  
## <a name="modify-an-existing-virtual-network"></a>Modifier un réseau virtuel existant  
Vous pouvez utiliser Windows PowerShell pour mettre à jour un sous-réseau virtuel ou un réseau existant.   
  
Lorsque vous exécutez l’exemple de script suivant, les ressources mises à jour sont simplement PLACÉes dans le contrôleur de réseau avec le même ID de ressource. Si votre locataire contoso souhaite ajouter un nouveau sous-réseau virtuel (24.30.2.0/24) à son réseau virtuel, vous ou l’administrateur contoso pouvez utiliser le script suivant.  
  
```PowerShell  
$acllist = Get-NetworkControllerAccessControlList -ConnectionUri $uri -ResourceId "AllowAll"  
  
$vnet = Get-NetworkControllerVirtualNetwork -ResourceId "Contoso_VNet1" -ConnectionUri $uri  
  
$vnet.properties.AddressSpace.AddressPrefixes += "24.30.2.0/24"  
  
$vsubnet = new-object Microsoft.Windows.NetworkController.VirtualSubnet  
$vsubnet.ResourceId = "Contoso_DBTier"  
$vsubnet.Properties = new-object Microsoft.Windows.NetworkController.VirtualSubnetProperties  
$vsubnet.Properties.AccessControlList = $acllist  
$vsubnet.Properties.AddressPrefix = "24.30.2.0/24"  
  
$vnet.properties.Subnets += $vsubnet  
  
New-NetworkControllerVirtualNetwork -ResourceId "Contoso_VNet1" -ConnectionUri $uri -properties $vnet.properties  
  
```  
  
## <a name="delete-a-virtual-network"></a>Supprimer un réseau virtuel  
  
Vous pouvez utiliser Windows PowerShell pour supprimer un réseau virtuel.  
  
L’exemple Windows PowerShell suivant supprime un réseau virtuel client en émettant une suppression HTTP à l’URI de l’ID de ressource.  

```PowerShell  
Remove-NetworkControllerVirtualNetwork -ResourceId "Contoso_Vnet1" -ConnectionUri $uri  
```

