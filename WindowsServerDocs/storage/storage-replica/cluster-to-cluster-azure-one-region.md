---
title: Cluster à cluster réplica de stockage dans la même région dans Azure
description: Cluster à cluster de réplication du stockage dans la même région dans Azure
keywords: Le réplica de stockage, le Gestionnaire de serveur, Windows Server, Azure, Cluster, la même région
author: arduppal
ms.author: arduppal
ms.date: 04/26/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-replica
manager: mchad
ms.openlocfilehash: 4371192d44878d3c953374b8d307b4d5612869f5
ms.sourcegitcommit: 7e54a1bcd31cd2c6b18fd1f21b03f5cfb6165bf3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65461977"
---
# <a name="cluster-to-cluster-storage-replica-within-the-same-region-in-azure"></a>Cluster à cluster réplica de stockage dans la même région dans Azure

> S’applique à : Windows Server 2019, Windows Server 2016, Windows Server (canal semi-annuel)

Vous pouvez configurer la réplication de stockage de cluster à cluster au sein de la même région dans Azure. Dans les exemples ci-dessous, nous utilisons un cluster à deux nœuds, mais le réplica de stockage de cluster à cluster n’est pas limité à un cluster à deux nœuds. L’illustration ci-dessous est un cluster à deux nœuds d’espace de stockage Direct qui peut communiquer entre eux, sont dans le même domaine et dans la même région.

Regardez les vidéos ci-dessous pour obtenir une présentation complète du processus.

Première partie
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE26f2Y]

Deuxième partie
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE269Pq]

![Le diagramme d’architecture présentant le réplica de stockage de Cluster à cluster dans Azure au sein de la même région.](media\Cluster-to-cluster-azure-one-region\architecture.png)
> [!IMPORTANT]
> Tous les exemples référencés sont spécifiques à l’illustration ci-dessus.

1. Créer un [groupe de ressources](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup) dans le portail Azure dans une région (**SR-AZ2AZ** dans **ouest des États-Unis 2**). 
2. Créez deux [à haute disponibilité](https://ms.portal.azure.com/#create/Microsoft.AvailabilitySet-ARM) dans le groupe de ressources (**SR-AZ2AZ**) créé ci-dessus, un pour chaque cluster. 
    a. Availability set (**az2azAS1**) b. Groupe à haute disponibilité (**az2azAS2**)
3. Créer un [réseau virtuel](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM) (**az2az-réseau virtuel**) dans le groupe de ressources créé précédemment (**SR-AZ2AZ**), ayant au moins un sous-réseau. 
4. Créer un [groupe de sécurité réseau](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM) (**az2az-NSG**) et ajouter une règle de sécurité de trafic entrant pour RDP:3389. Vous pouvez choisir de supprimer cette règle une fois que vous avez terminé votre installation. 
5. Créer des Windows Server [machines virtuelles](https://ms.portal.azure.com/#create/Microsoft.WindowsServer2016Datacenter-ARM) dans le groupe de ressources créé précédemment (**SR-AZ2AZ**). Utiliser le réseau virtuel créé précédemment (**az2az-réseau virtuel**) et le groupe de sécurité réseau (**az2az-NSG**). 
   
   Contrôleur de domaine (**az2azDC**). Vous pouvez choisir créer un troisième groupe à haute disponibilité pour votre contrôleur de domaine ou ajouter le contrôleur de domaine dans un des deux groupes à haute disponibilité. Si vous ajoutez ceci au créé pour les deux clusters à haute disponibilité, attribuez-lui une adresse IP publique Standard lors de la création de machine virtuelle. 
   - Installer le Service de domaine Active Directory.
   - Créer un domaine (Contoso.com)
   - Créer un utilisateur avec des privilèges d’administrateur (contosoadmin) 
   - Créez deux machines virtuelles (**az2az1**, **az2az2**) dans le premier groupe de disponibilité défini (**az2azAS1**). Affecter une adresse IP publique standard à chaque machine virtuelle lors de la création lui-même.
   - Ajouter des disques gérés 2 au moins à chaque machine
   - Installer la fonctionnalité de Clustering de basculement et de réplica de stockage
   - Créez deux machines virtuelles (**az2az3**, **az2az4**) dans le deuxième groupe de disponibilité défini (**az2azAS2**). Affecter l’adresse IP publique standard à chaque machine virtuelle lors de la création lui-même. 
   - Ajouter des disques gérés 2 au moins à chaque machine. 
   - Installez la fonctionnalité de Clustering de basculement et de réplica de stockage. 
   
6. Connecter tous les nœuds au domaine et fournir des privilèges d’administrateur à l’utilisateur créé précédemment. 

7. Modifier le serveur DNS du réseau virtuel à l’adresse IP privée de contrôleur de domaine. 
8. Dans notre exemple, le contrôleur de domaine **az2azDC** a l’adresse IP privée (10.3.0.8). Dans le réseau virtuel (**az2az-réseau virtuel**) modification du serveur DNS 10.3.0.8. Connecter tous les nœuds pour « Contoso.com » et fournir des privilèges d’administrateur pour « contosoadmin ».
   - Connectez-vous en tant que contosoadmin à partir de tous les nœuds. 
    
9. La création des clusters (**SRAZC1**, **SRAZC2**). Voici les commandes PowerShell pour notre exemple
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
   
   Pour chaque cluster créer le volume et le disque virtuel. Une pour les données et l’autre pour le journal. 
   
11. Créer une référence (SKU) Standard interne [équilibreur de charge](https://ms.portal.azure.com/#create/Microsoft.LoadBalancer-ARM) pour chaque cluster (**azlbr1**,**azlbr2**). 
   
   Indiquez l’adresse IP de Cluster en tant qu’adresse IP privée statique pour l’équilibrage de charge.
   - azlbr1 = > adresse IP frontale : 10.3.0.100 (choisir une adresse IP inutilisée à partir du réseau virtuel (**az2az-réseau virtuel**) sous-réseau)
   - Créer le Pool principal pour chaque équilibreur de charge. Ajoutez les nœuds de cluster associé.
   - Créer la sonde d’intégrité : port 59999
   - Créer la règle d’équilibrage de charge : Autoriser les ports de haute disponibilité, avec des IP flottante est activée. 
   
   Indiquez l’adresse IP de Cluster en tant qu’adresse IP privée statique pour l’équilibrage de charge.
   - azlbr2 = > adresse IP frontale : 10.3.0.101 (choisir une adresse IP inutilisée à partir du réseau virtuel (**az2az-réseau virtuel**) sous-réseau)
   - Créer le Pool principal pour chaque équilibreur de charge. Ajoutez les nœuds de cluster associé.
   - Créer la sonde d’intégrité : port 59999
   - Créer la règle d’équilibrage de charge : Autoriser les ports de haute disponibilité, avec des IP flottante est activée. 
   
12. Sur chaque nœud du cluster, ouvrez le port 59999 (sonde d’intégrité). 
   
    Exécutez la commande suivante sur chaque nœud :
```PowerShell
netsh advfirewall firewall add rule name=PROBEPORT dir=in protocol=tcp action=allow localport=59999 remoteip=any profile=any 
```   
13. Indiquer le cluster pour écouter les messages de sonde d’intégrité sur le Port 59999 et répondre à partir du nœud actuellement propriétaire de cette ressource. Exécutez-le une seule fois à partir de n’importe quel un nœud du cluster, pour chaque cluster. 
    
    Dans notre exemple, veillez à modifier le « ILBIP » en fonction de vos valeurs de configuration. Exécutez la commande suivante à partir de n’importe quel un nœud **az2az1**/**az2az2**:

    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.3.0.100" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}
    ```

14. Exécutez la commande suivante à partir de n’importe quel un nœud **az2az3**/**az2az4**. 

    ```PowerShell
    $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
    $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
    $ILBIP = "10.3.0.101" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
    [int]$ProbePort = 59999
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
    ```   
   Assurez-vous que les deux clusters peuvent se connecter / communiquent entre eux. 

   Soit utiliser la fonctionnalité de « Se connecter au Cluster » dans le Gestionnaire du cluster de basculement pour vous connecter à l’autre cluster ou vérifier le qu'autre cluster répond à partir d’un des nœuds du cluster actif.  
   
   ```PowerShell
     Get-Cluster -Name SRAZC1 (ran from az2az3)
   ```
   ```PowerShell
     Get-Cluster -Name SRAZC2 (ran from az2az1)
   ```   

15. Créer des témoins de cloud pour les deux clusters. Créez deux [comptes de stockage](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) (**az2azcw**, **az2azcw2**) dans azure, vous pour chaque cluster du même groupe de ressources (**SR-AZ2AZ**).

    - Copier le nom de compte de stockage et la clé à partir de « clés d’accès »
    - Créer le témoin de cloud à partir de « gestionnaire du cluster de basculement » et utiliser le nom du compte et la clé ci-dessus pour le créer.

16. Exécutez [les tests de validation de cluster](../../failover-clustering/create-failover-cluster.md#validate-the-configuration) avant de passer à l’étape suivante.

17. Démarrez Windows PowerShell et utilisez l’applet de commande [Test-SRTopology](https://docs.microsoft.com/powershell/module/storagereplica/test-srtopology?view=win10-ps) pour déterminer si toutes les conditions des réplicas de stockage sont satisfaites. Vous pouvez utiliser l’applet de commande dans un mode d’exigences uniquement pour un test rapide, mais aussi un mode d’évaluation de performances longue.

18. Configurer le réplica de stockage de cluster à cluster.
   
   Accorder l’accès à partir d’un cluster à un autre cluster dans les deux sens :

   Dans notre exemple :

   ```PowerShell
      Grant-SRAccess -ComputerName az2az1 -Cluster SRAZC2
   ```
Si vous utilisez Windows Server 2016, puis également exécuter cette commande :

   ```PowerShell
      Grant-SRAccess -ComputerName az2az3 -Cluster SRAZC1
   ```   
   
19. Créer SRPartnership pour les clusters :</ol>

 - Pour cluster **SRAZC1**.
   - Emplacement du volume :-c:\ClusterStorage\DataDisk1
   - Emplacement du journal :-g :
 - Pour cluster **SRAZC2**
    - Emplacement du volume :-c:\ClusterStorage\DataDisk2
    - Emplacement du journal :-g :

Exécutez la commande suivante :

```PowerShell

New-SRPartnership -SourceComputerName SRAZC1 -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\DataDisk1 -SourceLogVolumeName  g: -DestinationComputerName **SRAZC2** -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\DataDisk2 -DestinationLogVolumeName  g:
```