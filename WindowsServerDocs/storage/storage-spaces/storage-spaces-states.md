---
title: Espaces de stockage et état des espaces de stockage direct et des États opérationnels
description: Comment trouver et comprendre les différents États d’intégrité et de fonctionnement des espaces de stockage direct et des espaces de stockage (y compris les disques physiques, les pools et les disques virtuels) et les procédures à leur sujet.
keywords: Espaces de stockage, détachés, disque virtuel, disque physique, détérioré
author: jasongerend
ms.author: jgerend
ms.date: 12/06/2019
ms.topic: article
ms.prod: windows-server
ms.technology: storage-spaces
manager: brianlic
ms.openlocfilehash: 40e820971f9d2ab0ba48fe30b100f07302ed7206
ms.sourcegitcommit: e817a130c2ed9caaddd1def1b2edac0c798a6aa2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74945277"
---
# <a name="troubleshoot-storage-spaces-and-storage-spaces-direct-health-and-operational-states"></a>Résoudre les problèmes d’intégrité et de espaces de stockage direct des espaces de stockage et des États opérationnels

> S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server (canal semi-annuel), Windows 10 Windows 8.1

Cette rubrique décrit les États d’intégrité et opérationnels des pools de stockage, les disques virtuels (qui se trouvent sous les volumes dans les espaces de stockage) et les lecteurs dans [espaces de stockage direct](storage-spaces-direct-overview.md) et les [espaces de stockage](overview.md). Ces États peuvent être très utiles lorsque vous tentez de résoudre divers problèmes, par exemple la raison pour laquelle vous ne pouvez pas supprimer un disque virtuel en raison d’une configuration en lecture seule. Elle explique également pourquoi un lecteur ne peut pas être ajouté à un pool (le CannotPoolReason).

Les espaces de stockage possèdent trois objets principaux ( *disques physiques* , disques SSD, etc.) qui sont ajoutés à un *pool de stockage*, ce qui permet de virtualiser le stockage afin que vous puissiez créer des *disques virtuels* à partir de l’espace libre dans le pool, comme illustré ici. Les métadonnées du pool sont écrites sur chaque lecteur du pool. Les volumes sont créés sur les disques virtuels et stockent vos fichiers, mais nous n’allons pas parler des volumes ici.

![Les disques physiques sont ajoutés à un pool de stockage, puis les disques virtuels créés à partir de l’espace du pool](media/storage-spaces-states/storage-spaces-object-model.png)

Vous pouvez afficher les États d’intégrité et opérationnels dans Gestionnaire de serveur, ou avec PowerShell. Voici un exemple d’un grand nombre d’États opérationnels et d’intégrité (généralement incorrects) sur un cluster espaces de stockage direct qui ne contient pas la plupart de ses nœuds de cluster (cliquez avec le bouton droit sur les en-têtes de colonne pour ajouter l' **état opérationnel**). Ce n’est pas un bon cluster.

![Gestionnaire de serveur de l’indication des résultats de deux nœuds manquants dans un cluster espaces de stockage direct-un grand nombre de disques physiques et de disques virtuels manquants dans un État non sain](media/storage-spaces-states/unhealthy-disks-in-server-manager.png)

## <a name="storage-pool-states"></a>États du pool de stockage

Chaque pool de stockage a un état d’intégrité **sain**, **avertissement**ou **inconnu**/non **intègre**, ainsi qu’un ou plusieurs États opérationnels.

Pour déterminer l’état d’un pool, utilisez les commandes PowerShell suivantes :

```PowerShell
Get-StoragePool -IsPrimordial $False | Select-Object HealthStatus, OperationalStatus, ReadOnlyReason
```

Voici un exemple de sortie présentant un pool de stockage dans l’état d’intégrité inconnu avec l’état opérationnel en lecture seule :

```
FriendlyName                OperationalStatus HealthStatus IsPrimordial IsReadOnly
------------                ----------------- ------------ ------------ ----------
S2D on StorageSpacesDirect1 Read-only         Unknown      False        True
```

Les sections suivantes répertorient les états d’intégrité et opérationnels.

### <a name="pool-health-state-healthy"></a>État d’intégrité du pool : sain

| État opérationnel    | Description |
| ---------            | ---------  |
| OK | Le pool de stockage est sain. |

### <a name="pool-health-state-warning"></a>État d’intégrité du pool : AVERTISSEMENT

Lorsque le pool de stockage est dans l’état d' **Avertissement d’avertissement** , cela signifie que le pool est accessible, mais qu’un ou plusieurs lecteurs ont échoué ou sont manquants. Par conséquent, votre pool de stockage peut avoir une résilience réduite.

|État opérationnel    |Description|
|---------            |---------  |
|Détérioré|Le pool de stockage contient des lecteurs défaillants ou manquants. Cette condition se produit uniquement avec les lecteurs hébergeant des métadonnées de pool. <br><br>**Action**: Vérifiez l’état de vos lecteurs et remplacez tous les disques défaillants avant de procéder à d’autres défaillances.|

### <a name="pool-health-state-unknown-or-unhealthy"></a>État d’intégrité du pool : inconnu ou défectueux

Lorsqu’un pool de stockage est dans un état d’intégrité **inconnu** ou non **intègre** , cela signifie que le pool de stockage est en lecture seule et ne peut pas être modifié tant que le pool n’est pas renvoyé aux États d’intégrité **Avertissement** ou **OK** .

|État opérationnel    |Raison en lecture seule |Description|
|---------            |---------       |--------   |
|en lecture seule|Incomplet|Cela peut se produire si le pool de stockage perd son [quorum](understand-quorum.md), ce qui signifie que la plupart des lecteurs du pool ont échoué ou sont hors connexion pour une raison quelconque. Lorsqu’un pool perd son quorum, les espaces de stockage déplacent automatiquement la configuration du pool en lecture seule jusqu’à ce que suffisamment de lecteurs soient à nouveau disponibles.<br><br>**Action :** <br>1. reconnectez tous les lecteurs manquants et, si vous utilisez espaces de stockage direct, mettez tous les serveurs en ligne. <br>2. réaffectez la valeur lecture-écriture au pool en ouvrant une session PowerShell avec des autorisations d’administration, puis en tapant :<br><br> <code>Get-StoragePool <PoolName> -IsPrimordial $False \| Set-StoragePool -IsReadOnly $false</code>|
||Stratégie|Un administrateur a défini le pool de stockage en lecture seule.<br><br>**Action :** Pour configurer un pool de stockage en cluster pour l’accès en lecture-écriture dans Gestionnaire du cluster de basculement, accédez à **Pools**, cliquez avec le bouton droit sur le pool, puis sélectionnez **mettre en ligne**.<br><br>Pour les autres serveurs et PC, ouvrez une session PowerShell avec des autorisations d’administration, puis tapez :<br><br><code>Get-StoragePool <PoolName> \| Set-StoragePool -IsReadOnly $false</code><br><br> |
||Start|Les espaces de stockage sont en cours de démarrage ou en attente de la connexion des lecteurs dans le pool. Il doit s’agir d’un état temporaire. Une fois complètement démarré, le pool doit passer à un autre état opérationnel.<br><br>**Action :** Si le pool reste à l’état de *démarrage* , assurez-vous que tous les lecteurs du pool sont correctement connectés.|

Voir aussi [modification d’un pool de stockage qui a une configuration en lecture seule](https://social.technet.microsoft.com/wiki/contents/articles/14861.modifying-a-storage-pool-that-has-a-read-only-configuration.aspx).

## <a name="virtual-disk-states"></a>États des disques virtuels

Dans les espaces de stockage, les volumes sont placés sur des disques virtuels (espaces de stockage) qui sont issus de l’espace libre dans un pool. Chaque disque virtuel présente un état d’intégrité **sain**, **Avertissement**, **défectueux**ou **inconnu** , ainsi qu’un ou plusieurs États opérationnels.

Pour connaître l’état des disques virtuels, utilisez les commandes PowerShell suivantes :

```PowerShell
Get-VirtualDisk | Select-Object FriendlyName,HealthStatus, OperationalStatus, DetachedReason
```

Voici un exemple de sortie présentant un disque virtuel détaché et un disque virtuel détérioré/incomplet :

```
FriendlyName HealthStatus OperationalStatus      DetachedReason
------------ ------------ -----------------      --------------
Volume1      Unknown      Detached               By Policy
Volume2      Warning      {Degraded, Incomplete} None
```

Les sections suivantes répertorient les états d’intégrité et opérationnels.

### <a name="virtual-disk-health-state-healthy"></a>État d’intégrité du disque virtuel : sain

|État opérationnel    |Description|
|---------            |---------          |
|OK    |Le disque virtuel est sain.|
|Non optimal    |Les données ne sont pas écrites uniformément sur les disques. <br><br>**Action**: optimisez l’utilisation des disques dans le pool de stockage en exécutant l’applet de commande [optimize-StoragePool](https://technet.microsoft.com/itpro/powershell/windows/storage/optimize-storagepool) .|

### <a name="virtual-disk-health-state-warning"></a>État d’intégrité du disque virtuel : AVERTISSEMENT

Lorsque le disque virtuel est dans un état d' **Avertissement** , cela signifie qu’une ou plusieurs copies de vos données ne sont pas disponibles, mais que les espaces de stockage peuvent toujours lire au moins une copie de vos données.

|État opérationnel    |Description|
|---------            |---------          |
|En service            |Windows répare le disque virtuel, par exemple après l’ajout ou la suppression d’un lecteur. Une fois la réparation terminée, le disque virtuel doit revenir à l’état d’intégrité OK.|
|Incomplet           |La résilience du disque virtuel est réduite en raison de l’échec ou de l’absence d’un ou plusieurs lecteurs. Toutefois, les lecteurs manquants contiennent des copies à jour de vos données.<br><br> **Action** : <br>1. reconnectez les lecteurs manquants, remplacez les lecteurs défaillants et, si vous utilisez espaces de stockage direct, mettez en ligne tous les serveurs qui sont hors connexion. <br>2. Si vous n’utilisez pas espaces de stockage direct, réparez ensuite le disque virtuel à l’aide de l’applet de commande [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) .<br> Espaces de stockage direct démarre automatiquement une réparation, si nécessaire, après reconnexion ou remplacement d’un lecteur.|
|Détérioré             |La résilience du disque virtuel est réduite en raison de l’échec ou de l’absence d’un ou de plusieurs lecteurs, et il existe des copies obsolètes de vos données sur ces lecteurs. <br><br>**Action** : <br> 1. reconnectez les lecteurs manquants, remplacez les lecteurs défaillants et, si vous utilisez espaces de stockage direct, mettez en ligne tous les serveurs qui sont hors connexion. <br> 2. Si vous n’utilisez pas espaces de stockage direct, réparez ensuite le disque virtuel à l’aide de l’applet de commande [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) . <br>Espaces de stockage direct démarre automatiquement une réparation, si nécessaire, après reconnexion ou remplacement d’un lecteur.|

### <a name="virtual-disk-health-state-unhealthy"></a>État d’intégrité du disque virtuel : défectueux

Lorsqu’un disque virtuel se trouve dans **un état d’intégrité défectueux,** une partie ou la totalité des données du disque virtuel est actuellement inaccessible.

|État opérationnel    |Description|
|---------            |--------   |
|Aucune redondance|Le disque virtuel a perdu des données en raison de l’échec d’un trop grand nombre de disques.<br><br>**Action**: remplacez les disques défaillants, puis restaurez vos données à partir de la sauvegarde.|

### <a name="virtual-disk-health-state-informationunknown"></a>État d’intégrité du disque virtuel : informations/inconnu

Le disque virtuel peut également se trouver dans l’état d’intégrité des **informations** (comme indiqué dans l’élément du panneau de configuration espaces de stockage) ou dans un état d’intégrité **inconnu** (comme indiqué dans PowerShell) si un administrateur a mis le disque virtuel hors connexion ou si le disque virtuel a été détaché.

|État opérationnel    |Raison détachée |Description|
|---------            |---------       |--------   |
|Détaché             |Par stratégie            |Un administrateur a mis le disque virtuel hors connexion ou défini le disque virtuel pour exiger une pièce jointe manuelle. dans ce cas, vous devez attacher manuellement le disque virtuel à chaque redémarrage de Windows.<br><br>**Action**: Remettez le disque virtuel en ligne. Pour ce faire, lorsque le disque virtuel se trouve dans un pool de stockage en cluster, dans Gestionnaire du cluster de basculement sélectionnez **stockage** > **Pools** > **disques virtuels**, sélectionnez le disque virtuel qui affiche l’état **hors connexion** , puis sélectionnez **mettre en ligne**. <br><br>Pour remettre en ligne un disque virtuel lorsqu’il n’est pas dans un cluster, ouvrez une session PowerShell en tant qu’administrateur, puis essayez d’utiliser la commande suivante :<br><br> <code>Get-VirtualDisk \| Where-Object -Filter { $_.OperationalStatus -eq "Detached" } \| Connect-VirtualDisk</code><br><br>Pour attacher automatiquement tous les disques virtuels non-cluster après le redémarrage de Windows, ouvrez une session PowerShell en tant qu’administrateur, puis utilisez la commande suivante :<br><br> <code>Get-VirtualDisk \| Set-VirtualDisk -ismanualattach $false</code>|
|            |Disques majoritaires non intègres |Le nombre de lecteurs utilisés par ce disque virtuel a échoué, est manquant ou contient des données obsolètes.   <br><br>**Action** : <br> 1. reconnectez les lecteurs manquants et, si vous utilisez espaces de stockage direct, mettez en ligne tous les serveurs qui sont hors connexion. <br> 2. une fois que tous les lecteurs et serveurs sont en ligne, remplacez les lecteurs défaillants. Pour plus d’informations, consultez [service de contrôle d’intégrité](../../failover-clustering/health-service-overview.md) . <br>Espaces de stockage direct démarre automatiquement une réparation, si nécessaire, après reconnexion ou remplacement d’un lecteur.<br>3. Si vous n’utilisez pas espaces de stockage direct, réparez ensuite le disque virtuel à l’aide de l’applet de commande [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) .  <br><br>Si le nombre de disques ayant échoué est supérieur au nombre de copies de vos données et que le disque virtuel n’a pas été réparé entre les défaillances, toutes les données sur le disque virtuel sont définitivement perdues. Dans ce cas, supprimez le disque virtuel, créez un nouveau disque virtuel, puis restaurez à partir d’une sauvegarde.|
|                     |Incomplet |Les lecteurs sont insuffisants pour lire le disque virtuel.    <br><br>**Action** : <br> 1. reconnectez les lecteurs manquants et, si vous utilisez espaces de stockage direct, mettez en ligne tous les serveurs qui sont hors connexion. <br> 2. une fois que tous les lecteurs et serveurs sont en ligne, remplacez les lecteurs défaillants. Pour plus d’informations, consultez [service de contrôle d’intégrité](../../failover-clustering/health-service-overview.md) . <br>Espaces de stockage direct démarre automatiquement une réparation, si nécessaire, après reconnexion ou remplacement d’un lecteur.<br>3. Si vous n’utilisez pas espaces de stockage direct, réparez ensuite le disque virtuel à l’aide de l’applet de commande [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) .<br><br>Si le nombre de disques ayant échoué est supérieur au nombre de copies de vos données et que le disque virtuel n’a pas été réparé entre les défaillances, toutes les données sur le disque virtuel sont définitivement perdues. Dans ce cas, supprimez le disque virtuel, créez un nouveau disque virtuel, puis restaurez à partir d’une sauvegarde.|
| |Délai d’expiration dépassé|L’attachement du disque virtuel a pris trop de temps <br><br> **Action :** Ce n’est pas souvent le cas, donc vous pouvez essayer de déterminer si la condition est passée dans le temps. Vous pouvez essayer de déconnecter le disque virtuel avec l’applet de commande Disconnect [-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/disconnect-virtualdisk?view=win10-ps) , puis utiliser l’applet de commande [Connect-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/connect-virtualdisk?view=win10-ps) pour le reconnecter. |

## <a name="drive-physical-disk-states"></a>États du lecteur (disque physique)

Les sections suivantes décrivent les états d’intégrité d’un lecteur. Les lecteurs d’un pool sont représentés dans PowerShell en tant qu’objets de *disque physique* .

### <a name="drive-health-state-healthy"></a>État d’intégrité du lecteur : sain

|État opérationnel    |Description|
|---------            |---------          |
|OK|Le lecteur est sain.|
|En service|Le lecteur effectue certaines opérations de nettoyage interne. Une fois l’action terminée, le lecteur doit revenir à l’état d’intégrité *OK* .|

### <a name="drive-health-state-warning"></a>État d’intégrité du lecteur : AVERTISSEMENT

Un lecteur en état Avertissement peut lire et écrire des données, mais il a un problème.

|État opérationnel    |Description|
|---------            |---------          |
|Perte de communication|Le lecteur est manquant. Si vous utilisez espaces de stockage direct, cela peut être dû au fait qu’un serveur est en cours d’arrêt.<br><br>**Action**: Si vous utilisez espaces de stockage direct, remettez tous les serveurs en ligne. Si cela ne résout pas le problème, reconnectez le lecteur, remplacez-le ou essayez d’obtenir des informations de diagnostic détaillées sur ce lecteur en suivant les étapes de la section résolution des problèmes à l’aide de Rapport d’erreurs Windows > [disque physique dépassé](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-timed-out).|
|Suppression du pool|Les espaces de stockage sont en train de supprimer le lecteur de son pool de stockage. <br><br> Il s’agit d’un état temporaire. Une fois la suppression terminée, si le lecteur est toujours attaché au système, le lecteur passe à un autre état opérationnel (généralement OK) dans un pool primordial.|
|Démarrage du mode de maintenance|Les espaces de stockage sont en train de mettre le lecteur en mode maintenance après qu’un administrateur a mis le lecteur en mode de maintenance. Il s’agit d’un état temporaire : le lecteur doit bientôt être dans l’état *en mode maintenance* .|
|En mode de maintenance|Un administrateur a placé le lecteur en mode de maintenance, en arrêtant les lectures et les écritures à partir du lecteur. Cette opération est généralement effectuée avant la mise à jour du microprogramme du lecteur, ou lors du test des échecs.<br><br>**Action**: pour déconnecter le lecteur du mode maintenance, utilisez l’applet de commande [Disable-StorageMaintenanceMode](https://technet.microsoft.com/itpro/powershell/windows/storage/disable-storagemaintenancemode) .|
|Arrêt du mode de maintenance|Un administrateur a retiré le lecteur du mode de maintenance et les espaces de stockage sont en cours de remise en ligne du lecteur. Il s’agit d’un état temporaire : le lecteur devrait bientôt se trouver dans un autre État, idéalement *sain*.|
|Défaillance prédictive|Le lecteur a signalé qu’il est proche de l’échec.<br><br>**Action**: remplacez le lecteur.|
|Erreur E/S|Une erreur temporaire d’accès au lecteur s’est produite.<br><br>**Action** : <br>1. Si le lecteur ne passe pas à l’état **OK** , vous pouvez essayer d’utiliser l’applet de commande [Reset-PhysicalDisk pour réinitialiser](https://docs.microsoft.com/powershell/module/storage/reset-physicaldisk) le lecteur. <br> 2. Utilisez [Repair-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/repair-virtualdisk) pour restaurer la résilience des disques virtuels affectés. <br>3. Si le problème persiste, remplacez le lecteur.|
|Erreur temporaire|Une erreur temporaire du lecteur s’est produite. Cela signifie généralement que le lecteur ne répond pas, mais cela peut également signifier que la partition de protection des espaces de stockage a été supprimée de manière inappropriée du lecteur. <br><br>**Action** : <br>1. Si le lecteur ne passe pas à l’état **OK** , vous pouvez essayer d’utiliser l’applet de commande [Reset-PhysicalDisk pour réinitialiser](https://docs.microsoft.com/powershell/module/storage/reset-physicaldisk) le lecteur. <br> 2. Utilisez [Repair-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/repair-virtualdisk) pour restaurer la résilience des disques virtuels affectés. <br>3. Si le problème persiste, remplacez le lecteur ou essayez d’obtenir des informations de diagnostic détaillées sur ce lecteur en suivant les étapes décrites dans résolution des problèmes à l’aide de Rapport d’erreurs Windows > [disque physique n’a pas pu être mis en ligne](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online).|
|Latence anormale|Le lecteur fonctionne lentement, comme mesuré par le Service de contrôle d’intégrité dans espaces de stockage direct.<br><br>**Action**: si le problème persiste, remplacez le lecteur afin qu’il ne réduise pas les performances des espaces de stockage dans son ensemble.

### <a name="drive-health-state-unhealthy"></a>État d’intégrité du lecteur : défectueux

Un lecteur dans un état Non intègre n’est actuellement pas accessible et il est impossible d’y écrire.

|État opérationnel    |Description|
|---------            |---------          |
|Inutilisable|Ce lecteur ne peut pas être utilisé par les espaces de stockage. Pour plus d’informations, consultez [espaces de stockage direct configuration matérielle requise](storage-spaces-direct-hardware-requirements.md). Si vous n’utilisez pas espaces de stockage direct, consultez [vue d’ensemble des espaces de stockage](https://technet.microsoft.com/library/hh831739(v=ws.11).aspx#Requirements).|
|fractionnement|Le lecteur est séparé du pool.<br><br>**Action**: réinitialisez le lecteur, en effaçant toutes les données du lecteur et en le rajoutant au pool en tant que lecteur vide. Pour ce faire, ouvrez une session PowerShell en tant qu’administrateur, exécutez l’applet de commande [Reset-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) , puis exécutez [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk). <br><br>Pour obtenir des informations de diagnostic détaillées sur ce lecteur, suivez les étapes de la section résolution des problèmes à l’aide de Rapport d’erreurs Windows > [disque physique n’a pas pu être mis en ligne](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online).|
|Métadonnées obsolètes|Les espaces de stockage ont trouvé des métadonnées anciennes sur le lecteur.<br><br>**Action**: il doit s’agir d’un état temporaire. Si le lecteur n’est pas rétabli, vous pouvez exécuter [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) pour démarrer une opération de réparation sur les disques virtuels affectés. Si cela ne résout pas le problème, vous pouvez réinitialiser le lecteur à l’aide de l’applet de commande [Reset-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) , en effaçant toutes les données du lecteur, puis en exécutant [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk).|
|Métadonnées non reconnues|Les espaces de stockage ont trouvé des métadonnées non reconnues sur le lecteur, ce qui signifie généralement que le lecteur a des métadonnées provenant d’un pool différent.<br><br>**Action**: pour effacer le lecteur et l’ajouter au pool actuel, réinitialisez le lecteur. Pour réinitialiser le lecteur, ouvrez une session PowerShell en tant qu’administrateur, exécutez l’applet de commande [Reset-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) , puis exécutez [Repair-VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk).|
|Support défaillant|Le lecteur a échoué et ne plus être utilisé par des espaces de stockage.<br><br>**Action**: remplacez le lecteur. <br><br>Pour obtenir des informations de diagnostic détaillées sur ce lecteur, suivez les étapes de la section résolution des problèmes à l’aide de Rapport d’erreurs Windows > [disque physique n’a pas pu être mis en ligne](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online).|
|Panne matérielle d’appareil|Une panne matérielle s’est produite sur ce lecteur. <br><br>**Action**: remplacez le lecteur.|
|Mise à jour du microprogramme|Windows met à jour le microprogramme sur le lecteur. Il s’agit d’un état temporaire qui dure généralement moins d’une minute. Pendant ce temps, les autres lecteurs du pool gèrent toutes les lectures et écritures. Pour plus d’informations, consultez [mettre à jour le microprogramme du lecteur](../update-firmware.md).|
|Start|Le lecteur se prépare pour l’opération. Il s’agit d’un état temporaire. Le lecteur doit ensuite passer à un état opérationnel différent.|

## <a name="reasons-a-drive-cant-be-pooled"></a>Raisons pour lesquelles un lecteur ne peut pas être mis en pool

Certains lecteurs ne sont pas disponibles dans un pool de stockage. Vous pouvez déterminer la raison pour laquelle un lecteur n’est pas éligible pour le regroupement en regardant la propriété `CannotPoolReason` d’un disque physique. Voici un exemple de script PowerShell pour afficher la propriété CannotPoolReason :

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
|Dans un pool|Le lecteur appartient déjà à un pool de stockage. <br><br>Les lecteurs ne peuvent appartenir qu’à un seul pool de stockage à la fois. Pour utiliser ce lecteur dans un autre pool de stockage, supprimez d’abord le lecteur de son pool existant, ce qui indique aux espaces de stockage de déplacer les données sur le lecteur vers d’autres lecteurs du pool. Ou réinitialisez le lecteur si le lecteur a été déconnecté de son pool sans notifier les espaces de stockage. <br><br>Pour supprimer en toute sécurité un lecteur d’un pool de stockage, utilisez [Remove-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/remove-physicaldisk)ou accédez à gestionnaire de serveur **> services de fichiers et de stockage** > **pools de stockage**, > **disques physiques**, cliquez avec le bouton droit sur le lecteur, puis sélectionnez **supprimer le disque**.<br><br>Pour réinitialiser un lecteur, utilisez [Reset-PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk).|
|Défectueux|Le lecteur n’est pas dans un état sain et devra peut-être être remplacé.|
|Média amovible|Le lecteur est classé comme un lecteur amovible. <br><br>Les espaces de stockage ne prennent pas en charge les médias reconnus par Windows comme étant amovibles, tels que les lecteurs Blu-Ray. Bien que de nombreux lecteurs fixes soient dans des emplacements amovibles, en général, les médias qui sont *classés* par Windows comme amovible ne sont pas adaptés pour une utilisation avec les espaces de stockage.|
|En cours d’utilisation par le cluster|Le lecteur est actuellement utilisé par un Cluster de basculement.|
|Hors connexion|Le lecteur est en mode hors connexion. <br><br>Pour mettre tous les lecteurs hors connexion en ligne et en lecture/écriture, ouvrez une session PowerShell en tant qu’administrateur et utilisez les scripts suivants :<br><br><code>Get-Disk \| Where-Object -Property OperationalStatus -EQ "Offline" \| Set-Disk -IsOffline $false</code><br><br><code>Get-Disk \| Where-Object -Property IsReadOnly -EQ $true \| Set-Disk -IsReadOnly $false</code>|
|Capacité insuffisante|Cela se produit généralement lorsque des partitions occupent l’espace libre sur le lecteur. <br><br>**Action**: supprimez tous les volumes sur le lecteur, en effaçant toutes les données sur le lecteur. Une façon de procéder consiste à utiliser l’applet de commande PowerShell [Clear-Disk](https://docs.microsoft.com/powershell/module/storage/clear-disk?view=win10-ps) .|
|Vérification en cours|Le [service de contrôle d’intégrité](../../failover-clustering/health-service-overview.md#supported-components-document) vérifie si le lecteur ou le microprogramme sur le lecteur est approuvé pour une utilisation par l’administrateur du serveur.|
|Échec de la vérification|La [service de contrôle d’intégrité](../../failover-clustering/health-service-overview.md#supported-components-document) n’a pas pu vérifier si le lecteur ou le microprogramme sur le lecteur est approuvé pour une utilisation par l’administrateur du serveur.|
|Microprogramme non conforme|Le microprogramme sur le lecteur physique ne figure pas dans la liste des révisions de microprogrammes approuvées spécifiées par l’administrateur du serveur à l’aide de la [service de contrôle d’intégrité](../../failover-clustering/health-service-overview.md#supported-components-document). |
|Matériel non conforme|Le lecteur ne figure pas dans la liste des modèles de stockage approuvés spécifiée par l’administrateur du serveur à l’aide de la [service de contrôle d’intégrité](../../failover-clustering/health-service-overview.md#supported-components-document).|

## <a name="see-also"></a>Articles associés

- [Espaces de stockage directs](storage-spaces-direct-overview.md)
- [espaces de stockage direct configuration matérielle requise](storage-spaces-direct-hardware-requirements.md)
- [Présentation du quorum de cluster et du pool](understand-quorum.md)