---
title: Performances de la passerelle Windows Server 2019
description: ''
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/22/2018
ms.openlocfilehash: a6530b29ce7ffb0d18e0266e70cb2ca45188915c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845630"
---
# <a name="windows-server-2019-gateway-performance"></a>Performances de la passerelle Windows Server 2019

>S’applique à : Windows Server


Dans Windows Server 2016, une des préoccupations des clients a été l’incapacité de passerelle SDN pour satisfaire les besoins de débit des réseaux modernes. Le débit du réseau de tunnels IPsec et GRE présente des limitations avec le débit de connexion unique pour la connectivité IPsec en cours d’environ 300 Mbits/s et pour la connectivité GRE est d’environ 2,5 Gbits/s.

Nous avons considérablement amélioré dans Windows Server 2019, avec les numéros : envol à 1,8 Gbits/s et 15 Gbit/s pour les connexions IPsec et GRE, respectivement. Tout ceci, avec des réductions importantes dans les cycles de processeur / par octet, fournissant ainsi un débit très hautes performances avec beaucoup moins l’utilisation du processeur.

## <a name="enable-high-performance-with-gateways-in-windows-server-2019"></a>Activer les hautes performances avec les passerelles dans Windows Server 2019

Pour **connexions GRE**, une fois que vous déployez/mise à niveau vers Windows Server 2019 s’appuie sur les machines virtuelles de passerelle, vous devez voir automatiquement de l’amélioration des performances. Aucune opération manuelle n’est impliquées.

Pour **des connexions IPsec**, par défaut, lorsque vous créez la connexion pour vos réseaux virtuels, vous obtenez les numéros de chemin d’accès et les performances de données Windows Server 2016. Pour activer le chemin d’accès des données de Windows Server 2019, procédez comme suit :

   1. Sur une machine virtuelle de passerelle SDN, accédez à **Services** console (services.msc).
   2. Recherchez le service nommé **Service de passerelle Azure**et définissez le type de démarrage sur **automatique**.
   3. Redémarrez la machine virtuelle de passerelle.
      Les connexions actives sur ce basculement de passerelle vers une machine virtuelle de passerelle redondant.
   4. Répétez les étapes précédentes pour le reste de la passerelle de machines virtuelles.

>[!TIP]
>Pour de meilleurs résultats, vérifiez que le cipherTransformationConstant et authenticationTransformConstant dans quickMode des paramètres de la connexion IPsec utilise le **GCMAES256** suite de chiffrement.
>
>Pour optimiser les performances, le matériel hôte de passerelle doit prendre en charge AES-NI et jeux d’instructions PCLMULQDQ UC. Ils sont disponibles sur n’importe quel Westmere (32nm) et les versions ultérieures de processeur Intel, sauf sur les modèles où AES-NI a été désactivé. Vous pouvez examiner la documentation de votre fournisseur de matériel pour voir si le processeur prend en charge AES-NI et du processeur PCLMULQDQ instruction définit.

Voici un exemple REST de connexion IPsec avec les algorithmes de sécurité optimale :

```PowerShell
# NOTE: The virtual gateway must be created before creating the IPsec connection. More details here.
# Create a new object for Tenant Network IPsec Connection  
$nwConnectionProperties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties   

# Update the common object properties  
$nwConnectionProperties.ConnectionType = "IPSec"   
$nwConnectionProperties.OutboundKiloBitsPerSecond = 2000000   
$nwConnectionProperties.InboundKiloBitsPerSecond = 2000000  

# Update specific properties depending on the Connection Type  
$nwConnectionProperties.IpSecConfiguration = New-Object Microsoft.Windows.NetworkController.IpSecConfiguration   
$nwConnectionProperties.IpSecConfiguration.AuthenticationMethod = "PSK"   
$nwConnectionProperties.IpSecConfiguration.SharedSecret = "111_aaa"   

$nwConnectionProperties.IpSecConfiguration.QuickMode = New-Object Microsoft.Windows.NetworkController.QuickMode   
$nwConnectionProperties.IpSecConfiguration.QuickMode.PerfectForwardSecrecy = "PFS2048"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.AuthenticationTransformationConstant = "GCMAES256"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.CipherTransformationConstant = "GCMAES256"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.SALifeTimeSeconds = 3600   
$nwConnectionProperties.IpSecConfiguration.QuickMode.IdleDisconnectSeconds = 500   
$nwConnectionProperties.IpSecConfiguration.QuickMode.SALifeTimeKiloBytes = 2000   

$nwConnectionProperties.IpSecConfiguration.MainMode = New-Object Microsoft.Windows.NetworkController.MainMode   
$nwConnectionProperties.IpSecConfiguration.MainMode.DiffieHellmanGroup = "Group2"   
$nwConnectionProperties.IpSecConfiguration.MainMode.IntegrityAlgorithm = "SHA256"   
$nwConnectionProperties.IpSecConfiguration.MainMode.EncryptionAlgorithm = "AES256"   
$nwConnectionProperties.IpSecConfiguration.MainMode.SALifeTimeSeconds = 28800
$nwConnectionProperties.IpSecConfiguration.MainMode.SALifeTimeKiloBytes = 2000   

# L3 specific configuration (leave blank for IPSec)  
$nwConnectionProperties.IPAddresses = @()   
$nwConnectionProperties.PeerIPAddresses = @()   

# Update the IPv4 Routes that are reachable over the site-to-site VPN Tunnel  
$nwConnectionProperties.Routes = @()   
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo   
$ipv4Route.DestinationPrefix = "<<On premise subnet that must be reachable over the VPN tunnel. Ex: 10.0.0.0/24>>"   
$ipv4Route.metric = 10   
$nwConnectionProperties.Routes += $ipv4Route   

# Tunnel Destination (Remote Endpoint) Address  
$nwConnectionProperties.DestinationIPAddress = "<<Public IP address of the On-Premise VPN gateway. Ex: 192.168.3.4>>"   

# Add the new Network Connection for the tenant. Note that the virtual gateway must be created before creating the IPsec connection. $uri is the REST URI of your deployment and must be in the form of “https://<REST URI>”  
New-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -ResourceId "Contoso_IPSecGW" -Properties $nwConnectionProperties -Force
```

## <a name="testing-results"></a>Résultats des tests

Nous avons effectué des tests pour les passerelles SDN dans nos laboratoires de test de performances étendues. Dans les tests, nous avons comparé les performances réseau de la passerelle avec Windows Server 2019 dans les scénarios SDN et scénarios non SDN. Vous pouvez rechercher les résultats et tester les détails de la configuration capturées dans l’article de blog [ici](https://blogs.technet.microsoft.com/networking/2018/08/15/high-performance-gateways/).

---