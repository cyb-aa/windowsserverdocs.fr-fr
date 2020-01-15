---
title: Réplica du stockage de cluster à cluster entre régions dans Azure
description: Cluster à la réplication de stockage de cluster dans Azure
keywords: Réplica de stockage, Gestionnaire de serveur, Windows Server, Azure, cluster, inter-régions, région différente
author: arduppal
ms.author: arduppal
ms.date: 12/19/2018
ms.topic: article
ms.prod: windows-server
ms.technology: storage-replica
manager: mchad
ms.openlocfilehash: 806857d5de067c0f4640344ed80338b474dd758e
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950060"
---
# <a name="cluster-to-cluster-storage-replica-cross-region-in-azure"></a>Réplica du stockage de cluster à cluster entre régions dans Azure

> S'applique à : Windows Server 2019, Windows Server 2016, Windows Server (canal semi-annuel)

Vous pouvez configurer un cluster pour les réplicas de stockage de cluster pour les applications inter-régions dans Azure. Dans les exemples ci-dessous, nous utilisons un cluster à deux nœuds, mais le réplica de stockage de cluster à cluster n’est pas limité à un cluster à deux nœuds. L’illustration ci-dessous est un cluster d’espace de stockage direct à deux nœuds qui peut communiquer entre eux, se trouvent dans le même domaine et sont inter-régions.

Regardez la vidéo ci-dessous pour une procédure pas à pas complète du processus.
> [!video https://www.microsoft.com/videoplayer/embed/RE26xeW]

![Diagramme d’architecture présentant C2C SR dans Azure avec la même région.](media/Cluster-to-cluster-azure-cross-region/architecture.png)
> [!IMPORTANT]
> Tous les exemples référencés sont spécifiques à l’illustration ci-dessus.


1. Dans le Portail Azure, créez des [groupes de ressources](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup) dans deux régions différentes.

    Par exemple, **SR-AZ2AZ** dans l' **ouest des États-Unis 2** et **SR-AZCROSS** dans l' **ouest du Centre des États-Unis**, comme indiqué ci-dessus.

2. Créez deux [groupes à haute disponibilité](https://ms.portal.azure.com/#create/Microsoft.AvailabilitySet-ARM), un dans chaque groupe de ressources pour chaque cluster.
    - Groupe à haute disponibilité (**az2azAS1**) dans (**SR-AZ2AZ**)
    - Groupe à haute disponibilité (**azcross-As**) dans (**SR-azcross**)

3. Créer deux réseaux virtuels
   - Créez le [réseau virtuel](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM) (**az2az-vnet**) dans le premier groupe de ressources (**SR-az2az**), avec un sous-réseau et un sous-réseau de passerelle.
   - Créez le [réseau virtuel](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM) (**azcross-VNET**) dans le deuxième groupe de ressources (**SR-azcross**), avec un sous-réseau et un sous-réseau de passerelle.

4. Créer deux groupes de sécurité réseau
   - Créez le [groupe de sécurité réseau](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM) (**az2az-NSG**) dans le premier groupe de ressources (**SR-az2az**).
   - Créez le [groupe de sécurité réseau](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM) (**azcross-NSG**) dans le deuxième groupe de ressources (**SR-azcross**).

   Ajoutez une règle de sécurité de trafic entrant pour RDP : 3389 aux deux groupes de sécurité réseau. Vous pouvez choisir de supprimer cette règle une fois que vous avez terminé l’installation.

5. Créez des [machines virtuelles](https://ms.portal.azure.com/#create/Microsoft.WindowsServer2016Datacenter-ARM) Windows Server dans les groupes de ressources créés précédemment.

   Contrôleur de domaine (**az2azDC**). Vous pouvez choisir de créer un troisième groupe à haute disponibilité pour votre contrôleur de domaine ou d’ajouter le contrôleur de domaine dans l’un des deux groupe à haute disponibilité. Si vous l’ajoutez au groupe à haute disponibilité créé pour les deux clusters, attribuez-lui une adresse IP publique standard lors de la création de la machine virtuelle.
      - Installez domaine Active Directory Service.
      - Créer un domaine (contoso.com)
      - Créer un utilisateur avec des privilèges d’administrateur (contosoadmin)

   Créez deux machines virtuelles (**az2az1**, **az2az2**) dans le groupe de ressources (**SR-AZ2AZ**) à l’aide du réseau virtuel (**AZ2AZ-vnet**) et du groupe de sécurité réseau (**AZ2AZ-NSG**) dans le groupe à haute disponibilité (**az2azAS1**). Affectez une adresse IP publique standard à chaque ordinateur virtuel pendant la création.
      - Ajoutez au moins deux disques managés à chaque ordinateur
      - Installer le clustering de basculement et la fonctionnalité réplica de stockage

   Créez deux machines virtuelles (**azcross1**, **azcross2**) dans le groupe de ressources (**SR-AZCROSS**) à l’aide du réseau virtuel (**AZCROSS-VNET**) et du groupe de sécurité réseau (**AZCROSS-NSG**) dans le groupe à haute disponibilité (**AZCROSS-As**). Affecter une adresse IP publique standard à chaque ordinateur virtuel lors de la création proprement dite
      - Ajoutez au moins deux disques managés à chaque ordinateur
      - Installer le clustering de basculement et la fonctionnalité réplica de stockage

   Connectez tous les nœuds au domaine et fournissez des privilèges d’administrateur à l’utilisateur créé précédemment.

   Remplacez le serveur DNS du réseau virtuel par l’adresse IP privée du contrôleur de domaine.
   - Dans l’exemple, le contrôleur de domaine **az2azDC** a une adresse IP privée (10.3.0.8). Dans le réseau virtuel (**az2az-vnet** et **azcross-vnet**), modifiez le serveur DNS 10.3.0.8. 

     Dans l’exemple, connectez tous les nœuds à « contoso.com » et fournissez des privilèges d’administrateur à « contosoadmin ».
   - Connectez-vous en tant que contosoadmin à partir de tous les nœuds. 
 
6. Créez les clusters (**SRAZC1**, **SRAZCross**).

   Voici les commandes PowerShell pour l’exemple
   ```powershell
      New-Cluster -Name SRAZC1 -Node az2az1,az2az2 –StaticAddress 10.3.0.100
   ```
   ```powershell
      New-Cluster -Name SRAZCross -Node azcross1,azcross2 –StaticAddress 10.0.0.10
   ```

7. Activez les espaces de stockage direct.

   ```powershell
      Enable-clusterS2D
   ```

   > [!NOTE]
   > Pour chaque cluster, créez un disque virtuel et un volume. Une pour les données et une autre pour le journal.

8. Créez un [load balancer](https://ms.portal.azure.com/#create/Microsoft.LoadBalancer-ARM) de référence SKU standard interne pour chaque cluster (**azlbr1**, **azlbazcross**).

   Fournissez l’adresse IP de cluster comme adresse IP privée statique pour l’équilibrage de charge.
      - azlbr1 = > IP frontend : 10.3.0.100 (récupérer une adresse IP inutilisée à partir du sous-réseau de réseau virtuel (**az2az-vnet**))
      - Créez un pool backend pour chaque équilibrage de charge. Ajoutez les nœuds de cluster associés.
      - Créer une sonde d’intégrité : port 59999
      - Créer une règle d’équilibrage de charge : autoriser les ports haute disponibilité avec une adresse IP flottante activée.

   Fournissez l’adresse IP de cluster comme adresse IP privée statique pour l’équilibrage de charge. 
      - azlbazcross = > IP frontend : 10.0.0.10 (récupérer une adresse IP inutilisée à partir du sous-réseau de réseau virtuel (**azcross-VNET**))
      - Créez un pool backend pour chaque équilibrage de charge. Ajoutez les nœuds de cluster associés.
      - Créer une sonde d’intégrité : port 59999
      - Créer une règle d’équilibrage de charge : autoriser les ports haute disponibilité avec une adresse IP flottante activée. 

9. Créer une [passerelle de réseau virtuel pour la connectivité de réseau virtuel](https://ms.portal.azure.com/#create/Microsoft.VirtualNetworkGateway-ARM) à réseau virtuel.

   - Créer la première passerelle de réseau virtuel (**az2az-VNetGateway**) dans le premier groupe de ressources (**SR-az2az**)
   - Type de passerelle = VPN et type de VPN = basé sur l’itinéraire

   - Création de la deuxième passerelle de réseau virtuel (**azcross-VNetGateway**) dans le deuxième groupe de ressources (**SR-azcross**)
   - Type de passerelle = VPN et type de VPN = basé sur l’itinéraire

   - Créer une connexion de réseau virtuel à réseau virtuel à partir de la première passerelle de réseau virtuel vers la deuxième passerelle de réseau virtuel. Fournir une clé partagée

   - Créer une connexion de réseau virtuel à réseau virtuel depuis la deuxième passerelle de réseau virtuel vers la première passerelle de réseau virtuel. Fournissez la même clé partagée que celle fournie à l’étape ci-dessus. 

10. Sur chaque nœud de cluster, ouvrez le port 59999 (sonde d’intégrité).

    Exécutez la commande suivante sur chaque nœud :

    ```powershell
      netsh advfirewall firewall add rule name=PROBEPORT dir=in protocol=tcp action=allow localport=59999 remoteip=any profile=any 
    ```

11. Demandez au cluster d’écouter les messages de sonde d’intégrité sur le port 59999 et de répondre à partir du nœud qui possède actuellement cette ressource.

    Exécutez-le une seule fois à partir d’un nœud du cluster, pour chaque cluster. 
    
    Dans notre exemple, assurez-vous de modifier la valeur « ILBIP » en fonction de vos valeurs de configuration. Exécutez la commande suivante à partir de n’importe quel nœud **az2az1**/**az2az2**

    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.3.0.100" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
    ```

12. Exécutez la commande suivante à partir de n’importe quel nœud **azcross1**/**azcross2**
    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.0.0.10" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
    ```

    Assurez-vous que les deux clusters peuvent se connecter/communiquer entre eux.

    Utilisez la fonctionnalité « se connecter au cluster » dans le gestionnaire du cluster de basculement pour vous connecter à l’autre cluster ou vérifier si un autre cluster répond à l’un des nœuds du cluster actuel.

    Dans l’exemple que nous utilisons :
    ```powershell
      Get-Cluster -Name SRAZC1 (ran from azcross1)
    ```
    ```powershell
      Get-Cluster -Name SRAZCross (ran from az2az1) 
    ```

13. Créez un témoin Cloud pour les deux clusters. Créez deux [comptes de stockage](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) (**az2azcw**,**azcrosssa**) dans Azure, un pour chaque cluster dans chaque groupe de ressources (**SR-AZ2AZ**, **SR-AZCROSS**).
   
    - Copiez le nom et la clé du compte de stockage à partir de « clés d’accès ».
    - Créez le témoin Cloud à partir du « gestionnaire du cluster de basculement » et utilisez le nom et la clé du compte ci-dessus pour le créer. 

14. Exécuter les [tests de validation de cluster](../../failover-clustering/create-failover-cluster.md#validate-the-configuration) avant de passer à l’étape suivante

15. Démarrez Windows PowerShell et utilisez l’applet de commande [Test-SRTopology](https://docs.microsoft.com/powershell/module/storagereplica/test-srtopology?view=win10-ps) pour déterminer si toutes les conditions des réplicas de stockage sont satisfaites. Vous pouvez utiliser l’applet de commande dans un mode d’exigences uniquement pour un test rapide ainsi qu’un mode d’évaluation de performances à exécution longue.
 
16. Configurez le réplica de stockage de cluster à cluster.
    Accordez l’accès d’un cluster à un autre dans les deux sens :

    Dans notre exemple :
    ```powershell
     Grant-SRAccess -ComputerName az2az1 -Cluster SRAZCross
    ```
    Si vous utilisez Windows Server 2016, exécutez également la commande suivante :

    ```powershell
     Grant-SRAccess -ComputerName azcross1 -Cluster SRAZC1
    ```

17. Créer un partenariat SR pour les deux clusters :</ol>

    - Pour le cluster **SRAZC1**
      - Emplacement du volume :-c:\ClusterStorage\DataDisk1
      - Emplacement du journal :-g :
    - Pour le cluster **SRAZCross**
      - Emplacement du volume :-c:\ClusterStorage\DataDiskCross
      - Emplacement du journal :-g :

Exécutez la commande suivante :

```powershell
PowerShell

New-SRPartnership -SourceComputerName SRAZC1 -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\DataDisk1 -SourceLogVolumeName  g: -DestinationComputerName SRAZCross -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\DataDiskCross -DestinationLogVolumeName  g:
```