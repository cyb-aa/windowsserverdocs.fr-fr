---
title: Cluster vers réplica de stockage de cluster dans la même région dans Azure
description: Réplication de stockage de cluster à cluster dans la même région dans Azure
keywords: Réplica de stockage, Gestionnaire de serveur, Windows Server, Azure, cluster, la même région
author: arduppal
ms.author: arduppal
ms.date: 04/26/2019
ms.topic: article
ms.prod: windows-server
ms.technology: storage-replica
manager: mchad
ms.openlocfilehash: 55d9c600c86b6b64efdb5c7d4437697539f887ae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402946"
---
# <a name="cluster-to-cluster-storage-replica-within-the-same-region-in-azure"></a>Cluster vers réplica de stockage de cluster dans la même région dans Azure

> S’applique à : Windows Server 2019, Windows Server 2016, Windows Server (Canal semi-annuel)

Vous pouvez configurer un cluster pour la réplication de stockage dans la même région dans Azure. Dans les exemples ci-dessous, nous utilisons un cluster à deux nœuds, mais le réplica de stockage de cluster à cluster n’est pas limité à un cluster à deux nœuds. L’illustration ci-dessous est un cluster d’espace de stockage direct à deux nœuds qui peut communiquer entre eux, se trouvent dans le même domaine et dans la même région.

Regardez les vidéos ci-dessous pour une procédure pas à pas complète du processus.

Première partie
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE26f2Y]

Deuxième partie
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE269Pq]

![Diagramme d’architecture présentant le réplica de stockage cluster à cluster dans Azure dans la même région.](media/Cluster-to-cluster-azure-one-region/architecture.png)
> [!IMPORTANT]
> Tous les exemples référencés sont spécifiques à l’illustration ci-dessus.

1. Créez un [groupe de ressources](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup) dans la portail Azure d’une région (**SR-AZ2AZ** dans l' **ouest des États-Unis 2**). 
2. Créez deux [groupes à haute disponibilité](https://ms.portal.azure.com/#create/Microsoft.AvailabilitySet-ARM) dans le groupe de ressources (**SR-AZ2AZ**) créé ci-dessus, un pour chaque cluster. 
    a. Groupe à haute disponibilité (**az2azAS1**) b. Groupe à haute disponibilité (**az2azAS2**)
3. Créez un [réseau virtuel](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM) (**az2az-vnet**) dans le groupe de ressources créé précédemment (**SR-az2az**), avec au moins un sous-réseau. 
4. Créez un [groupe de sécurité réseau](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM) (**az2az-NSG**) et ajoutez une règle de sécurité de trafic entrant pour RDP : 3389. Vous pouvez choisir de supprimer cette règle une fois que vous avez terminé l’installation. 
5. Créez des [machines virtuelles](https://ms.portal.azure.com/#create/Microsoft.WindowsServer2016Datacenter-ARM) Windows Server dans le groupe de ressources créé précédemment (**SR-AZ2AZ**). Utilisez le réseau virtuel précédemment créé (**az2az-vnet**) et le groupe de sécurité réseau (**az2az-NSG**). 
   
   Contrôleur de domaine (**az2azDC**). Vous pouvez choisir de créer un troisième groupe à haute disponibilité pour votre contrôleur de domaine ou d’ajouter le contrôleur de domaine dans l’un des deux groupes à haute disponibilité. Si vous l’ajoutez au groupe à haute disponibilité créé pour les deux clusters, attribuez-lui une adresse IP publique standard lors de la création de la machine virtuelle. 
   - Installez domaine Active Directory Service.
   - Créer un domaine (Contoso.com)
   - Créer un utilisateur avec des privilèges d’administrateur (contosoadmin) 
   - Créez deux machines virtuelles (**az2az1**, **az2az2**) dans le premier groupe à haute disponibilité (**az2azAS1**). Affectez une adresse IP publique standard à chaque ordinateur virtuel pendant la création.
   - Ajouter au moins 2 disques managés à chaque ordinateur
   - Installer la fonctionnalité de clustering de basculement et de réplica de stockage
   - Créez deux machines virtuelles (**az2az3**, **az2az4**) dans le deuxième groupe à haute disponibilité (**az2azAS2**). Affectez une adresse IP publique standard à chaque ordinateur virtuel lors de la création. 
   - Ajoutez au moins 2 disques managés à chaque ordinateur. 
   - Installez la fonctionnalité de clustering de basculement et de réplica de stockage. 
   
6. Connectez tous les nœuds au domaine et fournissez des privilèges d’administrateur à l’utilisateur créé précédemment. 

7. Remplacez le serveur DNS du réseau virtuel par l’adresse IP privée du contrôleur de domaine. 
8. Dans notre exemple, le contrôleur de domaine **az2azDC** a une adresse IP privée (10.3.0.8). Dans le réseau virtuel (**az2az-vnet**), modifiez le serveur DNS 10.3.0.8. Connectez tous les nœuds à « Contoso.com » et fournissez des privilèges d’administrateur à « contosoadmin ».
   - Connectez-vous en tant que contosoadmin à partir de tous les nœuds. 
    
9. Créez les clusters (**SRAZC1**, **SRAZC2**). 
   Voici les commandes PowerShell pour notre exemple
   ```PowerShell
    New-Cluster -Name SRAZC1 -Node az2az1,az2az2 –StaticAddress 10.3.0.100
   ```
   ```PowerShell
    New-Cluster -Name SRAZC2 -Node az2az3,az2az4 –StaticAddress 10.3.0.101
   ```
10. Activer les espaces de stockage direct
    ```PowerShell
    Enable-clusterS2D
    ```   
   
    Pour chaque cluster, créez un disque virtuel et un volume. Une pour les données et une autre pour le journal. 
   
11. Créez un [load balancer](https://ms.portal.azure.com/#create/Microsoft.LoadBalancer-ARM) de référence SKU standard interne pour chaque cluster (**azlbr1**,**azlbr2**). 
   
    Fournissez l’adresse IP de cluster comme adresse IP privée statique pour l’équilibrage de charge.
    - azlbr1 = > adresse IP frontale : 10.3.0.100 (récupération d’une adresse IP inutilisée à partir du sous-réseau de réseau virtuel (**az2az**))
    - Créez un pool backend pour chaque équilibrage de charge. Ajoutez les nœuds de cluster associés.
    - Créer une sonde d’intégrité : port 59999
    - Créer une règle d’équilibrage de charge : Autorisez les ports HA, avec l’adresse IP flottante activée. 
   
    Fournissez l’adresse IP de cluster comme adresse IP privée statique pour l’équilibrage de charge.
    - azlbr2 = > adresse IP frontale : 10.3.0.101 (récupération d’une adresse IP inutilisée à partir du sous-réseau de réseau virtuel (**az2az**))
    - Créez un pool backend pour chaque équilibrage de charge. Ajoutez les nœuds de cluster associés.
    - Créer une sonde d’intégrité : port 59999
    - Créer une règle d’équilibrage de charge : Autorisez les ports HA, avec l’adresse IP flottante activée. 
   
12. Sur chaque nœud de cluster, ouvrez le port 59999 (sonde d’intégrité). 
   
    Exécutez la commande suivante sur chaque nœud :
    ```PowerShell
    netsh advfirewall firewall add rule name=PROBEPORT dir=in protocol=tcp action=allow localport=59999 remoteip=any profile=any 
    ```   
13. Demandez au cluster d’écouter les messages de sonde d’intégrité sur le port 59999 et de répondre à partir du nœud qui possède actuellement cette ressource. 
    Exécutez-le une seule fois à partir d’un nœud du cluster, pour chaque cluster. 
    
    Dans notre exemple, assurez-vous de modifier la valeur « ILBIP » en fonction de vos valeurs de configuration. Exécutez la commande suivante à partir d’un nœud **az2az1**/**az2az2**:

    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.3.0.100" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}
    ```

14. Exécutez la commande suivante à partir d’un nœud **az2az3**/**az2az4**. 

    ```PowerShell
    $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
    $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
    $ILBIP = "10.3.0.101" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
    [int]$ProbePort = 59999
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
    ```   
    Assurez-vous que les deux clusters peuvent se connecter/communiquer entre eux. 

    Utilisez la fonctionnalité « se connecter au cluster » dans le gestionnaire du cluster de basculement pour vous connecter à l’autre cluster ou vérifier si un autre cluster répond à l’un des nœuds du cluster actuel.  
   
    ```PowerShell
     Get-Cluster -Name SRAZC1 (ran from az2az3)
    ```
    ```PowerShell
     Get-Cluster -Name SRAZC2 (ran from az2az1)
    ```   

15. Créer des témoins Cloud pour les deux clusters. Créez deux [comptes de stockage](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) (**az2azcw**, **az2azcw2**) dans Azure One pour chaque cluster dans le même groupe de ressources (**SR-AZ2AZ**).

    - Copiez le nom et la clé du compte de stockage à partir de « clés d’accès ».
    - Créez le témoin Cloud à partir du « gestionnaire du cluster de basculement » et utilisez le nom et la clé du compte ci-dessus pour le créer.

16. Exécutez les [tests de validation de cluster](../../failover-clustering/create-failover-cluster.md#validate-the-configuration) avant de passer à l’étape suivante.

17. Démarrez Windows PowerShell et utilisez l’applet de commande [Test-SRTopology](https://docs.microsoft.com/powershell/module/storagereplica/test-srtopology?view=win10-ps) pour déterminer si toutes les conditions des réplicas de stockage sont satisfaites. Vous pouvez utiliser l’applet de commande en mode configuration requise uniquement pour un test rapide, ainsi qu’un mode d’évaluation des performances de longue durée.

18. Configurez le réplica de stockage de cluster à cluster.
   
    Accordez l’accès d’un cluster à un autre dans les deux sens :

    Dans notre exemple :

    ```PowerShell
      Grant-SRAccess -ComputerName az2az1 -Cluster SRAZC2
    ```
    Si vous utilisez Windows Server 2016, exécutez également la commande suivante :

    ```PowerShell
      Grant-SRAccess -ComputerName az2az3 -Cluster SRAZC1
    ```   
   
19. Créer des SRPartnership pour les clusters :</ol>

    - Pour le cluster **SRAZC1**.
    - Emplacement du volume :-c:\ClusterStorage\DataDisk1
    - Emplacement du journal :-g :
    - Pour le cluster **SRAZC2**
    - Emplacement du volume :-c:\ClusterStorage\DataDisk2
    - Emplacement du journal :-g :

Exécutez la commande suivante :

```PowerShell

New-SRPartnership -SourceComputerName SRAZC1 -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\DataDisk1 -SourceLogVolumeName  g: -DestinationComputerName **SRAZC2** -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\DataDisk2 -DestinationLogVolumeName  g:
```