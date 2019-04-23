---
title: Utiliser des appliances virtuelles réseau sur un réseau virtuel
description: Dans cette rubrique, vous allez apprendre à déployer des appliances virtuelles réseau sur les réseaux virtuels du locataire. Vous pouvez ajouter des appliances virtuelles réseau aux réseaux qui effectuent un routage défini par l’utilisateur et les fonctions de mise en miroir de port.
manager: dougkim
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.assetid: 3c361575-1050-46f4-ac94-fa42102f83c1
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: e715a782651a5b9867f3b45251fd6ea6e4a9e4f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847370"
---
# <a name="use-network-virtual-appliances-on-a-virtual-network"></a>Utiliser des appliances virtuelles réseau sur un réseau virtuel

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette rubrique, vous allez apprendre à déployer des appliances virtuelles réseau sur les réseaux virtuels du locataire. Vous pouvez ajouter des appliances virtuelles réseau aux réseaux qui effectuent un routage défini par l’utilisateur et les fonctions de mise en miroir de port.

## <a name="types-of-network-virtual-appliances"></a>Types d’appliances virtuelles réseau

Vous pouvez utiliser une des deux types d’équipements virtuels :

1. **Routage défini par l’utilisateur** -remplace les routeurs distribués sur le réseau virtuel avec les fonctionnalités de routage de l’appliance virtuelle.  Avec un routage défini par l’utilisateur, l’appliance virtuelle est utilisée comme un routeur entre les sous-réseaux virtuels sur le réseau virtuel.

2. **Mise en miroir de port** - tout le trafic réseau qui est entrant ou en laissant le port surveillé est dupliqué et envoyé vers une appliance virtuelle pour l’analyse. 


## <a name="deploying-a-network-virtual-appliance"></a>Déploiement d’une appliance virtuelle réseau

Pour déployer une appliance virtuelle réseau, vous devez tout d’abord créer une machine virtuelle qui contient l’appliance et puis connectez la machine virtuelle pour les sous-réseaux du réseau virtuel approprié. Pour plus d’informations, consultez [créer une machine virtuelle de locataire et de se connecter à un réseau virtuel locataire ou un réseau local virtuel](Create-a-Tenant-VM.md).

Certains appareils nécessitent plusieurs cartes réseau virtuelles. En règle générale, une carte réseau dédiée à la gestion de l’appliance tandis que les autres adaptateurs traitent le trafic.  Si votre application nécessite plusieurs cartes réseau, vous devez créer chaque interface réseau dans le contrôleur de réseau. Vous devez également affecter un ID d’interface sur chaque ordinateur hôte pour chacune des cartes supplémentaires qui se trouvent sur différents sous-réseaux virtuels.

Une fois que vous avez déployé l’appliance virtuelle réseau, vous pouvez utiliser l’appliance pour le routage défini, le portage de mise en miroir ou les deux. 


## <a name="example-user-defined-routing"></a>Exemple : Routage défini par l’utilisateur

Pour la plupart des environnements, vous devez uniquement les itinéraires système déjà définis par le routeur distribué du réseau virtuel. Toutefois, vous devrez peut-être créer une table de routage et d’ajouter un ou plusieurs itinéraires dans des cas spécifiques, tels que :

- Forcer le tunneling vers Internet via votre réseau local.
- Utiliser des appliances virtuelles dans votre environnement.

Pour ces scénarios, vous devez créer une table de routage et ajouter des itinéraires définis par l’utilisateur à la table. Vous pouvez avoir plusieurs tables de routage, et vous pouvez associer la même table de routage à un ou plusieurs sous-réseaux. Vous pouvez uniquement associer chaque sous-réseau à une seule table de routage. Toutes les machines virtuelles dans un sous-réseau utilisent la table de routage associée au sous-réseau.

Sous-réseaux s’appuient sur des itinéraires système jusqu'à ce qu’une table de routage est associée au sous-réseau. Une fois une association existe, routage repose sur le plus long préfixe (LPM) parmi les itinéraires définis par l’utilisateur et les itinéraires système. S’il existe plusieurs itinéraires avec la même correspondance de préfixe la plus longue, l’itinéraire défini par utilisateur est sélectionné en premier - avant de l’itinéraire du système.
 
**Procédure :**

1. Créer des propriétés de table, l’itinéraire qui contient tous les itinéraires définis par l’utilisateur.<p>Itinéraires système s’appliquent toujours selon les règles définies ci-dessus.

   ```PowerShell
    $routetableproperties = new-object Microsoft.Windows.NetworkController.RouteTableProperties
   ```

2. Ajouter un itinéraire pour les propriétés de table de routage.<p>N’importe quel itinéraire destiné à 12.0.0.0/8 sous-réseau est acheminé vers l’appliance virtuelle à 192.168.1.10. L’appliance doit avoir une carte réseau virtuelle connectée au réseau virtuel avec cette adresse IP affectée à une interface réseau.

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
   >Si vous souhaitez ajouter des itinéraires de plus, répétez cette étape pour chaque itinéraire que vous souhaitez définir.

3. Ajoutez la table de routage au contrôleur de réseau.

   ```PowerShell
    $routetable = New-NetworkControllerRouteTable -ConnectionUri $uri -ResourceId "Route1" -Properties $routetableproperties
   ```

4. Appliquer la table de routage au sous-réseau virtuel.<p>Lorsque vous appliquez la table de routage au sous-réseau virtuel, le premier sous-réseau virtuel dans le réseau Tenant1_Vnet1 utilise la table de routage. Vous pouvez affecter la table de routage pour autant des sous-réseaux dans le réseau virtuel que vous le souhaitez.

   ```PowerShell
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
    $vnet.properties.subnets[0].properties.RouteTable = $routetable
    new-networkcontrollervirtualnetwork -connectionuri $uri -properties $vnet.properties -resourceId $vnet.resourceid
   ```

Dès que vous appliquez la table de routage pour le réseau virtuel, le trafic est transféré vers l’appliance virtuelle. Vous devez configurer la table de routage dans l’appliance virtuelle pour transférer le trafic, d’une manière qui convient à votre environnement.

## <a name="example-port-mirroring"></a>Exemple : Mise en miroir de port

Dans cet exemple, vous configurez le trafic pour MyVM_Ethernet1 vers le miroir Appliance_Ethernet1.  Nous partons du principe que vous avez déployé deux machines virtuelles, un comme l’appliance et l’autre que la machine virtuelle à surveiller avec mise en miroir. 

L’appliance doit avoir une deuxième interface réseau pour la gestion. Après avoir activé la mise en miroir en tant que destination sur Appliciance_Ethernet1, il ne reçoive plus le trafic destiné à l’interface IP configuré.


**Procédure :**

1. Obtenir le réseau virtuel sur lequel se trouvent vos machines virtuelles.

   ```PowerShell
   $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
   ```

2. Obtenir les interfaces de réseau du contrôleur de réseau pour la source de mise en miroir et la destination.

   ```PowerShell
   $dstNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "Appliance_Ethernet1"
   $srcNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```

3. Créez un objet serviceinsertionproperties pour qu’il contienne le port de l’élément qui représente l’interface de destination et les règles de mise en miroir.

   ```PowerShell
   $portmirror = [Microsoft.Windows.NetworkController.ServiceInsertionProperties]::new()
   $portMirror.Priority = 1
   ```

4. Créez un objet serviceinsertionrules pour qu’il contienne les règles qui doivent être remplies afin que le trafic à envoyer à l’appliance.<p>Les règles définies ci-dessous correspondance tout le trafic, entrant et sortant, qui représente un miroir traditionnel.  Vous pouvez ajuster ces règles si vous êtes intéressé par la mise en miroir à un port spécifique, ou des sources et des destinations spécifiques.

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

5. Créez un objet serviceinsertionelements pour qu’il contienne l’interface réseau de l’appliance de mise en miroir.

   ```PowerShell
   $portmirror.ServiceInsertionElements = [Microsoft.Windows.NetworkController.ServiceInsertionElement[]]::new(1)

   $portmirror.ServiceInsertionElements[0] = [Microsoft.Windows.NetworkController.ServiceInsertionElement]::new()
   $portmirror.ServiceInsertionElements[0].ResourceId = "Element1"
   $portmirror.ServiceInsertionElements[0].Properties = [Microsoft.Windows.NetworkController.ServiceInsertionElementProperties]::new()

   $portmirror.ServiceInsertionElements[0].Properties.Description = "Port Mirror Element"
   $portmirror.ServiceInsertionElements[0].Properties.NetworkInterface = $dstNic
   $portmirror.ServiceInsertionElements[0].Properties.Order = 1
   ```

6. Ajoutez l’objet d’insertion de service dans le contrôleur de réseau.<p>Lorsque vous exécutez cette commande, tout le trafic vers l’appliance réseau interface spécifiée dans les taquets étape précédente.

   ```PowerShell
   $portMirror = New-NetworkControllerServiceInsertion -ConnectionUri $uri -Properties $portmirror -ResourceId "MirrorAll"
   ```

7. Mettre à jour de l’interface réseau de la source à mettre en miroir.

   ```PowerShell
   $srcNic.Properties.IpConfigurations[0].Properties.ServiceInsertion = $portMirror
   $srcNic = New-NetworkControllerNetworkInterface -ConnectionUri $uri  -Properties $srcNic.Properties -ResourceId $srcNic.ResourceId
   ```

Après avoir effectué ces étapes, l’interface Appliance_Ethernet1 reflète le trafic à partir de l’interface MyVM_Ethernet1.
 
---