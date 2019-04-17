---
title: Clustering invité dans un réseau virtuel
description: Cette rubrique fait partie du guide logiciel défini de mise en réseau sur la façon de gérer les charges de travail clientes et des réseaux virtuels dans Windows Server2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8e9e5c81-aa61-479e-abaf-64c5e95f90dc
ms.author: grcusanz
author: shortpatti
ms.openlocfilehash: 5cab7e7c0ca0af848b4b58362388701cc4357860
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="guest-clustering-in-a-virtual-network"></a>Clustering invité dans un réseau virtuel

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Les ordinateurs virtuels qui sont connectés à un réseau virtuel sont uniquement autorisés à utiliser les adresses IP que le contrôleur de réseau a attribué afin de communiquer sur le réseau.  Cela signifie que le clustering de technologies qui nécessitent une adresse IP à virgule flottante, telles que le Clustering de basculement Microsoft, nécessite quelques étapes supplémentaires pour fonctionner correctement.

La méthode permettant de l’adresse IP à virgule flottante accessible consiste à utiliser un \(SLB\) équilibreur de charge logicielle virtuel \(VIP\) IP.  L’équilibrage de charge logicielle doit être configuré avec une sonde d’intégrité sur un port de cette IP afin que SLB dirige le trafic vers l’ordinateur qui possède actuellement cette IP.

## <a name="example-load-balancer-configuration"></a>Exemple: Configuration de l’équilibrage de charge

Cet exemple suppose que vous avez déjà créé des ordinateurs virtuels qui deviendront des nœuds de cluster et les attaché à un réseau virtuel.  Pour obtenir des instructions, reportez-vous à [créer un ordinateur virtuel et se connecter à un réseau virtuel locataire ou un réseau local virtuel](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm).  

Dans cet exemple montre comment vous créer une adresse IP virtuelle (192.168.2.100) pour représenter l’adresse IP à virgule flottante du cluster et configurer une sonde d’intégrité pour analyser le port TCP 59999 pour déterminer quel nœud est actif.

### <a name="step-1-select-the-vip"></a>Étape 1: Sélectionnez l’adresse IP virtuelle
Préparer en affectant une adresse IP de l’adresse IP virtuelle.  Cette adresse peut être n’importe quelle adresse inutilisée ou réservé dans le même sous-réseau que les nœuds de cluster.  L’adresse IP virtuelle doit correspondre à l’adresse du cluster à virgule flottante.

    $VIP = "192.168.2.100"
    $subnet = "Subnet2"
    $VirtualNetwork = "MyNetwork"
    $ResourceId = "MyNetwork_InternalVIP"

### <a name="step-2-create-the-load-balancer-properties-object"></a>Étape 2: Créer l’objet des propriétés équilibrage de charge

Vous pouvez utiliser la commande suivante pour créer l’objet de propriétés d’équilibrage de charge.

    $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties

### <a name="step-3-create-a-front-end-ip-address"></a>Étape 3: Créer une adresse IP de bout en bout front\

Vous pouvez utiliser la commande suivante pour créer une adresse IP de bout en bout front\.

    $LoadBalancerProperties.frontendipconfigurations += $FrontEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfiguration
    $FrontEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfigurationProperties
    $FrontEnd.resourceId = "Frontend1"
    $FrontEnd.resourceRef = "/loadBalancers/$ResourceId/frontendIPConfigurations/$($FrontEnd.resourceId)"
    $FrontEnd.properties.subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $FrontEnd.properties.subnet.ResourceRef = "/VirtualNetworks/MyNetwork/Subnets/Subnet2"
    $FrontEnd.properties.privateIPAddress = $VIP
    $FrontEnd.properties.privateIPAllocationMethod = "Static"

### <a name="step-4-create-a-back-end-pool-to-contain-the-cluster-nodes"></a>Étape 4: Créer un pool de bout en bout back\ pour contenir les nœuds de cluster

Vous pouvez utiliser la commande suivante pour créer un pool de bout en bout back\

    $BackEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
    $BackEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties
    $BackEnd.resourceId = "Backend1"
    $BackEnd.resourceRef = "/loadBalancers/$ResourceId/backendAddressPools/$($BackEnd.resourceId)"
    $LoadBalancerProperties.backendAddressPools += $BackEnd

### <a name="step-5-add-a-probe"></a>Étape 5: Ajouter une sonde
La sonde est nécessaire pour détecter le nœud de cluster l’adresse à virgule flottante est actuellement actif sur.

>[!NOTE]
>La requête sonde adresse permanente de la machine virtuelle sur le port défini ci-dessous.  Le port doit répondre uniquement sur le nœud actif. 

    $LoadBalancerProperties.probes += $lbprobe = new-object Microsoft.Windows.NetworkController.LoadBalancerProbe
    $lbprobe.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerProbeProperties

    $lbprobe.ResourceId = "Probe1"
    $lbprobe.resourceRef = "/loadBalancers/$ResourceId/Probes/$($lbprobe.resourceId)"
    $lbprobe.properties.protocol = "TCP"
    $lbprobe.properties.port = "59999"
    $lbprobe.properties.IntervalInSeconds = 5
    $lbprobe.properties.NumberOfProbes = 11

### <a name="step-5-add-the-load-balancing-rules"></a>Étape 5: Ajouter la règles d’équilibrage de charge
Cette étape crée une charge de l’équilibrage de la règle de port TCP 1433.  Vous pouvez modifier le protocole et le port en fonction des besoins.  Vous pouvez également répéter cette étape plusieurs fois pour les autres ports et protocoles sur cette adresse IP virtuelle.  Il est important que EnableFloatingIP est défini sur $true car cela indique à l’équilibrage de charge pour envoyer le paquet vers le noeud comportant l’adresse IP virtuelle d’origine en place.

    $LoadBalancerProperties.loadbalancingRules += $lbrule = new-object Microsoft.Windows.NetworkController.LoadBalancingRule
    $lbrule.properties = new-object Microsoft.Windows.NetworkController.LoadBalancingRuleProperties
    $lbrule.ResourceId = "Rules1"

    $lbrule.properties.frontendipconfigurations += $FrontEnd
    $lbrule.properties.backendaddresspool = $BackEnd 
    $lbrule.properties.protocol = "TCP"
    $lbrule.properties.frontendPort = $lbrule.properties.backendPort = 1433 
    $lbrule.properties.IdleTimeoutInMinutes = 4
    $lbrule.properties.EnableFloatingIP = $true
    $lbrule.properties.Probe = $lbprobe

### <a name="step-5-create-the-load-balancer-in-network-controller"></a>Étape 5: Créer l’équilibrage de charge dans le contrôleur de réseau

Vous pouvez utiliser la commande suivante pour créer l’équilibrage de charge.

    $lb = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $ResourceId -Properties $LoadBalancerProperties -Force

### <a name="step-6-add-the-cluster-nodes-to-the-backend-pool"></a>Étape 6: Ajouter des nœuds de cluster pour le pool principal

Cet exemple illustre l’ajout de deux membres de la liste, mais vous pouvez ajouter autant de nœuds que vous avez besoin pour le cluster pour le pool.

    # Cluster Node 1

    $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid "ClusterNode1_Network-Adapter"
    $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
    $nic = new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid $nic.resourceid -properties $nic.properties -force

    # Cluster Node 2

    $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid "ClusterNode2_Network-Adapter"
    $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
    $nic = new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid $nic.resourceid -properties $nic.properties -force

Une fois que vous avez créé l’équilibrage de charge et ajouté les interfaces réseau pour le pool principal, vous êtes prêt à configurer le cluster.  Si vous utilisez un Microsoft Failover Cluster, vous pouvez continuer avec l’exemple suivant. 

## <a name="example-2-configuring-a-microsoft-failover-cluster"></a>Exemple 2: Configuration d’un Cluster de basculement Microsoft

Vous pouvez utiliser les étapes suivantes pour configurer un cluster de basculement.

### <a name="step-1-install-failover-clustering"></a>Étape 1: Installer le clustering de basculement

Vous pouvez utiliser les exemples de commandes suivantes pour installer et configurer les propriétés d’un cluster de basculement.

    add-windowsfeature failover-clustering -IncludeManagementTools
    Import-module failoverclusters

    $ClusterName = "MyCluster"
   
    $ClusterNetworkName = "Cluster Network 1"
    $IPResourceName =  
    $ILBIP = “192.168.2.100” 

    $nodes = @("DB1", "DB2")

### <a name="step-2-create-the-cluster-on-one-node"></a>Étape 2: Créer le cluster sur un nœud

Vous pouvez utiliser la commande suivante pour créer le cluster sur un nœud.

    New-Cluster -Name $ClusterName -NoStorage -Node $nodes[0]

### <a name="step-3-stop-the-cluster-resource"></a>Étape 3: Arrêter la ressource de cluster

Vous pouvez utiliser la commande suivante pour arrêter la ressource de cluster.

    Stop-ClusterResource "Cluster Name" 

### <a name="step-4-set-the-cluster-ip-and-probe-port"></a>Étape 4: Définir le cluster port IP et de sondage
L’adresse IP doit correspondre à l’adresse ip frontale utilisé dans l’exemple précédent, et le port de sonde doit correspondre au port de sonde dans l’exemple précédent.

    Get-ClusterResource "Cluster IP Address" | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

### <a name="step-5-start-the-cluster-resources"></a>Étape 5: Démarrer les ressources de cluster

Vous pouvez utiliser la commande suivante pour démarrer les ressources de cluster.

    Start-ClusterResource "Cluster IP Address"  -Wait 60 
    Start-ClusterResource "Cluster Name"  -Wait 60 

### <a name="step-6-add-the-remaining-nodes"></a>Étape 6: Ajouter les nœuds restants

Vous pouvez utiliser la commande suivante pour ajouter des nœuds de cluster.

    Add-ClusterNode $nodes[1]

À la fin de la dernière étape, votre cluster est actif. Le trafic à l’adresse IP virtuelle sur le port spécifié sera dirigé au niveau du nœud actif.