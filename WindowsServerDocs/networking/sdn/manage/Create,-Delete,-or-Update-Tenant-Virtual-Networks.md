---
title: Créer, supprimer ou mettre à jour le réseau virtuel locataire
description: Dans cette rubrique, vous allez apprendre à créer, supprimer et mettre à jour des réseaux virtuels de virtualisation de réseau Hyper-V après avoir déployé la mise en réseau SDN (Software Defined). Virtualisation de réseau Hyper-V vous permet d’isoler les réseaux clients afin que chaque réseau client est une entité distincte. Chaque entité n’a aucun risque de connexion croisée sauf si vous configurez des charges de travail de l’accès public.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a820826-e829-4ef2-9a20-f74235f8c25b
ms.author: pashort
author: shortpatti
ms.date: 08/24/2018
ms.openlocfilehash: a125ec220b4769a57a6be30f1425283afb7f0fe6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838350"
---
# <a name="create-delete-or-update-tenant-virtual-networks"></a>Créer, supprimer ou mettre à jour des réseaux virtuels locataires

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette rubrique, vous allez apprendre à créer, supprimer et mettre à jour des réseaux virtuels de virtualisation de réseau Hyper-V après avoir déployé la mise en réseau SDN (Software Defined). Virtualisation de réseau Hyper-V vous permet d’isoler les réseaux clients afin que chaque réseau client est une entité distincte. Chaque entité n’a aucun risque de connexion croisée sauf si vous configurez des charges de travail de l’accès public.   
  
## <a name="create-a-new-virtual-network"></a>Créer un réseau virtuel  
Création d’un réseau virtuel pour un client place au sein d’un domaine de routage unique sur l’ordinateur hôte Hyper-V. Sous chaque réseau virtuel, il existe au moins un sous-réseau virtuel. Sous-réseaux virtuels obtient définis par un préfixe IP et référencent une ACL précédemment définie.  

Les étapes pour créer un nouveau réseau virtuel sont :

1. Identifiez les préfixes d’adresse IP à partir de laquelle vous souhaitez créer des sous-réseaux virtuels.   
2. Identifiez le réseau de fournisseur logique sur lequel le trafic de client est acheminée.   
3. Créez au moins un sous-réseau virtuel pour chaque préfixe d’adresse IP que vous avez identifiée à l’étape 1. 
4. (Facultatif) Ajouter les ACL créés précédemment pour les sous-réseaux virtuels ou ajouter une connectivité de passerelle pour les clients. 

Le tableau suivant contient les préfixes et les ID de sous-réseau exemple pour les deux clients fictifs. Le locataire Fabrikam a deux sous-réseaux virtuels, tandis que le client Contoso a trois sous-réseaux virtuels.  
 
  
Nom du locataire  |ID de sous-réseau virtuel  |Préfixe de sous-réseau virtuel    
---------|---------|---------  
Fabrikam    |5001         |24.30.1.0/24           
Fabrikam     |5002         | 24.30.2.0/20          
Contoso    |6001         |  24.30.1.0/24         
Contoso    | 6002        |  24.30.2.0/24         
Contoso     | 6003        | 24.30.3.0/24          
  
L’exemple de script suivant utilise les commandes Windows PowerShell exportées à partir de la **NetworkController** module permettant de créer le réseau virtuel et un sous-réseau de Contoso :   
  
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
Vous pouvez utiliser Windows PowerShell pour mettre à jour d’un réseau ou sous-réseau virtuel existant.   
  
Lorsque vous exécutez l’exemple de script suivant, les ressources mises à jour sont simplement placés au contrôleur de réseau avec le même ID de ressource. Si votre locataire Contoso souhaite ajouter un nouveau sous-réseau virtuel (24.30.2.0/24) à son réseau virtuel, vous ou l’administrateur de Contoso peut utiliser le script suivant.  
  
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
  
L’exemple de Windows PowerShell suivant supprime un réseau virtuel locataire en émettant un delete HTTP à l’URI de l’ID de ressource.  

```PowerShell  
Remove-NetworkControllerVirtualNetwork -ResourceId "Contoso_Vnet1" -ConnectionUri $uri  
```

