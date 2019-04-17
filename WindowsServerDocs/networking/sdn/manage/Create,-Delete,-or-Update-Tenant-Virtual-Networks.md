---
title: Créer, supprimer ou mettre à jour de réseaux virtuels locataires
description: Cette rubrique fait partie du guide logiciel défini de mise en réseau sur la façon de gérer les charges de travail clientes et des réseaux virtuels dans Windows Server2016.
manager: brianlic
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
ms.openlocfilehash: 6ef30dcc31593e15c36f846cf6d64afcd4b85f19
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="create-delete-or-update-tenant-virtual-networks"></a>Créer, supprimer ou mettre à jour de réseaux virtuels locataires

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour savoir comment créer, supprimer et mettre à jour des réseaux virtuels de la virtualisation de réseau Hyper-V après le déploiement de logiciel défini de mise en réseau (SDN).  
  
À l’aide de la virtualisation de réseau Hyper-V, vous pouvez isoler des réseaux clients afin que chaque réseau client est complètement distincte sans possibilité de connexion croisée sauf si vous configurez l’accès public de charges de travail.  
  
Vous pouvez créer de nouveaux réseaux virtuels pour les locataires, vous pouvez modifier des réseaux virtuels existants, et si un client n’a plus besoin certaines ressources, ou si le client n’est plus votre client, vous pouvez supprimer le client des réseaux virtuels.  
  
## <a name="create-a-new-virtual-network"></a>Créer un nouveau réseau virtuel  
  
Lorsque vous créez un réseau virtuel pour un client, il est placé dans un domaine de routage unique sur l’ordinateur hôte Hyper-V.  
  
Voici les étapes de création d’un nouveau réseau virtuel.  
  
1. Identifier les préfixes d’adresses IP à partir de laquelle vous souhaitez créer des sous-réseaux virtuels.   
2. Identifier le réseau logique fournisseur tunnel sur laquelle le trafic de client.   
3. Créez au moins un sous-réseau virtuel pour chaque préfixe IP que vous avez définis à l’étape1.   
  
>[!NOTE]  
>Sous chaque réseau virtuel, il existe au moins un sous-réseau virtuel. Sous-réseaux virtuels sont définis par un préfixe IP et faire référence à une liste de contrôle d’accès défini précédemment.  
  
Si vous le souhaitez, après avoir effectué ces étapes, vous pouvez également ajouter les listes de contrôle d’accès créé précédemment pour les sous-réseaux virtuels, ou ajouter la connectivité de passerelle pour les locataires.    
  
Le tableau suivant répertorie les ID de sous-réseau exemple et les préfixes pour deux clients fictives. Le client Fabrikam a deux sous-réseaux virtuels, tandis que le client de Contoso a trois sous-réseaux virtuels.  
  
  
  
Nom de client  |ID de sous-réseau virtuel  |Préfixe de sous-réseau virtuel    
---------|---------|---------  
Fabrikam    |5001         |24.30.1.0/24           
Fabrikam     |5002         | 24.30.2.0/20          
Contoso    |6001         |  24.30.1.0/24         
Contoso    | 6002        |  24.30.2.0/24         
Contoso     | 6003        | 24.30.3.0/24          
  
L’exemple de script suivant utilise les commandes Windows PowerShell exportés à partir de la **NetworkController** module permettant de créer le réseau virtuel et un sous-réseau de Contoso:   
  
```  
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
  
## <a name="modify-an-existing-virtual-network"></a>Modifier un réseau virtuel  
Vous pouvez utiliser Windows PowerShell pour mettre à jour d’un sous-réseau virtuel existant ou un réseau.   
  
Lorsque vous exécutez l’exemple de script suivant, les ressources mises à jour sont simplement mis au contrôleur de réseau avec le même ID de ressource. Si votre client Contoso souhaite ajouter un nouveau (24.30.2.0 virtuel sous-réseau/24) à leur réseau virtuel, vous ou l’administrateur de Contoso peut utiliser le script suivant.  
  
```  
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
  
## <a name="delete-a-virtual-network"></a>Suppression d’un réseau virtuel  
  
Vous pouvez utiliser Windows PowerShell pour supprimer un réseau virtuel.  
  
L’exemple suivant de Windows PowerShell supprime un réseau virtuel locataire par l’émission d’une suppression HTTP à l’URI de l’ID de ressource.  
  
    Remove-NetworkControllerVirtualNetwork -ResourceId "Contoso_Vnet1" -ConnectionUri $uri  


