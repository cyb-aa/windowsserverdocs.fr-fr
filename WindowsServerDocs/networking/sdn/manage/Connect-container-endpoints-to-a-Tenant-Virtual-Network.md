---
title: Connecter les points de terminaison de conteneur à un réseau virtuel locataire
description: Dans cette rubrique, nous vous montrons comment connecter des points de terminaison de conteneur à un réseau virtuel client existant créé par le biais de SDN. Vous utilisez le pilote réseau l2bridge (et éventuellement l2tunnel) disponible avec le plug-in Windows libnetwork pour l’ordinateur de veille pour créer un réseau de conteneurs sur la machine virtuelle du locataire.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: f7af1eb6-d035-4f74-a25b-d4b7e4ea9329
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/24/2018
ms.openlocfilehash: 2b8927ec260b4f5a42aa59a25db1b18896ce91ef
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854532"
---
# <a name="connect-container-endpoints-to-a-tenant-virtual-network"></a>Connecter les points de terminaison de conteneur à un réseau virtuel locataire

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette rubrique, nous vous montrons comment connecter des points de terminaison de conteneur à un réseau virtuel client existant créé par le biais de SDN. Vous utilisez le pilote réseau *l2bridge* (et éventuellement *l2tunnel*) disponible avec le plug-in Windows libnetwork pour l’ordinateur de veille pour créer un réseau de conteneurs sur la machine virtuelle du locataire.

Dans la rubrique relative aux [pilotes réseau de conteneur](https://docs.microsoft.com/virtualization/windowscontainers/container-networking/network-drivers-topologies) , nous avons abordé les différents pilotes réseau disponibles par le biais de l’arrimeur sur Windows. Pour SDN, utilisez les pilotes *l2bridge* et *l2tunnel* . Pour les deux pilotes, chaque point de terminaison de conteneur se trouve dans le même sous-réseau virtuel que l’ordinateur virtuel de l’hôte de conteneur (locataire). 

Le service de mise en réseau hôte (HNS), via le plug-in de cloud privé, attribue dynamiquement les adresses IP pour les points de terminaison de conteneur. Les points de terminaison de conteneur ont des adresses IP uniques, mais partagent la même adresse MAC de l’ordinateur virtuel de l’hôte de conteneur (locataire) en raison de la traduction d’adresses de couche 2. 

La stratégie réseau (ACL, encapsulation et QoS) de ces points de terminaison de conteneur est appliquée à l’hôte Hyper-V physique comme reçu par le contrôleur de réseau et défini dans les systèmes de gestion de couche supérieure. 

La différence entre les pilotes *l2bridge* et *l2tunnel* est la suivante :


|                                                                                                                                                                                                                                                                            l2bridge                                                                                                                                                                                                                                                                            |                                                                                                 l2tunnel                                                                                                  |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Points de terminaison de conteneur résidant sur : <ul><li>La même machine virtuelle d’hôte de conteneur et sur le même sous-réseau ont tout le trafic réseau relié dans le commutateur virtuel Hyper-V. </li><li>Le trafic des machines virtuelles hôtes de conteneur ou sur des sous-réseaux différents est transféré à l’hôte Hyper-V physique. </li></ul>La stratégie réseau n’est pas appliquée car le trafic réseau entre les conteneurs sur le même ordinateur hôte et dans le même sous-réseau n’est pas transmis à l’hôte physique. La stratégie réseau s’applique uniquement au trafic réseau de conteneurs inter-hôtes ou entre sous-réseaux. | *Tout* le trafic réseau entre deux points de terminaison de conteneur est transféré à l’hôte Hyper-V physique, quel que soit l’hôte ou le sous-réseau. La stratégie réseau s’applique à la fois au trafic réseau entre les sous-réseaux et entre les hôtes. |

---

>[!NOTE]
>Ces modes de mise en réseau ne fonctionnent pas pour connecter des points de terminaison de conteneur Windows à un réseau virtuel de locataire dans le cloud public Azure.


## <a name="prerequisites"></a>Composants requis
-  Une infrastructure SDN déployée avec le contrôleur de réseau.
-  Un réseau virtuel client a été créé.
-  Une machine virtuelle cliente déployée avec la fonctionnalité de conteneur Windows activée, Dockr installé et la fonctionnalité Hyper-V activée. La fonctionnalité Hyper-V est requise pour installer plusieurs fichiers binaires pour les réseaux l2bridge et l2tunnel.

   ```powershell
   # To install HyperV feature without checks for nested virtualization
   dism /Online /Enable-Feature /FeatureName:Microsoft-Hyper-V /All 
   ```

>[!Note]
>[La virtualisation imbriquée](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/nesting) et l’exposition d’extensions de virtualisation ne sont pas nécessaires, sauf en cas d’utilisation de conteneurs Hyper-V. 


## <a name="workflow"></a>Flux de travail

[1. ajoutez plusieurs configurations IP à une ressource de carte réseau de machine virtuelle existante via le contrôleur de réseau (hôte Hyper-V)](#1-add-multiple-ip-configurations)
[2. Activez le proxy réseau sur l’ordinateur hôte pour allouer des adresses IP d’autorité de certification pour les points de terminaison de conteneur (hôte Hyper-V)](#2-enable-the-network-proxy)
[3. Installez le plug-in de cloud privé pour affecter des adresses IP d’autorité de certification aux points de terminaison de conteneur (machine virtuelle hôte de conteneur)](#3-install-the-private-cloud-plug-in)
[4. Créer un réseau *l2bridge* ou *l2tunnel* à l’aide de dockr (ordinateur hôte de conteneur)](#4-create-an-l2bridge-container-network)

>[!NOTE]
>Plusieurs configurations IP ne sont pas prises en charge sur les ressources de carte réseau de machine virtuelle créées via System Center Virtual Machine Manager. Pour ces types de déploiement, il est recommandé de créer la ressource de carte réseau de machine virtuelle hors bande à l’aide de PowerShell de contrôleur de réseau.

### <a name="1-add-multiple-ip-configurations"></a>1. ajouter plusieurs configurations IP
Dans cette étape, nous supposons que la carte réseau de machine virtuelle de la machine virtuelle cliente a une configuration IP avec l’adresse IP 192.168.1.9 et qu’elle est associée à l’ID de ressource de réseau virtuel « VNet1 » et à la ressource de sous-réseau de machine virtuelle « Subnet1 » dans le sous-réseau IP 192.168.1.0/24. Nous ajoutons 10 adresses IP pour les conteneurs à partir de 192.168.1.101-192.168.1.110.

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

### <a name="2-enable-the-network-proxy"></a>2. activer le proxy réseau
Au cours de cette étape, vous allez autoriser le proxy réseau à allouer plusieurs adresses IP pour la machine virtuelle de l’hôte de conteneur. 

Pour activer le proxy réseau, exécutez le script [ConfigureMCNP. ps1](https://github.com/Microsoft/SDN/blob/master/Containers/ConfigureMCNP.ps1) sur l' **hôte Hyper-V** hébergeant l’ordinateur virtuel de l’hôte de conteneur (locataire).

```powershell
PS C:\> ConfigureMCNP.ps1
```

### <a name="3-install-the-private-cloud-plug-in"></a>3. installer le plug-in de cloud privé
Dans cette étape, vous installez un plug-in pour permettre à HNS de communiquer avec le proxy réseau sur l’hôte Hyper-V.

Pour installer le plug-in, exécutez le script [InstallPrivateCloudPlugin. ps1](https://github.com/Microsoft/SDN/blob/master/Containers/InstallPrivateCloudPlugin.ps1) à l’intérieur de la **machine virtuelle de l’hôte de conteneur (locataire)** .


```powershell
PS C:\> InstallPrivateCloudPlugin.ps1
```

### <a name="4-create-an-l2bridge-container-network"></a>4. créer un réseau de conteneurs *l2bridge*
Dans cette étape, vous utilisez la commande `docker network create` sur la **machine virtuelle de l’hôte de conteneur (locataire)** pour créer un réseau l2bridge. 

```powershell
# Create the container network
C:\> docker network create -d l2bridge --subnet="192.168.1.0/24" --gateway="192.168.1.1" MyContainerOverlayNetwork

# Attach a container to the MyContainerOverlayNetwork 
C:\> docker run -it --network=MyContainerOverlayNetwork <image> <cmd>
```

>[!NOTE]
>L’attribution d’adresses IP statiques n’est pas prise en charge avec les réseaux de conteneurs *l2bridge* ou *l2tunnel* lorsqu’elle est utilisée avec la pile Microsoft Sdn.

## <a name="more-information"></a>Informations supplémentaires
Pour plus d’informations sur le déploiement d’une infrastructure SDN, consultez [déployer une infrastructure réseau à définition logicielle](https://docs.microsoft.com/windows-server/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure).

