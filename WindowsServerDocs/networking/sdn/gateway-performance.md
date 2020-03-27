---
title: Performances de la passerelle Windows Server 2019
description: ''
manager: dougkim
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: lizross
author: eross-msft
ms.date: 08/22/2018
ms.openlocfilehash: e8cdf4e100c65fae11637c681924746115444f71
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313025"
---
# <a name="windows-server-2019-gateway-performance"></a>Performances de la passerelle Windows Server 2019

>S’applique à : Windows Server


Dans Windows Server 2016, l’un des problèmes des clients était l’incapacité de la passerelle SDN à répondre aux exigences de débit des réseaux modernes. Le débit réseau des tunnels IPsec et GRE était limité avec le débit de connexion unique pour que la connectivité IPsec soit d’environ 300 Mbits/s et que la connectivité GRE soit d’environ 2,5 Gbits/s.

Nous avons amélioré de manière significative Windows Server 2019, avec les nombres qui dépassent 1,8 Gbit/s et 15 Gbit/s pour les connexions IPsec et GRE, respectivement. Tout cela, avec des réductions significatives dans les cycles d’UC/par octet, fournissant ainsi un débit ultra-hautes performances avec une utilisation beaucoup moins importante du processeur.

## <a name="enable-high-performance-with-gateways-in-windows-server-2019"></a>Activer les performances élevées avec les passerelles dans Windows Server 2019

Pour les **connexions GRE**, une fois le déploiement/la mise à niveau vers Windows Server 2019 généré sur les machines virtuelles de passerelle, vous devez automatiquement voir les performances améliorées. Aucune étape manuelle n’est impliquée.

Pour les **connexions IPsec**, par défaut, lorsque vous créez la connexion pour vos réseaux virtuels, vous recevez le chemin d’accès aux données et les numéros de performances de Windows Server 2016. Pour activer le chemin d’accès aux données Windows Server 2019, procédez comme suit :

   1. Sur une machine virtuelle de passerelle SDN, accédez à la console **services** (services. msc).
   2. Recherchez le service nommé **service de passerelle Azure**et définissez le type de démarrage sur **automatique**.
   3. Redémarrez la machine virtuelle de la passerelle.
      Les connexions actives sur cette passerelle basculent vers une machine virtuelle de passerelle redondante.
   4. Répétez les étapes précédentes pour les autres machines virtuelles de passerelle.

>[!TIP]
>Pour obtenir de meilleures performances, assurez-vous que les paramètres cipherTransformationConstant et authenticationTransformConstant dans les paramètres quickMode de la connexion IPsec utilisent la suite de chiffrement **GCMAES256** .
>
>Pour des performances optimales, le matériel hôte de la passerelle doit prendre en charge les jeux d’instructions d’UC AES-NI et PCLMULQDQ. Celles-ci sont disponibles sur n’importe quel processeur Intel Westmere (32nm) et ultérieur, sauf sur les modèles où AES-NI a été désactivé. Vous pouvez consulter la documentation de votre fournisseur de matériel pour déterminer si le processeur prend en charge les jeux d’instructions de processeur AES-NI et PCLMULQDQ.

Voici un exemple REST de connexion IPsec avec des algorithmes de sécurité optimaux :

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

Nous avons effectué des tests de performances considérables pour les passerelles SDN dans nos laboratoires de test. Dans les tests, nous avons comparé les performances du réseau de la passerelle avec Windows Server 2019 dans les scénarios SDN et les scénarios non SDN. Vous trouverez les résultats et les détails de la configuration de test capturés dans l’article de blog [ici](https://blogs.technet.microsoft.com/networking/2018/08/15/high-performance-gateways/).

---