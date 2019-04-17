---
title: Connecter des conteneurs à un réseau virtuel
description: Cette rubrique fait partie du guide logiciel défini de mise en réseau sur la façon de gérer les charges de travail clientes et des réseaux virtuels dans Windows Server2016.
manager: ravirao
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f7af1eb6-d035-4f74-a25b-d4b7e4ea9329
ms.author: pashort
author: jmesser81
ms.openlocfilehash: 801cf4b8f71935eb72d820d47e523a310fa64562
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="connect-container-endpoints-to-a-tenant-virtual-network"></a>Connecter les points de terminaison de conteneur à un réseau virtuel locataire

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique vous montre comment connecter les points de terminaison de conteneur à un réseau virtuel locataire créé par le biais de la pile Microsoft logiciel défini de mise en réseau (SDN). Nous allons utiliser le *l2bridge* (et éventuellement *l2tunnel*) pilote réseau disponible avec le plug-in libnetwork de Windows pour créer un réseau de conteneur sur l’ordinateur virtuel de l’hôte (client) conteneur Docker.

Comme indiqué dans le [mise en réseau de conteneur](https://msdn.microsoft.com/en-us/virtualization/windowscontainers/management/container_networking) rubrique sur MSDN, plusieurs pilotes réseau sont disponibles par le biais de Docker sur Windows. Le plus approprié pour SDN les pilotes est *l2bridge* et *l2tunnel *. Pour les deux pilotes, chaque point de terminaison de conteneur est dans le même sous-réseau virtuel en tant que l’ordinateur virtuel de l’hôte (client) conteneur. Les adresses IP des points de terminaison de conteneur sont affectés dynamiquement par hôte mise en réseau Service (SNPD) via le plug-in de cloud privé. Les points de terminaison de conteneur ont des adresses IP uniques mais partagent la même adresse MAC de l’ordinateur de virtuel conteneur hôte (client) en raison de la traduction d’adresse de couche 2. Stratégie réseau (par exemple: ACL, l’encapsulation et QoS) de ces points de terminaison de conteneur sont appliquées dans l’hôte physique Hyper-V, comme reçu par le contrôleur de réseau et définies dans les systèmes de gestion de la couche supérieure. Il existe une légère différence entre la *l2bridge* et *l2tunnel* pilotes qui est expliquée ci-dessous.

- **Pont L2** -points de terminaison de conteneur qui se trouvent sur l’ordinateur même conteneur hôte virtuel et qui sont dans le même sous-réseau ont tout le trafic de réseau reliées par un pont au sein du commutateur virtuel Hyper-V. Points de terminaison de conteneur qui résident sur un autre hôte de conteneur ordinateurs virtuels ou qui se trouvent dans des sous-réseaux différents ont leur trafic transmis à l’hôte Hyper-V physique. Dans la mesure où le trafic réseau entre les conteneurs sur le même hôte et dans le même sous-réseau ne passent pas à l’hôte physique, aucune stratégie réseau n’est appliquée. Stratégie est appliquée uniquement pour le trafic réseau de conteneur entre les hôtes ou entre sous-réseaux.  
 
- **Tunnel L2** - *tous les* le trafic réseau entre deux points de terminaison de conteneur est transmis à l’hôte Hyper-V physique quelle que soit l’ordinateur hôte ou le sous-réseau. Stratégie de réseau est appliquée pour le trafic réseau entre sous-réseaux et entre les hôtes.   

>[!NOTE]
>Ces modes de mise en réseau ne fonctionnent pas pour les points de terminaison de conteneur windows qui se connecte à un réseau virtuel locataire dans Azure cloud public

## <a name="prerequistes"></a>Conditions préalables
 * Une infrastructure SDN avec le contrôleur de réseau a été déployée.
 * Un réseau virtuel locataire a été créé.
 * Un ordinateur virtuel client a été déployé avec la fonctionnalité de conteneur Windows est activé, Docker installé et fonctionnalité de Hyper-V activé

>[!Note]
>[La virtualisation imbriquée](https://msdn.microsoft.com/en-us/virtualization/hyperv_on_windows/user_guide/nesting) et exposer les extensions de virtualisation n’est pas requise sauf si à l’aide de la fonctionnalité de l’Hyper-v de conteneurs de Hyper-V est requis pour installer plusieurs fichiers binaires pour les réseaux l2bridge et l2tunnel

```powershell
# To install HyperV feature without checks for nested virtualization
dism /Online /Enable-Feature /FeatureName:Microsoft-Hyper-V /All 
```

 

## <a name="workflow"></a>Flux de travail

1. [Ajouter plusieurs configurations IP à une ressource VM NIC existante par le biais de contrôleur de réseau](#Add) (hôte Hyper-V)
2. [Activer le proxy réseau sur l’ordinateur hôte à allouer des adresses IP des points de terminaison de conteneur](#Enable) (hôte Hyper-V) 
3. [Installez le plug-in pour affecter des adresses IP de l’autorité de certification aux points de terminaison de conteneur de cloud privé](#Install) (machine virtuelle d’hôte de conteneur) 
4. [Créer un *l2bridge* ou *l2tunnel* réseau à l’aide de docker](#Create) (machine virtuelle d’hôte de conteneur) 
 
>[!NOTE]
>Plusieurs configurations IP n’est pas pris en charge sur les ressources de VM NIC créées par le biais de SystemCenter Virtual Machine Manager. Il est recommandé pour ces types de déploiements que vous créez la ressource VM NIC hors bande à l’aide de PowerShell du contrôleur de réseau.

### <a name="Add"></a>1. Ajoutez plusieurs Configurations IP

Pour cet exemple, nous partons du principe que la NIC VM de l’ordinateur virtuel client dispose d’une configuration d’IP avec l’adresse IP de 192.168.1.9 déjà et est attachée à un ID de ressource de réseau virtuel de «VNet1» et les ressources de sous-réseau de machine virtuelle de sous-réseau «1» dans le sous-réseau IP 192.168.1.0/24. Nous allons ajouter 10adresses IP pour les conteneurs de 192.168.1.101 - 192.168.1.110.

```powershell
Import-Module NetworkController

# Specify Network Controller REST IP or FQDN
$uri = "<NC REST IP or FQDN>"
$vnetResourceId = "VNet1"
$vsubnetResourceId = "Subnet1"

$vmnic= Get-NetworkControllerNetworkInterface -ConnectionUri $uri | where {$_.properties.IpConfigurations.Properties.PrivateIPAddress -eq "192.168.1.9" }
$vmsubnet = Get-NetworkControllerVirtualSubnet -VirtualNetworkId $vnetResourceId -ResourceId $vsubnetResourceId -ConnectionUri $uri

# For this demo, we will assume an ACL has already been defined; any ACL can be applied here
$allowallacl = Get-NetworkControllerAccessControlList -ConnectionUri $uri -ResourceId "AllowAll"


foreach ($i in 1..10)
{
    $newipconfig = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfiguration
    $props = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfigurationProperties

    $resourceid = "IP_192_168_1_1"
    if ($i -eq 10) 
    {
        $resourceid += "10"
        $ipstr = "192.168.1.110"
    }
    else
    {
        $resourceid += "0$i"
        $ipstr = "192.168.1.10$i"
    }
    
    $newipconfig.ResourceId = $resourceid
    $props.PrivateIPAddress = $ipstr    
    
    $props.PrivateIPAllocationMethod = "Static"
    $props.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $props.Subnet.ResourceRef = $vmsubnet.ResourceRef
    $props.AccessControlList = new-object Microsoft.Windows.NetworkController.AccessControlList
    $props.AccessControlList.ResourceRef = $allowallacl.ResourceRef

    $newipconfig.Properties = $props
    $vmnic.Properties.IpConfigurations += $newipconfig
}

New-NetworkControllerNetworkInterface -ResourceId $vmnic.ResourceId -Properties $vmnic.Properties -ConnectionUri $uri
```

### <a name="Enable"></a>2. activer le Proxy réseau

[ConfigureMCNP.ps1](https://github.com/Microsoft/SDN/blob/master/Containers/ConfigureMCNP.ps1>)

Exécutez ce script le **hôte Hyper-V** qui héberge l’ordinateur de virtuel conteneur hôte (client) pour activer le proxy réseau à allouer plusieurs adresses IP pour l’ordinateur virtuel de hôte conteneur.

```powershell
PS C:\> ConfigureMCNP.ps1
```

### <a name="Install"></a>3. Installez Cloud privé plug-in

[InstallPrivateCloudPlugin.ps1](https://github.com/Microsoft/SDN/blob/master/Containers/InstallPrivateCloudPlugin.ps1)

Exécutez ce script à l’intérieur de la **conteneur hôte (client) virtuels** pour permettre à l’hôte de mise en réseau Service (SNPD) pour communiquer avec le proxy réseau sur l’ordinateur hôte Hyper-V.

```powershell
PS C:\> InstallPrivateCloudPlugin.ps1
```

### <a name="Create"></a>4. créer un *l2bridge* réseau de conteneur

Sur le **conteneur hôte (client) virtuels** utiliser le `docker network create`commande pour créer un réseau l2bridge

```powershell
# Create the container network
C:\> docker network create -d l2bridge --subnet="192.168.1.0/24" --gateway="192.168.1.1" MyContainerOverlayNetwork

# Attach a container to the MyContainerOverlayNetwork 
C:\> docker run -it --network=MyContainerOverlayNetwork <image> <cmd>
```

>[!NOTE]
>Affectation IP statique n’est pas pris en charge avec *l2bridge* ou *l2tunnel* conteneur réseaux lorsqu’il est utilisé avec la pile SDN de Microsoft.

## <a name="more-information"></a>Plus d’informations
Pour plus d’infortation sur le déploiement d’une infrastructure SDN, voir [déployer une Infrastructure de réseau défini par logiciel](https://technet.microsoft.com/en-us/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure).

