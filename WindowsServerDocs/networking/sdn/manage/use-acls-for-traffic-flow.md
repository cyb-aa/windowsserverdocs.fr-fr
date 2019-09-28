---
title: Utiliser des listes de contrôle d’accès (ACL) pour gérer le flux de trafic réseau du centre de distribution
description: Dans cette rubrique, vous allez apprendre à configurer des listes de contrôle d’accès (ACL) pour gérer le flux de trafic de données à l’aide du pare-feu et des ACL de centre de données sur des sous-réseaux virtuels. Vous activez et configurez le pare-feu de centre de centres en créant des listes de contrôle d’accès appliquées à un sous-réseau virtuel ou à une interface réseau.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a7ac5af-85e9-4440-a631-6a3a38e9015d
ms.author: pashort
author: shortpatti
ms.date: 08/27/2018
ms.openlocfilehash: 6a1d210d25309be322359add20da4eb8d0eee091
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355805"
---
# <a name="use-access-control-lists-acls-to-manage-datacenter-network-traffic-flow"></a>Utiliser des listes de contrôle d’accès (ACL) pour gérer le flux de trafic réseau du centre de distribution

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Dans cette rubrique, vous allez apprendre à configurer des listes de contrôle d’accès (ACL) pour gérer le flux de trafic de données à l’aide du pare-feu et des ACL de centre de données sur des sous-réseaux virtuels. Vous activez et configurez le pare-feu de centre de centres en créant des listes de contrôle d’accès appliquées à un sous-réseau virtuel ou à une interface réseau.   

Les exemples suivants de cette rubrique montrent comment utiliser Windows PowerShell pour créer ces listes de contrôle d’accès.  

## <a name="configure-datacenter-firewall-to-allow-all-traffic"></a>Configurer le pare-feu de centre de centres pour autoriser tout le trafic  

Une fois que vous avez déployé SDN, vous devez tester la connectivité réseau de base dans votre nouvel environnement.  Pour ce faire, créez une règle pour le pare-feu de centre de connaissances qui autorise tout le trafic réseau, sans restriction.   

Utilisez les entrées du tableau suivant pour créer un ensemble de règles qui autorisent tout le trafic réseau entrant et sortant.


| Adresse IP source | Adresse IP de destination | Protocol | Port source | Port de destination | Direction | Action | Priority |
|:---------:|:--------------:|:--------:|:-----------:|:----------------:|:---------:|:------:|:--------:|
|    \*     |       \*       |   Tous    |     \*      |        \*        |  Entrant  | Autoriser  |   100    |
|    \*     |       \*       |   Tous    |     \*      |        \*        | Sortant  | Autoriser  |   110    |

---       

### <a name="example-create-an-acl"></a>Exemple : Créer une liste de contrôle d’accès 
Dans cet exemple, vous créez une liste de contrôle d’accès avec deux règles :

1. **AllowAll_Inbound** : permet à tout le trafic réseau de passer dans l’interface réseau où cette liste de contrôle d’accès est configurée.    
2. **AllowAllOutbound** : autorise le transfert de tout le trafic hors de l’interface réseau. Cette liste de contrôle d’accès, identifiée par l’ID de ressource « AutoriserTout-1 », est maintenant prête à être utilisée dans les sous-réseaux virtuels et les interfaces réseau.  

L’exemple de script suivant utilise des commandes Windows PowerShell exportées à partir du module **NetworkController** pour créer cette liste de contrôle d’accès.  


```PowerShell
$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "100"  
$ruleproperties.Type = "Inbound"  
$ruleproperties.Logging = "Enabled"  
$aclrule1 = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule1.Properties = $ruleproperties  
$aclrule1.ResourceId = "AllowAll_Inbound"  
$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "110"  
$ruleproperties.Type = "Outbound"  
$ruleproperties.Logging = "Enabled"  
$aclrule2 = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule2.Properties = $ruleproperties  
$aclrule2.ResourceId = "AllowAll_Outbound"  
$acllistproperties = new-object Microsoft.Windows.NetworkController.AccessControlListProperties  
$acllistproperties.AclRules = @($aclrule1, $aclrule2)  
New-NetworkControllerAccessControlList -ResourceId "AllowAll" -Properties $acllistproperties -ConnectionUri <NC REST FQDN>  
```  

>[!NOTE]  
>Les informations de référence sur les commandes Windows PowerShell pour le contrôleur de réseau se trouvent dans la rubrique [applets](https://technet.microsoft.com/library/mt576401.aspx)de commande du contrôleur de réseau.  

## <a name="use-acls-to-limit-traffic-on-a-subnet"></a>Utiliser des listes de contrôle d’accès pour limiter le trafic sur un sous-réseau  
Dans cet exemple, vous créez une liste de contrôle d’accès qui empêche les machines virtuelles au sein du sous-réseau 192.168.0.0/24 de communiquer entre elles. Ce type d’ACL est utile pour limiter la capacité d’une personne malveillante à se répandre plus tard dans le sous-réseau, tout en permettant aux machines virtuelles de recevoir des demandes en provenance de l’extérieur du sous-réseau, ainsi que de communiquer avec d’autres services sur d’autres sous-réseaux.   


|   Adresse IP source    | Adresse IP de destination | Protocol | Port source | Port de destination | Direction | Action | Priority |
|:--------------:|:--------------:|:--------:|:-----------:|:----------------:|:---------:|:------:|:--------:|
|  192.168.0.1   |       \*       |   Tous    |     \*      |        \*        |  Entrant  | Autoriser  |   100    |
|       \*       |  192.168.0.1   |   Tous    |     \*      |        \*        | Sortant  | Autoriser  |   101    |
| 192.168.0.0/24 |       \*       |   Tous    |     \*      |        \*        |  Entrant  | Bloquer  |   102    |
|       \*       | 192.168.0.0/24 |   Tous    |     \*      |        \*        | Sortant  | Bloquer  |   103    |
|       \*       |       \*       |   Tous    |     \*      |        \*        |  Entrant  | Autoriser  |   104    |
|       \*       |       \*       |   Tous    |     \*      |        \*        | Sortant  | Autoriser  |   105    |

--- 

La liste de contrôle d’accès créée par l’exemple de script ci-dessous, identifiée par le sous-réseau d’ID **de ressource-192-168-0-0**, peut désormais être appliquée à un sous-réseau de réseau virtuel qui utilise l’adresse de sous-réseau « 192.168.0.0/24 ».  Toute interface réseau attachée à ce sous-réseau de réseau virtuel reçoit automatiquement les règles de liste de contrôle d’accès (ACL) ci-dessus appliquées.  

Voici un exemple de script utilisant des commandes Windows PowerShell pour créer cette liste de contrôle d’accès à l’aide de l’API REST du contrôleur de réseau :  

```PowerShell  
import-module networkcontroller  
$ncURI = "https://mync.contoso.local"  
$aclrules = @()  

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "192.168.0.1"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "100"  
$ruleproperties.Type = "Inbound"  
$ruleproperties.Logging = "Enabled"  

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "AllowRouter_Inbound"  
$aclrules += $aclrule  

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "192.168.0.1"  
$ruleproperties.Priority = "101"  
$ruleproperties.Type = "Outbound"  
$ruleproperties.Logging = "Enabled"  

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "AllowRouter_Outbound"  
$aclrules += $aclrule  

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Deny"  
$ruleproperties.SourceAddressPrefix = "192.168.0.0/24"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "102"  
$ruleproperties.Type = "Inbound"  
$ruleproperties.Logging = "Enabled"  

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "DenySubnet_Inbound"  
$aclrules += $aclrule  

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Deny"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "192.168.0.0/24"  
$ruleproperties.Priority = "103"  
$ruleproperties.Type = "Outbound"  
$ruleproperties.Logging = "Enabled"  

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "DenySubnet_Outbound"  

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "104"  
$ruleproperties.Type = "Inbound"  
$ruleproperties.Logging = "Enabled"  

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "AllowAll_Inbound"  
$aclrules += $aclrule  

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "105"  
$ruleproperties.Type = "Outbound"  
$ruleproperties.Logging = "Enabled"  

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "AllowAll_Outbound"  
$aclrules += $aclrule  

$acllistproperties = new-object Microsoft.Windows.NetworkController.AccessControlListProperties  
$acllistproperties.AclRules = $aclrules  

New-NetworkControllerAccessControlList -ResourceId "Subnet-192-168-0-0" -Properties $acllistproperties -ConnectionUri $ncURI   
```  

