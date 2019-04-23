---
title: Délimiter l’allocation de volumes dans les espaces de stockage Direct
ms.author: cosmosdarwin
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 03/29/2018
ms.openlocfilehash: c93cbf4ba418f6702cf8747508605952d993508d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889050"
---
# <a name="delimit-the-allocation-of-volumes-in-storage-spaces-direct"></a>Délimiter l’allocation de volumes dans les espaces de stockage Direct
> S'applique à : Windows Server Insider Preview, build 17093 et versions ultérieur

Windows Server Insider Preview introduit une option permettant de délimiter manuellement l’allocation de volumes dans les espaces de stockage Direct. Cette opération afin de peut augmenter considérablement d’une tolérance de panne dans certaines conditions, mais impose certaines considérations relatives à la gestion ajoutés et la complexité. Cette rubrique explique comment cela fonctionne et donne des exemples dans PowerShell.

   > [!IMPORTANT]
   > Cette fonctionnalité est une nouveauté dans Windows Server Insider Preview, build 17093 et versions ultérieur. Il n’est pas disponible dans Windows Server 2016. Nous invitons les professionnels de l’informatique pour joindre le [programme Insider de Windows Server](https://aka.ms/serverinsider) nous envoyer vos commentaires sur ce que nous pouvons faire pour que Windows Server fonctionne mieux pour votre organisation.

## <a name="prerequisites"></a>Prérequis

### <a name="green-checkmark-iconmediadelimit-volume-allocationsupportedpng-consider-using-this-option-if"></a>![Icône de coche verte.](media/delimit-volume-allocation/supported.png) Envisagez d’utiliser cette option si :

- Votre cluster comporte six ou plusieurs serveurs ; et
- Votre cluster utilise uniquement [triple](storage-spaces-fault-tolerance.md#mirroring) la résilience

### <a name="red-x-iconmediadelimit-volume-allocationunsupportedpng-do-not-use-this-option-if"></a>![Icône X rouge.](media/delimit-volume-allocation/unsupported.png) N’utilisez pas cette option si :

- Votre cluster a moins de six serveurs ; ou
- Votre cluster utilise [parité](storage-spaces-fault-tolerance.md#parity) ou [parité avec accélération miroir](storage-spaces-fault-tolerance.md#mirror-accelerated-parity) la résilience

## <a name="understand"></a>Comprendre

### <a name="review-regular-allocation"></a>Révision : allocation régulière

Avec régulière triple mise en miroir, le volume est divisé en plusieurs petits « dalles » trois fois de copiés et distribués uniformément sur chaque disque de chaque serveur dans le cluster. Pour plus d’informations, consultez [ce blog immersion](https://blogs.technet.microsoft.com/filecab/2016/11/21/deep-dive-pool-in-spaces-direct/).

![Diagramme montrant le volume qui est divisé en trois piles de dalles et distribué uniformément entre tous les serveurs.](media/delimit-volume-allocation/regular-allocation.png)

Cette allocation par défaut optimise parallèle lit et écrit, ce qui conduit à meilleures performances et est intéressante pour sa simplicité : chaque serveur est également occupé, chaque lecteur est tout aussi complet, et tous les volumes de resteront en ligne ou hors connexion assemblent. Chaque volume est garanti de résister à deux défaillances simultanées, en tant que [ces exemples](storage-spaces-fault-tolerance.md#examples) illustrer.

Toutefois, avec cette allocation, les volumes ne peut pas faire face aux défaillances simultanées trois. Si trois serveurs échouent en même temps, ou si dans les trois serveurs tombent en panne en même temps, les volumes deviennent inaccessibles, car au moins une certaine dalles (avec une probabilité très élevée) alloués pour les trois lecteurs exactes ou les serveurs qui ont échoué.

Dans l’exemple ci-dessous, les serveurs 1, 3 et 5 échouent en même temps. Bien que dalles de nombreuses copies de survivre, certains ne pas faire :

![Diagramme montrant trois des six serveurs mis en surbrillance en rouge et le volume total est rouge.](media/delimit-volume-allocation/regular-does-not-survive.png)

Le volume est mis hors connexion et devient inaccessible jusqu'à ce que les serveurs sont récupérées.

### <a name="new-delimited-allocation"></a>Nouveauté : délimités d’allocation

Avec allocation délimitée, vous spécifiez un sous-ensemble de serveurs à utiliser (trois minimale pour un miroir triple). Le volume est divisé en sections qui sont copiées trois fois, comme avant, mais au lieu d’allouer sur chaque serveur, **les sections sont allouées uniquement au sous-ensemble de serveurs que vous spécifiez**.

![Diagramme montrant le volume qui est divisé en trois piles de dalles et distribué uniquement aux trois des six serveurs.](media/delimit-volume-allocation/delimited-allocation.png)

#### <a name="advantages"></a>Avantages

Avec cette allocation, le volume est susceptible de faire face aux défaillances simultanées trois : en fait, sa probabilité de survie augmente à partir de 0 % (avec allocation régulière) à 95 % (avec allocation délimitée) dans ce cas ! Intuitivement, il s’agit, car il ne repose pas sur les serveurs, 4, 5 ou 6 afin qu’il n’est pas affectée par leurs échecs.

Dans l’exemple ci-dessus, les serveurs 1, 3 et 5 échouent en même temps. Étant donné qu’allocation délimitée assuré que ce serveur 2 contient une copie de chaque prépare, chaque prépare une copie survivant et le volume reste en ligne et accessible :

![Diagramme montrant trois des six serveurs mis en surbrillance en rouge, mais le volume total est vert.](media/delimit-volume-allocation/delimited-does-survive.png)

Probabilité de survie varie selon le nombre de serveurs et d’autres facteurs : consultez [Analysis](#analysis) pour plus d’informations.

#### <a name="disadvantages"></a>Inconvénients

Allocation délimitée impose certaines considérations relatives à la gestion ajoutés et la complexité :

1. L’administrateur est chargé pour délimiter l’allocation de chaque volume équilibrer l’utilisation du stockage entre les serveurs et de respecter une probabilité élevée de survie, comme décrit dans la [meilleures pratiques](#best-practices) section.

2. Avec allocation délimitée, réserver l’équivalent de **lecteur d’une capacité par serveur (avec aucun maximum)**. Il s’agit plus que le [publié recommandation](plan-volumes.md#choosing-the-size-of-volumes) pour l’allocation régulière, qui prend en charge une capacité quatre disques au total.

3. Si un serveur échoue et doit être remplacé, comme décrit dans [supprimer un serveur et ses lecteurs](remove-servers.md#remove-a-server-and-its-drives), l’administrateur est responsable de la mise à jour de la délimitation des volumes concernés en ajoutant le nouveau serveur et en supprimant l’échec : par exemple ci-dessous.

## <a name="usage-in-powershell"></a>Utilisation dans PowerShell

Vous pouvez utiliser le `New-Volume` applet de commande pour créer des volumes dans les espaces de stockage Direct.

Par exemple, pour créer un volume triple régulière :

```PowerShell
New-Volume -FriendlyName "MyRegularVolume" -Size 100GB
```

### <a name="create-a-volume-and-delimit-its-allocation"></a>Créer un volume et délimiter son allocation

Pour créer un volume en miroir triple et délimiter son allocation :

1. Tout d’abord affecter les serveurs dans votre cluster à la variable `$Servers`:

    ```PowerShell
    $Servers = Get-StorageFaultDomain -Type StorageScaleUnit | Sort FriendlyName
    ```

   > [!TIP]
   > Dans les espaces de stockage Direct, le terme « Unité d’échelle de stockage » fait référence à tout le stockage brut attaché à un seul serveur, y compris les lecteurs connectés directement et en attachement direct des boîtiers externes avec des lecteurs. Dans ce contexte, il est identique à « server ».

2. Spécifiez les serveurs à utiliser avec la nouvelle `-StorageFaultDomainsToUse` paramètre et en indexant dans `$Servers`. Par exemple, pour délimiter l’allocation à la première, deuxième et troisième serveurs (index 0, 1 et 2) :

    ```PowerShell
    New-Volume -FriendlyName "MyVolume" -Size 100GB -StorageFaultDomainsToUse $Servers[0,1,2]
    ```

### <a name="see-a-delimited-allocation"></a>Consultez une allocation délimitée

Pour voir comment *MyVolume* est alloué, utilisez la `Get-VirtualDiskFootprintBySSU.ps1` de script dans [annexe](#appendix):

```PowerShell
PS C:\> .\Get-VirtualDiskFootprintBySSU.ps1

VirtualDiskFriendlyName TotalFootprint Server1 Server2 Server3 Server4 Server5 Server6
----------------------- -------------- ------- ------- ------- ------- ------- -------
MyVolume                300 GB         100 GB  100 GB  100 GB  0       0       0      
```

Notez qu’uniquement Server1, Server2 et Server3 contient des sections de *MyVolume*.

### <a name="change-a-delimited-allocation"></a>Modifier une allocation délimitée

Utilisez la nouvelle `Add-StorageFaultDomain` et `Remove-StorageFaultDomain` pour modifier la façon dont l’allocation est délimitée par des applets de commande.

Par exemple, pour déplacer *MyVolume* en charge par un serveur :

1. Spécifier que le serveur quatrième **pouvez** stocker dalles de *MyVolume*:

    ```PowerShell
    Get-VirtualDisk MyVolume | Add-StorageFaultDomain -StorageFaultDomains $Servers[3]
    ```

2. Spécifier que le premier serveur **ne peut pas** stocker dalles de *MyVolume*:

    ```PowerShell
    Get-VirtualDisk MyVolume | Remove-StorageFaultDomain -StorageFaultDomains $Servers[0]
    ```

3. Rééquilibrer le pool de stockage pour que la modification prenne effet :

    ```PowerShell
    Get-StoragePool S2D* | Optimize-StoragePool
    ```

![Diagramme montrant que les dalles migrer fr-masse depuis des serveurs 1, 2 et 3 aux serveurs 2, 3 et 4.](media/delimit-volume-allocation/move.gif)

Vous pouvez surveiller la progression de la rééquilibrage avec `Get-StorageJob`.

Une fois terminée, vérifiez que *MyVolume* a été déplacé en exécutant `Get-VirtualDiskFootprintBySSU.ps1` à nouveau.

```PowerShell
PS C:\> .\Get-VirtualDiskFootprintBySSU.ps1

VirtualDiskFriendlyName TotalFootprint Server1 Server2 Server3 Server4 Server5 Server6
----------------------- -------------- ------- ------- ------- ------- ------- -------
MyVolume                300 GB         0       100 GB  100 GB  100 GB  0       0      
```

Notez que ne contient pas de Server1 dalles de *MyVolume* plus – au lieu de cela, SERVEUR04 est.

## <a name="best-practices"></a>Meilleures pratiques

Voici les meilleures pratiques à suivre lors de l’aide délimités allocation de volume :

### <a name="choose-three-servers"></a>Choisissez les trois serveurs

Délimiter chaque volume en miroir triple à trois serveurs, pas plus.

### <a name="balance-storage"></a>Stockage de solde

Équilibrer la quantité de stockage est alloué à chaque serveur, en tenant compte de la taille du volume.

### <a name="every-delimited-allocation-unique"></a>Chaque allocation délimitée unique

Pour optimiser la tolérance de panne, assurez-vous d’allocation d’un volume unique, ce qui signifie qu’il ne partage pas *tous les* ses serveurs avec un autre volume (un chevauchement est OK). Avec les serveurs de N, il existe des « N choisir 3 » les combinaisons uniques : Voici ce que cela signifie que pour certaines tailles de cluster courantes :

| Nombre de serveurs (N) | Nombre d’unique délimité par des allocations (Sélectionner les N 3) |
|-----------------------|-----------------------------------------------------|
| 6                     | 20                                                  |
| 8                     | 56                                                  |
| 12                    | 220                                                 |
| 16                    | 560                                                 |

   > [!TIP]
   > Prendre en compte cette revue utile de [combinatorics et choisissez la notation](https://betterexplained.com/articles/easy-permutations-and-combinations/).

Voici un exemple qui agrandit une tolérance de panne, chaque volume a une allocation délimitée unique :

![unique-allocation](media/delimit-volume-allocation/unique-allocation.png)

À l’inverse, dans l’exemple suivant, les trois premiers volumes utilisent la même allocation délimitée (pour les serveurs, 1, 2 et 3) et les trois dernières volumes utilisent la même allocation délimitée (pour les serveurs, 4, 5 et 6). Cela n’optimiser la tolérance de pannes : si trois serveurs échouent, plusieurs volumes pourraient hors connexion et inaccessibles à la fois.

![non-unique-allocation](media/delimit-volume-allocation/non-unique-allocation.png)

## <a name="analysis"></a>Analyse

Cette section est dérivée de la probabilité de mathématique qu’un volume reste en ligne et accessible (ou de façon équivalente, la fraction attendue de stockage global qui reste en ligne et accessible) en tant que fonction du nombre de pannes et la taille du cluster.

   > [!NOTE]
   > Cette section est facultative lecture. Si vous êtes prêt à voir les calculs, lisez la suite ! Mais dans le cas contraire, ne vous inquiétez pas : [Utilisation de PowerShell](#usage-in-powershell) et [meilleures pratiques](#best-practices) est tout ce que vous devez implémenter allocation délimitée avec succès.

### <a name="up-to-two-failures-is-always-okay"></a>Jusqu'à deux défaillances sont toujours OK

Tous les volumes en miroir triple peuvent survivre à des échecs jusqu'à deux en même temps, en tant que [ces exemples](storage-spaces-fault-tolerance.md#examples) illustrer, quel que soit son allocation. Si deux lecteurs échouent, ou échouent de deux serveurs, ou un de chaque, chaque volume en miroir triple reste en ligne et accessible, même avec allocation régulière.

### <a name="more-than-half-the-cluster-failing-is-never-okay"></a>Plus de la moitié du basculement de cluster n’est jamais OK

À l’inverse, dans le cas extrême où plus de la moitié des serveurs ou des lecteurs dans le cluster échouent en même temps, [quorum est perdu](understand-quorum.md) et tous les volumes en miroir triple est mis hors connexion et devient inaccessible, quel que soit son allocation.

### <a name="what-about-in-between"></a>Qu’en est-il des deux ?

Si trois ou plusieurs échecs se produisent en même temps, mais au moins la moitié des serveurs et des lecteurs sont toujours opérationnelles, les volumes avec allocation délimitée peuvent rester en ligne et accessible, selon les serveurs qui ont des échecs. Nous allons exécuter les nombres pour déterminer les probabilités de précis.

Par souci de simplicité, supposons les volumes sont indépendamment et distribués de manière identique (IID) selon les meilleures pratiques ci-dessus et ce combinaisons suffisamment uniques sont disponibles pour l’allocation de chaque volume unique. La probabilité que survit à n’importe quel volume donné est également la fraction attendue de stockage global qui survit par linéarité d’attente. 

Étant donné **N** serveurs dont **F** présentent des erreurs, un volume alloué à **3** d'entre eux passe hors connexion if-et-uniquement-si tous les **3** sont parmi les  **F** avec des erreurs. Il existe **(N Choisissez F)** façons pour **F** échecs se produisent, dont **(F choisissez 3)** entraîner le volume hors connexion et de devenir inaccessible. La probabilité peut être exprimée en tant que :

![P_offline = Fc3 / NcF](media/delimit-volume-allocation/probability-volume-offline.png)

Dans tous les autres cas, le volume reste en ligne et accessible :

![P_online = 1 – (Fc3 / NcF)](media/delimit-volume-allocation/probability-volume-online.png)

Les tableaux suivants d’évaluer la probabilité pour certaines tailles de cluster courants et jusqu'à 5 échecs, révélant qu’allocation délimitée augmente la tolérance de pannes par rapport à l’allocation régulière dans tous les cas considérée comme.

### <a name="with-6-servers"></a>Avec les 6 serveurs

| Allocation                           | Probabilité de survivre à 1 Échec | Probabilité de survivre à des 2 échecs | Probabilité de survivre à 3 échecs | Probabilité de survivre à des 4 échecs | Probabilité de survivre à 5 échecs |
|--------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| Regular, réparties sur tous les 6 serveurs | 100%                               | 100%                                | 0%                                  | 0%                                  | 0%                                  |
| Limitée à 3 serveurs uniquement          | 100%                               | 100%                                | 95.0%                               | 0%                                  | 0%                                  |

   > [!NOTE]
   > Après avoir plus de 3 échecs sur 289 6 serveurs total, le cluster perd le quorum.

### <a name="with-8-servers"></a>Avec 8 serveurs

| Allocation                           | Probabilité de survivre à 1 Échec | Probabilité de survivre à des 2 échecs | Probabilité de survivre à 3 échecs | Probabilité de survivre à des 4 échecs | Probabilité de survivre à 5 échecs |
|--------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| Regular, réparties sur tous les serveurs de 8 | 100%                               | 100%                                | 0%                                  | 0%                                  | 0%                                  |
| Limitée à 3 serveurs uniquement          | 100%                               | 100%                                | 98.2%                               | 94.3%                               | 0%                                  |

   > [!NOTE]
   > Après des incidents de plus de 4 serveurs total 8, le cluster perd le quorum.

### <a name="with-12-servers"></a>Avec 12 serveurs

| Allocation                            | Probabilité de survivre à 1 Échec | Probabilité de survivre à des 2 échecs | Probabilité de survivre à 3 échecs | Probabilité de survivre à des 4 échecs | Probabilité de survivre à 5 échecs |
|---------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| Regular, réparties sur tous les serveurs de 12 | 100%                               | 100%                                | 0%                                  | 0%                                  | 0%                                  |
| Limitée à 3 serveurs uniquement           | 100%                               | 100%                                | 99.5%                               | 99.2%                               | 98.7%                               |

### <a name="with-16-servers"></a>Avec 16 serveurs

| Allocation                            | Probabilité de survivre à 1 Échec | Probabilité de survivre à des 2 échecs | Probabilité de survivre à 3 échecs | Probabilité de survivre à des 4 échecs | Probabilité de survivre à 5 échecs |
|---------------------------------------|------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|-------------------------------------|
| Regular, réparties sur tous les 16 serveurs | 100%                               | 100%                                | 0%                                  | 0%                                  | 0%                                  |
| Limitée à 3 serveurs uniquement           | 100%                               | 100%                                | 99.8%                               | 99.8%                               | 99.8%                               |

## <a name="frequently-asked-questions"></a>Forum Aux Questions

### <a name="can-i-delimit-some-volumes-but-not-others"></a>Puis-je délimiter des volumes, mais pas pour d’autres ?

Oui. Vous pouvez choisir s’il faut délimiter d’allocation par volume.

### <a name="does-delimited-allocation-change-how-drive-replacement-works"></a>Allocation délimitée modifie le fonctionne de remplacement d’un lecteur ?

Non, il est identique à avec allocation régulière.

## <a name="see-also"></a>Voir aussi

- [Vue d’ensemble Direct des espaces de stockage](storage-spaces-direct-overview.md)
- [Tolérance de pannes dans les espaces de stockage Direct](storage-spaces-fault-tolerance.md)

## <a name="appendix"></a>Annexe

Ce script vous permet de voir comment vos volumes sont alloués.

Pour l’utiliser comme décrit ci-dessus, copier/coller et enregistrer en tant que `Get-VirtualDiskFootprintBySSU.ps1`.

```PowerShell
Function ConvertTo-PrettyCapacity {
    Param (
        [Parameter(
            Mandatory = $True,
            ValueFromPipeline = $True
            )
        ]
    [Int64]$Bytes,
    [Int64]$RoundTo = 0
    )
    If ($Bytes -Gt 0) {
        $Base = 1024
        $Labels = ("bytes", "KB", "MB", "GB", "TB", "PB", "EB", "ZB", "YB")
        $Order = [Math]::Floor( [Math]::Log($Bytes, $Base) )
        $Rounded = [Math]::Round($Bytes/( [Math]::Pow($Base, $Order) ), $RoundTo)
        [String]($Rounded) + " " + $Labels[$Order]
    }
    Else {
        "0"
    }
    Return
}

Function Get-VirtualDiskFootprintByStorageFaultDomain {

    ################################################
    ### Step 1: Gather Configuration Information ###
    ################################################

    Write-Progress -Activity "Get-VirtualDiskFootprintByStorageFaultDomain" -CurrentOperation "Gathering configuration information..." -Status "Step 1/4" -PercentComplete 00

    $ErrorCannotGetCluster = "Cannot proceed because 'Get-Cluster' failed."
    $ErrorNotS2DEnabled = "Cannot proceed because the cluster is not running Storage Spaces Direct."
    $ErrorCannotGetClusterNode = "Cannot proceed because 'Get-ClusterNode' failed."
    $ErrorClusterNodeDown = "Cannot proceed because one or more cluster nodes is not Up."
    $ErrorCannotGetStoragePool = "Cannot proceed because 'Get-StoragePool' failed."
    $ErrorPhysicalDiskFaultDomainAwareness = "Cannot proceed because the storage pool is set to 'PhysicalDisk' fault domain awareness. This cmdlet only supports 'StorageScaleUnit', 'StorageChassis', or 'StorageRack' fault domain awareness."

    Try  {
        $GetCluster = Get-Cluster -ErrorAction Stop
    }
    Catch {
        throw $ErrorCannotGetCluster
    }

    If ($GetCluster.S2DEnabled -Ne 1) {
        throw $ErrorNotS2DEnabled
    }

    Try  {
        $GetClusterNode = Get-ClusterNode -ErrorAction Stop
    }
    Catch {
        throw $ErrorCannotGetClusterNode
    }

    If ($GetClusterNode | Where State -Ne Up) {
        throw $ErrorClusterNodeDown
    }

    Try {
        $GetStoragePool = Get-StoragePool -IsPrimordial $False -ErrorAction Stop
    }
    Catch {
        throw $ErrorCannotGetStoragePool
    }

    If ($GetStoragePool.FaultDomainAwarenessDefault -Eq "PhysicalDisk") {
        throw $ErrorPhysicalDiskFaultDomainAwareness
    }

    ###########################################################
    ### Step 2: Create SfdList[] and PhysicalDiskToSfdMap{} ###
    ###########################################################

    Write-Progress -Activity "Get-VirtualDiskFootprintByStorageFaultDomain" -CurrentOperation "Analyzing physical disk information..." -Status "Step 2/4" -PercentComplete 25

    $SfdList = Get-StorageFaultDomain -Type ($GetStoragePool.FaultDomainAwarenessDefault) | Sort FriendlyName # StorageScaleUnit, StorageChassis, or StorageRack

    $PhysicalDiskToSfdMap = @{} # Map of PhysicalDisk.UniqueId -> StorageFaultDomain.FriendlyName
    $SfdList | ForEach {
        $StorageFaultDomain = $_
        $_ | Get-StorageFaultDomain -Type PhysicalDisk | ForEach {
            $PhysicalDiskToSfdMap[$_.UniqueId] = $StorageFaultDomain.FriendlyName
        }
    }

    ##################################################################################################
    ### Step 3: Create VirtualDisk.FriendlyName -> { StorageFaultDomain.FriendlyName -> Size } Map ###
    ##################################################################################################

    Write-Progress -Activity "Get-VirtualDiskFootprintByStorageFaultDomain" -CurrentOperation "Analyzing virtual disk information..." -Status "Step 3/4" -PercentComplete 50

    $GetVirtualDisk = Get-VirtualDisk | Sort FriendlyName

    $VirtualDiskMap = @{}

    $GetVirtualDisk | ForEach {
        # Map of PhysicalDisk.UniqueId -> Size for THIS virtual disk
        $PhysicalDiskToSizeMap = @{}
        $_ | Get-PhysicalExtent | ForEach {
            $PhysicalDiskToSizeMap[$_.PhysicalDiskUniqueId] += $_.Size
        }
        # Map of StorageFaultDomain.FriendlyName -> Size for THIS virtual disk
        $SfdToSizeMap = @{}
        $PhysicalDiskToSizeMap.keys | ForEach {
            $SfdToSizeMap[$PhysicalDiskToSfdMap[$_]] += $PhysicalDiskToSizeMap[$_]
        }
        # Store
        $VirtualDiskMap[$_.FriendlyName] = $SfdToSizeMap
    }

    #########################
    ### Step 4: Write-Out ###
    #########################

    Write-Progress -Activity "Get-VirtualDiskFootprintByStorageFaultDomain" -CurrentOperation "Formatting output..." -Status "Step 4/4" -PercentComplete 75

    $Output = $GetVirtualDisk | ForEach {
        $Row = [PsCustomObject]@{}

        $VirtualDiskFriendlyName = $_.FriendlyName
        $Row | Add-Member -MemberType NoteProperty "VirtualDiskFriendlyName" $VirtualDiskFriendlyName

        $TotalFootprint = $_.FootprintOnPool | ConvertTo-PrettyCapacity
        $Row | Add-Member -MemberType NoteProperty "TotalFootprint" $TotalFootprint

        $SfdList | ForEach {
            $Size = $VirtualDiskMap[$VirtualDiskFriendlyName][$_.FriendlyName] | ConvertTo-PrettyCapacity
            $Row | Add-Member -MemberType NoteProperty $_.FriendlyName $Size
        }

        $Row
    }

    # Calculate width, in characters, required to Format-Table
    $RequiredWindowWidth = ("TotalFootprint").length + 1 + ("VirtualDiskFriendlyName").length + 1
    $SfdList | ForEach {
        $RequiredWindowWidth += $_.FriendlyName.Length + 1
    }

    $ActualWindowWidth = (Get-Host).UI.RawUI.WindowSize.Width

    If (!($ActualWindowWidth)) {
        # Cannot get window width, probably ISE, Format-List
        Write-Warning "Could not determine window width. For the best experience, use a Powershell window instead of ISE"
        $Output | Format-Table
    }
    ElseIf ($ActualWindowWidth -Lt $RequiredWindowWidth) {
        # Narrower window, Format-List
        Write-Warning "For the best experience, try making your PowerShell window at least $RequiredWindowWidth characters wide. Current width is $ActualWindowWidth characters."
        $Output | Format-List
    }
    Else {
        # Wider window, Format-Table
        $Output | Format-Table
    }
}

Get-VirtualDiskFootprintByStorageFaultDomain
```
