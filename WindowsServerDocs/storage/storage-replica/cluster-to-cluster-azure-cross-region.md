---
title: Réplica du stockage de cluster à cluster entre régions dans Azure
description: Réplication de stockage de cluster à Cluster cross région dans Azure
keywords: Le réplica de stockage, le Gestionnaire de serveur, Windows Server, Azure, Cluster, inter-région autre région
author: arduppal
ms.author: arduppal
ms.date: 12/19/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-replica
manager: mchad
ms.openlocfilehash: 95f3e9e929d6a28279f526eec6dbdf0427bd74c0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447584"
---
# <a name="cluster-to-cluster-storage-replica-cross-region-in-azure"></a>Réplica du stockage de cluster à cluster entre régions dans Azure

> S’applique à : Windows Server 2019, Windows Server 2016, Windows Server (canal semi-annuel)

Vous pouvez configurer des réplicas de stockage de Cluster à Cluster pour les applications entre les régions dans Azure. Dans les exemples ci-dessous, nous utilisons un cluster à deux nœuds, mais le réplica de stockage de Cluster à Cluster n’est pas limité à un cluster à deux nœuds. L’illustration ci-dessous est un cluster à deux nœuds d’espace de stockage Direct qui peut communiquer entre eux, est dans le même domaine et est entre les régions.

Regardez la vidéo ci-dessous pour obtenir une présentation complète du processus.
> [!video https://www.microsoft.com/en-us/videoplayer/embed/RE26xeW]

![L’architecture de diagramme illustrant SR C2C dans Azure au sein même région.](media/Cluster-to-cluster-azure-cross-region/architecture.png)
> [!IMPORTANT]
> Tous les exemples référencés sont spécifiques à l’illustration ci-dessus.


1. Dans le portail Azure, créez [groupes de ressources](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup) dans deux régions différentes.

    Par exemple, **SR-AZ2AZ** dans **ouest des États-Unis 2** et **SR-AZCROSS** dans **ouest des États-Unis**, comme indiqué ci-dessus.

2. Créez deux [à haute disponibilité](https://ms.portal.azure.com/#create/Microsoft.AvailabilitySet-ARM), une dans chaque groupe de ressources pour chaque cluster.
    - Groupe à haute disponibilité (**az2azAS1**) dans (**SR-AZ2AZ**)
    - Groupe à haute disponibilité (**azcross-AS**) dans (**SR-AZCROSS**)

3. Créer deux réseaux virtuels
   - Créer le [réseau virtuel](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM) (**az2az-réseau virtuel**) dans le premier groupe de ressources (**SR-AZ2AZ**), avoir un sous-réseau et un sous-réseau de passerelle.
   - Créer le [réseau virtuel](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM) (**azcross-réseau virtuel**) du deuxième groupe de ressources (**SR-AZCROSS**), avoir un sous-réseau et un sous-réseau de passerelle.

4. Créez deux groupes de sécurité réseau
   - Créer le [groupe de sécurité réseau](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM) (**az2az-NSG**) dans le premier groupe de ressources (**SR-AZ2AZ**).
   - Créer le [groupe de sécurité réseau](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM) (**azcross-NSG**) du deuxième groupe de ressources (**SR-AZCROSS**).

   Ajouter une règle de sécurité de trafic entrant pour RDP:3389 aux deux groupes de sécurité réseau. Vous pouvez choisir de supprimer cette règle une fois que vous avez terminé votre installation.

5. Créer des Windows Server [machines virtuelles](https://ms.portal.azure.com/#create/Microsoft.WindowsServer2016Datacenter-ARM) dans les groupes de ressources créé précédemment.

   Contrôleur de domaine (**az2azDC**). Vous pouvez choisir créer un 3e à haute disponibilité pour votre contrôleur de domaine ou ajouter le contrôleur de domaine dans un de l’ensemble de la disponibilité de deux. Si vous ajoutez ceci au créé pour les deux clusters à haute disponibilité, attribuez-lui une adresse IP publique Standard lors de la création de machine virtuelle.
      - Installer le Service de domaine Active Directory.
      - Créer un domaine (contoso.com)
      - Créer un utilisateur avec des privilèges d’administrateur (contosoadmin)

   Créez deux machines virtuelles (**az2az1**, **az2az2**) dans le groupe de ressources (**SR-AZ2AZ**) à l’aide de réseau virtuel (**az2az-réseau virtuel**) et groupe de sécurité réseau (**az2az-NSG**) dans à haute disponibilité (**az2azAS1**). Affecter une adresse IP publique standard à chaque machine virtuelle lors de la création lui-même.
      - Ajouter au moins deux des disques gérés à chaque machine
      - Installez le Clustering de basculement et de la fonctionnalité de réplica de stockage

   Créez deux machines virtuelles (**azcross1**, **azcross2**) dans le groupe de ressources (**SR-AZCROSS**) à l’aide de réseau virtuel (**azcross-réseau virtuel**) et le groupe de sécurité réseau (**azcross-NSG**) dans à haute disponibilité (**azcross-AS**). Affecter l’adresse IP publique standard à chaque machine virtuelle lors de la création lui-même
      - Ajouter au moins deux disques gérés à chaque machine
      - Installez le Clustering de basculement et de la fonctionnalité de réplica de stockage

   Connecter tous les nœuds au domaine et fournir des privilèges d’administrateur à l’utilisateur créé précédemment.

   Modifier le serveur DNS du réseau virtuel à l’adresse IP privée de contrôleur de domaine.
   - Dans l’exemple, le contrôleur de domaine **az2azDC** a l’adresse IP privée (10.3.0.8). Dans le réseau virtuel (**az2az-réseau virtuel** et **azcross-réseau virtuel**) modification du serveur DNS 10.3.0.8. 

     Dans l’exemple, connecter tous les nœuds pour « contoso.com » et fournir des privilèges d’administrateur pour « contosoadmin ».
   - Connectez-vous en tant que contosoadmin à partir de tous les nœuds. 
 
6. La création des clusters (**SRAZC1**, **SRAZCross**).

   Voici les commandes PowerShell pour l’exemple
   ```powershell
      New-Cluster -Name SRAZC1 -Node az2az1,az2az2 –StaticAddress 10.3.0.100
   ```
   ```powershell
      New-Cluster -Name SRAZCross -Node azcross1,azcross2 –StaticAddress 10.0.0.10
   ```

7. Activer les espaces de stockage direct.

   ```powershell
      Enable-clusterS2D
   ```

   > [!NOTE]
   > Pour chaque cluster créer le volume et le disque virtuel. Une pour les données et l’autre pour le journal.

8. Créer une référence (SKU) Standard interne [équilibreur de charge](https://ms.portal.azure.com/#create/Microsoft.LoadBalancer-ARM) pour chaque cluster (**azlbr1**, **azlbazcross**).

   Indiquez l’adresse IP de Cluster en tant qu’adresse IP privée statique pour l’équilibrage de charge.
      - azlbr1 = > adresse IP frontale : 10.3.0.100 (choisir une adresse IP inutilisée à partir du réseau virtuel (**az2az-réseau virtuel**) sous-réseau)
      - Créer le Pool principal pour chaque équilibreur de charge. Ajoutez les nœuds de cluster associé.
      - Créer la sonde d’intégrité : port 59999
      - Créer la règle d’équilibrage de charge : Autoriser les ports de haute disponibilité, avec des IP flottante est activée.

   Indiquez l’adresse IP de Cluster en tant qu’adresse IP privée statique pour l’équilibrage de charge. 
      - azlbazcross = > Frontend IP : 10.0.0.10 (choisir une adresse IP inutilisée à partir du réseau virtuel (**azcross-réseau virtuel**) sous-réseau)
      - Créer le Pool principal pour chaque équilibreur de charge. Ajoutez les nœuds de cluster associé.
      - Créer la sonde d’intégrité : port 59999
      - Créer la règle d’équilibrage de charge : Autoriser les ports de haute disponibilité, avec des IP flottante est activée. 

9. Créer [passerelle de réseau virtuel](https://ms.portal.azure.com/#create/Microsoft.VirtualNetworkGateway-ARM) pour la connectivité de réseau virtuel à réseau virtuel.

   - Créer la première passerelle de réseau virtuel (**az2az-VNetGateway**) dans le premier groupe de ressources (**SR-AZ2AZ**)
   - Type de passerelle = VPN, VPN et tapez = Route-based

   - Créer la deuxième passerelle de réseau virtuel (**azcross-VNetGateway**) du deuxième groupe de ressources (**SR-AZCROSS**)
   - Type de passerelle = VPN, VPN et tapez = Route-based

   - Créer une connexion de réseau virtuel à réseau virtuel à partir de la première passerelle de réseau virtuel à la seconde passerelle de réseau virtuel. Fournir une clé partagée

   - Créer une connexion de réseau virtuel à réseau virtuel à partir de la deuxième passerelle de réseau virtuel pour la première passerelle de réseau virtuel. Fournir la même clé partagée, comme indiqué dans l’étape ci-dessus. 

10. Sur chaque nœud du cluster, ouvrez le port 59999 (sonde d’intégrité).

    Exécutez la commande suivante sur chaque nœud :

    ```powershell
      netsh advfirewall firewall add rule name=PROBEPORT dir=in protocol=tcp action=allow localport=59999 remoteip=any profile=any 
    ```

11. Indiquer le cluster pour écouter les messages de sonde d’intégrité sur le Port 59999 et répondre à partir du nœud actuellement propriétaire de cette ressource.

    Exécutez-le une seule fois à partir de n’importe quel un nœud du cluster, pour chaque cluster. 
    
    Dans notre exemple, veillez à modifier le « ILBIP » en fonction de vos valeurs de configuration. Exécutez la commande suivante à partir de n’importe quel un nœud **az2az1**/**az2az2**

    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.3.0.100" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
    ```

12. Exécutez la commande suivante à partir de n’importe quel un nœud **azcross1**/**azcross2**
    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.0.0.10" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
    ```

    Assurez-vous que les deux clusters peuvent se connecter / communiquent entre eux.

    Soit utiliser la fonctionnalité de « Se connecter au Cluster » dans le Gestionnaire du cluster de basculement pour vous connecter à l’autre cluster ou vérifier le qu'autre cluster répond à partir d’un des nœuds du cluster actif.

    À partir de l’exemple nous avons été à l’aide de :
    ```powershell
      Get-Cluster -Name SRAZC1 (ran from azcross1)
    ```
    ```powershell
      Get-Cluster -Name SRAZCross (ran from az2az1) 
    ```

13. Créez un témoin cloud pour les deux clusters. Créez deux [comptes de stockage](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) (**az2azcw**,**azcrosssa**) dans Azure, un pour chaque cluster dans chaque groupe de ressources (**SR-AZ2AZ**,  **SR-AZCROSS**).
   
    - Copier le nom de compte de stockage et la clé à partir de « clés d’accès »
    - Créer le témoin de cloud à partir de « gestionnaire du cluster de basculement » et utiliser le nom du compte et la clé ci-dessus pour le créer. 

14. Exécutez [les tests de validation de cluster](../../failover-clustering/create-failover-cluster.md#validate-the-configuration) avant de passer à l’étape suivante

15. Démarrez Windows PowerShell et utilisez l’applet de commande [Test-SRTopology](https://docs.microsoft.com/powershell/module/storagereplica/test-srtopology?view=win10-ps) pour déterminer si toutes les conditions des réplicas de stockage sont satisfaites. Vous pouvez utiliser l’applet de commande dans un mode d’exigences uniquement pour un test rapide ainsi qu’un mode d’évaluation de performances à exécution longue.
 
16. Configurer le réplica de stockage de cluster à cluster.
    Accorder l’accès à partir d’un cluster à un autre cluster dans les deux sens :

    À partir de notre exemple :
    ```powershell
     Grant-SRAccess -ComputerName az2az1 -Cluster SRAZCross
    ```
    Si vous utilisez Windows Server 2016, également exécuter cette commande :

    ```powershell
     Grant-SRAccess -ComputerName azcross1 -Cluster SRAZC1
    ```

17. Créez SR-partenariat pour les deux clusters :</ol>

    - Pour cluster **SRAZC1**
      - Emplacement du volume :-c:\ClusterStorage\DataDisk1
      - Emplacement du journal :-g :
    - Pour cluster **SRAZCross**
      - Emplacement du volume :-c:\ClusterStorage\DataDiskCross
      - Emplacement du journal :-g :

Exécutez la commande :

```powershell
PowerShell

New-SRPartnership -SourceComputerName SRAZC1 -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\DataDisk1 -SourceLogVolumeName  g: -DestinationComputerName SRAZCross -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\DataDiskCross -DestinationLogVolumeName  g:
```