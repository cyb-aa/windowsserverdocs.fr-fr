---
title: Clustering invité dans un réseau virtuel
description: Les machines virtuelles connectées à un réseau virtuel sont uniquement autorisées à utiliser les adresses IP affectées par le contrôleur de réseau pour communiquer sur le réseau.  Les technologies de clustering qui requièrent une adresse IP flottante, telles que le clustering de basculement Microsoft, nécessitent des étapes supplémentaires pour fonctionner correctement.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8e9e5c81-aa61-479e-abaf-64c5e95f90dc
ms.author: grcusanz
author: shortpatti
ms.date: 08/26/2018
ms.openlocfilehash: 05704beeae27bd9de9ad0c5cf578581c650a976f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406041"
---
# <a name="guest-clustering-in-a-virtual-network"></a>Clustering invité dans un réseau virtuel

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Les machines virtuelles connectées à un réseau virtuel sont uniquement autorisées à utiliser les adresses IP affectées par le contrôleur de réseau pour communiquer sur le réseau.  Les technologies de clustering qui requièrent une adresse IP flottante, telles que le clustering de basculement Microsoft, nécessitent des étapes supplémentaires pour fonctionner correctement.

La méthode permettant de rendre l’adresse IP flottante accessible consiste à utiliser un logiciel Load Balancer \(SLB @ no__t-1 IP virtuel \(VIP @ no__t-3.  L’équilibreur de charge logiciel doit être configuré avec une sonde d’intégrité sur un port de cette adresse IP afin que SLB dirige le trafic vers l’ordinateur qui a actuellement cette adresse IP.


## <a name="example-load-balancer-configuration"></a>Exemple : Configuration de l’équilibrage de charge

Cet exemple suppose que vous avez déjà créé les machines virtuelles qui deviendront des nœuds de cluster et que vous les avez attachées à un réseau virtuel.  Pour obtenir de l’aide, consultez [créer une machine virtuelle et se connecter à un réseau virtuel locataire ou à un réseau local virtuel](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm).  

Dans cet exemple, vous allez créer une adresse IP virtuelle (192.168.2.100) pour représenter l’adresse IP flottante du cluster, puis configurer une sonde d’intégrité pour analyser le port TCP 59999 afin de déterminer le nœud actif.

1. Sélectionnez l’adresse IP virtuelle.<p>Préparez-vous en affectant une adresse IP VIP, qui peut être n’importe quelle adresse inutilisée ou réservée dans le même sous-réseau que les nœuds du cluster.  L’adresse IP virtuelle doit correspondre à l’adresse flottante du cluster.

   ```PowerShell
   $VIP = "192.168.2.100"
   $subnet = "Subnet2"
   $VirtualNetwork = "MyNetwork"
   $ResourceId = "MyNetwork_InternalVIP"
   ```

2. Créez l’objet de propriétés d’équilibrage de charge.

   ```PowerShell
   $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties
   ```

3. Créez une adresse IP front @ no__t-0end.

   ```PowerShell
   $LoadBalancerProperties.frontendipconfigurations += $FrontEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfiguration
   $FrontEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfigurationProperties
   $FrontEnd.resourceId = "Frontend1"
   $FrontEnd.resourceRef = "/loadBalancers/$ResourceId/frontendIPConfigurations/$($FrontEnd.resourceId)"
   $FrontEnd.properties.subnet = new-object Microsoft.Windows.NetworkController.Subnet
   $FrontEnd.properties.subnet.ResourceRef = "/VirtualNetworks/MyNetwork/Subnets/Subnet2"
   $FrontEnd.properties.privateIPAddress = $VIP
   $FrontEnd.properties.privateIPAllocationMethod = "Static"
   ```

4. Créez un pool @ no__t-0end de retour pour contenir les nœuds de cluster.

   ```PowerShell
   $BackEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
   $BackEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties
   $BackEnd.resourceId = "Backend1"
   $BackEnd.resourceRef = "/loadBalancers/$ResourceId/backendAddressPools/$($BackEnd.resourceId)"
   $LoadBalancerProperties.backendAddressPools += $BackEnd
   ```

5. Ajoutez une sonde pour détecter le nœud de cluster sur lequel l’adresse flottante est actuellement active. 

   >[!NOTE]
   >Requête de sondage sur l’adresse permanente de la machine virtuelle au niveau du port défini ci-dessous.  Le port doit répondre uniquement sur le nœud actif. 

   ```PowerShell
   $LoadBalancerProperties.probes += $lbprobe = new-object Microsoft.Windows.NetworkController.LoadBalancerProbe
   $lbprobe.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerProbeProperties

   $lbprobe.ResourceId = "Probe1"
   $lbprobe.resourceRef = "/loadBalancers/$ResourceId/Probes/$($lbprobe.resourceId)"
   $lbprobe.properties.protocol = "TCP"
   $lbprobe.properties.port = "59999"
   $lbprobe.properties.IntervalInSeconds = 5
   $lbprobe.properties.NumberOfProbes = 11
   ```

6. Ajoutez les règles d’équilibrage de charge pour le port TCP 1433.<p>Vous pouvez modifier le protocole et le port selon vos besoins.  Vous pouvez également répéter cette étape plusieurs fois pour les ports supplémentaires et les protcols sur cette adresse IP virtuelle.  Il est important que une enablefloatingip soit défini sur $true, car cela indique à l’équilibreur de charge d’envoyer le paquet au nœud avec l’adresse IP virtuelle d’origine en place.

   ```PowerShell
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
   ```

7. Créez l’équilibreur de charge dans le contrôleur de réseau.

   ```PowerShell
   $lb = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $ResourceId -Properties $LoadBalancerProperties -Force
   ```

8. Ajoutez les nœuds de cluster au pool principal.<p>Vous pouvez ajouter autant de nœuds au pool que nécessaire pour le cluster.

   ```PowerShell
   # Cluster Node 1

   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid "ClusterNode1_Network-Adapter"
   $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
   $nic = new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid $nic.resourceid -properties $nic.properties -force

    # Cluster Node 2

   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid "ClusterNode2_Network-Adapter"
   $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
   $nic = new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid $nic.resourceid -properties $nic.properties -force
   ```

   Une fois que vous avez créé l’équilibrage de charge et ajouté les interfaces réseau au pool principal, vous êtes prêt à configurer le cluster.  

9. Facultatif Si vous utilisez un cluster de basculement Microsoft, passez à l’exemple suivant. 

## <a name="example-2-configuring-a-microsoft-failover-cluster"></a>Exemple 2 : Configuration d’un cluster de basculement Microsoft

Vous pouvez utiliser les étapes suivantes pour configurer un cluster de basculement.

1. Installez et configurez les propriétés d’un cluster de basculement.

   ```PowerShell
   add-windowsfeature failover-clustering -IncludeManagementTools
   Import-module failoverclusters

   $ClusterName = "MyCluster"
   
   $ClusterNetworkName = "Cluster Network 1"
   $IPResourceName =  
   $ILBIP = “192.168.2.100” 

   $nodes = @("DB1", "DB2")
   ```

2. Créez le cluster sur un nœud.

   ```PowerShell
   New-Cluster -Name $ClusterName -NoStorage -Node $nodes[0]
   ```

3. Arrêtez la ressource de cluster.

   ```PowerShell
   Stop-ClusterResource "Cluster Name" 
   ```

4. Définissez l’adresse IP du cluster et le port de la sonde.<p>L’adresse IP doit correspondre à l’adresse IP frontale utilisée dans l’exemple précédent, et le port de la sonde doit correspondre au port de la sonde dans l’exemple précédent.

   ```PowerShell
   Get-ClusterResource "Cluster IP Address" | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

5. Démarrez les ressources du cluster.

   ```PowerShell
    Start-ClusterResource "Cluster IP Address"  -Wait 60 
    Start-ClusterResource "Cluster Name"  -Wait 60 
   ```

6. Ajoutez les nœuds restants.

   ```PowerShell
   Add-ClusterNode $nodes[1]
   ```

_**Votre cluster est actif.**_ Le trafic adressé à l’adresse IP virtuelle sur le port spécifié est dirigé vers le nœud actif.

---