---
title: Historique des performances pour les espaces de stockage Direct
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 828a3265c9770bab0158067c4f856866d03e3d42
ms.sourcegitcommit: d31e266130b3b082372f7af4024e6089cb347d74
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/28/2018
ms.locfileid: "4239256"
---
# Historique des performances pour les espaces de stockage Direct

> S’applique à: Windows Server 2019

Historique des performances sont une nouvelle fonctionnalité qui permet aux administrateurs [d’Espaces de stockage Direct](storage-spaces-direct-overview.md) accéder facilement à calcul Historique, mémoire, réseau et mesures de stockage entre les serveurs hôtes de lecteurs, volumes, machines virtuelles et bien plus encore. Historique des performances collecte automatiquement et le stockage sur le cluster pendant un an.

   > [!IMPORTANT]
   > Cette fonctionnalité est une nouveauté de Windows Server 2019. Il n’est pas disponible dans Windows Server 2016.

## Prise en main

Historique des performances sont collectées par défaut avec les espaces de stockage Direct dans Windows Server 2019. Vous n’avez pas besoin installer, configurer ou démarrer quoi que ce soit. Une connexion Internet n’est pas obligatoire, System Center n’est pas obligatoire, et une base de données externe n’est pas obligatoire.

Pour afficher l’historique de performances de votre cluster graphiquement, utiliser [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md):

![Historique des performances dans Windows Admin Center](media/performance-history/perf-history-in-wac.png)

Pour rechercher et traiter par programmation, utilisez la nouvelle `Get-ClusterPerf` applet de commande. Voir [l’utilisation de PowerShell](#usage-in-powershell).

## Ce que sont collecté

Historique des performances sont collecté pour 7 types d’objets:

![Types d’objets](media/performance-history/types-of-object.png)

Chaque type d’objet a de nombreuses série: par exemple, `ClusterNode.Cpu.Usage` sont collectées pour chaque serveur.

Pour plus d’informations de ce qui est collecté pour chaque type d’objet et comment les interpréter, consultez ces rubriques secondaire:

| Object             | Série                                                                               |
|--------------------|--------------------------------------------------------------------------------------|
| Lecteurs             | [Ce qui est collecté pour les lecteurs](performance-history-for-drives.md)                     |
| Cartes réseau   | [Ce qui est collecté pour les cartes réseau](performance-history-for-network-adapters.md) |
| Serveurs            | [Ce que sont collecté pour les serveurs](performance-history-for-servers.md)                   |
| Disques durs virtuels | [Ce qui est collecté pour les disques durs virtuels](performance-history-for-vhds.md)           |
| Machines virtuelles   | [Ce qui est collecté pour les machines virtuelles](performance-history-for-vms.md)              |
| Volumes            | [Ce qui est collecté pour les volumes](performance-history-for-volumes.md)                   |
| Clusters           | [Ce qui est collecté pour les clusters](performance-history-for-clusters.md)                 |

Nombre de séries est agrégée entre les objets d’homologue à leur parent: par exemple, `NetAdapter.Bandwidth.Inbound` est collecté séparément pour chaque carte réseau et agrégées au serveur global; de même `ClusterNode.Cpu.Usage` sont agrégées au cluster globale; et ainsi de suite.

## Délais

Historique des performances sont stocké jusqu'à un an, avec une granularité dégressif. L’heure de la plus récente, les mesures sont disponibles pour toutes les 10 secondes. Par la suite, elles sont fusionnées intelligemment (en moyenne ou addition, selon le cas) moins précis série couvrant plus de temps. Pour le jour de la plus récent, les mesures sont disponibles toutes les cinq minutes; pour la semaine la plus récente, toutes les 15 minutes; et ainsi de suite.

Dans Windows Admin Center, vous pouvez sélectionner l’époque dans l’angle supérieur droit au-dessus du graphique.

![Délais dans Windows Admin Center](media/performance-history/timeframes-in-honolulu.png)

Dans PowerShell, utilisez le `-TimeFrame` paramètre.

Voici les délais disponibles:

| Époque   | Fréquence de mesure | Conservé pour |
|-------------|-----------------------|--------------|
| `LastHour`  | Toutes les 10 secondes         | 1heure       |
| `LastDay`   | Toutes les 5 minutes       | 25 heures     |
| `LastWeek`  | Toutes les 15 minutes      | 8 jours       |
| `LastMonth` | Toutes les heures          | 35 jours      |
| `LastYear`  | Tous les jours 1           | 400 jours     |

## Utilisation de PowerShell

Utilisez le `Get-ClusterPerformanceHistory` applet de commande pour la requête et le processus de l’historique des performances dans PowerShell.

```PowerShell
Get-ClusterPerformanceHistory
```

   > [!TIP]
   > Utilisez l’alias de **Get-ClusterPerf** pour enregistrer certaines combinaisons de touches.

### Exemple

Obtenir l’utilisation du processeur de l’ordinateur virtuel *MyVM* pour la dernière heure:

```PowerShell
Get-VM "MyVM" | Get-ClusterPerf -VMSeriesName "VM.Cpu.Usage" -TimeFrame LastHour
```

Pour obtenir des exemples plus avancés, consultez les [exemples de scripts](performance-history-scripting.md) publiée qui fournissent un code de démarrage pour rechercher les valeurs de pointe, calcul des moyennes, tracer des lignes de tendance, exécutez isolés de détection et bien plus encore.

### Spécifiez l’objet

Vous pouvez spécifier l’objet que vous souhaitez que par le pipeline. Cela fonctionne avec 7 types d’objets:

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

### Indiquez la série

Vous pouvez spécifier la série de que votre choix avec ces paramètres:


| Paramètre                 | Exemple                       | List                                                                                 |
|---------------------------|-------------------------------|--------------------------------------------------------------------------------------|
| `-PhysicalDiskSeriesName` | `"PhysicalDisk.Iops.Read"`    | [Ce qui est collecté pour les lecteurs](performance-history-for-drives.md)                     |
| `-NetAdapterSeriesName`   | `"NetAdapter.Bandwidth.Outbound"` | [Ce qui est collecté pour les cartes réseau](performance-history-for-network-adapters.md) |
| `-ClusterNodeSeriesName`  | `"ClusterNode.Cpu.Usage"`     | [Ce que sont collecté pour les serveurs](performance-history-for-servers.md)                   |
| `-VHDSeriesName`          | `"Vhd.Size.Current"`          | [Ce qui est collecté pour les disques durs virtuels](performance-history-for-vhds.md)           |
| `-VMSeriesName`           | `"Vm.Memory.Assigned"`        | [Ce qui est collecté pour les machines virtuelles](performance-history-for-vms.md)              |
| `-VolumeSeriesName`       | `"Volume.Latency.Write"`      | [Ce qui est collecté pour les volumes](performance-history-for-volumes.md)                   |
| `-ClusterSeriesName`      | `"PhysicalDisk.Size.Total"`   | [Ce qui est collecté pour les clusters](performance-history-for-clusters.md)                 |


   > [!TIP]
   > Utiliser la tabulation pour détecter série disponibles.

Si vous ne spécifiez pas, chaque série disponible pour l’objet spécifié est retourné.

### Spécifier la plage de temps

Vous pouvez spécifier le délai entre l’historique de votre choix avec le `-TimeFrame` paramètre.

   > [!TIP]
   > Utiliser la tabulation pour détecter les délais disponibles.

Si vous ne spécifiez pas, la `MostRecent` mesure est renvoyée.

## Fonctionnement

### Stockage de l’historique des performances

Peu après les espaces de stockage Direct est activée, un volume Go environ 10 nommé `ClusterPerformanceHistory` est créé et une instance du moteur de stockage Extensible (également appelée Microsoft JET) est mis en service il. Cette base de données légère stocke l’historique des performances sans intervention du administrateur ou gestion.

![Volume pour le stockage de l’historique des performances](media/performance-history/perf-history-volume.png)

Le volume est renforcé par des espaces de stockage et utilise la mise en miroir simple et double, ou la résilience en miroir triple, en fonction du nombre de nœuds du cluster. Il est de réparation après que les défaillances de disque ou serveur exactement comme tout autre volume dans les espaces de stockage Direct.

Le volume utilise ReFS, mais n’est pas partagé Volume Cluster (CSV), afin qu’il s’affiche uniquement sur le nœud propriétaire du groupe de Cluster. Outre automatiquement créé, il n’existe rien de spécial sur ce volume: vous pouvez voir tout cela, le parcourir, redimensionner ou supprimez-la (non recommandé). En cas de problème, consultez la [résolution des problèmes](#troubleshooting). 

### Collection de découverte et de données d’objet

Historique des performances détecte des objets appropriés, par exemple, les machines virtuelles, n’importe où dans le cluster automatiquement et commençant à leurs compteurs de performance de diffusion en continu. Les compteurs sont agrégées synchronisés et insérés dans la base de données. Diffusion en continu s’exécute en continu et est optimisée pour un impact minimal système.

Collection est gérée par le Service d’intégrité, qui est hautement disponible: si le nœud où il est en cours d’exécution tombe en panne, elle reprend instants ultérieurement sur un autre nœud du cluster. Historique des performances peuvent s’écouler brièvement, mais elle reprend automatiquement. Vous pouvez voir le Service d’intégrité et son nœud propriétaire en exécutant `Get-ClusterResource Health` dans PowerShell.

### Gestion des lacunes de mesure

Lorsque les mesures sont fusionnées moins précis série couvrant plus de temps, comme décrit dans les [délais](#Timeframes), les points de données manquantes sont exclues. Par exemple, si le serveur a été enfoncée pendant 30 minutes, puis exécute à 50 % du processeur pendant les 30 minutes suivantes, le `ClusterNode.Cpu.Usage` moyenne pour l’heure est correctement enregistrée en tant que 50 % (pas de 25 %).

### Extensibilité et la personnalisation

Historique des performances sont adaptée aux scripts. Utiliser PowerShell pour extraire tout historique disponible directement à partir de la base de données pour générer des rapports automatisés ou d’alerte, exporter l’historique pour la conserver, roulis vos propres visualisations, etc.. Consultez les [exemples de scripts](performance-history-scripting.md) de publiée pour un code de démarrage utile.

Il n’est pas possible de collecter l’historique pour les objets supplémentaires, délais ou série.

La fréquence de mesure et la période de rétention ne sont pas actuellement configurables.

## Démarrer ou arrêter l’historique des performances

### Comment faire pour activer cette fonctionnalité?

À moins que vous `Stop-ClusterPerformanceHistory`, l’historique des performances sont activée par défaut.

Pour réactiver, exécutez cette applet de commande PowerShell en tant qu’administrateur:

```PowerShell
Start-ClusterPerformanceHistory
```

### Comment désactiver cette fonctionnalité?

Pour arrêter la collecte de l’historique des performances, exécutez cette applet de commande PowerShell en tant qu’administrateur:

```PowerShell
Stop-ClusterPerformanceHistory
```

Pour supprimer les mesures existants, utilisez la `-DeleteHistory` indicateur:

```PowerShell
Stop-ClusterPerformanceHistory -DeleteHistory
```

   > [!TIP]
   > Pendant le déploiement initial, vous pouvez empêcher l’historique des performances de démarrage en définissant le `-CollectPerformanceHistory` paramètre de `Enable-ClusterStorageSpacesDirect` à `$False`.

## Résolution des problèmes

### L’applet de commande ne fonctionne pas

Un message d’erreur comme «*le terme «Get-ClusterPerf» n’est pas reconnu comme le nom d’une applet de commande*» signifie que la fonctionnalité n’est pas disponible ou installé. Vérifiez que vous disposez de build de Windows Server Insider Preview 17692 ou version ultérieure, que vous avez installé le Clustering de basculement, et que vous exécutez espaces de stockage Direct.

   > [!NOTE]
   > Cette fonctionnalité n’est pas disponible sur Windows Server 2016 ou version antérieure.

### Aucune donnée disponible 

Si un graphique indique «*aucune donnée disponible*» comme illustré, voici comment résoudre les problèmes:

![Aucune donnée disponible](media/performance-history/no-data-available.png)

1. Si l’objet a été qui vient d’être ajouté ou créé, attendez qu’elle soit découvert (jusqu'à 15 minutes).

2. Actualisez la page, ou attendre que la prochaine actualisation en arrière-plan (jusqu'à 30 secondes).

3. Certains objets spéciaux sont exclues de l’historique des performances: par exemple, les machines virtuelles qui ne sont pas mis en cluster et les volumes qui n’utilisent pas le système de fichiers de Volume partagé de Cluster (CSV). Consultez la rubrique secondaire pour le type d’objet, comme [l’historique des performances pour les volumes](performance-history-for-volumes.md), pour les petits caractères.

4. Si le problème persiste, ouvrez PowerShell en tant qu’administrateur et exécutez le `Get-ClusterPerf` applet de commande. L’applet de commande inclut la résolution des problèmes de logique pour identifier les problèmes courants, par exemple, si le volume ClusterPerformanceHistory est manquant et fournit des instructions de la mise à jour.

5. Si la commande à l’étape précédente ne renvoie rien, vous pouvez essayer de redémarrer le Service d’intégrité (qui recueille l’historique des performances) en exécutant `Stop-ClusterResource Health ; Start-ClusterResource Health` dans PowerShell.

## Voir également

- [Présentation des espaces de stockage direct](storage-spaces-direct-overview.md)
