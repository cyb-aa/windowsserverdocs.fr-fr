---
title: Utiliser des appliances virtuelles réseau sur un réseau virtuel
description: Dans cette rubrique, vous allez apprendre à déployer des appliances virtuelles réseau sur des réseaux virtuels locataires. Vous pouvez ajouter des appliances virtuelles réseau à des réseaux qui effectuent des fonctions de mise en miroir de port et de routage définies par l’utilisateur.
manager: dougkim
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.prod: windows-server
ms.technology: networking-sdn
ms.assetid: 3c361575-1050-46f4-ac94-fa42102f83c1
ms.author: lizross
author: eross-msft
ms.date: 08/30/2018
ms.openlocfilehash: db634af114610cce0bdbcacd58986ceb5f00dd99
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317583"
---
# <a name="use-network-virtual-appliances-on-a-virtual-network"></a>Utiliser des appliances virtuelles réseau sur un réseau virtuel

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette rubrique, vous allez apprendre à déployer des appliances virtuelles réseau sur des réseaux virtuels locataires. Vous pouvez ajouter des appliances virtuelles réseau à des réseaux qui effectuent des fonctions de mise en miroir de port et de routage définies par l’utilisateur.

## <a name="types-of-network-virtual-appliances"></a>Types d’appliances virtuelles réseau

Vous pouvez utiliser l’un des deux types d’appliances virtuelles :

1. **Routage défini** par l’utilisateur : remplace les routeurs distribués sur le réseau virtuel par les fonctionnalités de routage de l’appliance virtuelle.  Avec le routage défini par l’utilisateur, l’appliance virtuelle est utilisée en tant que routeur entre les sous-réseaux virtuels sur le réseau virtuel.

2. **Mise en miroir des ports** : tout le trafic réseau entrant ou sortant du port analysé est dupliqué et envoyé à une appliance virtuelle pour analyse. 


## <a name="deploying-a-network-virtual-appliance"></a>Déploiement d’une appliance virtuelle réseau

Pour déployer une appliance virtuelle réseau, vous devez d’abord créer une machine virtuelle contenant l’appliance, puis connecter la machine virtuelle aux sous-réseaux du réseau virtuel appropriés. Pour plus d’informations, consultez [créer une machine virtuelle cliente et se connecter à un réseau virtuel locataire ou à un réseau local virtuel](Create-a-Tenant-VM.md).

Certains appareils nécessitent plusieurs cartes réseau virtuelles. En règle générale, une carte réseau dédiée à la gestion de l’appliance alors que des adaptateurs supplémentaires traitent le trafic.  Si votre appliance nécessite plusieurs cartes réseau, vous devez créer chaque interface réseau dans le contrôleur de réseau. Vous devez également attribuer un ID d’interface sur chaque hôte pour chaque adaptateur supplémentaire qui se trouvent sur des sous-réseaux virtuels différents.

Une fois que vous avez déployé l’appliance virtuelle réseau, vous pouvez utiliser l’appliance pour le routage défini, le portage des mises en miroir, ou les deux. 


## <a name="example-user-defined-routing"></a>Exemple : routage défini par l’utilisateur

Pour la plupart des environnements, vous avez uniquement besoin des itinéraires système déjà définis par le routeur distribué du réseau virtuel. Toutefois, vous devrez peut-être créer une table de routage et ajouter un ou plusieurs itinéraires dans des cas spécifiques, par exemple :

- Forcer le tunneling vers Internet via votre réseau local.
- Utilisation d’appliances virtuelles dans votre environnement.

Pour ces scénarios, vous devez créer une table de routage et ajouter des itinéraires définis par l’utilisateur à la table. Vous pouvez avoir plusieurs tables de routage, et vous pouvez associer la même table de routage à un ou plusieurs sous-réseaux. Vous pouvez associer chaque sous-réseau à une seule table de routage. Toutes les machines virtuelles d’un sous-réseau utilisent la table de routage associée au sous-réseau.

Les sous-réseaux s’appuient sur les itinéraires système jusqu’à ce qu’une table de routage soit associée au sous-réseau. Une fois l’association établie, le routage s’effectue en fonction de la correspondance de préfixe la plus longue parmi les itinéraires définis par l’utilisateur et les itinéraires système. S’il existe plusieurs itinéraires avec la même correspondance LPM, l’itinéraire défini par l’utilisateur est sélectionné en premier, avant l’itinéraire du système.
 
**Procédures**

1. Créez les propriétés de la table de routage, qui contient tous les itinéraires définis par l’utilisateur.<p>Les itinéraires système s’appliquent toujours en fonction des règles définies ci-dessus.

   ```PowerShell
    $routetableproperties = new-object Microsoft.Windows.NetworkController.RouteTableProperties
   ```

2. Ajoutez un itinéraire aux propriétés de la table de routage.<p>Tout itinéraire destiné à un sous-réseau 12.0.0.0/8 est acheminé vers l’appliance virtuelle à l’adresse 192.168.1.10. L’appliance doit avoir une carte réseau virtuelle attachée au réseau virtuel avec cette adresse IP affectée à une interface réseau.

   ```PowerShell
    $route = new-object Microsoft.Windows.NetworkController.Route
    $route.ResourceID = "0_0_0_0_0"
    $route.properties = new-object Microsoft.Windows.NetworkController.RouteProperties
    $route.properties.AddressPrefix = "0.0.0.0/0"
    $route.properties.nextHopType = "VirtualAppliance"
    $route.properties.nextHopIpAddress = "192.168.1.10"
    $routetableproperties.routes += $route
   ```
   >[!TIP]
   >Si vous souhaitez ajouter d’autres itinéraires, répétez cette étape pour chaque itinéraire que vous souhaitez définir.

3. Ajoutez la table de routage au contrôleur de réseau.

   ```PowerShell
    $routetable = New-NetworkControllerRouteTable -ConnectionUri $uri -ResourceId "Route1" -Properties $routetableproperties
   ```

4. Appliquez la table de routage au sous-réseau virtuel.<p>Lorsque vous appliquez la table d’itinéraires au sous-réseau virtuel, le premier sous-réseau virtuel du réseau Tenant1_Vnet1 utilise la table de routage. Vous pouvez affecter la table de routage à autant de sous-réseaux que vous le souhaitez dans le réseau virtuel.

   ```PowerShell
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
    $vnet.properties.subnets[0].properties.RouteTable = $routetable
    new-networkcontrollervirtualnetwork -connectionuri $uri -properties $vnet.properties -resourceId $vnet.resourceid
   ```

Dès que vous appliquez la table de routage au réseau virtuel, le trafic est transféré à l’appliance virtuelle. Vous devez configurer la table de routage dans l’appliance virtuelle pour transférer le trafic, de manière appropriée pour votre environnement.

## <a name="example-port-mirroring"></a>Exemple : mise en miroir des ports

Dans cet exemple, vous configurez le trafic pour MyVM_Ethernet1 en miroir Appliance_Ethernet1.  Nous supposons que vous avez déployé deux machines virtuelles, l’une comme l’appliance et l’autre en tant que machine virtuelle à surveiller avec la mise en miroir. 

L’appliance doit disposer d’une deuxième interface réseau pour la gestion. Une fois que vous avez activé la mise en miroir en tant que destination sur Appliciance_Ethernet1, elle ne reçoit plus de trafic destiné à l’interface IP configurée.


**Procédures**

1. Procurez-vous le réseau virtuel sur lequel se trouvent vos machines virtuelles.

   ```PowerShell
   $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
   ```

2. Obtenir les interfaces réseau du contrôleur de réseau pour la source et la destination de mise en miroir.

   ```PowerShell
   $dstNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "Appliance_Ethernet1"
   $srcNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```

3. Créez un objet serviceinsertionproperties pour contenir les règles de mise en miroir des ports et l’élément qui représente l’interface de destination.

   ```PowerShell
   $portmirror = [Microsoft.Windows.NetworkController.ServiceInsertionProperties]::new()
   $portMirror.Priority = 1
   ```

4. Créez un objet serviceinsertionrules pour contenir les règles qui doivent être mises en correspondance pour que le trafic soit envoyé à l’appliance.<p>Les règles définies ci-dessous correspondent à l’ensemble du trafic, entrant et sortant, qui représente un miroir traditionnel.  Vous pouvez ajuster ces règles si vous vous intéressez à la mise en miroir d’un port spécifique, ou à une source/destination spécifique.

   ```PowerShell
   $portmirror.ServiceInsertionRules = [Microsoft.Windows.NetworkController.ServiceInsertionRule[]]::new(1)

   $portmirror.ServiceInsertionRules[0] = [Microsoft.Windows.NetworkController.ServiceInsertionRule]::new()
   $portmirror.ServiceInsertionRules[0].ResourceId = "Rule1"
   $portmirror.ServiceInsertionRules[0].Properties = [Microsoft.Windows.NetworkController.ServiceInsertionRuleProperties]::new()

   $portmirror.ServiceInsertionRules[0].Properties.Description = "Port Mirror Rule"
   $portmirror.ServiceInsertionRules[0].Properties.Protocol = "All"
   $portmirror.ServiceInsertionRules[0].Properties.SourcePortRangeStart = "0"
   $portmirror.ServiceInsertionRules[0].Properties.SourcePortRangeEnd = "65535"
   $portmirror.ServiceInsertionRules[0].Properties.DestinationPortRangeStart = "0"
   $portmirror.ServiceInsertionRules[0].Properties.DestinationPortRangeEnd = "65535"
   $portmirror.ServiceInsertionRules[0].Properties.SourceSubnets = "*"
   $portmirror.ServiceInsertionRules[0].Properties.DestinationSubnets = "*"
   ```

5. Créez un objet serviceinsertionelements pour contenir l’interface réseau de l’appliance mise en miroir.

   ```PowerShell
   $portmirror.ServiceInsertionElements = [Microsoft.Windows.NetworkController.ServiceInsertionElement[]]::new(1)

   $portmirror.ServiceInsertionElements[0] = [Microsoft.Windows.NetworkController.ServiceInsertionElement]::new()
   $portmirror.ServiceInsertionElements[0].ResourceId = "Element1"
   $portmirror.ServiceInsertionElements[0].Properties = [Microsoft.Windows.NetworkController.ServiceInsertionElementProperties]::new()

   $portmirror.ServiceInsertionElements[0].Properties.Description = "Port Mirror Element"
   $portmirror.ServiceInsertionElements[0].Properties.NetworkInterface = $dstNic
   $portmirror.ServiceInsertionElements[0].Properties.Order = 1
   ```

6. Ajoutez l’objet d’insertion de service dans le contrôleur de réseau.<p>Lorsque vous exécutez cette commande, tout le trafic vers l’interface réseau de l’appliance spécifiée à l’étape précédente s’arrête.

   ```PowerShell
   $portMirror = New-NetworkControllerServiceInsertion -ConnectionUri $uri -Properties $portmirror -ResourceId "MirrorAll"
   ```

7. Mettez à jour l’interface réseau de la source à mettre en miroir.

   ```PowerShell
   $srcNic.Properties.IpConfigurations[0].Properties.ServiceInsertion = $portMirror
   $srcNic = New-NetworkControllerNetworkInterface -ConnectionUri $uri  -Properties $srcNic.Properties -ResourceId $srcNic.ResourceId
   ```

Une fois ces étapes terminées, l’interface Appliance_Ethernet1 reflète le trafic à partir de l’interface MyVM_Ethernet1.
 
---