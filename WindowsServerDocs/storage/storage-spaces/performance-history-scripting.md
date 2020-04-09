---
title: Écriture de scripts avec l’historique des performances de espaces de stockage direct
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 05/15/2018
ms.localizationpriority: medium
ms.openlocfilehash: 4c25ed4112035fa729ccf17792a846263ec68dfc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856172"
---
# <a name="scripting-with-powershell-and-storage-spaces-direct-performance-history"></a>Écriture de scripts avec PowerShell et l’historique des performances de espaces de stockage direct

> S’applique à : Windows Server 2019

Dans Windows Server 2019, [espaces de stockage direct](storage-spaces-direct-overview.md) enregistre et stocke de nombreux [historiques de performances](performance-history.md) pour les machines virtuelles, les serveurs, les lecteurs, les volumes, les cartes réseau, etc. L’historique des performances est facile à interroger et à traiter dans PowerShell afin que vous puissiez rapidement passer des *données brutes* aux *réponses réelles* à des questions telles que :

1. Y a-t-il des pics d’UC la semaine dernière ?
2. Tous les disques physiques présentent-ils une latence anormale ?
3. Quelles machines virtuelles consomment le plus d’IOPS de stockage maintenant ?
4. La bande passante réseau est-elle saturée ?
5. Quand ce volume manque-t-il d’espace libre ?
6. Dans le mois dernier, quelles machines virtuelles utilisaient le plus de mémoire ?

L’applet de commande `Get-ClusterPerf` est générée pour les scripts. Il accepte l’entrée d’applets de commande comme `Get-VM` ou `Get-PhysicalDisk` par le pipeline pour gérer l’Association, et vous pouvez diriger sa sortie vers des applets de commande d’utilitaire comme `Sort-Object`, `Where-Object`et `Measure-Object` pour composer rapidement des requêtes puissantes.

**Cette rubrique fournit et explique 6 exemples de scripts qui répondent aux 6 questions ci-dessus.** Ils présentent des modèles que vous pouvez appliquer pour rechercher des pics, Rechercher des moyennes, tracer des courbes de tendance, exécuter une détection de valeurs hors norme, et bien plus encore, sur divers types de données et de périodes. Ils sont fournis en tant que code de démarrage gratuit pour vous permettre de les copier, de les étendre et de les réutiliser.

   > [!NOTE]
   > Par souci de concision, les exemples de scripts omettent des choses telles que la gestion des erreurs que vous pouvez vous attendre à obtenir du code PowerShell de haute qualité. Ils sont principalement destinés à l’inspiration et à l’éducation plutôt qu’à l’utilisation de production.

## <a name="sample-1-cpu-i-see-you"></a>Exemple 1 : processeur, je vous vois !

Cet exemple utilise la série `ClusterNode.Cpu.Usage` de la plage de `LastWeek` pour afficher la valeur maximale (« limite supérieure »), minimale et l’utilisation moyenne du processeur pour chaque serveur du cluster. Il effectue également une analyse de quartile simple pour indiquer le nombre d’heures d’utilisation du processeur supérieure à 25%, 50% et 75% au cours des 8 derniers jours.

### <a name="screenshot"></a>Capture d'écran

Dans la capture d’écran ci-dessous, nous voyons que *le serveur-02* a subi un pic non expliqué la semaine dernière :

![Capture d’écran de PowerShell](media/performance-history/Show-CpuMinMaxAvg.png)

### <a name="how-it-works"></a>Fonctionnement

La sortie de `Get-ClusterPerf` pipes dans l’applet de commande intégrée `Measure-Object`, nous spécifions simplement la propriété `Value`. Avec ses indicateurs `-Maximum`, `-Minimum`et `-Average`, `Measure-Object` nous donne les trois premières colonnes presque gratuitement. Pour effectuer l’analyse du quartile, nous pouvons diriger vers `Where-Object` et compter le nombre de valeurs qui étaient `-Gt` (supérieur à) 25, 50 ou 75. La dernière étape consiste à Beautify avec `Format-Hours` et `Format-Percent` fonctions d’assistance, certainement facultatives.

### <a name="script"></a>Script

Voici le script :

```
Function Format-Hours {
    Param (
        $RawValue
    )
    # Weekly timeframe has frequency 15 minutes = 4 points per hour
    [Math]::Round($RawValue/4)
}

Function Format-Percent {
    Param (
        $RawValue
    )
    [String][Math]::Round($RawValue) + " " + "%"
}

$Output = Get-ClusterNode | ForEach-Object {
    $Data = $_ | Get-ClusterPerf -ClusterNodeSeriesName "ClusterNode.Cpu.Usage" -TimeFrame "LastWeek"

    $Measure = $Data | Measure-Object -Property Value -Minimum -Maximum -Average
    $Min = $Measure.Minimum
    $Max = $Measure.Maximum
    $Avg = $Measure.Average

    [PsCustomObject]@{
        "ClusterNode"    = $_.Name
        "MinCpuObserved" = Format-Percent $Min
        "MaxCpuObserved" = Format-Percent $Max
        "AvgCpuObserved" = Format-Percent $Avg
        "HrsOver25%"     = Format-Hours ($Data | Where-Object Value -Gt 25).Length
        "HrsOver50%"     = Format-Hours ($Data | Where-Object Value -Gt 50).Length
        "HrsOver75%"     = Format-Hours ($Data | Where-Object Value -Gt 75).Length
    }
}

$Output | Sort-Object ClusterNode | Format-Table
```

## <a name="sample-2-fire-fire-latency-outlier"></a>Exemple 2 : incendie, incendie, hors norme de latence

Cet exemple utilise la série `PhysicalDisk.Latency.Average` de la période `LastHour` pour rechercher les valeurs hors norme statistiques, définies en tant que lecteurs avec une latence moyenne horaire supérieure à +3 σ (trois écarts types) au-dessus de la moyenne de population.

   > [!IMPORTANT]
   > Par souci de concision, ce script n’implémente pas de protections contre une faible variance, ne gère pas les données manquantes partielles, ne fait pas de distinction entre le modèle et le microprogramme, etc. Veuillez faire preuve d’un bon jugement et ne vous fiez pas à ce script seul pour déterminer s’il faut remplacer un disque dur. Elle est présentée ici à titre éducatif uniquement.

### <a name="screenshot"></a>Capture d'écran

Dans la capture d’écran ci-dessous, nous voyons qu’il n’y a pas de valeurs hors norme :

![Capture d’écran de PowerShell](media/performance-history/Show-LatencyOutlierHDD.png)

### <a name="how-it-works"></a>Fonctionnement

Tout d’abord, nous excluons les lecteurs inactifs ou presque inactifs en vérifiant que `PhysicalDisk.Iops.Total` est régulièrement `-Gt 1`. Pour chaque disque dur actif, nous traitons sa période de `LastHour`, composée de 360 mesures à intervalles de 10 secondes, à `Measure-Object -Average` pour obtenir sa latence moyenne au cours de la dernière heure. Cela configure notre alimentation.

Nous mettons en œuvre la formule la plus [connue](http://www.mathsisfun.com/data/standard-deviation.html) pour trouver la `μ` moyenne et l’écart type `σ` de la population. Pour chaque disque dur actif, nous comparons sa latence moyenne à la moyenne de population et divisez par l’écart type. Nous conservons les valeurs brutes. nous pouvons donc `Sort-Object` nos résultats, mais vous pouvez utiliser des fonctions d’assistance `Format-Latency` et `Format-StandardDeviation` pour Beautify ce que nous affichons, certainement facultatif.

Si un lecteur est plus grand que +3 σ, nous `Write-Host` en rouge ; dans le cas contraire, en vert.

### <a name="script"></a>Script

Voici le script :

```
Function Format-Latency {
    Param (
        $RawValue
    )
    $i = 0 ; $Labels = ("s", "ms", "μs", "ns") # Petabits, just in case!
    Do { $RawValue *= 1000 ; $i++ } While ( $RawValue -Lt 1 )
    # Return
    [String][Math]::Round($RawValue, 2) + " " + $Labels[$i]
}

Function Format-StandardDeviation {
    Param (
        $RawValue
    )
    If ($RawValue -Gt 0) {
        $Sign = "+"
    }
    Else {
        $Sign = "-"
    }
    # Return
    $Sign + [String][Math]::Round([Math]::Abs($RawValue), 2) + "σ"
}

$HDD = Get-StorageSubSystem Cluster* | Get-PhysicalDisk | Where-Object MediaType -Eq HDD

$Output = $HDD | ForEach-Object {

    $Iops = $_ | Get-ClusterPerf -PhysicalDiskSeriesName "PhysicalDisk.Iops.Total" -TimeFrame "LastHour"
    $AvgIops = ($Iops | Measure-Object -Property Value -Average).Average

    If ($AvgIops -Gt 1) { # Exclude idle or nearly idle drives

        $Latency = $_ | Get-ClusterPerf -PhysicalDiskSeriesName "PhysicalDisk.Latency.Average" -TimeFrame "LastHour"
        $AvgLatency = ($Latency | Measure-Object -Property Value -Average).Average

        [PsCustomObject]@{
            "FriendlyName"  = $_.FriendlyName
            "SerialNumber"  = $_.SerialNumber
            "MediaType"     = $_.MediaType
            "AvgLatencyPopulation" = $null # Set below
            "AvgLatencyThisHDD"    = Format-Latency $AvgLatency
            "RawAvgLatencyThisHDD" = $AvgLatency
            "Deviation"            = $null # Set below
            "RawDeviation"         = $null # Set below
        }
    }
}

If ($Output.Length -Ge 3) { # Minimum population requirement

    # Find mean μ and standard deviation σ
    $μ = ($Output | Measure-Object -Property RawAvgLatencyThisHDD -Average).Average
    $d = $Output | ForEach-Object { ($_.RawAvgLatencyThisHDD - $μ) * ($_.RawAvgLatencyThisHDD - $μ) }
    $σ = [Math]::Sqrt(($d | Measure-Object -Sum).Sum / $Output.Length)

    $FoundOutlier = $False

    $Output | ForEach-Object {
        $Deviation = ($_.RawAvgLatencyThisHDD - $μ) / $σ
        $_.AvgLatencyPopulation = Format-Latency $μ
        $_.Deviation = Format-StandardDeviation $Deviation
        $_.RawDeviation = $Deviation
        # If distribution is Normal, expect >99% within 3σ
        If ($Deviation -Gt 3) {
            $FoundOutlier = $True
        }
    }

    If ($FoundOutlier) {
        Write-Host -BackgroundColor Black -ForegroundColor Red "Oh no! There's an HDD significantly slower than the others."
    }
    Else {
        Write-Host -BackgroundColor Black -ForegroundColor Green "Good news! No outlier found."
    }

    $Output | Sort-Object RawDeviation -Descending | Format-Table FriendlyName, SerialNumber, MediaType, AvgLatencyPopulation, AvgLatencyThisHDD, Deviation

}
Else {
    Write-Warning "There aren't enough active drives to look for outliers right now."
}
```

## <a name="sample-3-noisy-neighbor-thats-write"></a>Exemple 3 : voisin bruyant ? C’est l’écriture !

L’historique des performances peut également répondre à des questions sur *le moment*. De nouvelles mesures sont disponibles en temps réel, toutes les 10 secondes. Cet exemple utilise la série `VHD.Iops.Total` de la période de `MostRecent` pour identifier les machines virtuelles les plus occupées (certaines peuvent indiquer « noisiest ») qui consomment le plus d’e/s par seconde de stockage sur chaque hôte du cluster, et indiquent la répartition en lecture/écriture de leur activité.

### <a name="screenshot"></a>Capture d'écran

Dans la capture d’écran ci-dessous, nous voyons les 10 principales machines virtuelles par activité de stockage :

![Capture d’écran de PowerShell](media/performance-history/Show-TopIopsVMs.png)

### <a name="how-it-works"></a>Fonctionnement

Contrairement à `Get-PhysicalDisk`, l’applet de commande `Get-VM` ne prend pas en charge les clusters, mais retourne uniquement les machines virtuelles sur le serveur local. Pour effectuer une requête à partir de chaque serveur en parallèle, nous encapsulerons notre appel dans `Invoke-Command (Get-ClusterNode).Name { ... }`. Pour chaque machine virtuelle, nous obtenons les mesures `VHD.Iops.Total`, `VHD.Iops.Read`et `VHD.Iops.Write`. Si vous ne spécifiez pas le paramètre `-TimeFrame`, nous obtenons le point de données unique `MostRecent` pour chacun.

   > [!TIP]
   > Ces séries reflètent la somme de l’activité de cette machine virtuelle pour tous ses fichiers VHD/VHDX. Voici un exemple dans lequel l’historique des performances est automatiquement agrégé pour nous. Pour connaître la répartition par VHD/VHDX, vous pouvez diriger un `Get-VHD` individuel vers `Get-ClusterPerf` au lieu de la machine virtuelle.

Les résultats de chaque serveur sont regroupés en tant que `$Output`, que nous pouvons `Sort-Object` puis `Select-Object -First 10`. Notez que `Invoke-Command` décore les résultats avec une propriété `PsComputerName` indiquant d’où ils proviennent, que nous pouvons imprimer pour savoir où la machine virtuelle est en cours d’exécution.

### <a name="script"></a>Script

Voici le script :

```
$Output = Invoke-Command (Get-ClusterNode).Name {
    Function Format-Iops {
        Param (
            $RawValue
        )
        $i = 0 ; $Labels = (" ", "K", "M", "B", "T") # Thousands, millions, billions, trillions...
        Do { if($RawValue -Gt 1000){$RawValue /= 1000 ; $i++ } } While ( $RawValue -Gt 1000 )
        # Return
        [String][Math]::Round($RawValue) + " " + $Labels[$i]
    }

    Get-VM | ForEach-Object {
        $IopsTotal = $_ | Get-ClusterPerf -VMSeriesName "VHD.Iops.Total"
        $IopsRead  = $_ | Get-ClusterPerf -VMSeriesName "VHD.Iops.Read"
        $IopsWrite = $_ | Get-ClusterPerf -VMSeriesName "VHD.Iops.Write"
        [PsCustomObject]@{
            "VM" = $_.Name
            "IopsTotal" = Format-Iops $IopsTotal.Value
            "IopsRead"  = Format-Iops $IopsRead.Value
            "IopsWrite" = Format-Iops $IopsWrite.Value
            "RawIopsTotal" = $IopsTotal.Value # For sorting...
        }
    }
}

$Output | Sort-Object RawIopsTotal -Descending | Select-Object -First 10 | Format-Table PsComputerName, VM, IopsTotal, IopsRead, IopsWrite
```

## <a name="sample-4-as-they-say-25-gig-is-the-new-10-gig"></a>Exemple 4 : comme on dit, « 25 Go est le nouveau 10 Go »

Cet exemple utilise la série `NetAdapter.Bandwidth.Total` de la période `LastDay` pour rechercher des signes de saturation du réseau, définis comme > 90% de la bande passante maximale théorique. Pour chaque carte réseau du cluster, elle compare l’utilisation de la bande passante la plus élevée observée au cours du dernier jour à sa vitesse de liaison indiquée.

### <a name="screenshot"></a>Capture d'écran

Dans la capture d’écran ci-dessous, nous voyons qu’un *#2 Fabrikam NX-4 Pro* a atteint le dernier jour :

![Capture d’écran de PowerShell](media/performance-history/Show-NetworkSaturation.png)

### <a name="how-it-works"></a>Fonctionnement

Nous répétons notre astuce de `Invoke-Command` ci-dessus pour `Get-NetAdapter` sur chaque serveur et canal dans `Get-ClusterPerf`. En cours de route, nous prenons deux propriétés pertinentes : sa `LinkSpeed` chaîne comme « 10 Gbit/s » et son entier `Speed` brut comme 10 milliards. Nous utilisons `Measure-Object` pour obtenir la moyenne et le pic du dernier jour (rappel : chaque mesure de la plage de `LastDay` représente 5 minutes) et elle est multipliée par 8 bits par octet pour obtenir une comparaison entre les pommes et les pommes.

   > [!NOTE]
   > Certains fournisseurs, comme Chelsio, incluent une activité d’accès direct à la mémoire à distance (RDMA) dans leurs compteurs de performances de *carte réseau* . il est donc inclus dans la série `NetAdapter.Bandwidth.Total`. D’autres, comme Mellanox, peuvent ne pas l’être. Si votre fournisseur ne le fait pas, ajoutez simplement la série `NetAdapter.Bandwidth.RDMA.Total` dans votre version de ce script.

### <a name="script"></a>Script

Voici le script :

```
$Output = Invoke-Command (Get-ClusterNode).Name {

    Function Format-BitsPerSec {
        Param (
            $RawValue
        )
        $i = 0 ; $Labels = ("bps", "kbps", "Mbps", "Gbps", "Tbps", "Pbps") # Petabits, just in case!
        Do { $RawValue /= 1000 ; $i++ } While ( $RawValue -Gt 1000 )
        # Return
        [String][Math]::Round($RawValue) + " " + $Labels[$i]
    }

    Get-NetAdapter | ForEach-Object {

        $Inbound = $_ | Get-ClusterPerf -NetAdapterSeriesName "NetAdapter.Bandwidth.Inbound" -TimeFrame "LastDay"
        $Outbound = $_ | Get-ClusterPerf -NetAdapterSeriesName "NetAdapter.Bandwidth.Outbound" -TimeFrame "LastDay"

        If ($Inbound -Or $Outbound) {

            $InterfaceDescription = $_.InterfaceDescription
            $LinkSpeed = $_.LinkSpeed
    
            $MeasureInbound = $Inbound | Measure-Object -Property Value -Maximum
            $MaxInbound = $MeasureInbound.Maximum * 8 # Multiply to bits/sec
    
            $MeasureOutbound = $Outbound | Measure-Object -Property Value -Maximum
            $MaxOutbound = $MeasureOutbound.Maximum * 8 # Multiply to bits/sec
    
            $Saturated = $False
    
            # Speed property is Int, e.g. 10000000000
            If (($MaxInbound -Gt (0.90 * $_.Speed)) -Or ($MaxOutbound -Gt (0.90 * $_.Speed))) {
                $Saturated = $True
                Write-Warning "In the last day, adapter '$InterfaceDescription' on server '$Env:ComputerName' exceeded 90% of its '$LinkSpeed' theoretical maximum bandwidth. In general, network saturation leads to higher latency and diminished reliability. Not good!"
            }
    
            [PsCustomObject]@{
                "NetAdapter"  = $InterfaceDescription
                "LinkSpeed"   = $LinkSpeed
                "MaxInbound"  = Format-BitsPerSec $MaxInbound
                "MaxOutbound" = Format-BitsPerSec $MaxOutbound
                "Saturated"   = $Saturated
            }
        }
    }
}

$Output | Sort-Object PsComputerName, InterfaceDescription | Format-Table PsComputerName, NetAdapter, LinkSpeed, MaxInbound, MaxOutbound, Saturated
```

## <a name="sample-5-make-storage-trendy-again"></a>Exemple 5 : réactiver le stockage.

Pour examiner les tendances des macros, l’historique des performances est conservé jusqu’à 1 an. Cet exemple utilise la série `Volume.Size.Available` de la période de `LastYear` pour déterminer le taux de remplissage du stockage et estimer le moment où il sera plein.

### <a name="screenshot"></a>Capture d'écran

Dans la capture d’écran ci-dessous, nous voyons que le volume de *sauvegarde* ajoute environ 15 Go par jour :

![Capture d’écran de PowerShell](media/performance-history/Show-StorageTrend.png)

À ce rythme, il atteindra sa capacité dans un autre délai de 42 jours.

### <a name="how-it-works"></a>Fonctionnement

La période de `LastYear` a un point de données par jour. Bien que vous ayez uniquement besoin de deux points pour s’adapter à une courbe de tendance, en pratique, il est préférable de demander plus d’informations, comme 14 jours. Nous utilisons `Select-Object -Last 14` pour configurer un tableau de points *(x, y)* pour *x* dans la plage [1, 14]. Avec ces points, nous implémentons l' [algorithme simple Linearly Linearly](http://mathworld.wolfram.com/LeastSquaresFitting.html) pour rechercher des `$A` et des `$B` qui paramétrent la ligne de la mieux adaptée *y = ax + b*. Bienvenue dans le lycée.

La Division de la propriété `SizeRemaining` du volume par la tendance (la pente `$A`) nous permet d’estimer de manière brute le nombre de jours, au taux actuel de croissance du stockage, jusqu’à ce que le volume soit plein. Les fonctions d’assistance `Format-Bytes`, `Format-Trend`et `Format-Days` Beautify la sortie.

   > [!IMPORTANT]
   > Cette estimation est linéaire et est basée uniquement sur les 14 dernières mesures quotidiennes. Des techniques plus sophistiquées et plus précises existent. N’hésitez pas à nous faire un jugement et ne vous fiez pas à ce script seul pour déterminer s’il faut investir dans le développement de votre stockage. Elle est présentée ici à titre éducatif uniquement.

### <a name="script"></a>Script

Voici le script :

```

Function Format-Bytes {
    Param (
        $RawValue
    )
    $i = 0 ; $Labels = ("B", "KB", "MB", "GB", "TB", "PB", "EB", "ZB", "YB")
    Do { $RawValue /= 1024 ; $i++ } While ( $RawValue -Gt 1024 )
    # Return
    [String][Math]::Round($RawValue) + " " + $Labels[$i]
}

Function Format-Trend {
    Param (
        $RawValue
    )
    If ($RawValue -Eq 0) {
        "0"
    }
    Else {
        If ($RawValue -Gt 0) {
            $Sign = "+"
        }
        Else {
            $Sign = "-"
        }
        # Return
        $Sign + $(Format-Bytes [Math]::Abs($RawValue)) + "/day"
    }
}

Function Format-Days {
    Param (
        $RawValue
    )
    [Math]::Round($RawValue)
}

$CSV = Get-Volume | Where-Object FileSystem -Like "*CSV*"

$Output = $CSV | ForEach-Object {

    $N = 14 # Require 14 days of history

    $Data = $_ | Get-ClusterPerf -VolumeSeriesName "Volume.Size.Available" -TimeFrame "LastYear" | Sort-Object Time | Select-Object -Last $N

    If ($Data.Length -Ge $N) {

        # Last N days as (x, y) points
        $PointsXY = @()
        1..$N | ForEach-Object {
            $PointsXY += [PsCustomObject]@{ "X" = $_ ; "Y" = $Data[$_-1].Value }
        }

        # Linear (y = ax + b) least squares algorithm
        $MeanX = ($PointsXY | Measure-Object -Property X -Average).Average
        $MeanY = ($PointsXY | Measure-Object -Property Y -Average).Average
        $XX = $PointsXY | ForEach-Object { $_.X * $_.X }
        $XY = $PointsXY | ForEach-Object { $_.X * $_.Y }
        $SSXX = ($XX | Measure-Object -Sum).Sum - $N * $MeanX * $MeanX
        $SSXY = ($XY | Measure-Object -Sum).Sum - $N * $MeanX * $MeanY
        $A = ($SSXY / $SSXX)
        $B = ($MeanY - $A * $MeanX)
        $RawTrend = -$A # Flip to get daily increase in Used (vs decrease in Remaining)
        $Trend = Format-Trend $RawTrend

        If ($RawTrend -Gt 0) {
            $DaysToFull = Format-Days ($_.SizeRemaining / $RawTrend)
        }
        Else {
            $DaysToFull = "-"
        }
    }
    Else {
        $Trend = "InsufficientHistory"
        $DaysToFull = "-"
    }

    [PsCustomObject]@{
        "Volume"     = $_.FileSystemLabel
        "Size"       = Format-Bytes ($_.Size)
        "Used"       = Format-Bytes ($_.Size - $_.SizeRemaining)
        "Trend"      = $Trend
        "DaysToFull" = $DaysToFull
    }
}

$Output | Format-Table
```

## <a name="sample-6-memory-hog-you-can-run-but-you-cant-hide"></a>Exemple 6 : gourmande en mémoire, vous pouvez exécuter, mais vous ne pouvez pas masquer

Étant donné que l’historique des performances est collecté et stocké de manière centralisée pour l’ensemble du cluster, vous n’avez jamais besoin de regrouper les données provenant de différents ordinateurs, quel que soit le nombre de déplacements des machines virtuelles entre les ordinateurs hôtes. Cet exemple utilise la série `VM.Memory.Assigned` de la période de `LastMonth` pour identifier les machines virtuelles consomment la plus grande partie de la mémoire au cours des 35 derniers jours.

### <a name="screenshot"></a>Capture d'écran

Dans la capture d’écran ci-dessous, nous voyons les 10 premières machines virtuelles par utilisation de la mémoire le mois dernier :

![Capture d’écran de PowerShell](media/performance-history/Show-TopMemoryVMs.png)

### <a name="how-it-works"></a>Fonctionnement

Nous répétons notre Astuce `Invoke-Command`, présentée ci-dessus, pour `Get-VM` sur chaque serveur. Nous utilisons `Measure-Object -Average` pour obtenir la moyenne mensuelle pour chaque machine virtuelle, puis `Sort-Object` suivi par `Select-Object -First 10` pour obtenir notre classement. (Ou il s’agit peut-être de *la liste la plus souhaitée* ?)

### <a name="script"></a>Script

Voici le script :

```
$Output = Invoke-Command (Get-ClusterNode).Name {
    Function Format-Bytes {
        Param (
            $RawValue
        )
        $i = 0 ; $Labels = ("B", "KB", "MB", "GB", "TB", "PB", "EB", "ZB", "YB")
        Do { if( $RawValue -Gt 1024 ){ $RawValue /= 1024 ; $i++ } } While ( $RawValue -Gt 1024 )
        # Return
        [String][Math]::Round($RawValue) + " " + $Labels[$i]
    }
    
    Get-VM | ForEach-Object {
        $Data = $_ | Get-ClusterPerf -VMSeriesName "VM.Memory.Assigned" -TimeFrame "LastMonth"
        If ($Data) {
            $AvgMemoryUsage = ($Data | Measure-Object -Property Value -Average).Average
            [PsCustomObject]@{
                "VM" = $_.Name
                "AvgMemoryUsage" = Format-Bytes $AvgMemoryUsage.Value
                "RawAvgMemoryUsage" = $AvgMemoryUsage.Value # For sorting...
            }
        }
    }
}

$Output | Sort-Object RawAvgMemoryUsage -Descending | Select-Object -First 10 | Format-Table PsComputerName, VM, AvgMemoryUsage
```

C’est tout ! Nous espérons que ces exemples vous inspireront et vous aideront à commencer. Avec espaces de stockage direct historique des performances et l’applet de commande puissante et conviviale de script `Get-ClusterPerf`, vous êtes habilité à demander et à répondre ! : questions complexes lorsque vous gérez et surveillez votre infrastructure Windows Server 2019.

## <a name="see-also"></a>Voir aussi

- [Prise en main de Windows PowerShell](https://docs.microsoft.com/powershell/scripting/getting-started/getting-started-with-windows-powershell)
- [Présentation de espaces de stockage direct](storage-spaces-direct-overview.md)
- [Historique des performances](performance-history.md)
