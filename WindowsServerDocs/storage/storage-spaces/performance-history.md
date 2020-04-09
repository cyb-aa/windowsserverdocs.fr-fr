---
title: Historique des performances pour espaces de stockage direct
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.openlocfilehash: ab9b6016d49725b7f25d2ad3c40bd6265ac811a9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856152"
---
# <a name="performance-history-for-storage-spaces-direct"></a>Historique des performances pour espaces de stockage direct

> S’applique à : Windows Server 2019

L’historique des performances est une nouvelle fonctionnalité qui permet aux administrateurs de [espaces de stockage direct](storage-spaces-direct-overview.md) d’accéder facilement aux mesures historiques de calcul, de mémoire, de réseau et de stockage sur les serveurs hôtes, les lecteurs, les volumes, les machines virtuelles et bien plus encore. L’historique des performances est collecté automatiquement et stocké sur le cluster pendant un an.

   > [!IMPORTANT]
   > Cette fonctionnalité est une nouveauté de Windows Server 2019. Elle n’est pas disponible dans Windows Server 2016.

## <a name="get-started"></a>Prise en main

L’historique des performances est collecté par défaut avec espaces de stockage direct dans Windows Server 2019. Vous n’avez pas besoin d’installer, de configurer ou de démarrer quoi que ce soit. Aucune connexion Internet n’est requise, System Center n’est pas requis et aucune base de données externe n’est requise.

Pour afficher l’historique des performances de votre cluster graphiquement, utilisez le [Centre d’administration Windows](../../manage/windows-admin-center/understand/windows-admin-center.md):

![Historique des performances dans le centre d’administration Windows](media/performance-history/perf-history-in-wac.png)

Pour interroger et traiter par programme, utilisez l’applet de commande New `Get-ClusterPerf`. Consultez [utilisation dans PowerShell](#usage-in-powershell).

## <a name="whats-collected"></a>Éléments collectés

L’historique des performances est collecté pour 7 types d’objets :

![Types d’objets](media/performance-history/types-of-object.png)

Chaque type d’objet possède plusieurs séries : par exemple, `ClusterNode.Cpu.Usage` est collecté pour chaque serveur.

Pour plus d’informations sur les éléments collectés pour chaque type d’objet et sur leur interprétation, reportez-vous aux sous-rubriques suivantes :

| Object             | Série                                                                               |
|--------------------|--------------------------------------------------------------------------------------|
| Lecteurs             | [Éléments collectés pour les lecteurs](performance-history-for-drives.md)                     |
| Cartes réseau   | [Éléments collectés pour les cartes réseau](performance-history-for-network-adapters.md) |
| Serveurs            | [Éléments collectés pour les serveurs](performance-history-for-servers.md)                   |
| disques durs virtuels ; | [Éléments collectés pour les disques durs virtuels](performance-history-for-vhds.md)           |
| Ordinateurs virtuels   | [Éléments collectés pour les machines virtuelles](performance-history-for-vms.md)              |
| Volumes            | [Éléments collectés pour les volumes](performance-history-for-volumes.md)                   |
| Clusters           | [Éléments collectés pour les clusters](performance-history-for-clusters.md)                 |

De nombreuses séries sont regroupées entre des objets homologues à leur parent : par exemple, `NetAdapter.Bandwidth.Inbound` est collecté séparément pour chaque carte réseau et regroupé sur le serveur global ; de même `ClusterNode.Cpu.Usage` est agrégé au cluster global ; et ainsi de suite.

## <a name="timeframes"></a>Délais

L’historique des performances est stocké pendant un an maximum, avec une granularité plus réduite. Pour l’heure la plus récente, les mesures sont disponibles toutes les dix secondes. Par la suite, elles sont fusionnées intelligemment (en calculant la moyenne ou la somme, le cas échéant) dans des séries moins granulaires qui couvrent plus de temps. Pour la journée la plus récente, les mesures sont disponibles toutes les cinq minutes. pour la semaine la plus récente, toutes les quinze minutes ; et ainsi de suite.

Dans le centre d’administration Windows, vous pouvez sélectionner le laps de temps dans l’angle supérieur droit au-dessus du graphique.

![Périodes dans le centre d’administration Windows](media/performance-history/timeframes-in-honolulu.png)

Dans PowerShell, utilisez le paramètre `-TimeFrame`.

Voici les délais disponibles :

| Période   | Fréquence de mesure | Conservé pour |
|-------------|-----------------------|--------------|
| `LastHour`  | Toutes les 10 secondes         | 1 heure       |
| `LastDay`   | Toutes les 5 minutes       | 25 heures     |
| `LastWeek`  | Toutes les 15 minutes      | 8 jours       |
| `LastMonth` | Toutes les heures          | 35 jours      |
| `LastYear`  | Tous les jours           | 400 jours     |

## <a name="usage-in-powershell"></a>Utilisation dans PowerShell

Utilisez l’applet de commande `Get-ClusterPerformanceHistory` pour interroger et traiter l’historique des performances dans PowerShell.

```PowerShell
Get-ClusterPerformanceHistory
```

   > [!TIP]
   > Utilisez l’alias **ClusterPerf** pour enregistrer des séquences de touches.

### <a name="example"></a>Exemple

Obtenir l’utilisation de l’UC de la machine virtuelle *MyVM* pendant la dernière heure :

```PowerShell
Get-VM "MyVM" | Get-ClusterPerf -VMSeriesName "VM.Cpu.Usage" -TimeFrame LastHour
```

Pour obtenir des exemples plus avancés, consultez les exemples de [scripts](performance-history-scripting.md) publiés qui fournissent un code de démarrage pour rechercher les valeurs maximales, calculer des moyennes, tracer des courbes de tendance, exécuter une détection de valeurs hors norme et bien plus encore.

### <a name="specify-the-object"></a>Spécifier l’objet

Vous pouvez spécifier l’objet souhaité par le pipeline. Cela fonctionne avec 7 types d’objets :

| Objet du pipeline | Exemple     |
|----------------------|-------------|
| `Get-PhysicalDisk`   | <code>Get-PhysicalDisk -SerialNumber "XYZ456" &#124; Get-ClusterPerf</code>         |
| `Get-NetAdapter`     | <code>Get-NetAdapter "Ethernet" &#124; Get-ClusterPerf</code>                       |
| `Get-ClusterNode`    | <code>Get-ClusterNode "Server123" &#124; Get-ClusterPerf</code>                     |
| `Get-VHD`            | <code>Get-VHD "C:\ClusterStorage\MyVolume\MyVHD.vhdx" &#124; Get-ClusterPerf</code> |
| `Get-VM`             | <code>Get-VM "MyVM" &#124; Get-ClusterPerf</code>                                   |
| `Get-Volume`         | <code>Get-Volume -FriendlyName "MyVolume"  &#124; Get-ClusterPerf</code>            |
| `Get-Cluster`        | <code>Get-Cluster "MyCluster" &#124; Get-ClusterPerf</code>                         |

Si vous ne spécifiez pas, l’historique des performances de l’ensemble du cluster est retourné.

### <a name="specify-the-series"></a>Spécifier la série

Vous pouvez spécifier la série souhaitée avec ces paramètres :


| Paramètre                 | Exemple                       | Liste                                                                                 |
|---------------------------|-------------------------------|--------------------------------------------------------------------------------------|
| `-PhysicalDiskSeriesName` | `"PhysicalDisk.Iops.Read"`    | [Éléments collectés pour les lecteurs](performance-history-for-drives.md)                     |
| `-NetAdapterSeriesName`   | `"NetAdapter.Bandwidth.Outbound"` | [Éléments collectés pour les cartes réseau](performance-history-for-network-adapters.md) |
| `-ClusterNodeSeriesName`  | `"ClusterNode.Cpu.Usage"`     | [Éléments collectés pour les serveurs](performance-history-for-servers.md)                   |
| `-VHDSeriesName`          | `"Vhd.Size.Current"`          | [Éléments collectés pour les disques durs virtuels](performance-history-for-vhds.md)           |
| `-VMSeriesName`           | `"Vm.Memory.Assigned"`        | [Éléments collectés pour les machines virtuelles](performance-history-for-vms.md)              |
| `-VolumeSeriesName`       | `"Volume.Latency.Write"`      | [Éléments collectés pour les volumes](performance-history-for-volumes.md)                   |
| `-ClusterSeriesName`      | `"PhysicalDisk.Size.Total"`   | [Éléments collectés pour les clusters](performance-history-for-clusters.md)                 |


   > [!TIP]
   > Utilisez la saisie semi-automatique via la touche Tab pour découvrir les séries disponibles.

Si vous ne spécifiez pas, toutes les séries disponibles pour l’objet spécifié sont retournées.

### <a name="specify-the-timeframe"></a>Spécifier la période

Vous pouvez spécifier le laps de temps de l’historique souhaité avec le paramètre `-TimeFrame`.

   > [!TIP]
   > Utilisez la saisie semi-automatique via la touche Tab pour détecter les périodes disponibles.

Si vous ne spécifiez pas, la mesure `MostRecent` est retournée.

## <a name="how-it-works"></a>Fonctionnement

### <a name="performance-history-storage"></a>Stockage historique des performances

Peu après l’activation de espaces de stockage direct, un volume d’environ 10 Go nommé `ClusterPerformanceHistory` est créé et une instance du moteur de stockage extensible (également appelée Microsoft JET) est configurée ici. Cette base de données légère stocke l’historique des performances sans implication ou gestion de l’administrateur.

![Volume pour le stockage de l’historique des performances](media/performance-history/perf-history-volume.png)

Le volume est stocké par des espaces de stockage et utilise soit un miroir bidirectionnel simple, soit une résilience miroir triple, en fonction du nombre de nœuds dans le cluster. Il est réparé après une défaillance du lecteur ou du serveur, comme n’importe quel autre volume dans espaces de stockage direct.

Le volume utilise ReFS, mais n’est pas Volume partagé de cluster (CSV), il n’apparaît donc que sur le nœud propriétaire du groupe de clusters. Outre la création automatique, il n’y a rien de spécial sur ce volume : vous pouvez le voir, le parcourir, le redimensionner ou le supprimer (non recommandé). En cas de problème, consultez [résolution des problèmes](#troubleshooting).

### <a name="object-discovery-and-data-collection"></a>Détection d’objets et collecte de données

L’historique des performances Découvre automatiquement les objets pertinents, tels que les machines virtuelles, n’importe où dans le cluster et commence à diffuser leurs compteurs de performances. Les compteurs sont agrégés, synchronisés et insérés dans la base de données. La diffusion en continu s’exécute en continu et est optimisée pour un impact minimal sur le système.

La collecte est gérée par le Service de contrôle d’intégrité, qui offre un haut niveau de disponibilité : si le nœud sur lequel il s’exécute tombe en panne, il reprendra les moments ultérieurs sur un autre nœud du cluster. L’historique des performances peut expirer brièvement, mais il reprendra automatiquement. Vous pouvez voir le Service de contrôle d’intégrité et son nœud propriétaire en exécutant `Get-ClusterResource Health` dans PowerShell.

### <a name="handling-measurement-gaps"></a>Gestion des écarts de mesure

Lorsque les mesures sont fusionnées dans des séries moins granulaires qui couvrent plus de temps [, comme décrit dans périodes](#timeframes), les périodes de données manquantes sont exclues. Par exemple, si le serveur est en cours d’exécution pendant 30 minutes, puis s’exécute à 50% de l’UC pendant les 30 prochaines minutes, la `ClusterNode.Cpu.Usage` moyenne pour l’heure est enregistrée correctement sous la forme 50% (non 25%).

### <a name="extensibility-and-customization"></a>Extensibilité et personnalisation

L’historique des performances est compatible avec les scripts. Utilisez PowerShell pour extraire l’historique disponible directement à partir de la base de données afin de générer des rapports ou des alertes automatisés, exporter l’historique pour la conservation, déployer vos propres visualisations, etc. Consultez les [exemples de scripts](performance-history-scripting.md) publiés pour obtenir un code de démarrage utile.

Il n’est pas possible de collecter l’historique pour d’autres objets, périodes ou séries.

La fréquence de mesure et la période de rétention ne sont pas configurables actuellement.

## <a name="start-or-stop-performance-history"></a>Démarrer ou arrêter l’historique des performances

### <a name="how-do-i-enable-this-feature"></a>Comment faire activer cette fonctionnalité ?

À moins que vous `Stop-ClusterPerformanceHistory`, l’historique des performances est activé par défaut.

Pour la réactiver, exécutez cette applet de commande PowerShell en tant qu’administrateur :

```PowerShell
Start-ClusterPerformanceHistory
```

### <a name="how-do-i-disable-this-feature"></a>Comment faire désactiver cette fonctionnalité ?

Pour arrêter la collecte de l’historique des performances, exécutez cette applet de commande PowerShell en tant qu’administrateur :

```PowerShell
Stop-ClusterPerformanceHistory
```

Pour supprimer des mesures existantes, utilisez l’indicateur `-DeleteHistory` :

```PowerShell
Stop-ClusterPerformanceHistory -DeleteHistory
```

   > [!TIP]
   > Pendant le déploiement initial, vous pouvez empêcher le démarrage de l’historique des performances en définissant le paramètre `-CollectPerformanceHistory` de `Enable-ClusterStorageSpacesDirect` sur `$False`.

## <a name="troubleshooting"></a>Résolution des problèmes

### <a name="the-cmdlet-doesnt-work"></a>L’applet de commande ne fonctionne pas

Un message d’erreur tel que «*le terme «obtenir-ClusterPerf » n’est pas reconnu comme nom d’applet de*commande» signifie que la fonctionnalité n’est pas disponible ou installée. Vérifiez que vous disposez de Windows Server Insider preview version 17692 ou ultérieure, que vous avez installé le clustering de basculement et que vous exécutez espaces de stockage direct.

   > [!NOTE]
   > Cette fonctionnalité n’est pas disponible sur Windows Server 2016 ou une version antérieure.

### <a name="no-data-available"></a>Aucune donnée disponible 

Si un graphique indique «*aucune donnée disponible*» comme illustré, voici comment résoudre les problèmes :

![Aucune donnée disponible](media/performance-history/no-data-available.png)

1. Si l’objet a été ajouté ou créé récemment, attendez qu’il soit découvert (jusqu’à 15 minutes).

2. Actualisez la page ou attendez la prochaine actualisation en arrière-plan (jusqu’à 30 secondes).

3. Certains objets spéciaux sont exclus de l’historique des performances (par exemple, les machines virtuelles qui ne sont pas en cluster) et les volumes qui n’utilisent pas le système de fichiers Volume partagé de cluster (CSV). Vérifiez la sous-rubrique relative au type d’objet, comme [historique des performances pour les volumes](performance-history-for-volumes.md), pour l’impression fine.

4. Si le problème persiste, ouvrez PowerShell en tant qu’administrateur et exécutez l’applet de commande `Get-ClusterPerf`. L’applet de commande inclut une logique de dépannage permettant d’identifier les problèmes courants, par exemple si le volume ClusterPerformanceHistory est manquant et fournit des instructions de correction.

5. Si la commande de l’étape précédente ne retourne rien, vous pouvez essayer de redémarrer le Service de contrôle d’intégrité (qui collecte l’historique des performances) en exécutant `Stop-ClusterResource Health ; Start-ClusterResource Health` dans PowerShell.

## <a name="see-also"></a>Voir aussi

- [Présentation de espaces de stockage direct](storage-spaces-direct-overview.md)
