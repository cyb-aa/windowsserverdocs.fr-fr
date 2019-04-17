---
title: Écriture de scripts avec des espaces de stockage Direct historique des performances
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 05/15/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: cc8ebcaaf7cc39cfadb0ebcec71ed573b436b466
ms.sourcegitcommit: 78ecb64cac789751abf9fd3f55b4a1fcbbe4dad2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/10/2018
ms.locfileid: "8843107"
---
# Écriture de scripts avec PowerShell et les espaces de stockage Direct historique de performances

> S’applique à: Windows Server Insider Preview-build 17692 version ultérieure

Dans Windows Server 2019, [Espaces de stockage Direct](storage-spaces-direct-overview.md) enregistre et stocke l' [historique des performances](performance-history.md) a été étendu pour les machines virtuelles, des serveurs, lecteurs, des volumes, cartes réseau et bien plus encore. Historique des performances est facile de requête et de processus dans PowerShell afin que vous pouvez accéder rapidement à partir des *données brutes* à *réelles réponses* aux questions telles que:

1. Y a-t-il eu toute UC dernière semaine?
2. N’importe quel disque physique est présentant une latence anormale?
3. Les machines virtuelles consomment le plus de stockage par seconde maintenant?
4. Mon la bande passante réseau est saturée?
5. Lorsque ce volume s’exécute en dehors de l’espace libre?
6. Dans au cours du mois, les machines virtuelles utilisé le plus de mémoire?

Le `Get-ClusterPerf` applet de commande est conçu pour l’écriture de scripts. Il accepte les entrées à partir d’applets de commande comme `Get-VM` ou `Get-PhysicalDisk` par le pipeline à gérer l’association et vous pouvez rediriger sa sortie dans l’utilitaire des applets de commande comme `Sort-Object`, `Where-Object`, et `Measure-Object` pour composer rapidement des requêtes puissantes.

**Cette rubrique fournit et explique 6 exemples de scripts qui répondent aux 6 questions ci-dessus.** Ils présentent des modèles que vous pouvez appliquer pour trouver des pics, rechercher moyennes, tracer des courbes de tendance, exécuter isolés de détection et bien plus encore, sur une variété de données et les délais. Ils sont fournis en tant que code de démarrage gratuit vous permettant de copier, étendre et réutiliser.

   > [!NOTE]
   > Par souci de concision, les exemples de scripts omettre des éléments tels que la gestion des erreurs susceptibles de code PowerShell de haute qualité. Elles visent principalement d’inspiration et d’éducation au lieu de production utiliser.

## Exemple 1: Configuration requise du processeur, je vous voir!

Cet exemple utilise le `ClusterNode.Cpu.Usage` série à partir de la `LastWeek` époque pour afficher le maximum («seuil supérieur»), le minimum et moyenne d’utilisation du processeur pour chaque serveur du cluster. Ce nuanceur effectue également une analyse quartiles simple pour afficher le nombre heures UC utilisation était plus 25 %, 50 % et 75 % au cours des 8 derniers jours.

### Screenshot

Dans la capture d’écran ci-dessous, nous voyons que *Server-02* avait une pointe inexpliquée dernière semaine:

![Capture d’écran de PowerShell](media/performance-history/Show-CpuMinMaxAvg.png)

### Fonctionnement

La sortie de `Get-ClusterPerf` canaux harmonieusement dans intégrés `Measure-Object` applet de commande, il suffit de spécifier le `Value` propriété. Avec son `-Maximum`, `-Minimum`, et `-Average` indicateurs, `Measure-Object` nous obtenons les trois premières colonnes presque pour libre. Pour effectuer l’analyse quartiles, nous pouvons diriger vers `Where-Object` et comptez le nombre de valeurs ont été `-Gt` (supérieur à) 25, 50 ou 75. La dernière étape consiste à beautify avec `Format-Hours` et `Format-Percent` fonctions d’assistance – certainement facultatif.

### Script

Voici le script:

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

## Exemple 2: TIR, tir, isolés de latence

Cet exemple utilise le `PhysicalDisk.Latency.Average` série à partir de la `LastHour` époque pour rechercher des valeurs extrêmes statistiques, défini en tant que lecteurs avec une latence moyenne de toutes les heures dépassant + 3σ (trois écarts) au-dessus de la moyenne de population.

   > [!IMPORTANT]
   > Par souci de concision, ce script n’implémente pas de protection contre l’écart faible, ne gère pas les données manquantes partielles, ne fait pas de distinction par modèle ou du microprogramme, etc.. Veuillez exercer une bonne appréciation et ne comptez pas sur ce script seul pour déterminer si vous souhaitez remplacer un disque dur. Elle est présentée ici uniquement à des fins éducatives.

### Screenshot

Dans la capture d’écran ci-dessous, nous voyons qu'il n’y a aucune valeurs extrêmes:

![Capture d’écran de PowerShell](media/performance-history/Show-LatencyOutlierHDD.png)

### Fonctionnement

Tout d’abord, nous exclure les lecteurs inactifs ou presque inactifs en vérifiant que `PhysicalDisk.Iops.Total` est constamment `-Gt 1`. Pour chaque HDD active, nous diriger son `LastHour` époque, composée de 360 mesures à toutes les 10 secondes, `Measure-Object -Average` pour obtenir sa latence moyenne de la dernière heure. Cela configure notre population.

Nous implémentons la [formule largement connu](http://www.mathsisfun.com/data/standard-deviation.html) pour trouver la moyenne `μ` et l’écart `σ` de la population. Pour chaque HDD active, nous comparer sa latence moyenne à la moyenne de population, diviser par l’écart. Nous conserver les valeurs brutes, afin que nous `Sort-Object` nos résultats, mais utilisez `Format-Latency` et `Format-StandardDeviation` fonctions d’assistance pour beautify ce que nous allons montrer – certainement facultatifs.

Si tout lecteur n’est plus de + 3σ, nous `Write-Host` en rouge; non, en vert.

### Script

Voici le script:

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

## Exemple 3: Voisinage? C’est écriture!

Historique des performances peuvent répondre aux questions sur l' *instant*, trop. Nouvelles dimensions sont disponibles en temps réel, toutes les 10 secondes. Cet exemple utilise le `VHD.Iops.Total` série à partir de la `MostRecent` époque pour identifier la plus chargée (certaines peuvent dire «plus bruyants») des machines virtuelles consomme le plus de stockage par seconde, sur tous les hôtes dans le cluster et afficher la répartition en lecture/écriture de leur activité.

### Screenshot

Dans la capture d’écran ci-dessous, nous voyons les machines virtuelles de 10 premiers par l’activité de stockage:

![Capture d’écran de PowerShell](media/performance-history/Show-TopIopsVMs.png)

### Fonctionnement

Contrairement aux `Get-PhysicalDisk`, la `Get-VM` applet de commande n’est pas adaptée aux clusters – elle retourne uniquement les ordinateurs virtuels sur le serveur local. Pour exécuter une requête à chaque serveur en parallèle, nous allons encapsuler notre appel dans `Invoke-Command (Get-ClusterNode).Name { ... }`. Pour chaque ordinateur virtuel, nous obtenons le `VHD.Iops.Total`, `VHD.Iops.Read`, et `VHD.Iops.Write` mesures. En spécifiant ne pas le `-TimeFrame` paramètre, nous obtenons le `MostRecent` avec un seul point de données pour chacune.

   > [!TIP]
   > Ces séries correspondent à la somme de tous ses fichiers VHD/VHDX l’activité de cet ordinateur virtuel. Il s’agit d’un exemple où l’historique des performances sont automatiquement agréger pour nous. Pour obtenir la répartition des VHD/VHDX, vous pouvez diriger un individu `Get-VHD` dans `Get-ClusterPerf` au lieu de la machine virtuelle.

Les résultats à partir de chaque serveur sont regroupées en tant que `$Output`, ce qui nous pouvons `Sort-Object` , puis `Select-Object -First 10`. Notez que `Invoke-Command` décore résultats avec un `PsComputerName` propriété indiquant leur provenance, ce qui nous pouvons imprimer de savoir où l’ordinateur virtuel est en cours d’exécution.

### Script

Voici le script:

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

## Exemple 4: Comme on dit, «25 Go est la nouvelle Go 10»

Cet exemple utilise le `NetAdapter.Bandwidth.Total` série à partir de la `LastDay` époque pour rechercher des signes de saturation du réseau, à la soustraction > 90 % de la bande passante maximale théorique. Pour chaque carte réseau du cluster, il compare l’utilisation de la bande passante observée plus élevée dans le dernier jour de sa vitesse de liaison indiquée.

### Screenshot

Dans la capture d’écran ci-dessous, nous voyons qu' un seul *NX Fabrikam-4 Professionnel n ° 2* pointu dans le dernier jour:

![Capture d’écran de PowerShell](media/performance-history/Show-NetworkSaturation.png)

### Fonctionnement

Nous répéter notre `Invoke-Command` «truqué» ci-dessus pour `Get-NetAdapter` sur chaque serveur et une barre verticale dans `Get-ClusterPerf`. Tout au long du processus, nous prendre deux propriétés pertinentes: son `LinkSpeed` chaîne comme «10 Gbits/s» et ses brutes `Speed` entier comme 10000000000. Nous utilisons `Measure-Object` pour obtenir la moyenne et maximale à partir du jour de dernière (rappel: chaque mesure dans le `LastDay` époque représente les 5 minutes) et multiplier par 8 bits par octet pour obtenir une comparaison pommes pertinentes.

   > [!NOTE]
   > Certains fournisseurs, tels que Chelsio, incluent l’activité (RDMA) un accès direct à distance à la mémoire dans leurs compteurs de performance de *Carte réseau* , c’est pourquoi il est inclus dans le `NetAdapter.Bandwidth.Total` série. Autres, comme Mellanox, ne peuvent pas. Si votre fournisseur ne prend pas, ajoutez simplement le `NetAdapter.Bandwidth.RDMA.Total` série dans votre version de ce script.

### Script

Voici le script:

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

## Exemple 5: Redéfinir stockage trendy!

Pour consulter les tendances macro, l’historique des performances sont conservé pour jusqu'à 1 an. Cet exemple utilise le `Volume.Size.Available` série à partir de la `LastYear` époque pour déterminer le taux de stockage est saturée et l’estimation lorsqu’il sera complet.

### Screenshot

Dans la capture d’écran ci-dessous, nous voyons que le volume de *sauvegarde* ajoute environ 15 Go par jour:

![Capture d’écran de PowerShell](media/performance-history/Show-StorageTrend.png)

À cette fréquence, il atteindra sa capacité dans un autre 42 jours.

### Fonctionnement

Le `LastYear` époque dispose d’un point de données par jour. Bien que vous devez uniquement strictement deux points pour s’adapter à une ligne de tendances, dans la pratique, il est préférable de nécessitent plus d’informations, comme les 14 jours. Nous utilisons `Select-Object -Last 14` pour configurer un tableau de points *(x, y)* , pour *x* dans la plage [1, 14]. Avec ces points, nous implémentons simple [algorithme linéaire moindres carrés](http://mathworld.wolfram.com/LeastSquaresFitting.html) permet de trouver `$A` et `$B` qui paramétrer la ligne d’ajuster *y = ax + b*. Bienvenue dans l’université partout à nouveau.

Répartition du volume `SizeRemaining` propriété par la tendance (la pente `$A`) nous permet d’estimer crûment combien de jours, au taux en cours de la croissance de stockage, jusqu'à ce que le volume est plein. Le `Format-Bytes`, `Format-Trend`, et `Format-Days` fonctions d’assistance beautify la sortie.

   > [!IMPORTANT]
   > Cette estimation est linéaire et repose uniquement sur les mesures quotidiennes 14 la plus récentes. Il existe des techniques plus sophistiquées et précise. Veuillez exercer une bonne appréciation et ne comptez pas sur ce script seul pour déterminer s’il convient d’investir dans le développement de votre stockage. Elle est présentée ici uniquement à des fins éducatives.

### Script

Voici le script:

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

## Exemple 6: Gourmand de mémoire, vous pouvez exécuter, mais vous ne pouvez pas masquer

Étant donné que l’historique des performances sont collectées et stockées de manière centralisée pour l’ensemble du cluster, vous ne jamais ont besoin composer des données à partir d’ordinateurs différents, quel que soit l’autant de fois machines virtuelles déplacent entre les hôtes. Cet exemple utilise le `VM.Memory.Assigned` série à partir de la `LastMonth` époque pour identifier les machines virtuelles consomme le plus de mémoire 35 derniers jours.

### Screenshot

Dans la capture d’écran ci-dessous, nous voyons les machines virtuelles de 10 premiers par l’utilisation de la mémoire mois dernier:

![Capture d’écran de PowerShell](media/performance-history/Show-TopMemoryVMs.png)

### Fonctionnement

Nous répéter notre `Invoke-Command` astuce, introduit ci-dessus, à `Get-VM` sur chaque serveur. Nous utilisons `Measure-Object -Average` pour obtenir la moyenne mensuelle pour chaque ordinateur virtuel, puis `Sort-Object` suivie `Select-Object -First 10` pour obtenir nos classement. (Ou il est peut-être notre list? *Plus souhaitions* )

### Script

Voici le script:

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

Et voilà! Nous espérons que ces exemples inspirent vous et vous guideront. Avec l’historique des espaces de stockage Direct des performances et la puissante, adaptée aux scripts `Get-ClusterPerf` applet de commande, vous sont habilité à demander – et de réponses! – complexe questions que vous gérez et surveiller votre infrastructure de Windows Server 2019.

## Voir aussi

- [Prise en main avec Windows PowerShell](https://docs.microsoft.com/powershell/scripting/getting-started/getting-started-with-windows-powershell)
- [Présentation des espaces de stockage direct](storage-spaces-direct-overview.md)
- [Historique des performances](performance-history.md)
