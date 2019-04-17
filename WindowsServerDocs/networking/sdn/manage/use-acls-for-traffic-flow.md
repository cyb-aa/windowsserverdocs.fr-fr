---
title: Utiliser des listes de contrôle d’accès (ACL) pour gérer le flux de trafic réseau de centre de données
description: Cette rubrique fait partie du guide logiciel défini de mise en réseau sur la façon de gérer les charges de travail clientes et des réseaux virtuels dans Windows Server2016.
manager: brianlic
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
ms.openlocfilehash: 64b7e1abf1ddb8132a8c6692fe82521c589f32df
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="use-access-control-lists-acls-to-manage-datacenter-network-traffic-flow"></a>Utiliser des listes de contrôle d’accès (ACL) pour gérer le flux de trafic réseau de centre de données

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour savoir comment configurer des listes de contrôle d’accès pour gérer le flux de trafic des données à l’aide de pare-feu de centre de données et les ACL sur les sous-réseaux virtuels.  
  
Vous pouvez activer et configurer le pare-feu de centre de données en créant des listes ACL qui sont appliqués à un sous-réseau virtuel ou d’une interface réseau.  
  
Les exemples suivants montrent comment utiliser Windows PowerShell pour créer ces ACL.  
  
## <a name="configure-datacenter-firewall-to-allow-all-traffic"></a>Configurer le pare-feu de centre de données pour autoriser tout le trafic  
  
Après le déploiement de type SDN, il est recommandé de tester la connectivité réseau de base dans votre nouvel environnement.  
  
Pour ce faire, vous pouvez créer une règle de pare-feu de centre de données qui permet à tout le trafic réseau, sans restriction.   
  
Vous pouvez utiliser les entrées dans le tableau suivant pour créer un ensemble de règles qui autorisent tout le trafic réseau entrant et sortant.  
  
  
  
Adresse IP source|Destination IP|Protocole|Port source|Port de destination|Direction|Action|Priorité   
---------|---------|---------|---------|---------|---------|---------|---------  
*    |   *      |   Tous      |    *     |      *   |     Trafic entrant    |   Autoriser      |   100        
*    |     *    |     Tous    |     *    |     *    |   Sortant      |  Autoriser       |  110         
  
  
Cet exemple de script crée une liste ACL qui contient deux règles:    
- La première règle «AllowAll_Inbound» permet de tout le trafic réseau à passer à l’interface réseau sur lequel cette liste ACL est configurée.    
- La deuxième règle, «AllowAllOutbound» permet de passer de l’interface réseau du trafic.  
Cette liste ACL, identifiée par l’id de ressource «AutoriserTout-1» est maintenant prête à être utilisé dans des sous-réseaux virtuels et les interfaces réseau.  
  
L’exemple de script suivant utilise les commandes Windows PowerShell exportés à partir de la **NetworkController** module permettant de créer cette liste ACL.  
  
  
```  
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
  
## <a name="use-acls-to-limit-traffic-on-a-subnet"></a>Utiliser les ACL pour limiter le trafic sur un sous-réseau  
  
Vous pouvez utiliser cet exemple montre comment créer une liste ACL qui empêche les machines virtuelles (VM) au sein du sous-réseau 192.168.0.0/24 de communiquer entre eux.   
  
Ce type d’ACL est utile pour limiter la possibilité d’une personne malveillante de propagation latéralement dans le sous-réseau, tout en autorisant les ordinateurs virtuels à recevoir des demandes d’en dehors du sous-réseau, ainsi que pour communiquer avec d’autres services sur d’autres sous-réseaux.  
  
  
Adresse IP source|Destination IP|Protocole|Port source|Port de destination|Direction|Action|Priorité   
---------|---------|---------|---------|---------|---------|---------|---------  
192.168.0.1    | * | Tous | * | * | Trafic entrant|   Autoriser      |   100        
* |192.168.0.1 | Tous | * | * | Sortant|  Autoriser       |  101         
192.168.0.0/24 | * | Tous | * | * | Trafic entrant| Bloc | 102  
* | 192.168.0.0/24 |Tous| * | * | Sortant | Bloc |103  
* | *  |Tous| * | * | Trafic entrant | Autoriser |104  
* | *  |Tous| * | * | Sortant | Autoriser |105  
  
La liste ACL créé par l’exemple de script ci-dessous, identifiés par l’id de ressource **sous-réseau-192-168-0-0**, peuvent désormais être appliquées à un sous-réseau de réseau virtuel qui utilise l’adresse de sous-réseau «192.168.0.0/24».  Une interface réseau qui est automatiquement connectée à ce sous-réseau de réseau virtuel obtient règles ACL ci-dessus.  
  
Voici un exemple de script à l’aide des commandes Windows Powershell pour créer cette liste ACL à l’aide de l’API REST de contrôleur de réseau:  
  
```  
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
  
