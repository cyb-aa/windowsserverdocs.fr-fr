---
title: Contrôle d’intégrité directe des espaces de stockage et les états opérationnels
description: Comment trouver et à comprendre les états opérationnels des espaces de stockage Direct et des espaces de stockage (y compris les disques physiques, des pools et des disques virtuels) d’intégrité et que faire à leur sujet.
keywords: Espaces de stockage, détachés, disque virtuel, le disque physique, dégradée
author: jasongerend
ms.author: jgerend
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-spaces
manager: brianlic
ms.openlocfilehash: 5090a68270438bd9a06c7d50f9d4abca066d31e6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849140"
---
# <a name="troubleshoot-storage-spaces-direct-health-and-operational-states"></a>Résoudre les problèmes d’intégrité d’espaces de stockage Direct et états opérationnels

> S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server (canal semi-annuel), Windows 10, Windows 8.1

Cette rubrique décrit l’intégrité et les états opérationnels de pools de stockage, des disques virtuels (qui se trouvent en dessous de volumes dans les espaces de stockage) et les lecteurs [espaces de stockage Direct](storage-spaces-direct-overview.md) et [espaces de stockage](overview.md). Ces États peuvent être très utiles lorsque vous tentez de résoudre différents problèmes tels que la raison pour laquelle vous ne pouvez pas supprimer un disque virtuel en raison d’une configuration en lecture seule. Il explique également pourquoi un lecteur ne peut pas être ajouté à un pool (le CannotPoolReason).

Espaces de stockage a trois objets principales - *disques physiques* (disques durs, les disques SSD, etc.) qui sont ajoutés à un *pool de stockage*, virtualiser le stockage afin que vous puissiez créer *dedisquesvirtuels* à partir d’espace libre dans le pool, comme illustré ici. Métadonnées de pool sont écrit à chaque lecteur dans le pool. Les volumes sont créés sur les disques virtuels et stocker vos fichiers, mais nous n’allons pas parler ici des volumes.

![Disques physiques sont ajoutés à un pool de stockage, et ensuite les disques virtuels créés à partir de l’espace de pool](media/storage-spaces-states/storage-spaces-object-model.png)

Vous pouvez afficher l’intégrité et les états opérationnels dans le Gestionnaire de serveur, ou avec PowerShell. Voici un exemple d’un ensemble de (principalement) fonctionnent-elles et états opérationnels sur un cluster d’espaces de stockage Direct qui n’a pas la plupart de ses nœuds de cluster (avec le bouton droit pour ajouter les en-têtes de colonne **état opérationnel**). Ce n’est pas un cluster heureux.

![Gestionnaire de serveur affichant les résultats de deux nœuds manquants dans un cluster d’espaces de stockage Direct - un grand nombre de manque de disques physiques et les disques virtuels dans un état non intègre](media/storage-spaces-states/unhealthy-disks-in-server-manager.png)

## <a name="storage-pool-states"></a>États du pool de stockage

Chaque pool de stockage a un état d’intégrité - **sain**, **avertissement**, ou **inconnu**/**défectueux**, ainsi qu’un ou plusieurs états opérationnels.

Pour trouver un pool est en état, utilisez les commandes PowerShell suivantes :

```PowerShell
Get-StoragePool -IsPrimordial $False | Select-Object HealthStatus, OperationalStatus, ReadOnlyReason
```

Voici un exemple de sortie affichant un pool de stockage dans l’état d’intégrité inconnu avec l’état de fonctionnement en lecture seule :

```
FriendlyName                OperationalStatus HealthStatus IsPrimordial IsReadOnly
------------                ----------------- ------------ ------------ ----------
S2D on StorageSpacesDirect1 Read-only         Unknown      False        True
```

Les sections suivantes répertorient l’intégrité et les états opérationnels.

### <a name="pool-health-state-healthy"></a>État d’intégrité du pool : Healthy

|État opérationnel    |Description|
|---------            |---------  |
|OK|Le pool de stockage est sain.|

### <a name="pool-health-state-warning"></a>État d’intégrité du pool : Warning

Lorsque le pool de stockage est dans le **avertissement** l’état d’intégrité, cela signifie que le pool est accessible, mais un ou plusieurs disques ont échoué ou sont manquants. Par conséquent, votre pool de stockage peut-être avoir réduites de la résilience.

|État opérationnel    |Description|
|---------            |---------  |
|Détérioré|Il existe des lecteurs ayant échoués ou manquants dans le pool de stockage. Cette condition se produit uniquement avec les lecteurs de métadonnées de pool d’hébergement. <br><br>**Action**: Vérifier l’état de vos lecteurs et remplacez tous les lecteurs ayant échoués avant la défaillance supplémentaire.|

### <a name="pool-health-state-unknown-or-unhealthy"></a>État d’intégrité du pool : Inconnu ou non intègre

Lorsqu’un pool de stockage est dans le **inconnu** ou **défectueux** l’état d’intégrité, cela signifie que le pool de stockage est en lecture seule et ne peut pas être modifié jusqu'à ce que le pool est renvoyé à la **avertissement**ou **OK** États d’intégrité.

|État opérationnel    |En lecture seule raison |Description|
|---------            |---------       |--------   |
|en lecture seule|Incomplet|Cela peut se produire si le pool de stockage perd son [quorum](understand-quorum.md), ce qui signifie que la plupart des lecteurs du pool ont échoué ou sont hors connexion pour une raison quelconque. Lorsqu’un pool perd son quorum, espaces de stockage définit automatiquement la configuration du pool en lecture seule jusqu'à ce que suffisamment de lecteurs redeviennent disponibles.<br><br>**Action :** <br>1. Reconnectez tous les lecteurs manquants et si vous utilisez des espaces de stockage Direct, mettre tous les serveurs en ligne. <br>2. Définissez le pool précédent en lecture-écriture en ouvrant une session PowerShell avec des autorisations administratives et en tapant :<br><br> <code>Get-StoragePool <PoolName> -IsPrimordial $False \| Set-StoragePool -IsReadOnly $false</code>|
||Stratégie|Un administrateur de définie le pool de stockage en lecture seule.<br><br>**Action :** Pour définir un pool de stockage en cluster en lecture-écriture accès dans le Gestionnaire de Cluster de basculement, accédez à **Pools**, cliquez sur le pool, puis sélectionnez **mettre en ligne**.<br><br>Pour d’autres serveurs et les PC, ouvrez une session PowerShell avec des autorisations administratives et tapez :<br><br><code>Get-StoragePool <PoolName> \| Set-StoragePool -IsReadOnly $false</code><br><br> |
||Start|Espaces de stockage démarre ou en attente pour les lecteurs d’être connecté dans le pool. Il s’agit d’un état temporaire. Une fois complètement démarré, le pool doit passer à un état opérationnel différent.<br><br>**Action :** Si le pool reste dans le *démarrage* l’état, assurez-vous que tous les lecteurs dans le pool sont correctement connectées.|

Voir aussi [modification d’un Pool de stockage qui possède une Configuration en lecture seule](https://social.technet.microsoft.com/wiki/contents/articles/14861.modifying-a-storage-pool-that-has-a-read-only-configuration.aspx).

## <a name="virtual-disk-states"></a>États de disque virtuel

Dans les espaces de stockage, les volumes sont placés sur des disques virtuels (espaces de stockage) qui sont issus d’espace libre dans un pool. Chaque disque virtuel a un état d’intégrité - **sain**, **avertissement**, **défectueux**, ou **inconnu** ainsi que d’un ou plusieurs états opérationnels.

Pour savoir quelles sont les disques virtuels dans, utilisez les commandes PowerShell suivantes :

```PowerShell
Get-VirtualDisk | Select-Object FriendlyName,HealthStatus, OperationalStatus, DetachedReason
```

Voici un exemple de sortie indiquant un disque virtuel détaché et un disque virtuel incomplet/détérioré :

```
FriendlyName HealthStatus OperationalStatus      DetachedReason
------------ ------------ -----------------      --------------
Volume1      Unknown      Detached               By Policy
Volume2      Warning      {Degraded, Incomplete} None
```

Les sections suivantes répertorient l’intégrité et les états opérationnels.

### <a name="virtual-disk-health-state-healthy"></a>État d’intégrité de disque virtuel : Healthy

|État opérationnel    |Description|
|---------            |---------          |
|OK    |Le disque virtuel est sain.|
|Suboptimal    |Les données n’est pas écrites uniformément sur les disques. <br><br>**Action**: Optimiser l’utilisation du disque du pool de stockage en exécutant la [Optimize-StoragePool](https://technet.microsoft.com/itpro/powershell/windows/storage/optimize-storagepool) applet de commande.|

### <a name="virtual-disk-health-state-warning"></a>État d’intégrité de disque virtuel : Warning

Lorsque le disque virtuel est dans un **avertissement** l’état d’intégrité, cela signifie qu’une ou plusieurs copies de vos données ne sont pas disponibles, mais les espaces de stockage peuvent toujours lire au moins une copie de vos données.

|État opérationnel    |Description|
|---------            |---------          |
|Dans le service            |Windows répare le disque virtuel, comme après l’ajout ou retrait d’un lecteur. Lorsque la réparation est terminée, le disque virtuel doit retourner l’état d’intégrité OK.|
|Incomplet           |La résilience du disque virtuel est réduite car un ou plusieurs disques a échoué ou sont manquantes. Toutefois, les lecteurs manquants contiennent à jour les copies de vos données.<br><br> **Action**: <br>1. Se reconnecter tous les lecteurs manquants, remplacez tous les lecteurs ayant échoués et si vous utilisez des espaces de stockage Direct, mettre en ligne tous les serveurs qui sont hors connexion. <br>2. Si vous n’utilisez pas des espaces de stockage Direct, ensuite réparer le disque virtuel à l’aide de la [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) applet de commande.<br> Espaces de stockage Direct démarre automatiquement une réparation si nécessaire après la reconnexion ou de remplacement d’un disque.|
|Détérioré             |La résilience du disque virtuel est réduite car un ou plusieurs disques a échoué ou sont manquantes, et il existe obsolètes copies de vos données sur ces lecteurs. <br><br>**Action**: <br> 1. Se reconnecter tous les lecteurs manquants, remplacez tous les lecteurs ayant échoués et si vous utilisez des espaces de stockage Direct, mettre en ligne tous les serveurs qui sont hors connexion. <br> 2. Si vous n’utilisez pas des espaces de stockage Direct, ensuite réparer le disque virtuel à l’aide de la [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) applet de commande. <br>Espaces de stockage Direct démarre automatiquement une réparation si nécessaire après la reconnexion ou de remplacement d’un disque.|

### <a name="virtual-disk-health-state-unhealthy"></a>État d’intégrité de disque virtuel : Unhealthy

Quand un disque virtuel est dans un **défectueux** état d’intégrité, certaines ou toutes les données sur le disque virtuel sont actuellement inaccessible.

|État opérationnel    |Description|
|---------            |--------   |
|Pas de redondance|Le disque virtuel a perdu des données, car trop de lecteurs a échoué.<br><br>**Action**: Remplacer les lecteurs ayant échoués, puis restaurez vos données à partir de la sauvegarde.|

### <a name="virtual-disk-health-state-informationunknown"></a>État d’intégrité de disque virtuel : Informations/inconnu

Le disque virtuel peut également être dans le **informations** état d’intégrité (comme indiqué dans l’élément du Panneau de configuration de stockage espaces) ou **inconnu** l’intégrité d’état (comme indiqué dans PowerShell) si un administrateur a eu le disque virtuel en mode hors connexion ou le disque virtuel a se détacher.

|État opérationnel    |Raison détaché |Description|
|---------            |---------       |--------   |
|Détaché             |Par la stratégie            |Un administrateur a placé le disque virtuel hors connexion, ou définie le disque virtuel à nécessitent jointe manuelle, auquel cas vous devrez attacher manuellement le disque virtuel à chaque redémarrage de Windows.,<br><br>**Action**: Mettez le disque virtuel en ligne. Pour ce faire, lorsque le disque virtuel est dans un pool de stockage en cluster, sélectionnez Gestionnaire du Cluster de basculement **stockage** > **Pools** > **disques virtuels** , sélectionnez le disque virtuel qui montre le **hors connexion** état, puis sélectionnez **mettre en ligne**. <br><br>Pour mettre en ligne un disque virtuel lorsque dans un cluster, ouvrez une session PowerShell en tant qu’administrateur, puis essayez à l’aide de la commande suivante :<br><br> <code>Get-VirtualDisk \| Where-Object -Filter { $_.OperationalStatus -eq "Detached" } \| Connect-VirtualDisk</code><br><br>Pour joindre automatiquement tous les disques virtuels non cluster après le redémarrage de Windows, ouvrez une session PowerShell en tant qu’administrateur, puis utilisez la commande suivante :<br><br> <code>Get-VirtualDisk \| Set-VirtualDisk -ismanualattach $false</code>|
|            |Majorité des disques défectueux |Trop de disques utilisés par ce disque virtuel a échoué, sont manquantes ou les données obsolètes.   <br><br>**Action**: <br> 1. Se reconnecter tous les lecteurs manquants et si vous utilisez des espaces de stockage Direct, mettre en ligne tous les serveurs qui sont hors connexion. <br> 2. Une fois que tous les lecteurs et les serveurs sont en ligne, remplacez tous les lecteurs ayant échoués. Consultez [Service d’intégrité](../../failover-clustering/health-service-overview.md) pour plus d’informations. <br>Espaces de stockage Direct démarre automatiquement une réparation si nécessaire après la reconnexion ou de remplacement d’un disque.<br>3. Si vous n’utilisez pas des espaces de stockage Direct, ensuite réparer le disque virtuel à l’aide de la [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) applet de commande.  <br><br>Si plus de disques a échoué que vous avez des copies de vos données et le disque virtuel n’a pas été réparés échecs intermédiaires, toutes les données sur le disque virtuel est définitivement perdu. Dans ce cas malheureux, supprimez le disque virtuel, créer un nouveau disque virtuel, puis restaurez à partir d’une sauvegarde.|
|                     |Incomplet |Pas assez de disques est présents pour lire le disque virtuel.    <br><br>**Action**: <br> 1. Se reconnecter tous les lecteurs manquants et si vous utilisez des espaces de stockage Direct, mettre en ligne tous les serveurs qui sont hors connexion. <br> 2. Une fois que tous les lecteurs et les serveurs sont en ligne, remplacez tous les lecteurs ayant échoués. Consultez [Service d’intégrité](../../failover-clustering/health-service-overview.md) pour plus d’informations. <br>Espaces de stockage Direct démarre automatiquement une réparation si nécessaire après la reconnexion ou de remplacement d’un disque.<br>3. Si vous n’utilisez pas des espaces de stockage Direct, ensuite réparer le disque virtuel à l’aide de la [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) applet de commande.<br><br>Si plus de disques a échoué que vous avez des copies de vos données et le disque virtuel n’a pas été réparés échecs intermédiaires, toutes les données sur le disque virtuel est définitivement perdu. Dans ce cas malheureux, supprimez le disque virtuel, créer un nouveau disque virtuel, puis restaurez à partir d’une sauvegarde.|
| |Délai d’expiration dépassé|Attacher le disque virtuel est trop long <br><br> **Action :** Cela ne devrait pas se produire souvent, donc vous pouvez essayer de voir si la condition est transmise dans le temps. Ou vous pouvez essayer de déconnecter le disque virtuel avec le [Disconnect-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/disconnect-virtualdisk?view=win10-ps) applet de commande, puis en utilisant le [Connect-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/connect-virtualdisk?view=win10-ps) applet de commande pour le reconnecter. |

## <a name="drive-physical-disk-states"></a>États du disque (disque physique)

Les sections suivantes décrivent les États d’intégrité un lecteur peut être dans. Les lecteurs dans un pool sont représentées dans PowerShell en tant que *disque physique* objets.

### <a name="drive-health-state-healthy"></a>État d’intégrité du lecteur : Healthy

|État opérationnel    |Description|
|---------            |---------          |
|OK|Le lecteur est sain.|
|Dans le service|Le lecteur effectue certaines opérations de tâches de nettoyage internes. Lorsque l’action est terminée, le lecteur doit revenir à la *OK* l’état d’intégrité.|

### <a name="drive-health-state-warning"></a>État d’intégrité du lecteur : Warning

Un lecteur dans le peuvent d’état avertissement lire et écrire des données avec succès, mais a un problème.

|État opérationnel    |Description|
|---------            |---------          |
|Perte de communication|Le lecteur est manquant. Si vous utilisez des espaces de stockage Direct, peut-être parce que le serveur est en panne.<br><br>**Action**: Si vous utilisez des espaces de stockage Direct, mettez tous les serveurs en ligne. Si elle n’est pas résolu, reconnectez le lecteur, remplacer ou essayer d’obtenir des informations de diagnostique détaillées sur ce lecteur en suivant les étapes de résolution des problèmes avec le rapport d’erreurs Windows > [disque physique a dépassé le délai](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-timed-out).|
|Suppression de pool|Espaces de stockage est en cours de retrait du disque à partir de son pool de stockage. <br><br> Il s’agit d’un état temporaire. Une fois la suppression terminée, si le lecteur est attaché au système, le disque passe à un autre état opérationnel (généralement OK) dans un pool primordial.|
|Départ de mode de maintenance|Espaces de stockage est en train de mettre le lecteur en mode maintenance, une fois que l’administrateur mettre le lecteur en mode maintenance. Il s’agit d’un état temporaire : le lecteur doit être bientôt dans le *en mode maintenance* état.|
|En mode maintenance|Un administrateur a placé le lecteur en mode maintenance, arrêt lit et écrit à partir du lecteur. Cela s’effectue habituellement avant la mise à jour du microprogramme de lecteur, ou lorsque vous testez des échecs.<br><br>**Action**: Pour tirer le lecteur hors du mode maintenance, utilisez le [Disable-StorageMaintenanceMode](https://technet.microsoft.com/itpro/powershell/windows/storage/disable-storagemaintenancemode) applet de commande.|
|Arrêt du mode maintenance|Un administrateur a pris le lecteur hors du mode de maintenance, et les espaces de stockage est en train de mettre en ligne le lecteur. Il s’agit d’un état temporaire : le lecteur doit être bientôt dans l’idéal, dans un autre état - *sain*.|
|Prévention d’erreur|Le lecteur a signalé qu’il est proche ayant échoué.<br><br>**Action**: Remplacez le lecteur.|
|Erreur d’e/s|Une erreur temporaire l’accès au disque s’est produite.<br><br>**Action**: <br>1. Si le lecteur ne le reconvertir en la **OK** d’état, vous pouvez essayer d’utiliser le [Reset-PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/reset-physicaldisk) applet de commande pour réinitialiser le lecteur. <br> 2. Utilisez [Repair-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/repair-virtualdisk) pour restaurer la résilience des disques virtuels affectés. <br>3. Si cela se répète, remplacez le lecteur.|
|Erreur temporaire|Une erreur temporaire avec le lecteur s’est produite. Cela signifie généralement le lecteur n’a pas répondu, mais cela peut également signifier que les espaces de stockage protection partition a été supprimée de façon inappropriée à partir du lecteur. <br><br>**Action**: <br>1. Si le lecteur ne le reconvertir en la **OK** d’état, vous pouvez essayer d’utiliser le [Reset-PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/reset-physicaldisk) applet de commande pour réinitialiser le lecteur. <br> 2. Utilisez [Repair-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/repair-virtualdisk) pour restaurer la résilience des disques virtuels affectés. <br>3. Si cela se répète, remplacez le lecteur, ou essayer d’obtenir des informations de diagnostique détaillées sur ce lecteur en suivant les étapes de résolution des problèmes avec le rapport d’erreurs Windows > [Échec de la mise en ligne du disque physique](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online).|
|Latence anormale|Le lecteur s’exécute lentement, mesurée par le Service de contrôle d’intégrité dans les espaces de stockage Direct.<br><br>**Action**: Si cela se répète, remplacez le disque afin qu’il ne réduit pas les performances des espaces de stockage dans son ensemble.

### <a name="drive-health-state-unhealthy"></a>État d’intégrité du lecteur : Unhealthy

Un lecteur dans un état non intègre actuellement ne peut pas être écrit dans ou accessible.

|État opérationnel    |Description|
|---------            |---------          |
|Non utilisable|Ce lecteur ne peut pas être utilisé par des espaces de stockage. Pour plus d’informations, consultez [espaces de stockage Direct configuration matérielle requise](storage-spaces-direct-hardware-requirements.md); si vous n’utilisez pas des espaces de stockage Direct, consultez [vue d’ensemble des espaces de stockage](https://technet.microsoft.com/library/hh831739(v=ws.11).aspx#Requirements).|
|Split|Le lecteur a deviennent séparé à partir du pool.<br><br>**Action**: Réinitialiser le lecteur, effacer toutes les données à partir du lecteur et en l’ajoutant au pool comme un disque vide. Pour ce faire, ouvrez une session PowerShell en tant qu’administrateur, exécutez le [Reset-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) applet de commande, puis exécutez [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk). <br><br>Pour obtenir des informations de diagnostique détaillées sur ce lecteur, suivez les étapes de résolution des problèmes avec le rapport d’erreurs Windows > [Échec de la mise en ligne du disque physique](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online).|
|Métadonnées obsolètes|Espaces de stockage a trouvé des métadonnées ancien sur le lecteur.<br><br>**Action**: Il s’agit d’un état temporaire. Si le lecteur ne la transition vers OK, vous pouvez exécuter [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) pour démarrer une opération de réparation sur les disques virtuels affectés. Si cela ne résout pas le problème, vous pouvez réinitialiser le lecteur avec le [Reset-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) applet de commande, réinitialiser toutes les données dans le lecteur, puis exécutez [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk).|
|Métadonnées non reconnues|Espaces de stockage trouvé des métadonnées non reconnues sur le lecteur, ce qui signifie généralement que le lecteur a des métadonnées à partir d’un autre pool dessus.<br><br>**Action**: Pour réinitialiser le lecteur et l’ajouter au pool actuel, réinitialiser le lecteur. Pour réinitialiser le lecteur, ouvrez une session PowerShell en tant qu’administrateur, exécutez le [Reset-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) applet de commande, puis exécutez [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk).|
|Support défaillant|Le lecteur a échoué et ne plus être utilisé par des espaces de stockage.<br><br>**Action**: Remplacez le lecteur. <br><br>Pour obtenir des informations de diagnostique détaillées sur ce lecteur, suivez les étapes de résolution des problèmes avec le rapport d’erreurs Windows > [Échec de la mise en ligne du disque physique](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online).|
|Panne de matériel de périphérique|Une défaillance matérielle s’est produite sur ce lecteur. <br><br>**Action**: Remplacez le lecteur.|
|Mise à jour du microprogramme|Windows met à jour le microprogramme sur le lecteur. Il s’agit d’un état temporaire qui dure généralement moins d’une minute et au cours de laquelle temps qu'autres disques dans le pool de gérer toutes les lectures et écritures. Pour plus d’informations, consultez [mettre à jour du microprogramme de lecteur](../update-firmware.md).|
|Start|Le lecteur se prépare pour l’opération. Il s’agit d’un état temporaire - une fois terminé, que le lecteur doit passer à un état opérationnel différent.|

## <a name="reasons-a-drive-cant-be-pooled"></a>Raisons de qu'un lecteur ne peut pas être mis en pool.

Certains lecteurs ne sont pas uniquement prêts à être dans un pool de stockage. Vous pouvez déterminer pourquoi un lecteur n’est pas éligible pour le regroupement en examinant le `CannotPoolReason` propriété d’un disque physique. Voici un exemple de script PowerShell pour afficher la propriété CannotPoolReason :

```PowerShell
Get-PhysicalDisk | Format-Table FriendlyName,MediaType,Size,CanPool,CannotPoolReason
```

Voici un exemple de sortie :

```
FriendlyName          MediaType          Size CanPool CannotPoolReason
------------          ---------          ---- ------- ----------------
ATA MZ7LM120HCFD00D3  SSD        120034123776   False Insufficient Capacity
Msft Virtual Disk     SSD         10737418240    True
Generic Physical Disk SSD        119990648832   False In a Pool
```

Le tableau suivant donne un peu plus de détails sur chacune des raisons.

|Raison|Description|
|---|---|
|Dans un pool|Le lecteur appartient déjà à un pool de stockage. <br><br>Les lecteurs peuvent appartenir à uniquement un seul pool de stockage à la fois. Pour utiliser ce lecteur dans un autre pool de stockage, supprimez tout d’abord le lecteur à partir de son pool existant, ce qui indique les espaces de stockage pour déplacer les données sur le lecteur à d’autres lecteurs sur le pool. Ou de réinitialiser le lecteur si le lecteur a été déconnecté de son pool sans en notifier les espaces de stockage. <br><br>Pour supprimer en toute sécurité un lecteur à partir d’un pool de stockage, utilisez [Remove-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/remove-physicaldisk), ou accédez au Gestionnaire de serveur > **File and Storage Services** > **Pools de stockage**, > **Disques physiques**, cliquez sur le lecteur, puis sélectionnez **supprimer le disque**.<br><br>Pour réinitialiser un lecteur, utilisez [Reset-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk).|
|Non intègre|Le lecteur n’est pas dans un état sain et peut devoir être remplacées.|
|Média amovible|Le lecteur est classé comme un lecteur amovible. <br><br>Espaces de stockage ne prend pas en charge les médias qui sont reconnus par Windows comme amovible, tels que les lecteurs de disque Blu-Ray. Bien que la plupart fixe lecteurs sont dans des emplacements amovibles, en général, les médias *classé* par Windows comme amovible ne conviennent pas pour une utilisation avec des espaces de stockage.|
|En cours d’utilisation par cluster|Le lecteur est actuellement utilisé par un Cluster de basculement.|
|Hors connexion|Le lecteur est en mode hors connexion. <br><br>Pour afficher les lecteurs hors connexion en ligne et la valeur en lecture/écriture, ouvrez une session PowerShell en tant qu’administrateur et utilisez les scripts suivants :<br><br><code>Get-Disk \| Where-Object -Property OperationalStatus -EQ "Offline" \| Set-Disk -IsOffline $false</code><br><br><code>Get-Disk \| Where-Object -Property IsReadOnly -EQ $true \| Set-Disk -IsReadOnly $false</code>|
|Capacité insuffisante|Cela se produit généralement lorsqu’il existe des partitions occupe l’espace libre sur le lecteur. <br><br>**Action**: Supprimez les volumes sur le lecteur, effacer toutes les données sur le lecteur. Un moyen d’y parvenir consiste à utiliser le [Clear-Disk](https://docs.microsoft.com/powershell/module/storage/clear-disk?view=win10-ps) applet de commande PowerShell.|
|Vérification en cours|Le [Service d’intégrité](../../failover-clustering/health-service-overview.md#supported-components-document) vérifie si le lecteur ou le microprogramme sur le lecteur est approuvé pour une utilisation par l’administrateur du serveur.|
|Échoué de la vérification|Le [Service d’intégrité](../../failover-clustering/health-service-overview.md#supported-components-document) n’a pas pu vérifier si le lecteur ou le microprogramme sur le lecteur est approuvé pour une utilisation par l’administrateur du serveur.|
|Microprogramme non conforme|Le microprogramme sur le lecteur physique n’est pas dans la liste des révisions de microprogramme approuvé est spécifié par l’administrateur du serveur à l’aide de la [Service d’intégrité](../../failover-clustering/health-service-overview.md#supported-components-document). |
|Matériel non conforme|Le lecteur n’est pas dans la liste des modèles de stockage approuvées spécifiés par l’administrateur du serveur en utilisant le [Service d’intégrité](../../failover-clustering/health-service-overview.md#supported-components-document).|

## <a name="see-also"></a>Voir aussi

- [Espaces de stockage Direct](storage-spaces-direct-overview.md)
- [Configuration matérielle directe des espaces de stockage](storage-spaces-direct-hardware-requirements.md)
- [Quorum de cluster et pool de présentation](understand-quorum.md)