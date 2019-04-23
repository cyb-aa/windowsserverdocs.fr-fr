---
title: Historique des performances pour les espaces de stockage Direct
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
Keywords: Espaces de stockage directs
ms.localizationpriority: medium
ms.openlocfilehash: 828a3265c9770bab0158067c4f856866d03e3d42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870860"
---
# <a name="performance-history-for-storage-spaces-direct"></a>Historique des performances pour les espaces de stockage Direct

> S'applique à : Windows Server 2019

Historique des performances sont une nouvelle fonctionnalité qui donne [espaces de stockage Direct](storage-spaces-direct-overview.md) aux administrateurs un accès facile aux mesures historiques de calcul, de mémoire, de réseau et de stockage entre les serveurs hôtes, disques, volumes, les machines virtuelles et bien plus encore. Historique des performances sont collectées automatiquement et stockées sur le cluster pendant un an.

   > [!IMPORTANT]
   > Cette fonctionnalité est une nouveauté dans Windows Server 2019. Il n’est pas disponible dans Windows Server 2016.

## <a name="get-started"></a>Prise en main

Historique des performances sont collectées par défaut avec les espaces de stockage Direct dans Windows Server 2019. Vous n’avez pas besoin installer, configurer ou démarrer quoi que ce soit. Une connexion Internet n’est pas obligatoire, System Center n’est pas obligatoire, et une base de données externe n’est pas obligatoire.

Pour afficher graphiquement historique des performances de votre cluster, utilisez [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md):

![Historique des performances dans Windows Admin Center](media/performance-history/perf-history-in-wac.png)

Pour interroger et traiter par programmation, utilisez la nouvelle `Get-ClusterPerf` applet de commande. Consultez [l’utilisation de PowerShell](#usage-in-powershell).

## <a name="whats-collected"></a>Les données collectées

Historique des performances sont collectées pour 7 types d’objets :

![Types d’objets](media/performance-history/types-of-object.png)

Chaque type d’objet a le nombre de séries : par exemple, `ClusterNode.Cpu.Usage` sont collectées pour chaque serveur.

Pour des informations sur les données collectées pour chaque type d’objet et comment les interpréter, consultez les sous-rubriques :

| Objet             | série                                                                               |
|--------------------|--------------------------------------------------------------------------------------|
| Lecteurs             | [Les données collectées pour les lecteurs](performance-history-for-drives.md)                     |
| Cartes réseau   | [Les données collectées pour les cartes réseau](performance-history-for-network-adapters.md) |
| Serveurs            | [Les données collectées pour les serveurs](performance-history-for-servers.md)                   |
| disques durs virtuels ; | [Les données collectées pour les disques durs virtuels](performance-history-for-vhds.md)           |
| Ordinateurs virtuels   | [Les données collectées pour les machines virtuelles](performance-history-for-vms.md)              |
| Volumes            | [Les données collectées pour les volumes](performance-history-for-volumes.md)                   |
| Clusters           | [Les données collectées pour les clusters](performance-history-for-clusters.md)                 |

Nombre de séries est regroupée par des objets de pair pour leur parent : par exemple, `NetAdapter.Bandwidth.Inbound` sont collectées pour chaque carte réseau séparément et agrégées au serveur global ; de même `ClusterNode.Cpu.Usage` est agrégé pour le cluster global ; et ainsi de suite.

## <a name="timeframes"></a>Délais

Historique des performances sont stocké pendant un an, avec une granularité dégressif. La dernière heure, des mesures sont disponibles pour toutes les dix secondes. Par la suite, elles sont fusionnées intelligemment (en faisant la moyenne ou une somme, selon le cas) en série moins granulaires qui s’étendent sur plus de temps. Pour le jour plus récente, des mesures sont disponibles toutes les cinq minutes ; pour la semaine dernière, toutes les quinze minutes ; et ainsi de suite.

Dans Windows Admin Center, vous pouvez sélectionner l’intervalle de temps dans l’angle supérieur droit au-dessus du graphique.

![Délais dans Windows Admin Center](media/performance-history/timeframes-in-honolulu.png)

Dans PowerShell, utilisez le `-TimeFrame` paramètre.

Voici les délais disponibles :

| Période   | Fréquence de mesure | Conservé pour |
|-------------|-----------------------|--------------|
| `LastHour`  | Toutes les 10 secondes         | 1 heure       |
| `LastDay`   | Toutes les 5 minutes       | 25 heures     |
| `LastWeek`  | Toutes les 15 minutes      | 8 jours       |
| `LastMonth` | Toutes les heures          | 35 jours      |
| `LastYear`  | Tous les jours           | 400 jours     |

## <a name="usage-in-powershell"></a>Utilisation dans PowerShell

Utilisez le `Get-ClusterPerformanceHistory` applet de commande à l’historique des performances de requêtes et de processus dans PowerShell.

```PowerShell
Get-ClusterPerformanceHistory
```

   > [!TIP]
   > Utilisez le **Get-ClusterPerf** alias pour enregistrer quelques séquences de touches.

### <a name="example"></a>Exemple

Obtenir l’utilisation du processeur d’ordinateur virtuel *MyVM* pendant la dernière heure :

```PowerShell
Get-VM "MyVM" | Get-ClusterPerf -VMSeriesName "VM.Cpu.Usage" -TimeFrame LastHour
```

Pour obtenir des exemples plus avancés, consultez publié [exemples de scripts](performance-history-scripting.md) qui fournissent le code de démarrage pour rechercher les valeurs de crête, de calculer des moyennes, de tracer des courbes de tendance, d’exécuter des valeurs hors norme détection et bien plus encore.

### <a name="specify-the-object"></a>Spécifiez l’objet

Vous pouvez spécifier l’objet souhaité par le pipeline. Cela fonctionne avec 7 types d’objets :

| Objet de pipeline | Exemple     |
|----------------------|-------------|
| `Get-PhysicalDisk`   | <code>Get-PhysicalDisk -SerialNumber "XYZ456" &#124; Get-ClusterPerf</code>         |
| `Get-NetAdapter`     | <code>Get-NetAdapter "Ethernet" &#124; Get-ClusterPerf</code>                       |
| `Get-ClusterNode`    | <code>Get-ClusterNode "Server123" &#124; Get-ClusterPerf</code>                     |
| `Get-VHD`            | <code>Get-VHD "C:\ClusterStorage\MyVolume\MyVHD.vhdx" &#124; Get-ClusterPerf</code> |
| `Get-VM`             | <code>Get-VM "MyVM" &#124; Get-ClusterPerf</code>                                   |
| `Get-Volume`         | <code>Get-Volume -FriendlyName "MyVolume"  &#124; Get-ClusterPerf</code>            |
| `Get-Cluster`        | <code>Get-Cluster "MyCluster" &#124; Get-ClusterPerf</code>                         |

Si vous ne spécifiez pas, l’historique des performances pour le cluster global sont retourné.

### <a name="specify-the-series"></a>Spécifiez la série

Vous pouvez spécifier la série d’avec ces paramètres :


| Paramètre                 | Exemple                       | List                                                                                 |
|---------------------------|-------------------------------|--------------------------------------------------------------------------------------|
| `-PhysicalDiskSeriesName` | `"PhysicalDisk.Iops.Read"`    | [Les données collectées pour les lecteurs](performance-history-for-drives.md)                     |
| `-NetAdapterSeriesName`   | `"NetAdapter.Bandwidth.Outbound"` | [Les données collectées pour les cartes réseau](performance-history-for-network-adapters.md) |
| `-ClusterNodeSeriesName`  | `"ClusterNode.Cpu.Usage"`     | [Les données collectées pour les serveurs](performance-history-for-servers.md)                   |
| `-VHDSeriesName`          | `"Vhd.Size.Current"`          | [Les données collectées pour les disques durs virtuels](performance-history-for-vhds.md)           |
| `-VMSeriesName`           | `"Vm.Memory.Assigned"`        | [Les données collectées pour les machines virtuelles](performance-history-for-vms.md)              |
| `-VolumeSeriesName`       | `"Volume.Latency.Write"`      | [Les données collectées pour les volumes](performance-history-for-volumes.md)                   |
| `-ClusterSeriesName`      | `"PhysicalDisk.Size.Total"`   | [Les données collectées pour les clusters](performance-history-for-clusters.md)                 |


   > [!TIP]
   > Utilisez la touche TAB pour découvrir les série disponibles.

Si vous ne spécifiez pas, chaque série disponible pour l’objet spécifié est retourné.

### <a name="specify-the-timeframe"></a>Spécifiez l’intervalle de temps

Vous pouvez spécifier la période de conservation souhaitée avec la `-TimeFrame` paramètre.

   > [!TIP]
   > Utilisez la touche TAB pour découvrir les délais disponibles.

Si vous ne spécifiez pas le `MostRecent` mesure est retournée.

## <a name="how-it-works"></a>Fonctionnement

### <a name="performance-history-storage"></a>Stockage de l’historique des performances

Peu de temps après les espaces de stockage Direct est activé, un volume de Go environ 10 nommé `ClusterPerformanceHistory` est créé et configurée une instance du moteur de stockage Extensible (également appelé Microsoft JET) il. Cette base de données léger stocke l’historique des performances sans intervention de l’administrateur ni gestion.

![Volume de stockage de l’historique des performances](media/performance-history/perf-history-volume.png)

Le volume est soutenu par des espaces de stockage et utilise simple, en miroir bidirectionnel, ou une résilience en miroir triple, selon le nombre de nœuds du cluster. Il est réparé une fois que les défaillances de disque ou serveur comme tout autre volume dans les espaces de stockage Direct.

Le volume utilise ReFS mais n’est pas partagé Volume Cluster (CSV), afin qu’il apparaisse uniquement sur le nœud propriétaire de groupe de clusters. Outre soient créés automatiquement, il n’existe rien de spécial à ce volume : vous pouvez voir, le parcourir, redimensionner ou supprimez-le (non recommandé). En cas de problème, consultez [dépannage](#troubleshooting). 

### <a name="object-discovery-and-data-collection"></a>Collection de données et de détection d’objets

Historique des performances découvre les objets pertinents, tels que des machines virtuelles, n’importe où dans le cluster automatiquement et commence la diffusion en continu leurs compteurs de performance. Les compteurs sont agrégées, synchronisés et insérés dans la base de données. Diffusion en continu s’exécute en continu et est optimisée pour l’impact sur le système minimale.

Collection est gérée par le Service de contrôle d’intégrité, qui est hautement disponible : si le nœud où il s’exécute tombe en panne, il reprendra instants ultérieurement sur un autre nœud du cluster. Historique des performances peuvent s’écouler brièvement, mais il reprendra automatiquement. Vous pouvez voir le Service d’intégrité et son nœud propriétaire en exécutant `Get-ClusterResource Health` dans PowerShell.

### <a name="handling-measurement-gaps"></a>Gestion des intervalles de mesure

Lorsque les mesures sont fusionnées dans la série moins granulaires qui s’étendent sur plus de temps, comme décrit dans [délais](#Timeframes), périodes de données manquantes sont exclus. Par exemple, si le serveur est arrêté pendant 30 minutes, puis en exécutant à 50 % du processeur au cours des 30 prochaines minutes, le `ClusterNode.Cpu.Usage` moyenne pour l’heure est correctement enregistrée en tant que 50 % (pas de 25 %).

### <a name="extensibility-and-customization"></a>Extensibilité et personnalisation

Historique des performances sont adaptée aux scripts. Utiliser PowerShell pour extraire tout historique disponible directement à partir de la base de données pour générer des rapports automatisés ou alertes, exporter l’historique en lieu sûr, de déployer vos propres visualisations, un etc. Consultez publié [exemples de scripts](performance-history-scripting.md) pour le code de démarrage utile.

Il n’est pas possible de collecter l’historique pour des objets supplémentaires, des délais ou série.

La fréquence de mesure et de la période de rétention ne sont pas actuellement configurables.

## <a name="start-or-stop-performance-history"></a>Démarrer ou arrêter l’historique des performances

### <a name="how-do-i-enable-this-feature"></a>Comment faire pour activer cette fonctionnalité ?

À moins que vous `Stop-ClusterPerformanceHistory`, l’historique des performances sont activée par défaut.

Pour réactiver, exécutez cette applet de commande PowerShell en tant qu’administrateur :

```PowerShell
Start-ClusterPerformanceHistory
```

### <a name="how-do-i-disable-this-feature"></a>Comment désactiver cette fonctionnalité ?

Pour arrêter la collecte de l’historique des performances, exécutez cette applet de commande PowerShell en tant qu’administrateur :

```PowerShell
Stop-ClusterPerformanceHistory
```

Pour supprimer des mesures existantes, utilisez le `-DeleteHistory` indicateur :

```PowerShell
Stop-ClusterPerformanceHistory -DeleteHistory
```

   > [!TIP]
   > Lors du déploiement initial, vous pouvez empêcher l’historique des performances de démarrage en définissant le `-CollectPerformanceHistory` paramètre de `Enable-ClusterStorageSpacesDirect` à `$False`.

## <a name="troubleshooting"></a>Résolution des problèmes

### <a name="the-cmdlet-doesnt-work"></a>L’applet de commande ne fonctionne pas

Un message d’erreur comme «*le terme 'Get-ClusterPerf' n’est pas reconnu comme le nom d’applet de commande*» signifie que la fonctionnalité n’est pas disponible ou n’est installée. Vérifiez que vous disposez de build de Windows Server Insider Preview 17692 ou version ultérieure, que vous avez installé le Clustering de basculement, et que vous exécutez espaces de stockage Direct.

   > [!NOTE]
   > Cette fonctionnalité n’est pas disponible sur Windows Server 2016 ou une version antérieure.

### <a name="no-data-available"></a>Aucune donnée disponible 

Si un graphique affiche «*aucune donnée disponible*» comme illustré, voici comment résoudre les problèmes :

![Aucune donnée disponible](media/performance-history/no-data-available.png)

1. Si l’objet a été récemment ajouté ou créé, attendez qu’elle soit découvert (jusqu'à 15 minutes).

2. Actualisez la page, ou attendre la prochaine actualisation en arrière-plan (jusqu'à 30 secondes).

3. Certains objets spéciaux sont exclus de l’historique des performances – par exemple, les machines virtuelles qui ne sont pas en cluster et les volumes qui n’utilisent pas le système de fichiers de Volume partagé de Cluster (CSV). Consultez la sous-rubrique pour le type d’objet, tel que [l’historique des performances pour les volumes](performance-history-for-volumes.md), pour les petits caractères.

4. Si le problème persiste, ouvrez PowerShell en tant qu’administrateur et exécutez le `Get-ClusterPerf` applet de commande. L’applet de commande inclut la résolution des problèmes de logique permettant d’identifier les problèmes courants, par exemple si le volume ClusterPerformanceHistory est manquant et fournit des instructions de mise à jour.

5. Si la commande à l’étape précédente ne retourne rien, vous pouvez essayer de redémarrer le Service de contrôle d’intégrité (qui collecte l’historique des performances) en exécutant `Stop-ClusterResource Health ; Start-ClusterResource Health` dans PowerShell.

## <a name="see-also"></a>Voir aussi

- [Vue d’ensemble Direct des espaces de stockage](storage-spaces-direct-overview.md)
