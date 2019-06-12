---
title: Connecter les points de terminaison de conteneur à un réseau virtuel locataire
description: Dans cette rubrique, nous vous montrons comment se connecter les points de terminaison de conteneur à un réseau virtuel locataire existant créé par le biais SDN. Vous utilisez la l2bridge (et éventuellement l2tunnel) pilote réseau disponible avec le plug-in de libnetwork Windows pour Docker créer un réseau de conteneurs sur la machine virtuelle locataire.
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
ms.date: 08/24/2018
ms.openlocfilehash: cb9c7157ffb07233e41e1c933f6775f1cd0766a9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446349"
---
# <a name="connect-container-endpoints-to-a-tenant-virtual-network"></a>Connecter les points de terminaison de conteneur à un réseau virtuel locataire

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette rubrique, nous vous montrons comment se connecter les points de terminaison de conteneur à un réseau virtuel locataire existant créé par le biais SDN. Vous utilisez le *l2bridge* (et éventuellement *l2tunnel*) pilote réseau disponible avec le plug-in de libnetwork Windows pour Docker créer un réseau de conteneurs sur la machine virtuelle locataire.

Dans le [pilotes réseau de conteneur](https://docs.microsoft.com/virtualization/windowscontainers/container-networking/network-drivers-topologies) rubrique, nous avons abordé les pilotes réseau plusieurs sont disponibles via Docker sur Windows. Pour les SDN, utilisez le *l2bridge* et *l2tunnel* pilotes. Pour les deux pilotes, chaque point de terminaison de conteneur est dans le même sous-réseau virtuel que la machine de virtuelle conteneur hôte (client). 

L’hôte de mise en réseau Service HNS (), via le plug-in de cloud privé, attribue dynamiquement les adresses IP des points de terminaison de conteneur. Les points de terminaison de conteneur ont des adresses IP uniques, mais partagent la même adresse MAC de la machine de virtuelle conteneur hôte (client) en raison de la traduction d’adresses de couche 2. 

Stratégie réseau (ACL, l’encapsulation et QoS) pour ces points de terminaison de conteneur sont appliquées dans l’hôte Hyper-V physique comme reçu par le contrôleur de réseau et définis dans les systèmes de gestion de la couche supérieure. 

La différence entre la *l2bridge* et *l2tunnel* pilotes sont :


|                                                                                                                                                                                                                                                                            l2bridge                                                                                                                                                                                                                                                                            |                                                                                                 l2tunnel                                                                                                  |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Points de terminaison de conteneur qui résident sur : <ul><li>Le même conteneur hôte de machine virtuelle et sur le même sous-réseau a tout le trafic réseau ponté au sein du commutateur virtuel Hyper-V. </li><li>Autre conteneur héberger les machines virtuelles ou sur des sous-réseaux différents leur trafic transmis à l’hôte Hyper-V physique. </li></ul>Stratégie de réseau ne pas obtenir appliquée dans la mesure où le trafic réseau entre les conteneurs sur le même hôte et dans le même sous-réseau ne sont pas acheminées à l’hôte physique. Stratégie de réseau s’applique le trafic réseau de conteneur uniquement entre les hôtes ou entre sous-réseaux. | *Tous les* le trafic réseau entre deux points de terminaison de conteneur est transmis à l’hôte Hyper-V physique, quel que soit l’hôte ou au sous-réseau. Stratégie de réseau s’applique au trafic de réseau entre sous-réseaux et entre les hôtes. |

---

>[!NOTE]
>Ces modes de mise en réseau ne fonctionnent pas pour les points de terminaison de conteneur windows qui se connecte à un réseau virtuel locataire dans le cloud public Azure.


## <a name="prerequisites"></a>Prérequis
-  Une infrastructure SDN déployée avec le contrôleur de réseau.
-  Un réseau virtuel locataire a été créé.
-  Une machine virtuelle de locataire déployé avec la fonctionnalité de conteneur de Windows est activé, Docker est installé et la fonctionnalité Hyper-V est activée. La fonctionnalité Hyper-V est nécessaire pour installer plusieurs fichiers binaires pour les réseaux l2bridge et l2tunnel.

   ```powershell
   # To install HyperV feature without checks for nested virtualization
   dism /Online /Enable-Feature /FeatureName:Microsoft-Hyper-V /All 
   ```

>[!Note]
>[Virtualisation imbriquée](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/nesting) et exposer les extensions de virtualisation n’est nécessaire, sauf si l’utilisation de conteneurs de Hyper-V. 


## <a name="workflow"></a>Flux de travail

[1. Ajouter plusieurs configurations IP à une ressource VM NIC existante via le contrôleur de réseau (hôte Hyper-V)](#1-add-multiple-ip-configurations)
[2. Activer le proxy réseau sur l’ordinateur hôte à allouer des adresses IP autorité de certification pour les points de terminaison de conteneur (hôte Hyper-V)](#2-enable-the-network-proxy)
[3. Installer le plug-in pour affecter des adresses IP de l’autorité de certification aux points de terminaison de conteneur (conteneur hôte VM) de cloud privé](#3-install-the-private-cloud-plug-in)
[4. Créer un *l2bridge* ou *l2tunnel* réseau à l’aide de docker (conteneur hôte VM)](#4-create-an-l2bridge-container-network)

>[!NOTE]
>Plusieurs configurations IP n’est pas pris en charge sur les ressources de VM NIC créées par le biais de System Center Virtual Machine Manager. Il est recommandé pour ces types de déploiements que vous créez la ressource VM NIC hors bande à l’aide de PowerShell du contrôleur de réseau.

### <a name="1-add-multiple-ip-configurations"></a>1. Ajouter plusieurs Configurations IP
Dans cette étape, nous partons du principe la VM NIC de la machine virtuelle locataire a une configuration d’adresse IP avec l’adresse IP de 192.168.1.9 et associée à un ID de ressource de réseau virtuel de « VNet1 » et les ressources de sous-réseau de machine virtuelle de « Subnet1 » dans le sous-réseau IP 192.168.1.0/24. Nous ajoutons 10 adresses IP pour les conteneurs à partir de 192.168.1.101 - 192.168.1.110.

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

### <a name="2-enable-the-network-proxy"></a>2. Activer le proxy réseau
Dans cette étape, vous allez autoriser le proxy réseau à allouer des adresses IP pour la machine virtuelle hôte de conteneur. 

Pour activer le proxy réseau, exécutez le [ConfigureMCNP.ps1](https://github.com/Microsoft/SDN/blob/master/Containers/ConfigureMCNP.ps1) de script sur le **hôte Hyper-V** héberge la machine de virtuelle conteneur hôte (client).

```powershell
PS C:\> ConfigureMCNP.ps1
```

### <a name="3-install-the-private-cloud-plug-in"></a>3. Installer le plug-in de Cloud privé
Dans cette étape, vous installez un plug-in pour autoriser le HNS communiquer avec le proxy réseau sur l’ordinateur hôte Hyper-V.

Pour installer le plug-in, exécutez le [InstallPrivateCloudPlugin.ps1](https://github.com/Microsoft/SDN/blob/master/Containers/InstallPrivateCloudPlugin.ps1) de script à l’intérieur de la **machine virtuelle de conteneur hôte (locataire)** .


```powershell
PS C:\> InstallPrivateCloudPlugin.ps1
```

### <a name="4-create-an-l2bridge-container-network"></a>4. Créer un *l2bridge* réseau de conteneurs
Dans cette étape, vous utilisez le `docker network create` commande sur le **machine virtuelle de conteneur hôte (locataire)** pour créer un réseau l2bridge. 

```powershell
# Create the container network
C:\> docker network create -d l2bridge --subnet="192.168.1.0/24" --gateway="192.168.1.1" MyContainerOverlayNetwork

# Attach a container to the MyContainerOverlayNetwork 
C:\> docker run -it --network=MyContainerOverlayNetwork <image> <cmd>
```

>[!NOTE]
>Attribution IP statique n’est pas pris en charge avec *l2bridge* ou *l2tunnel* conteneur réseaux lorsqu’il est utilisé avec la pile SDN de Microsoft.

## <a name="more-information"></a>Informations supplémentaires
Pour plus d’informations sur le déploiement d’une infrastructure SDN, consultez [déployer une Infrastructure de réseau défini par logiciel](https://docs.microsoft.com/windows-server/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure).

