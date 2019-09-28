---
title: Configurer l’équilibreur de charge logicielle pour l’équilibrage de charge et la traduction d’adresses réseau (NAT)
description: Cette rubrique fait partie du Guide de mise en réseau à définition logicielle sur la gestion des charges de travail client et des réseaux virtuels dans Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 73bff8ba-939d-40d8-b1e5-3ba3ed5439c3
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 80f1319c1abc845d7e63a2d53868bf7a3c381019
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406094"
---
# <a name="configure-the-software-load-balancer-for-load-balancing-and-network-address-translation-nat"></a>Configurer l’équilibreur de charge logicielle pour l’équilibrage de charge et la traduction d’adresses réseau (NAT)

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour apprendre à utiliser la mise \(en réseau logicielle définie SDN\) programme d’équilibrage \(de\) charge logiciel (SLB) pour fournir \(une\)traduction d’adresses réseau sortante NAT, NAT entrant, ou équilibrage de charge entre plusieurs instances d’une application.

## <a name="software-load-balancer-overview"></a>Présentation des Load Balancer de logiciels

Le logiciel SDN load balancer \(SLB\) offre une haute disponibilité et des performances réseau à vos applications. Il s’agit d’un \(TCP de couche\) 4, un équilibreur de charge UDP qui répartit le trafic entrant parmi les instances de service saines dans les services Cloud ou les machines virtuelles définies dans un jeu d’équilibrage de charge.

Configurez SLB pour effectuer les opérations suivantes :

- Équilibrer la charge du trafic entrant externe à un réseau virtuel \(vers\)des machines virtuelles de machines virtuelles, également appelé équilibrage de charge d’adresse IP virtuelle publique.
- Équilibrer la charge du trafic entrant entre les machines virtuelles d’un réseau virtuel, entre les machines virtuelles dans les services Cloud, ou entre les ordinateurs locaux et les machines virtuelles dans un réseau virtuel intersite. 
- Transférer le trafic réseau de machine virtuelle du réseau virtuel vers des destinations externes à l’aide de la traduction d’adresses réseau (NAT), également appelée NAT de trafic sortant.
- Transférer le trafic externe vers une machine virtuelle spécifique, également appelée NAT entrant.




## <a name="example-create-a-public-vip-for-load-balancing-a-pool-of-two-vms-on-a-virtual-network"></a>Exemple : Créer une adresse IP virtuelle publique pour l’équilibrage de charge d’un pool de deux machines virtuelles sur un réseau virtuel

Dans cet exemple, vous créez un objet d’équilibrage de charge avec une adresse IP virtuelle publique et deux machines virtuelles en tant que membres du pool pour traiter les demandes à l’adresse IP virtuelle. Cet exemple de code ajoute également une sonde d’intégrité HTTP pour détecter si l’un des membres du pool devient non réactif.

1. Préparez l’objet d’équilibrage de charge.

   ```PowerShell
    import-module NetworkController

    $URI = "https://sdn.contoso.com"

    $LBResourceId = "LB2"

    $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties
   ```

2. Attribuez une adresse IP frontale, communément appelée adresse IP virtuelle (VIP).<p>L’adresse IP virtuelle doit provenir d’une adresse IP inutilisée dans l’un des pools d’adresses IP de réseau logique donné au gestionnaire d’équilibrage de charge. 

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

3. Allouez un pool d’adresses principales, qui contient les adresses IP dynamiques (DIP) qui composent les membres de l’ensemble de machines virtuelles à charge équilibrée. 

   ```PowerShell 
    $BackEndAddressPool = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
    $BackEndAddressPool.ResourceId = "BE1"
    $BackEndAddressPool.ResourceRef = "/loadBalancers/$LBResourceId/backendAddressPools/$($BackEndAddressPool.ResourceId)"

    $BackEndAddressPool.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties

    $LoadBalancerProperties.backendAddressPools += $BackEndAddressPool
   ```

4. Définissez une sonde d’intégrité utilisée par l’équilibreur de charge pour déterminer l’état d’intégrité des membres du pool principal.<p>Dans cet exemple, vous définissez une sonde HTTP qui interroge le RequestPath de « /Health.htm. ».  La requête s’exécute toutes les 5 secondes, comme spécifié par la propriété IntervalInSeconds.<p>La sonde d’intégrité doit recevoir le code de réponse HTTP 200 pour 11 requêtes consécutives pour que la sonde considère que l’adresse IP principale est saine. Si l’adresse IP principale n’est pas intègre, elle ne reçoit pas de trafic de l’équilibreur de charge.

   >[!IMPORTANT]
   >Ne bloquez pas le trafic vers ou depuis la première adresse IP du sous-réseau pour les listes de Access Control (ACL) que vous appliquez à l’adresse IP principale, car il s’agit du point d’origine des sondes.

   Utilisez l’exemple suivant pour définir une sonde d’intégrité.

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

5. Définissez une règle d’équilibrage de charge pour envoyer le trafic qui arrive à l’adresse IP frontale vers l’adresse IP principale.  Dans cet exemple, le pool principal reçoit le trafic TCP sur le port 80.<p>Utilisez l’exemple suivant pour définir une règle d’équilibrage de charge :

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

6. Ajoutez la configuration de l’équilibrage de charge au contrôleur de réseau.<p>Utilisez l’exemple suivant pour ajouter la configuration de l’équilibrage de charge au contrôleur de réseau :

   ```PowerShell
    $LoadBalancerResource = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $LBResourceId -Properties $LoadBalancerProperties -Force -PassInnerException
   ```

7. Suivez l’exemple suivant pour ajouter les interfaces réseau à ce pool principal.


## <a name="example-use-slb-for-outbound-nat"></a>Exemple : Utiliser SLB pour le NAT sortant

Dans cet exemple, vous configurez SLB avec un pool principal pour fournir une fonctionnalité NAT sortante pour une machine virtuelle sur l’espace d’adressage privé d’un réseau virtuel pour atteindre le trafic sortant vers Internet. 

1. Créez les propriétés de l’équilibrage de charge, l’adresse IP frontale et le pool principal.

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

2. Définissez la règle NAT de trafic sortant.

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

4. Suivez l’exemple suivant pour ajouter les interfaces réseau auxquelles vous souhaitez fournir l’accès à Internet.

## <a name="example-add-network-interfaces-to-the-back-end-pool"></a>Exemple : Ajouter des interfaces réseau au pool principal
Dans cet exemple, vous ajoutez des interfaces réseau au pool principal.  Vous devez répéter cette étape pour chaque interface réseau qui peut traiter les demandes adressées à l’adresse IP virtuelle. 

Vous pouvez également répéter ce processus sur une seule interface réseau pour l’ajouter à plusieurs objets d’équilibrage de charge. Par exemple, si vous avez un objet d’équilibrage de charge pour une adresse IP virtuelle de serveur Web et un objet d’équilibrage de charge distinct pour fournir un NAT sortant.
    
1. Obtenez l’objet d’équilibrage de charge contenant le pool principal pour ajouter une interface réseau.

   ```PowerShell
   $lbresourceid = "LB2"
   $lb = get-networkcontrollerloadbalancer -connectionuri $uri -resourceID $LBResourceId -PassInnerException
   ```

2. Récupérez l’interface réseau et ajoutez le pool backendaddress au tableau loadbalancerbackendaddresspools.

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06 -PassInnerException
   $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
   ```  

3. Mettez l’interface réseau pour appliquer la modification. 

   ```PowerShell
   new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06 -properties $nic.properties -force -PassInnerException
   ``` 


## <a name="example-use-the-software-load-balancer-for-forwarding-traffic"></a>Exemple : Utiliser le Load Balancer logiciel pour transférer le trafic
Si vous avez besoin de mapper une adresse IP virtuelle à une seule interface réseau sur un réseau virtuel sans définir de ports individuels, vous pouvez créer une règle de transfert L3.  Cette règle transfère tout le trafic vers et depuis la machine virtuelle via l’adresse IP virtuelle affectée contenue dans un objet PublicIPAddress.

Si vous avez défini l’adresse IP virtuelle et DIP comme le même sous-réseau, cela revient à effectuer un transfert L3 sans NAT.

>[!NOTE]
>Ce processus ne nécessite pas la création d’un objet d’équilibrage de charge.  L’attribution du PublicIPAddress à l’interface réseau est suffisamment d’informations pour que le Load Balancer logiciel effectue sa configuration.


1. Créez un objet d’adresse IP publique pour contenir l’adresse IP virtuelle.

   ```PowerShell
   $publicIPProperties = new-object Microsoft.Windows.NetworkController.PublicIpAddressProperties
   $publicIPProperties.ipaddress = "10.127.134.7"
   $publicIPProperties.PublicIPAllocationMethod = "static"
   $publicIPProperties.IdleTimeoutInMinutes = 4
   $publicIP = New-NetworkControllerPublicIpAddress -ResourceId "MyPIP" -Properties $publicIPProperties -ConnectionUri $uri -Force -PassInnerException
   ```

2. Affectez PublicIPAddress à une interface réseau.

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
   $nic.properties.IpConfigurations[0].Properties.PublicIPAddress = $publicIP
   New-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId $nic.ResourceId -Properties $nic.properties -PassInnerException
   ```

## <a name="example-use-the-software-load-balancer-for-forwarding-traffic-with-a-dynamically-allocated-vip"></a>Exemple : Utiliser le Load Balancer logiciel pour transférer le trafic avec une adresse IP virtuelle allouée dynamiquement
Cet exemple répète la même action que dans l’exemple précédent, mais il alloue automatiquement l’adresse IP virtuelle à partir du pool disponible de VIP dans l’équilibreur de charge au lieu de spécifier une adresse IP spécifique. 

1. Créez un objet d’adresse IP publique pour contenir l’adresse IP virtuelle.

   ```PowerShell
   $publicIPProperties = new-object Microsoft.Windows.NetworkController.PublicIpAddressProperties
   $publicIPProperties.PublicIPAllocationMethod = "dynamic"
   $publicIPProperties.IdleTimeoutInMinutes = 4
   $publicIP = New-NetworkControllerPublicIpAddress -ResourceId "MyPIP" -Properties $publicIPProperties -ConnectionUri $uri -Force -PassInnerException
   ```

2. Interrogez la ressource PublicIPAddress pour déterminer l’adresse IP qui a été attribuée.

   ```PowerShell
    (Get-NetworkControllerPublicIpAddress -ConnectionUri $uri -ResourceId "MyPIP").properties
   ```

   La propriété IpAddress contient l’adresse assignée.  La sortie doit ressembler à ceci :
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
 
3. Affectez PublicIPAddress à une interface réseau.

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
   $nic.properties.IpConfigurations[0].Properties.PublicIPAddress = $publicIP
   New-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId $nic.ResourceId -Properties $nic.properties -PassInnerException
   ```
   ## <a name="example-remove-a-publicip-address-that-is-being-used-for-forwarding-traffic-and-return-it-to-the-vip-pool"></a>Exemple : Supprimer une adresse adresse IP publique utilisée pour transférer le trafic et la renvoyer au pool d’adresses IP virtuelles
   Cet exemple supprime la ressource PublicIPAddress qui a été créée par les exemples précédents.  Une fois le PublicIPAddress supprimé, la référence au PublicIPAddress est automatiquement supprimée de l’interface réseau, le trafic ne sera pas transféré et l’adresse IP sera renvoyée au pool d’adresses IP VIRTUELles publiques pour une réutilisation.  

4. Supprimer le adresse IP publique

   ```PowerShell
   Remove-NetworkControllerPublicIPAddress -ConnectionURI $uri -ResourceId "MyPIP"
   ```

---


 