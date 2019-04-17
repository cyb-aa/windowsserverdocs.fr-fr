---
title: Configurer l’équilibrage de charge logicielle d’équilibrage de charge et la traduction d’adresses réseau (NAT)
description: Cette rubrique fait partie du guide logiciel défini de mise en réseau sur la façon de gérer les charges de travail clientes et des réseaux virtuels dans Windows Server2016.
manager: brianlic
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
ms.openlocfilehash: 7f0393db564061caa0bc8f18b1d623f24749b46c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-software-load-balancer-for-load-balancing-and-network-address-translation-nat"></a>Configurer l’équilibrage de charge logicielle d’équilibrage de charge et la traduction d’adresses réseau (NAT)

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour savoir comment utiliser l’équilibrage de charge logicielle \(SDN\) logiciel défini de mise en réseau \(SLB\) de fournir sortant NAT network address translation, NAT entrant, ou d’équilibrage de charge entre plusieurs instances d’une application.

Cette rubrique contient les sections suivantes.

- [Vue d’ensemble d’équilibrage de charge logicielle](#bkmk_slbover)
- [Exemple: Création d’une adresse IP virtuelle publique pour un pool de deux ordinateurs virtuels sur un réseau virtuel d’équilibrage de charge](#bkmk_publicvip)
- [Exemple: Utiliser les différentes pour NAT sortant](#bkmk_obnat)
- [Exemple: Ajoutez des interfaces réseau pour le pool principal](#bkmk_backend)
- [Exemple: Utiliser l’équilibrage de charge logicielle pour transférer le trafic](#bkmk_forward)

## <a name="bkmk_slbover"></a>Vue d’ensemble d’équilibrage de charge logicielle

L’équilibreur de charge logicielle SDN \(SLB\) offre une haute disponibilité et les performances réseau à vos applications. Il s’agit d’une couche 4 \ équilibrage qui distribue le trafic entrant sur les instances de service intègre dans les services cloud ou les ordinateurs virtuels définis dans un ensemble de l’équilibrage de la charge de la charge (TCP, UDP\).

Vous pouvez configurer les différentes pour effectuer les opérations suivantes.

* Charge équilibrer le trafic entrant externe à un réseau virtuel pour les ordinateurs virtuels \(VMs\). Il s’agit d’équilibrage de charge adresse IP virtuelle publique.
* Charge équilibrer le trafic entrant entre les ordinateurs virtuels dans un réseau virtuel, entre les ordinateurs virtuels dans les services cloud ou entre les ordinateurs locaux et les ordinateurs virtuels dans un réseau virtuel intersite. 
* Transférer le trafic réseau de machine virtuelle à partir du réseau virtuel pour les destinations externes à l’aide de la traduction d’adresses réseau (NAT).  Il s’agit sortant NAT.
* Transférer le trafic externe à un ordinateur virtuel spécifique.  Il s’agit NAT entrant.

>[!IMPORTANT]
>Un problème connu empêche les objets de l’équilibrage de charge dans le module NetworkController Windows PowerShell de fonctionner correctement dans Windows Server2016 5. La solution de contournement consiste à utiliser les tables de hachage dynamique et Invoke-WebRequest à la place. Cette méthode est illustrée dans les exemples suivants.


## <a name="bkmk_publicvip"></a>Exemple: Création d’une adresse IP virtuelle publique pour un pool de deux ordinateurs virtuels sur un réseau virtuel d’équilibrage de charge

Vous pouvez utiliser cet exemple montre comment créer un objet d’équilibrage de charge avec une adresse IP virtuelle publique et deux ordinateurs virtuels en tant que membres du pool pour traiter les demandes à l’adresse IP virtuelle.  Cet exemple de code ajoute également un sondage de l’intégrité HTTP pour détecter si un des membres pool ne réagit.

###<a name="step-1-prepare-the-load-balancer-object"></a>Étape1: Préparer l’objet d’équilibrage de charge
Vous pouvez utiliser l’exemple suivant pour préparer l’objet d’équilibrage de charge.

    $lbresourceId = "LB2"

    $lbproperties = @{}
    $lbproperties.frontendipconfigurations = @()
    $lbproperties.backendAddressPools = @()
    $lbproperties.probes = @()
    $lbproperties.loadbalancingRules = @()
    $lbproperties.OutboundNatRules = @()

###<a name="step-2-assign-a-front-end-ip"></a>Étape2: Attribuer une adresse IP frontale
L’adresse IP frontal est généralement désigné comme un IP virtuelle (VIP).  L’adresse IP virtuelle doit être prise à partir d’une adresse IP non utilisée dans un des Pool IP qui a été précédemment donné pour le Gestionnaire d’équilibrage de charge réseau logique.

Vous pouvez utiliser l’exemple suivant pour attribuer une adresse IP frontale.

    $vipip = "10.127.132.5"
    $vipln = get-networkcontrollerlogicalnetwork -ConnectionUri $uri -resourceid "f8f67956-3906-4303-94c5-09cf91e7e311"

    $fe = @{}
    $fe.resourceId = "FE1"
    $fe.resourceRef = "/loadBalancers/$lbresourceId/frontendIPConfigurations/$($fe.resourceId)"
    $fe.properties = @{}
    $fe.properties.subnet = @{}
    $fe.properties.subnet.ResourceRef = $vipln.properties.Subnets[0].ResourceRef
    $fe.properties.privateIPAddress = $vipip
    $fe.properties.privateIPAllocationMethod = "Static"
    $lbproperties.frontendipconfigurations += $fe

###<a name="step-3-allocate-a-backend-address-pool"></a>Étape3: Allouer un pool d’adresses principal
Le pool d’adresses principal contient les adresses IP dynamiques (DIP) constituent les membres de l’ensemble de l’équilibrage de la charge d’ordinateurs virtuels. Dans cette étape, vous allouez uniquement le pool; les configurations IP sont ajoutées dans une étape ultérieure.

Vous pouvez utiliser l’exemple suivant à allouer un pool d’adresses principal.
 
    $backend = @{}
    $backend.resourceId = "BE1"
    $backend.resourceRef = "/loadBalancers/$lbresourceId/backendAddressPools/$($backend.resourceId)"
    $lbproperties.backendAddressPools += $backend

###<a name="step-4-define-a-health-probe"></a>Étape4: Définir une sonde d’intégrité
Les sondes d’intégrité sont utilisés par l’équilibrage de charge pour déterminer l’état d’intégrité des membres pool principal. Dans cet exemple, vous définissez une sonde HTTP qui interroge vers le RequestPath de «/ health.htm».  La requête est exécutée toutes les 5secondes, tel que spécifié par la propriété IntervalInSeconds.

L’analyse d’intégrité doit recevoir un code de réponse HTTP de 200 pour les requêtes consécutives 11 pour la sonde à prendre en compte l’adresse IP principale pour être sain. Si l’adresse IP de serveur principal n’est pas intègre, l’équilibrage de charge n’envoie pas le trafic à l’adresse IP.

>[!Note]
>Il est important que les listes de contrôle d’accès que vous appliquez à l’adresse IP principale ne bloquent pas le trafic vers ou depuis le premier IP dans le sous-réseau, car c’est le point d’origine pour les sondes.

Vous pouvez utiliser l’exemple suivant pour définir une sonde d’intégrité.
 
    $lbprobe = @{}
    $lbprobe.ResourceId = "Probe1"
    $lbprobe.resourceRef = "/loadBalancers/$lbresourceId/Probes/$($lbprobe.resourceId)"
    $lbprobe.properties = @{}
    $lbprobe.properties.protocol = "HTTP"
    $lbprobe.properties.port = "80"
    $lbprobe.properties.RequestPath = "/health.htm"
    $lbprobe.properties.IntervalInSeconds = 5
    $lbprobe.properties.NumberOfProbes = 11
    $lbproperties.probes += $lbprobe

###<a name="step-5-define-a-load-balancing-rule"></a>Étape5: Définir une règle d’équilibrage de charge
Cette règle d’équilibrage de charge définit comment le trafic qui arrive à l’adresse IP frontal doit être envoyé à l’adresse IP de serveur principal.  Dans cet exemple, le trafic vers le port 80 TCP est envoyé au pool de serveur principal.

Vous pouvez utiliser l’exemple suivant pour définir une règle d’équilibrage de charge.

    $lbrule = @{}
    $lbrule.ResourceId = "webserver1"
    $lbrule.properties = @{}
    $lbrule.properties.FrontEndIPConfigurations = @()
    $lbrule.properties.FrontEndIPConfigurations += $fe
    $lbrule.properties.backendaddresspool = $backend 
    $lbrule.properties.protocol = "TCP"
    $lbrule.properties.frontendPort = 80
    $lbrule.properties.Probe = $lbprobe
    $lbproperties.loadbalancingRules += $lbrule

###<a name="step-6-add-the-load-balancer-configuration-to-network-controller"></a>Étape6: Ajouter la configuration de l’équilibrage de charge pour le contrôleur de réseau
Jusqu'à présent dans cet exemple, tous les objets créés sont dans la mémoire de la session Windows PowerShell. Cette étape ajoute les objets au contrôleur de réseau.

Vous pouvez utiliser l’exemple suivant pour ajouter la configuration de l’équilibrage de charge pour le contrôleur de réseau.

    $lb = @{}
    $lb.ResourceId = $lbresourceid
    $lb.properties = $lbproperties

    $body = convertto-json $lb -Depth 100

    Invoke-WebRequest -Headers @{"Accept"="application/json"} -ContentType "application/json; charset=UTF-8" -Method "Put" -Uri "$uri/Networking/v1/loadbalancers/$lbresourceid" -Body $body -DisableKeepAlive -UseBasicParsing

Après cette étape, vous devez suivre l’exemple ci-dessous pour ajouter les interfaces réseau à ce pool principal.

## <a name="bkmk_obnat"></a>Exemple: Utiliser les différentes pour NAT sortant

Vous pouvez utiliser cet exemple pour configurer les différentes avec un pool principal de la fourniture sortante fonctionnalité NAT pour un ordinateur virtuel sur l’espace d’adressage privé d’un réseau virtuel atteindre sortant à internet.

###<a name="step-1-create-the-loadbalancer-properties-front-end-ip-and-backend-pool"></a>Étape1: Créer les propriétés loadbalancer, frontal IP et Pool principal.
Vous pouvez utiliser l’exemple suivant pour créer les propriétés loadbalancer, frontal IP et Pool principal.

    $lbresourceId = "OutboundNATMembers"
    $vipip = "10.127.132.7"

    $vipln = get-networkcontrollerlogicalnetwork -ConnectionUri $uri -resourceid "f8f67956-3906-4303-94c5-09cf91e7e311"

    $lbproperties = @{}
    $lbproperties.frontendipconfigurations = @()
    $lbproperties.backendAddressPools = @()
    $lbproperties.probes = @()
    $lbproperties.loadbalancingRules = @()
    $lbproperties.OutboundNatRules = @()

    $fe = @{}
    $fe.resourceId = "FE1"
    $fe.resourceRef = "/loadBalancers/$lbresourceId/frontendIPConfigurations/$($fe.resourceId)"
    $fe.properties = @{}
    $fe.properties.subnet = @{}
    $fe.properties.subnet.ResourceRef = $vipln.properties.Subnets[0].ResourceRef
    $fe.properties.privateIPAddress = $vipip
    $fe.properties.privateIPAllocationMethod = "Static"
    $lbproperties.frontendipconfigurations += $fe

    $backend = @{}
    $backend.resourceId = "BE1"
    $backend.resourceRef = "/loadBalancers/$lbresourceId/backendAddressPools/$($backend.resourceId)"
    $lbproperties.backendAddressPools += $backend

###<a name="step-2-define-the-outbound-nat-rule"></a>Étape2: Définir la règle NAT sortante
Vous pouvez utiliser l’exemple suivant pour définir la règle NAT sortante. 

    $onat = @{}
    $onat.ResourceId = "onat1"
    $onat.properties = @{}
    $onat.properties.frontendipconfigurations = @()
    $onat.properties.frontendipconfigurations += $fe
    $onat.properties.backendaddresspool = $backend
    $onat.properties.protocol = "ALL"
    $lbproperties.OutboundNatRules += $onat

###<a name="step-3-add-the-load-balancer-object-in-network-controller"></a>Étape3: Ajouter l’objet d’équilibrage de charge dans le contrôleur de réseau
Vous pouvez utiliser l’exemple suivant pour ajouter l’objet d’équilibrage de charge dans le contrôleur de réseau.

    $lb = @{}
    $lb.ResourceId = $lbresourceid
    $lb.properties = $lbproperties

    $body = convertto-json $lb -Depth 100

    Invoke-WebRequest -Headers @{"Accept"="application/json"} -ContentType "application/json; charset=UTF-8" -Method "Put" -Uri "$uri/Networking/v1/loadbalancers/$lbresourceid" -Body $body -DisableKeepAlive -UseBasicParsing

Dans l’étape suivante, vous pouvez ajouter les interfaces réseau auquel vous voulez fournir l’accès à internet.

## <a name="bkmk_backend"></a>Exemple: Ajoutez des interfaces réseau pour le pool principal
Vous pouvez utiliser cet exemple pour ajouter des interfaces réseau pour le pool principal.

Vous devez répéter cette étape pour chaque interface réseau qui peut traiter les requêtes effectuées à l’adresse IP virtuelle. Vous pouvez également Répétez ce processus sur une seule interface réseau pour l’ajouter à plusieurs objets d’équilibrage de charge. Par exemple, si vous disposez d’un objet d’équilibrage de charge pour un serveur Web VIP et un objet d’équilibrage de charge distincts pour fournir des NAT sortant.
    
### <a name="step-1-get-the-load-balancer-object-containing-the-back-end-pool-to-which-you-will-add-a-network-interface"></a>Étape1: Obtenir l’objet d’équilibrage de charge contenant le pool principal auquel vous allez ajouter une interface réseau
Vous pouvez utiliser l’exemple suivant pour récupérer l’objet d’équilibrage de charge.

    $lbresourceid = "LB2"
    $lb = (Invoke-WebRequest -Headers @{"Accept"="application/json"} -ContentType "application/json; charset=UTF-8" -Method "Get" -Uri "$uri/Networking/v1/loadbalancers/$lbresourceid" -DisableKeepAlive -UseBasicParsing).content | convertfrom-json 

### <a name="step-2-get-the-network-interface-and-add-the-backendaddress-pool-to-the-loadbalancerbackendaddresspools-array"></a>Étape2: Obtenir de l’interface réseau et ajouter le pool backendaddress dans le tableau loadbalancerbackendaddresspools.
Vous pouvez utiliser l’exemple suivant pour obtenir de l’interface réseau et ajouter le pool backendaddress dans le tableau loadbalancerbackendaddresspools.

    $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
    $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
    
### <a name="step-3-put-the-network-interface-to-apply-the-change"></a>Étape3: Placer l’interface réseau pour appliquer la modification
Vous pouvez utiliser l’exemple suivant pour placer l’interface réseau pour appliquer la modification.

    new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06 -properties $nic.properties -force
 
## <a name="bkmk_forward"></a>Exemple: Utiliser l’équilibrage de charge logicielle pour transférer le trafic
Si vous avez besoin mapper une adresse IP virtuelle à une seule interface réseau sur un réseau virtuel sans définir les ports individuels, vous pouvez créer une règle de transfert L3.  Cette règle transmet tout le trafic vers et depuis l’ordinateur virtuel via l’adresse IP virtuelle affectée, qui doit être contenu dans un objet PublicIPAddress.

Si l’adresse IP virtuelle et DIP sont définies dans le même sous-réseau, puis cela équivaut à effectuer le transfert L3 sans NAT.

>[!NOTE]
>Ce processus ne nécessite pas de créer un objet d’équilibrage de charge.  Affectation de la PublicIPAddress à l’interface réseau est suffisamment d’informations pour l’équilibrage de charge logicielle effectuer sa configuration.

###<a name="step-1-create-a-public-ip-object-to-contain-the-vip"></a>Étape1: Créer un objet IP public pour contenir l’adresse IP virtuelle
Vous pouvez utiliser l’exemple suivant pour créer un objet IP public.

    $publicIPProperties = new-object Microsoft.Windows.NetworkController.PublicIpAddressProperties
    $publicIPProperties.ipaddress = "10.127.132.6"
    $publicIPProperties.PublicIPAllocationMethod = "static"
    $publicIPProperties.IdleTimeoutInMinutes = 4
    $publicIP = New-NetworkControllerPublicIpAddress -ResourceId "MyPIP" -Properties $publicIPProperties -ConnectionUri $uri

####<a name="step-2-assign-the-publicipaddress-to-a-network-interface"></a>Étape2: Affecter les PublicIPAddress à une interface réseau
Vous pouvez utiliser l’exemple suivant pour affecter le PublicIPAddress à une interface réseau.

    $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
    $nic.properties.IpConfigurations[0].Properties.PublicIPAddress = $publicIP
    New-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId $nic.ResourceId -Properties $nic.properties



 