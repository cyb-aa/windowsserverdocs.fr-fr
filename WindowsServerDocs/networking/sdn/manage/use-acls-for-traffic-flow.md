---
title: Utilisez le contrôle d’accès répertorie (ACL) pour gérer les flux de trafic réseau de centre de données
description: Dans cette rubrique, vous allez apprendre à configurer des listes de contrôle d’accès (ACL) pour gérer les flux de trafic de données à l’aide de pare-feu de centre de données et les listes ACL sur les sous-réseaux virtuels. Vous activez et configurez les pare-feu de centre de données en créant des ACL soient appliqués à un sous-réseau virtuel ou d’une interface réseau.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a7ac5af-85e9-4440-a631-6a3a38e9015d
ms.author: pashort
author: shortpatti
ms.date: 08/27/2018
ms.openlocfilehash: 7bfb74e0964735d357226ab1e5af826796c48d81
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446303"
---
# <a name="use-access-control-lists-acls-to-manage-datacenter-network-traffic-flow"></a>Utilisez le contrôle d’accès répertorie (ACL) pour gérer les flux de trafic réseau de centre de données

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette rubrique, vous allez apprendre à configurer des listes de contrôle d’accès (ACL) pour gérer les flux de trafic de données à l’aide de pare-feu de centre de données et les listes ACL sur les sous-réseaux virtuels. Vous activez et configurez les pare-feu de centre de données en créant des ACL soient appliqués à un sous-réseau virtuel ou d’une interface réseau.   

Les exemples suivants dans cette rubrique montrent comment utiliser Windows PowerShell pour créer ces ACL.  

## <a name="configure-datacenter-firewall-to-allow-all-traffic"></a>Configurer le pare-feu de centre de données pour autoriser tout le trafic  

Une fois que vous déployez SDN, vous devez tester pour la connectivité de réseau de base dans votre nouvel environnement.  Pour ce faire, créez une règle pour le pare-feu de centre de données qui autorise tout le trafic réseau, sans aucune restriction.   

Utilisez les entrées dans le tableau suivant pour créer un ensemble de règles qui autorisent tout le trafic réseau entrant et sortant.


| Adresse IP source | Adresse IP de destination | Protocol | Port source | Port de destination | Direction | Action | Priority |
|:---------:|:--------------:|:--------:|:-----------:|:----------------:|:---------:|:------:|:--------:|
|    \*     |       \*       |   Tous    |     \*      |        \*        |  Entrant  | Autoriser  |   100    |
|    \*     |       \*       |   Tous    |     \*      |        \*        | Sortant  | Autoriser  |   110    |

---       

### <a name="example-create-an-acl"></a>Exemple : Créer une liste ACL 
Dans cet exemple, vous créez une liste ACL avec deux règles :

1. **AllowAll_Inbound** -permet à tout le trafic réseau à passer à l’interface réseau dans lequel cette liste ACL est configurée.    
2. **AllowAllOutbound** -autorise tout le trafic à passer en dehors de l’interface réseau. Cette liste ACL, identifiée par l’id de ressource « AutoriserTout-1 » est maintenant prête à être utilisé dans les interfaces réseau et sous-réseaux virtuels.  

L’exemple de script suivant utilise les commandes Windows PowerShell exportées à partir de la **NetworkController** module permettant de créer cette liste ACL.  


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
>La référence des commandes Windows PowerShell pour le contrôleur de réseau se trouve dans la rubrique [applets de commande de contrôleur de réseau](https://technet.microsoft.com/library/mt576401.aspx).  

## <a name="use-acls-to-limit-traffic-on-a-subnet"></a>Utilisez des listes ACL pour limiter le trafic sur un sous-réseau  
Dans cet exemple, vous créez une liste ACL qui empêche les machines virtuelles au sein du sous-réseau 192.168.0.0/24 de communiquer entre eux. Ce type d’ACL est utile pour limiter la capacité d’une personne malveillante à répartir latéralement au sein du sous-réseau, tout en autorisant les machines virtuelles pour recevoir les demandes à partir en dehors du sous-réseau, ainsi que pour communiquer avec d’autres services sur d’autres sous-réseaux.   


|   Adresse IP source    | Adresse IP de destination | Protocol | Port source | Port de destination | Direction | Action | Priority |
|:--------------:|:--------------:|:--------:|:-----------:|:----------------:|:---------:|:------:|:--------:|
|  192.168.0.1   |       \*       |   Tous    |     \*      |        \*        |  Entrant  | Autoriser  |   100    |
|       \*       |  192.168.0.1   |   Tous    |     \*      |        \*        | Sortant  | Autoriser  |   101    |
| 192.168.0.0/24 |       \*       |   Tous    |     \*      |        \*        |  Entrant  | Bloquer  |   102    |
|       \*       | 192.168.0.0/24 |   Tous    |     \*      |        \*        | Sortant  | Bloquer  |   103    |
|       \*       |       \*       |   Tous    |     \*      |        \*        |  Entrant  | Autoriser  |   104    |
|       \*       |       \*       |   Tous    |     \*      |        \*        | Sortant  | Autoriser  |   105    |

--- 

L’ACL créée par l’exemple de script ci-dessous, identifié par l’id de ressource **sous-réseau-192-168-0-0**, peut désormais être appliqué à un sous-réseau de réseau virtuel qui utilise l’adresse de sous-réseau « 192.168.0.0/24 ».  N’importe quelle interface réseau qui est automatiquement connecté à ce sous-réseau de réseau virtuel Obtient les règles ACL ci-dessus appliquées.  

Voici un exemple de script à l’aide des commandes Windows Powershell pour créer cette liste ACL à l’aide de l’API REST du contrôleur de réseau :  

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

