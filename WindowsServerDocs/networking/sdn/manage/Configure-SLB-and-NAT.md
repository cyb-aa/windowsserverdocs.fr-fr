---
title: Configurer l’équilibreur de charge logicielle pour l’équilibrage de charge et la traduction d’adresses réseau (NAT)
description: Cette rubrique fait partie du guide Sdn sur la façon de gérer les charges de travail clientes et des réseaux virtuels dans Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 73bff8ba-939d-40d8-b1e5-3ba3ed5439c3
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 55847bfbc0362887497514009f6efe1312d79906
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819350"
---
# <a name="configure-the-software-load-balancer-for-load-balancing-and-network-address-translation-nat"></a>Configurer l’équilibreur de charge logicielle pour l’équilibrage de charge et la traduction d’adresses réseau (NAT)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour savoir comment utiliser le Sdn \(SDN\) équilibreur de charge logiciel \(SLB\) pour fournir la traduction d’adresses réseau sortantes \(NAT\), trafic entrant NAT ou équilibrage de charge entre plusieurs instances d’une application.

## <a name="software-load-balancer-overview"></a>Présentation de l’équilibrage de charge logiciel

L’équilibreur de charge logiciel SDN \(SLB\) offre une haute disponibilité et des performances réseau pour vos applications. Il est une couche 4 \(TCP, UDP\) qui répartit le trafic entrant entre les instances de service intègres dans services cloud ou machines virtuelles définies dans un jeu d’équilibrage de charge l’équilibrage de charge.

Configurer SLB pour effectuer les opérations suivantes :

- Répartir le trafic entrant externe à un réseau virtuel pour les machines virtuelles \(machines virtuelles\), également appelée équilibrage de charge des adresses IP virtuelles publiques.
- Charge équilibre le trafic entrant entre les machines virtuelles dans un réseau virtuel, entre les machines virtuelles dans les services cloud ou entre des ordinateurs locaux et les machines virtuelles dans un réseau virtuel entre différents locaux. 
- Transférer le trafic de réseau de machine virtuelle à partir du réseau virtuel vers des destinations externes à l’aide de la traduction d’adresses réseau (NAT), également appelée sortant NAT.
- Transférer le trafic externe vers une machine virtuelle spécifique, également appelée NAT de trafic entrant.




## <a name="example-create-a-public-vip-for-load-balancing-a-pool-of-two-vms-on-a-virtual-network"></a>Exemple : Créer une adresse IP virtuelle publique pour un pool de deux machines virtuelles sur un réseau virtuel d’équilibrage de charge

Dans cet exemple, vous créez un objet d’équilibrage de charge avec une adresse IP virtuelle publique et deux machines virtuelles en tant que membres du pool pour répondre aux requêtes à l’adresse IP virtuelle. Cet exemple de code ajoute également une sonde d’intégrité HTTP pour détecter si un des membres du pool ne répond plus.

1. Préparer l’objet d’équilibrage de charge.

   ```PowerShell
    import-module NetworkController

    $URI = "https://sdn.contoso.com"

    $LBResourceId = "LB2"

    $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties
   ```

2. Affecter une adresse IP frontale, communément comme une adresse IP virtuelle (VIP).<p>L’adresse IP virtuelle doit provenir d’une adresse IP inutilisée dans un des pools IP de réseau logique données pour le Gestionnaire d’équilibrage de charge. 

   ```PowerShell
    $VIPIP = "10.127.134.5"
    $VIPLogicalNetwork = get-networkcontrollerlogicalnetwork -ConnectionUri $uri -resourceid "PublicVIP" -PassInnerException
    
    $FrontEndIPConfig = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfiguration
    $FrontEndIPConfig.ResourceId = "FE1"
    $FrontEndIPConfig.ResourceRef = "/loadBalancers/$LBResourceId/frontendIPConfigurations/$($FrontEndIPConfig.ResourceId)"

    $FrontEndIPConfig.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfigurationProperties
    $FrontEndIPConfig.Properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $FrontEndIPConfig.Properties.Subnet.ResourceRef = $VIPLogicalNetwork.Properties.Subnets[0].ResourceRef
    $FrontEndIPConfig.Properties.PrivateIPAddress = $VIPIP
    $FrontEndIPConfig.Properties.PrivateIPAllocationMethod = "Static"
      
    $LoadBalancerProperties.FrontEndIPConfigurations += $FrontEndIPConfig
   ```

3. Allouer un pool d’adresses back-end, qui contient les adresses IP dynamiques (adresses IP dynamiques) qui composent les membres du jeu d’équilibrage de charge de machines virtuelles. 

   ```PowerShell 
    $BackEndAddressPool = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
    $BackEndAddressPool.ResourceId = "BE1"
    $BackEndAddressPool.ResourceRef = "/loadBalancers/$LBResourceId/backendAddressPools/$($BackEndAddressPool.ResourceId)"

    $BackEndAddressPool.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties

    $LoadBalancerProperties.backendAddressPools += $BackEndAddressPool
   ```

4. Définir une sonde d’intégrité que l’équilibreur de charge utilise pour déterminer l’état d’intégrité des membres du pool de serveur principal.<p>Dans cet exemple, vous définissez une sonde HTTP qui interroge pour le RequestPath de « / health.htm. »  La requête s’exécute toutes les 5 secondes, tel que spécifié par la propriété IntervalInSeconds.<p>La sonde d’intégrité doit recevoir un code de réponse HTTP 200 pour 11 requêtes consécutives de la sonde à prendre en compte l’adresse IP de serveur principal pour être sain. Si l’adresse IP de serveur principal n’est pas intègre, il ne reçoit pas le trafic d’équilibrage de charge.

   >[!IMPORTANT]
   >Ne bloquez pas le trafic vers ou à partir de la première adresse IP dans le sous-réseau n’importe quel contrôle des listes d’accès (ACL) que vous appliquez à l’adresse IP de serveur principal, car c’est le point d’origine pour les sondes.

   L’exemple suivant permet de définir une sonde d’intégrité.

   ```PowerShell
    $Probe = new-object Microsoft.Windows.NetworkController.LoadBalancerProbe
    $Probe.ResourceId = "Probe1"
    $Probe.ResourceRef = "/loadBalancers/$LBResourceId/Probes/$($Probe.ResourceId)"

    $Probe.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerProbeProperties
    $Probe.properties.Protocol = "HTTP"
    $Probe.properties.Port = "80"
    $Probe.properties.RequestPath = "/health.htm"
    $Probe.properties.IntervalInSeconds = 5
    $Probe.properties.NumberOfProbes = 11

    $LoadBalancerProperties.Probes += $Probe
   ```

5.  Définir une règle pour envoyer le trafic qui arrive au niveau de l’adresse IP frontale à l’adresse IP de serveur principal d’équilibrage de charge.  Dans cet exemple, le pool back-end reçoit le trafic TCP vers le port 80.<p>L’exemple suivant permet de définir une règle d’équilibrage de charge :

   ```PowerShell
    $Rule = new-object Microsoft.Windows.NetworkController.LoadBalancingRule
    $Rule.ResourceId = "webserver1"

    $Rule.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancingRuleProperties
    $Rule.Properties.FrontEndIPConfigurations += $FrontEndIPConfig
    $Rule.Properties.backendaddresspool = $BackEndAddressPool 
    $Rule.Properties.protocol = "TCP"
    $Rule.Properties.FrontEndPort = 80
    $Rule.Properties.BackEndPort = 80
    $Rule.Properties.IdleTimeoutInMinutes = 4
    $Rule.Properties.Probe = $Probe

    $LoadBalancerProperties.loadbalancingRules += $Rule
   ```

6. Ajouter la configuration d’équilibrage de charge au contrôleur de réseau.<p>Pour ajouter la configuration d’équilibrage de charge au contrôleur de réseau, utilisez l’exemple suivant :

   ```PowerShell
    $LoadBalancerResource = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $LBResourceId -Properties $LoadBalancerProperties -Force -PassInnerException
   ```

7. Suivez l’exemple suivant pour ajouter les interfaces réseau à ce pool back-end.


## <a name="example-use-slb-for-outbound-nat"></a>Exemple : Utiliser SLB pour NAT de trafic sortant

Dans cet exemple, vous configurez SLB avec un pool back-end pour fournir la fonctionnalité NAT sortante pour une machine virtuelle sur l’espace d’adressage privé d’un réseau virtuel atteindre le trafic sortant vers internet. 

1. Créez les propriétés de l’équilibrage de charge, adresse IP frontale et pool back-end.

   ```PowerShell
    import-module NetworkController
    $URI = "https://sdn.contoso.com"

    $LBResourceId = "OutboundNATMMembers"
    $VIPIP = "10.127.134.6"

    $VIPLogicalNetwork = get-networkcontrollerlogicalnetwork -ConnectionUri $uri -resourceid "PublicVIP" -PassInnerException

    $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties

    $FrontEndIPConfig = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfiguration
    $FrontEndIPConfig.ResourceId = "FE1"
    $FrontEndIPConfig.ResourceRef = "/loadBalancers/$LBResourceId/frontendIPConfigurations/$($FrontEndIPConfig.ResourceId)"

    $FrontEndIPConfig.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfigurationProperties
    $FrontEndIPConfig.Properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $FrontEndIPConfig.Properties.Subnet.ResourceRef = $VIPLogicalNetwork.Properties.Subnets[0].ResourceRef
    $FrontEndIPConfig.Properties.PrivateIPAddress = $VIPIP
    $FrontEndIPConfig.Properties.PrivateIPAllocationMethod = "Static"

    $LoadBalancerProperties.FrontEndIPConfigurations += $FrontEndIPConfig

    $BackEndAddressPool = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
    $BackEndAddressPool.ResourceId = "BE1"
    $BackEndAddressPool.ResourceRef = "/loadBalancers/$LBResourceId/backendAddressPools/$($BackEndAddressPool.ResourceId)"
    $BackEndAddressPool.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties

    $LoadBalancerProperties.backendAddressPools += $BackEndAddressPool
   ```

2. Définir la règle NAT sortante.

   ```PowerShell
    $OutboundNAT = new-object Microsoft.Windows.NetworkController.LoadBalancerOutboundNatRule
    $OutboundNAT.ResourceId = "onat1"
    
    $OutboundNAT.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerOutboundNatRuleProperties
    $OutboundNAT.properties.frontendipconfigurations += $FrontEndIPConfig
    $OutboundNAT.properties.backendaddresspool = $BackEndAddressPool
    $OutboundNAT.properties.protocol = "ALL"

    $LoadBalancerProperties.OutboundNatRules += $OutboundNAT
   ```

3. Ajoutez l’objet d’équilibrage de charge dans le contrôleur de réseau.

   ```PowerShell
    $LoadBalancerResource = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $LBResourceId -Properties $LoadBalancerProperties -Force -PassInnerException
   ```

4. Suivez l’exemple suivant pour ajouter les interfaces réseau à laquelle vous souhaitez fournir un accès internet.

## <a name="example-add-network-interfaces-to-the-back-end-pool"></a>Exemple : Ajouter des interfaces réseau pour le pool back-end
Dans cet exemple, vous ajoutez des interfaces réseau pour le pool back-end.  Vous devez répéter cette étape pour chaque interface réseau qui peut traiter les demandes adressées à l’adresse IP virtuelle. 

Vous pouvez également répéter ce processus sur une seule interface réseau pour l’ajouter à plusieurs objets d’équilibreur de charge. Par exemple, si vous avez un objet d’équilibrage de charge pour une adresse IP virtuelle du serveur web et un objet d’équilibreur de charge distinct pour fournir de NAT sortant
    
1. Obtenir l’objet d’équilibrage de charge contenant le pool back-end pour ajouter une interface réseau.

   ```PowerShell
   $lbresourceid = "LB2"
   $lb = get-networkcontrollerloadbalancer -connectionuri $uri -resourceID $LBResourceId -PassInnerException
  ```

2. Obtenir l’interface réseau et ajouter le pool backendaddress au tableau loadbalancerbackendaddresspools.

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06 -PassInnerException
   $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
   ```  

3. Placez l’interface réseau pour appliquer la modification. 

   ```PowerShell
   new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06 -properties $nic.properties -force -PassInnerException
   ``` 


## <a name="example-use-the-software-load-balancer-for-forwarding-traffic"></a>Exemple : Utiliser l’équilibrage de charge logiciel pour transférer le trafic
Si vous avez besoin mapper une adresse IP virtuelle à une seule interface réseau sur un réseau virtuel sans définir des ports individuels, vous pouvez créer une règle de transfert L3.  Cette règle transmet tout le trafic vers et à partir de la machine virtuelle via l’adresse IP virtuelle attribuée contenu dans un objet de l’adresse IP publique.

Si vous avez défini les paramètres VIP et DIP que le même sous-réseau, puis cela équivaut à effectuer de transfert L3 sans NAT.

>[!NOTE]
>Ce processus ne nécessite pas vous permettent de créer un objet d’équilibrage de charge.  Affectez l’adresse IP publique à l’interface réseau est suffisamment d’informations pour l’équilibrage de charge logiciel effectuer sa configuration.


1. Créer un objet d’adresse IP publique pour qu’il contienne l’adresse IP virtuelle.

   ```PowerShell
   $publicIPProperties = new-object Microsoft.Windows.NetworkController.PublicIpAddressProperties
   $publicIPProperties.ipaddress = "10.127.134.7"
   $publicIPProperties.PublicIPAllocationMethod = "static"
   $publicIPProperties.IdleTimeoutInMinutes = 4
   $publicIP = New-NetworkControllerPublicIpAddress -ResourceId "MyPIP" -Properties $publicIPProperties -ConnectionUri $uri -Force -PassInnerException
   ```

2. Affecter l’adresse IP publique à une interface réseau.

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
   $nic.properties.IpConfigurations[0].Properties.PublicIPAddress = $publicIP
   New-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId $nic.ResourceId -Properties $nic.properties -PassInnerException
   ```

## <a name="example-use-the-software-load-balancer-for-forwarding-traffic-with-a-dynamically-allocated-vip"></a>Exemple : Utiliser l’équilibrage de charge logiciel pour transférer le trafic avec une adresse IP virtuelle allouée de manière dynamique
Cet exemple répète la même action que l’exemple précédent, mais il alloue automatiquement l’adresse IP virtuelle à partir du pool d’adresses IP virtuelles disponible dans l’équilibreur de charge au lieu de spécifier une adresse IP spécifique. 

1. Créer un objet d’adresse IP publique pour qu’il contienne l’adresse IP virtuelle.

   ```PowerShell
   $publicIPProperties = new-object Microsoft.Windows.NetworkController.PublicIpAddressProperties
   $publicIPProperties.PublicIPAllocationMethod = "dynamic"
   $publicIPProperties.IdleTimeoutInMinutes = 4
   $publicIP = New-NetworkControllerPublicIpAddress -ResourceId "MyPIP" -Properties $publicIPProperties -ConnectionUri $uri -Force -PassInnerException
   ```

2. Interroger la ressource d’adresse IP publique afin de déterminer les adresse IP a été attribuée.

   ```PowerShell
    (Get-NetworkControllerPublicIpAddress -ConnectionUri $uri -ResourceId "MyPIP").properties
   ```

   La propriété IpAddress contient l’adresse attribuée.  La sortie doit ressembler à ceci :
   ```
    Counters                 : {}
    ConfigurationState       :
    IpAddress                : 10.127.134.2
    PublicIPAddressVersion   : IPv4
    PublicIPAllocationMethod : Dynamic
    IdleTimeoutInMinutes     : 4
    DnsSettings              :
    ProvisioningState        : Succeeded
    IpConfiguration          :
    PreviousIpConfiguration  :
   ```
 
1. Affecter l’adresse IP publique à une interface réseau.

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
   $nic.properties.IpConfigurations[0].Properties.PublicIPAddress = $publicIP
   New-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId $nic.ResourceId -Properties $nic.properties -PassInnerException
   ```
## <a name="example-remove-a-publicip-address-that-is-being-used-for-forwarding-traffic-and-return-it-to-the-vip-pool"></a>Exemple : Supprimer une adresse de l’adresse IP publique qui est utilisée pour transférer le trafic et le retourner au pool d’adresses IP virtuelles
Cet exemple supprime la ressource d’adresse IP publique qui a été créée par les exemples précédents.  Une fois que l’adresse IP publique est supprimé, la référence à l’adresse IP publique sera automatiquement supprimée de l’interface réseau, le trafic cesse de transfert et l’adresse IP s’affichera pour le pool d’adresses IP virtuelles publiques pour une réutilisation.  

1. Supprimer l’adresse IP publique

   ```PowerShell
   Remove-NetworkControllerPublicIPAddress -ConnectionURI $uri -ResourceId "MyPIP"
   ```

---


 