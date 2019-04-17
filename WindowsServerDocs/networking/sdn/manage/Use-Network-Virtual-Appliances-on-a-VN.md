---
title: Utiliser des Appliances virtuelles réseau sur un réseau virtuel
description: Cette rubrique fait partie du guide logiciel défini de mise en réseau sur la façon de gérer les charges de travail clientes et des réseaux virtuels dans Windows Server2016.
manager: brianlic
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
ms.openlocfilehash: db46189931263d230f013431f319eb2497589dee
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="use-network-virtual-appliances-on-a-virtual-network"></a>Utiliser des Appliances virtuelles réseau sur un réseau virtuel

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour savoir comment déployer des appliances virtuelles réseau sur les clients des réseaux virtuels.

Vous pouvez ajouter des appliances virtuelles réseau aux réseaux qui effectuent des fonctions de la mise en miroir de port et de routage défini par l’utilisateur.

Cette rubrique contient les sections suivantes.

- [Types de matériel de réseau virtuel](#bkmk_types)
- [Déploiement d’un matériel de réseau virtuel](#bkmk_deploy)
- [: Exemple de défini par l’utilisateur routage](#bkmk_routing)
- [Exemple: La mise en miroir de Port](#bkmk_port)

## <a name="bkmk_types"></a>Types de matériel de réseau virtuel

Il existe deux types d’équipements virtuels que vous pouvez utiliser sur des réseaux virtuels:

1. **Routage défini par l’utilisateur**. Routage défini par l’utilisateur remplace les routeurs distribués sur le réseau virtuel avec les fonctionnalités de routage de l’application virtuelle.  Avec défini par l’utilisateur le routage, l’application virtuelle est utilisée comme un routeur entre les sous-réseaux virtuels sur le réseau virtuel.
2. **La mise en miroir de port**. Avec la mise en miroir de port, tout le trafic réseau entrant ou quitter le port analysé est dupliqué et envoyé à une appliance virtuelle pour l’analyse. 
## <a name="bkmk_deploy"></a>Déploiement d’un matériel de réseau virtuel

Pour déployer un appareil virtuel, vous devez tout d’abord créer une machine virtuelle (VM) qui contient le matériel et les sous-réseaux du réseau virtuel approprié puis connecter l’ordinateur virtuel.

>[!NOTE]
>Pour plus d’informations, voir [créer un ordinateur virtuel client et se connecter à un réseau virtuel locataire ou un réseau local virtuel](Create-a-Tenant-VM.md)

Certains appareils nécessitent plusieurs cartes réseau virtuelles. Généralement une carte réseau est dédiée à la gestion du matériel, alors que les cartes supplémentaires sont utilisés pour traiter le trafic. 

Si votre application nécessite plusieurs cartes réseau, vous devez créer chaque interface réseau dans le contrôleur de réseau. 

Vous devez également affecter un ID d’interface sur chaque ordinateur hôte pour chacune des cartes supplémentaires qui se trouvent sur différents sous-réseaux virtuels.

Après avoir terminé un déploiement appliance virtuelle de réseau, vous pouvez utiliser le dispositif de routage défini par l’utilisateur, la mise en miroir de port ou les deux.

##<a name="bkmk_routing"></a>: Exemple de défini par l’utilisateur routage

Pour la plupart des environnements, vous devez uniquement les itinéraires système déjà définis par le routeur distribué de réseau virtuel. Toutefois, vous devrez peut-être créer une table de routage et d’ajouter un ou plusieurs itinéraires dans des cas spécifiques, tels que:

* Le tunneling forcé à Internet via votre réseau local.
* Utilisation des équipements virtuels dans votre environnement.

Pour ces scénarios, vous devez créer une table d’itinéraires et ajouter les itinéraires défini par l’utilisateur à la table. Vous pouvez avoir plusieurs tables de routage et la table d’itinéraires peut être associée à un ou plusieurs sous-réseaux. 

Chaque sous-réseau ne peut être associé à une table de routage unique. Tous les ordinateurs virtuels dans un sous-réseau, utilisent la table d’itinéraires qui est associée à ce sous-réseau.

Sous-réseaux s’appuient sur les itinéraires système jusqu'à ce qu’une table de routage est associée au sous-réseau. Lorsqu’une association existe, le routage est effectué en fonction sur plus long préfixe correspondance (stratégies locales) entre les itinéraires défini par l’utilisateur et les itinéraires de système. 

S’il existe plusieurs itinéraires avec la même correspondance de stratégies locales, l’itinéraire défini utilisateur est sélectionné en premier - avant l’itinéraire système. 

###<a name="step-1-create-the-route-table-properties"></a>Étape 1: Créer la gamme de propriétés du tableau

Cette table d’itinéraires contient tous les itinéraires défini par l’utilisateur.  Itinéraires système seront appliquent en fonction des règles définies ci-dessus.

Vous pouvez utiliser les exemples de commandes suivantes pour créer des propriétés de table de routage.

    $routetableproperties = new-object Microsoft.Windows.NetworkController.RouteTableProperties

###<a name="step-2-add-a-route-to-the-route-table-properties"></a>Étape 2: Ajoutez un itinéraire de propriétés de la table de routage

Cet itinéraire indiquant que tout le trafic destiné pour le sous-réseau 12.0.0.0/8 doit obtenir envoyé le l’appliance virtuelle à 192.168.1.10 à être routés.  Il est important que le matériel possède une carte réseau virtuelle connectée au réseau virtuel avec cette adresse IP attribuée à une interface réseau.

Vous pouvez utiliser les exemples de commandes suivante pour ajouter un itinéraire à des propriétés de la table de routage.

    $route = new-object Microsoft.Windows.NetworkController.Route
    $route.ResourceID = "0_0_0_0_0"
    $route.properties = new-object Microsoft.Windows.NetworkController.RouteProperties
    $route.properties.AddressPrefix = "0.0.0.0/0"
    $route.properties.nextHopType = "VirtualAppliance"
    $route.properties.nextHopIpAddress = "192.168.1.10"
    $routetableproperties.routes += $route

Vous pouvez ajouter des itinéraires supplémentaires en répétant cette étape pour chaque itinéraire que vous souhaitez définir.
s
###<a name="step-3-add-the-route-table-to-network-controller"></a>Étape 3: Ajouter la table de routage pour le contrôleur de réseau
Vous pouvez utiliser les exemples de commandes suivantes pour ajouter la table d’itinéraires au contrôleur de réseau.

    $routetable = New-NetworkControllerRouteTable -ConnectionUri $uri -ResourceId "Route1" -Properties $routetableproperties

###<a name="step-4-apply-the-route-table-to-the-virtual-subnet"></a>Étape 4: Appliquer la table d’itinéraires vers le sous-réseau virtuel
 
Lorsque vous appliquez la table d’itinéraires vers le sous-réseau virtuel, le premier sous-réseau virtuel dans le réseau Tenant1_Vnet1 utilise la table d’itinéraires. Vous pouvez affecter la table de routage pour qu’un grand nombre des sous-réseaux dans le réseau virtuel que vous le souhaitez.

Vous pouvez utiliser les exemples de commandes suivantes pour appliquer la table d’itinéraires vers le sous-réseau virtuel.

    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
    $vnet.properties.subnets[0].properties.RouteTable = $routetable
    new-networkcontrollervirtualnetwork -connectionuri $uri -properties $vnet.properties -resourceId $vnet.resourceid

Dès que vous appliquez la table de routage pour le réseau virtuel, le trafic est transféré vers l’application virtuelle. Vous devez configurer la table de routage au niveau du matériel pour transférer le trafic, de manière appropriée pour votre environnement virtuel.

##<a name="bkmk_port"></a>Exemple: La mise en miroir de Port

Cet exemple montre comment vous autorise à configurer le trafic de MyVM_Ethernet1 afin que le trafic est mis en miroir à Appliance_Ethernet1.

Cet exemple suppose que vous avez déjà déployé les deux ordinateurs virtuels, que l’application et que l’ordinateur virtuel à surveiller avec la mise en miroir.

Il est important que le matériel possède une autre interface réseau pour la gestion, car une fois la mise en miroir est activé en tant que destination sur Appliance_Ethernet1, il ne recevra plus le trafic destiné à l’interface IP configuré.

###<a name="step-1-get-the-virtual-network-on-which-your-vms-are-located"></a>Étape 1: Obtenir le réseau virtuel sur lequel se trouvent vos ordinateurs virtuels
Vous pouvez utiliser la commande suivante pour obtenir le réseau virtuel.

    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"

###<a name="step-2-get-the-network-controller-network-interfaces-for-the-mirroring-source-and-destination"></a>Étape 2: Obtenir les interfaces réseau de contrôleur de réseau pour la source de mise en miroir et la destination
Vous pouvez utiliser les exemples de commandes suivants pour obtenir les interfaces réseau de contrôleur de réseau pour la source de mise en miroir et la destination.

    $dstNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "Appliance_Ethernet1"
    $srcNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"

###<a name="step-3-create-a-serviceinsertionproperties-object-to-contain-the-port-mirroring-rules-and-the-element-which-represents-the-destination-interface"></a>Étape 3: Créer un objet serviceinsertionproperties pour contenir le port de la mise en miroir des règles et l’élément qui représente l’interface de destination
Vous pouvez utiliser les exemples de commandes suivantes pour créer un objet serviceinsertionproperties de destination.

    $portmirror = [Microsoft.Windows.NetworkController.ServiceInsertionProperties]::new()
    $portMirror.Priority = 1

###<a name="step-4-create-a-serviceinsertionrules-object-to-contain-the-rules-that-must-be-matched-in-order-for-the-traffic-to-be-sent-to-the-appliance"></a>Étape 4: Créer un objet serviceinsertionrules pour contenir les règles qui doivent être remplis dans l’ordre pour le trafic à envoyer à l’application

Les règles définies ci-dessous correspondance tout le trafic, entrant et sortant, qui représente une mise en miroir traditionnel.  Vous pouvez ajuster ces règles si vous êtes intéressé par la mise en miroir à un port spécifique ou sources et des destinations spécifiques.

Vous pouvez utiliser les exemples de commandes suivantes pour créer un objet serviceinsertionproperties.

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

###<a name="step-5-create-a-serviceinsertionelements-object-to-contain-the-network-interface-of-the-appliance-you-are-mirroring-to"></a>Étape 5: Créer un objet serviceinsertionelements pour contenir l’interface réseau de l’application que vous mettez en miroir pour
Vous pouvez utiliser les exemples de commandes suivantes pour créer un objet serviceinsertionelements d’interface réseau.

    $portmirror.ServiceInsertionElements = [Microsoft.Windows.NetworkController.ServiceInsertionElement[]]::new(1)

    $portmirror.ServiceInsertionElements[0] = [Microsoft.Windows.NetworkController.ServiceInsertionElement]::new()
    $portmirror.ServiceInsertionElements[0].ResourceId = "Element1"
    $portmirror.ServiceInsertionElements[0].Properties = [Microsoft.Windows.NetworkController.ServiceInsertionElementProperties]::new()

    $portmirror.ServiceInsertionElements[0].Properties.Description = "Port Mirror Element"
    $portmirror.ServiceInsertionElements[0].Properties.NetworkInterface = $dstNic
    $portmirror.ServiceInsertionElements[0].Properties.Order = 1

###<a name="step-6-add-the-service-insertion-object-in-network-controller"></a>Étape 6: Ajouter l’objet du service d’insertion dans le contrôleur de réseau
Lorsque vous exécutez cette commande, tout le trafic vers l’interface réseau de matériel spécifié dans l’étape précédente s’arrête.

Vous pouvez utiliser les exemples de commandes suivante pour ajouter l’objet du service d’insertion dans le contrôleur de réseau.

    $portMirror = New-NetworkControllerServiceInsertion -ConnectionUri $uri -Properties $portmirror -ResourceId "MirrorAll"

###<a name="step-7-update-the-network-interface-of-the-source-to-be-mirrored"></a>Étape 7: Mettre à jour l’interface réseau de la source de mise en miroir
Vous pouvez utiliser les exemples de commandes suivantes pour mettre à jour l’interface réseau.

    $srcNic.Properties.IpConfigurations[0].Properties.ServiceInsertion = $portMirror
    $srcNic = New-NetworkControllerNetworkInterface -ConnectionUri $uri  -Properties $srcNic.Properties -ResourceId $srcNic.ResourceId

Lorsque vous avez effectué ces étapes, le trafic de l’interface MyVM_Ethernet1 est mis en miroir par l’interface Appliance_Ethernet1.
 
