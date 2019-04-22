---
title: Écriture de scripts avec les espaces de stockage Direct historique des performances
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 05/15/2018
Keywords: Espaces de stockage directs
ms.localizationpriority: medium
ms.openlocfilehash: cc8ebcaaf7cc39cfadb0ebcec71ed573b436b466
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816320"
---
# <a name="scripting-with-powershell-and-storage-spaces-direct-performance-history"></a>Écriture de scripts avec l’historique des performances PowerShell et espaces de stockage Direct

> S'applique à : Génération de Windows Server Insider Preview 17692 et versions ultérieure

Dans Windows Server 2019, [espaces de stockage Direct](storage-spaces-direct-overview.md) enregistrements et les magasins complètes [l’historique des performances](performance-history.md) pour les machines virtuelles, des serveurs, lecteurs, volumes, des cartes réseau et bien plus encore. Historique des performances est facile de requête et les processus dans PowerShell afin d’accéder rapidement à partir de *données brutes* à *des réponses réelles* aux questions telles que :

1. Y a-t-il eu des pics de consommation UC semaine dernière ?
2. N’importe quel disque physique est présentant une latence anormale ?
3. Auquel les machines virtuelles consomment le plus de stockage IOPS dès maintenant ?
4. Ma bande passante réseau est saturée ?
5. Lorsque ce volume manquera d’espace libre ?
6. Le mois dernier, auquel les machines virtuelles utilisées le plus de mémoire ?

Le `Get-ClusterPerf` applet de commande est généré pour le script. Il accepte l’entrée d’applets de commande comme `Get-VM` ou `Get-PhysicalDisk` par le pipeline pour gérer l’association et vous pouvez diriger sa sortie dans les applets de commande utilitaire comme `Sort-Object`, `Where-Object`, et `Measure-Object` pour composer des requêtes puissantes rapidement.

**Cette rubrique fournit et explique les 6 exemples de scripts qui répondent aux 6 questions ci-dessus.** Ils présentent des modèles que vous pouvez appliquer pour trouver des pics, trouver des moyennes, tracer des courbes de tendance, exécuter des valeurs hors norme détection, etc., toute une variété de données et les délais. Elles sont fournies en tant que code de démarrage gratuit pour vous permettre de copier, d’étendre et de réutiliser.

   > [!NOTE]
   > Par souci de concision, les exemples de scripts omettre les éléments tels que la gestion des erreurs que vous pourriez vous attendre de code PowerShell de haute qualité. Ils sont destinés principalement inspiration et de formation plutôt que de production utiliser.

## <a name="sample-1-cpu-i-see-you"></a>Exemple 1 : Processeur, je vois vous !

Cet exemple utilise le `ClusterNode.Cpu.Usage` série à partir de la `LastWeek` délai d’exécution pour afficher le maximum (« Borne haute »), l’utilisation du processeur minimale et moyenne pour chaque serveur dans le cluster. Il effectue également analysis quartile simples pour montrer combien heures UC utilisation était plus 25 %, 50 % et 75 % au cours des 8 derniers jours.

### <a name="screenshot"></a>Capture d'écran

Dans la capture d’écran ci-dessous, nous voyons que *Server-02* avait un pic inexpliqué semaine dernière :

![Capture d’écran de PowerShell](media/performance-history/Show-CpuMinMaxAvg.png)

### <a name="how-it-works"></a>Fonctionnement

La sortie de `Get-ClusterPerf` canaux parfaitement dans intégrés `Measure-Object` applet de commande, nous spécifiez simplement le `Value` propriété. Avec ses `-Maximum`, `-Minimum`, et `-Average` indicateurs, `Measure-Object` nous donne les trois premières colonnes presque pour gratuitement. Pour effectuer l’analyse de quartile, nous pouvons vous diriger vers `Where-Object` et compter le nombre de valeurs ont été `-Gt` (supérieur à) 25, 50 ou 75. La dernière étape consiste à beautify avec `Format-Hours` et `Format-Percent` fonctions d’assistance – certainement facultatif.

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

## <a name="sample-2-fire-fire-latency-outlier"></a>Exemple 2 : Incendie, les incendies, les valeurs hors norme de latence

Cet exemple utilise le `PhysicalDisk.Latency.Average` série à partir de la `LastHour` laps de temps à rechercher les valeurs hors norme statistique, défini en tant que lecteurs avec une latence moyenne de toutes les heures dépassant + la 3σ (trois écarts types) au-dessus de la moyenne de la population.

   > [!IMPORTANT]
   > Par souci de concision, ce script n’implémente pas les dispositifs de protection contre l’écart faible, ne gère pas les données manquantes partielles, ne fait pas la distinction par le modèle ou du microprogramme, etc. Veuillez exercice jugements et ne reposent pas sur ce script seul pour déterminer s’il faut remplacer un disque dur. Il est présenté ici à titre éducatif uniquement.

### <a name="screenshot"></a>Capture d'écran

Dans la capture d’écran ci-dessous, nous voyons qu'il n’y a aucune valeurs hors norme :

![Capture d’écran de PowerShell](media/performance-history/Show-LatencyOutlierHDD.png)

### <a name="how-it-works"></a>Fonctionnement

Tout d’abord, nous excluons lecteurs inactifs ou presque inactifs en vérifiant que `PhysicalDisk.Iops.Total` est régulièrement `-Gt 1`. Pour chaque disque dur actif, nous diriger son `LastHour` laps de temps, constitué de 360 mesures toutes les 10 secondes, à `Measure-Object -Average` pour obtenir sa latence moyenne de la dernière heure. Cela définit le remplissage de notre.

Nous implémentons le [répandus formule](http://www.mathsisfun.com/data/standard-deviation.html) pour trouver la moyenne `μ` et l’écart `σ` du remplissage. Pour chaque disque dur actif, nous comparer sa latence moyenne à la moyenne de la population et à diviser par l’écart. Nous conservons les valeurs brutes, afin que nous `Sort-Object` nos résultats, mais utilisez `Format-Latency` et `Format-StandardDeviation` fonctions d’assistance pour beautify ce que nous allons montrer – certainement facultatifs.

Si n’importe quel lecteur n’est plus de + 3σ, nous `Write-Host` en rouge ; sinon, en vert.

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

## <a name="sample-3-noisy-neighbor-thats-write"></a>Exemple 3 : Voisin bruyant ? C’est à écriture !

Historique des performances peuvent répondre aux questions sur *maintenant*, trop. Nouvelles mesures sont disponibles en temps réel, toutes les 10 secondes. Cet exemple utilise le `VHD.Iops.Total` séries à partir la `MostRecent` laps de temps pour identifier la plus occupée (certains peuvent contenir le message « bruyant ») des machines virtuelles consomment le plus de stockage IOPS, sur chaque hôte du cluster et afficher la répartition en lecture/écriture de leur activité.

### <a name="screenshot"></a>Capture d'écran

Dans la capture d’écran ci-dessous, nous voyons les machines virtuelles de 10 principales par activité de stockage :

![Capture d’écran de PowerShell](media/performance-history/Show-TopIopsVMs.png)

### <a name="how-it-works"></a>Fonctionnement

Contrairement aux `Get-PhysicalDisk`, le `Get-VM` applet de commande n’est pas adaptée aux clusters : elle renvoie uniquement des machines virtuelles sur le serveur local. Pour interroger à chaque serveur en parallèle, nous encapsuler notre appel dans `Invoke-Command (Get-ClusterNode).Name { ... }`. Pour chaque machine virtuelle, nous obtenons le `VHD.Iops.Total`, `VHD.Iops.Read`, et `VHD.Iops.Write` des mesures. En ne spécifiant la `-TimeFrame` paramètre, nous obtenons le `MostRecent` unique point de données pour chacun.

   > [!TIP]
   > Ces séries reflètent la somme de l’activité de cette machine virtuelle à tous ses fichiers VHD/VHDX. Il s’agit d’un exemple où l’historique des performances sont automatiquement agrégée pour nous. Pour obtenir la répartition des VHD/VHDX, vous pouvez diriger un individu `Get-VHD` dans `Get-ClusterPerf` au lieu de la machine virtuelle.

Les résultats de chaque serveur sont combinent en tant que `$Output`, ce qui nous pouvons `Sort-Object` , puis `Select-Object -First 10`. Notez que `Invoke-Command` décore résultats avec un `PsComputerName` propriété indiquant leur provenance, ce qui nous pouvons imprimer de savoir où la machine virtuelle est en cours d’exécution.

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

## <a name="sample-4-as-they-say-25-gig-is-the-new-10-gig"></a>Exemple 4 : Comme on le dit, « 25 Go est la nouvelle Go 10 »

Cet exemple utilise le `NetAdapter.Bandwidth.Total` série à partir de la `LastDay` laps de temps pour rechercher des signes de saturation du réseau, défini comme > 90 % de bande passante maximale théorique. Pour chaque carte réseau du cluster, il compare l’utilisation la plus élevée de la bande passante observées dans le dernier jour à sa vitesse de liaison indiquées.

### <a name="screenshot"></a>Capture d'écran

Dans la capture d’écran ci-dessous, nous voyons qu’un *NX de Fabrikam-4 2 Pro* a atteint un pic dans le dernier jour :

![Capture d’écran de PowerShell](media/performance-history/Show-NetworkSaturation.png)

### <a name="how-it-works"></a>Fonctionnement

Nous répétons notre `Invoke-Command` astuce ci-dessus à `Get-NetAdapter` sur chaque serveur et le canal dans `Get-ClusterPerf`. Tout au long du processus, nous extrayons les deux propriétés pertinentes : son `LinkSpeed` chaîne telle que « 10 Gbits/s » et ses brutes `Speed` entier comme 10000000000. Nous utilisons `Measure-Object` pour obtenir la moyenne et maximale du dernier jour (rappel : chaque mesure dans le `LastDay` période représente 5 minutes) et le multiplier par 8 bits par octet pour obtenir une comparaison de pommes de point à point.

   > [!NOTE]
   > Certains fournisseurs, tels que Chelsio, incluent l’activité d’accès (RDMA) de mémoire à distance directs dans leurs *carte réseau* des compteurs de performance, afin qu’il est inclus dans le `NetAdapter.Bandwidth.Total` série. Autres, telles que Mellanox, ne peuvent pas. Si votre fournisseur ne, ajoutez simplement le `NetAdapter.Bandwidth.RDMA.Total` série dans votre version de ce script.

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

## <a name="sample-5-make-storage-trendy-again"></a>Exemple 5 : Réeffectuer stockage trendy !

Pour examiner les tendances de macro, l’historique des performances sont conservées pendant 1 an. Cet exemple utilise le `Volume.Size.Available` série à partir de la `LastYear` délai d’exécution pour déterminer le taux de stockage est saturé et l’estimation quand il s’avère complète.

### <a name="screenshot"></a>Capture d'écran

Dans la capture d’écran ci-dessous, nous voyons la *sauvegarde* volume consiste à ajouter environ 15 Go par jour :

![Capture d’écran de PowerShell](media/performance-history/Show-StorageTrend.png)

Avec ce taux, il sera atteint sa capacité dans un autre 42 jours.

### <a name="how-it-works"></a>Fonctionnement

Le `LastYear` laps de temps a un point de données par jour. Bien que vous devez uniquement strictement deux points pour tenir une courbe de tendance, dans la pratique, il est préférable de nécessitent plus d’informations, comme les 14 jours. Nous utilisons `Select-Object -Last 14` pour configurer un tableau de *(x, y)* points, pour *x* dans la plage [1, 14]. Avec ces points, nous implémentons le simple [linéaire des moindres carrés algorithme](http://mathworld.wolfram.com/LeastSquaresFitting.html) trouver `$A` et `$B` qui paramétrer la ligne de mieux *y = ax + b*. Bienvenue dans lycée tout à nouveau.

Division du volume `SizeRemaining` propriété par la tendance (la pente `$A`) nous permet d’estimer grossièrement combien de jours, le taux actuel de croissance du stockage, jusqu'à ce que le volume est plein. Le `Format-Bytes`, `Format-Trend`, et `Format-Days` fonctions d’assistance beautify la sortie.

   > [!IMPORTANT]
   > Cette estimation est linéaire et repose uniquement sur les dernières mesures quotidiennes 14. Des techniques plus sophistiquées et précis existent. Veuillez exercice jugements et ne reposent pas sur ce script seul pour déterminer s’il faut investir dans le développement de votre stockage. Il est présenté ici à titre éducatif uniquement.

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

## <a name="sample-6-memory-hog-you-can-run-but-you-cant-hide"></a>Exemple 6 : Utilisation de la mémoire, vous pouvez exécuter, mais vous ne pouvez pas masquer

Étant donné que l’historique des performances sont collectées et stockées de manière centralisée pour l’ensemble du cluster, jamais, vous devez combiner les données à partir de différents ordinateurs, quel nombre de fois où les machines virtuelles déplacent entre des hôtes. Cet exemple utilise le `VM.Memory.Assigned` série à partir de la `LastMonth` laps de temps pour identifier les machines virtuelles consommant le plus de mémoire au cours des 35 derniers jours.

### <a name="screenshot"></a>Capture d'écran

Dans la capture d’écran ci-dessous, nous voyons les machines virtuelles de 10 principales par utilisation de la mémoire le mois dernier :

![Capture d’écran de PowerShell](media/performance-history/Show-TopMemoryVMs.png)

### <a name="how-it-works"></a>Fonctionnement

Nous répétons notre `Invoke-Command` astuce, introduite ci-dessus, à `Get-VM` sur chaque serveur. Nous utilisons `Measure-Object -Average` pour obtenir la moyenne mensuelle pour chaque machine virtuelle, puis `Sort-Object` suivie `Select-Object -First 10` obtenir notre classement. (Ou peut-être que c’est notre *les plus attendues* liste ?)

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

C’est tout ! Nous espérons que ces exemples inspirent vous et à commencer. Avec les espaces de stockage Direct historique des performances et le puissant, adaptée aux scripts `Get-ClusterPerf` applet de commande, vous sont habilités à – et réponses ! – complexe questions que vous gérez et surveillez votre infrastructure Windows Server 2019.

## <a name="see-also"></a>Voir aussi

- [Mise en route avec Windows PowerShell](https://docs.microsoft.com/powershell/scripting/getting-started/getting-started-with-windows-powershell)
- [Vue d’ensemble Direct des espaces de stockage](storage-spaces-direct-overview.md)
- [Historique des performances](performance-history.md)
