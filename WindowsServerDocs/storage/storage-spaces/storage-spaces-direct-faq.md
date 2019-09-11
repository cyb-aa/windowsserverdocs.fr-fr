---
title: Espaces de stockage direct-Forum aux questions
description: En savoir plus sur espaces de stockage direct
keywords: Espaces de stockage
ms.prod: windows-server-threshold
ms.author: kaushik
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: 1aafac9a994e907e8b8ee3b556618d630cdf8418
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872069"
---
# <a name="storage-spaces-direct---frequently-asked-questions-faq"></a>Espaces de stockage direct-Forum aux questions (FAQ)

Cet article répertorie certaines questions fréquentes et fréquemment posées relatives à [espaces de stockage direct](storage-spaces-direct-overview.md).

## <a name="when-you-use-storage-spaces-direct-with-3-nodes-can-you-get-both-performance-and-capacity-tiers"></a>Quand vous utilisez espaces de stockage direct avec 3 nœuds, pouvez-vous bénéficier de niveaux de performances et de capacité ?

Oui, vous pouvez obtenir un niveau de performances et de capacité dans une configuration espaces de stockage direct à 2 nœuds ou à 3 nœuds. Toutefois, vous devez vous assurer que vous disposez de 2 unités de capacité. Cela signifie que vous devez utiliser les trois types d’appareils : NVME, SSD et HDD.
 
## <a name="refs-file-system-provides-real-time-tiaring-with-storage-spaces-direct-does-refs-provides-the-same-functionality-with-shared-storage-spaces-in-2016"></a>Le système de fichiers REFS fournit des tiaring en temps réel avec espaces de stockage direct. REFS fournit-il la même fonctionnalité avec les espaces de stockage partagés dans 2016 ?

Non, vous n’obtiendrez pas la hiérarchisation en temps réel avec des espaces de stockage partagé avec 2016. C’est le cas uniquement pour espaces de stockage direct. 
 
## <a name="can-i-use-an-ntfs-file-system-with-storage-spaces-direct"></a>Puis-je utiliser un système de fichiers NTFS avec espaces de stockage direct ?
  
Oui, vous pouvez utiliser le système de fichiers NTFS avec espaces de stockage direct. Toutefois, REFS est recommandé. NTFS ne fournit pas de hiérarchisation en temps réel. 
 
## <a name="i-have-configured-2-node-storage-spaces-direct-clusters-where-the-virtual-disk-is-configured-as-2-way-mirror-resiliency-if-i-add-a-new-fault-domain-will-the-resiliency-of-the-existing-virtual-disk-change"></a>J’ai configuré 2 clusters de espaces de stockage direct de nœud, où le disque virtuel est configuré comme résilience en miroir bidirectionnel. Si j’ajoute un nouveau domaine d’erreur, la résilience du disque virtuel existant change-t-elle ?

Une fois que vous avez ajouté le nouveau domaine d’erreur, les nouveaux disques virtuels que vous créez vont passer à la mise en miroir 3 voies. Toutefois, le disque virtuel existant reste un disque en miroir bidirectionnel. Vous pouvez copier les données vers les nouveaux disques virtuels à partir des volumes existants pour bénéficier des avantages de la nouvelle résilience.
 
## <a name="the-storage-spaces-direct-was-created-using-the-autoconfig0-switch-and-the-pool-created-manually-when-i-try-to-query-the-storage-spaces-direct-pool-to-create-a-new-volume-i-get-a-message-that-says-enable-clusters2d-again-what-should-i-do"></a>La espaces de stockage direct a été créée à l’aide de la configuration automatique : 0 le commutateur et le pool créés manuellement. Lorsque j’essaie d’interroger le pool de espaces de stockage direct pour créer un nouveau volume, j’obtiens un message indiquant « Enable-ClusterS2D ». Que dois-je faire ?

Par défaut, quand vous configurez espaces de stockage direct à l’aide de l’applet de commande Enable-S2D, l’applet de commande effectue tout pour vous. Il crée le pool et les niveaux. Lors de l’utilisation de la configuration automatique : 0, tout doit être effectué manuellement. Si vous avez créé uniquement le pool, le niveau n’est pas nécessairement créé. Vous recevrez un message d’erreur « Enable-ClusterS2D » si vous n’avez pas créé de niveaux ou des niveaux non créés de manière à ce qu’ils correspondent aux appareils attachés. Nous vous recommandons de ne pas utiliser le commutateur AutoConfig dans un environnement de production. 
 
## <a name="is-it-possible-to-add-a-spinning-disk-hdd-to-the-storage-spaces-direct-pool-after-you-have-created-storage-spaces-direct-with-ssd-devices"></a>Est-il possible d’ajouter un disque en rotation au pool de espaces de stockage direct après avoir créé espaces de stockage direct avec des appareils SSD ?

Non. Par défaut, si vous utilisez le type d’appareil unique pour créer le pool, il ne configure pas les disques de cache et tous les disques sont utilisés pour la capacité. Vous pouvez ajouter des disques NVME à la configuration, et les disques NVME seraient configurés pour le cache.
 
## <a name="i-have-configured-a-2-rack-fault-domain-rack-1-has-2-fault-domains-rack-2-has-1-fault-domain-each-server-has-4-capacity-100-gb-devices-can-i-use-all-1200-gb-of-space-from-the-pool"></a>J’ai configuré un domaine d’erreur à 2 racks : Le RACK 1 comporte 2 domaines d’erreur, le RACK 2 a 1 domaine d’erreur. Chaque serveur dispose de 4 unités 100 Go de capacité. Puis-je utiliser tous les 1 200 Go d’espace du pool ?

Non, vous ne pouvez utiliser que 800 Go. Dans un domaine d’erreur de rack, vous devez vous assurer que vous disposez d’une configuration de miroir bidirectionnel, de sorte que chaque mandrin et son terrain dupliqué dans un autre rack.
 
## <a name="what-should-the-cache-size-be-when-i-am-configuring-storage-spaces-direct"></a>Quelle est la taille du cache lorsque je configure espaces de stockage direct ?

Le cache doit être dimensionné pour s’adapter à la plage de travail (les données qui sont activement lues ou écrites à un moment donné) de vos applications et charges de travail.

## <a name="how-can-i-determine-the-size-of-cache-that-is-being-used-by-storage-spaces-direct"></a>Comment déterminer la taille du cache utilisé par espaces de stockage direct ?

Utilisez l’utilitaire PerfMon intégré pour inspecter les absences dans le cache. Examinez les lectures d’absence dans le cache à partir du compteur de disque hybride stockage en cluster. N’oubliez pas que si le cache ne contient pas trop de lectures, votre cache est sous-dimensionné et vous pouvez le développer. 
 
## <a name="is-there-a-calculator-that-shows-the-exact-size-of-the-disks-that-are-being-set-aside-for-cache-capacity-and-resiliency-that-would-enable-me-to-plan-better"></a>Existe-t-il une calculatrice qui indique la taille exacte des disques qui sont mis de côté pour le cache, la capacité et la résilience qui me permettent de planifier mieux ?

Vous pouvez utiliser la calculatrice des espaces de stockage pour vous aider dans votre planification. Il est disponible à http://aka.ms/s2dcalc l’adresse.
 
## <a name="what-is-the-best-configuration-that-you-would-recommend-when-configuring-6-servers-and-3-racks"></a>Quelle est la meilleure configuration recommandée lors de la configuration de 6 serveurs et de 3 racks ?

Utilisez 2 serveurs sur chacun des racks pour obtenir la résilience de disque virtuel d’un miroir 3 voies. N’oubliez pas que la configuration du rack ne fonctionne correctement que si vous fournissez la configuration au système d’exploitation de la manière qu’il est placé dans le rack. 
 
## <a name="can-i-enable-maintenance-mode-for-a-specific-disk-on-a-specific-server-in-storage-spaces-direct-cluster"></a>Puis-je activer le mode de maintenance pour un disque spécifique sur un serveur spécifique dans espaces de stockage direct cluster ?

Oui, vous pouvez activer le mode de maintenance de stockage sur un disque spécifique et un domaine d’erreur spécifique. La commande Enable-StorageMaintenanceMode est automatiquement appelée lorsque vous suspendez un nœud. Vous pouvez l’activer pour un disque spécifique en exécutant la commande suivante :

```powershell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Enable-StorageMaintenanceMode
```

## <a name="is-storage-spaces-direct-supported-on-my-hardware"></a>La espaces de stockage direct est-elle prise en charge sur mon matériel ?

Nous vous recommandons de contacter votre fournisseur de matériel pour vérifier la prise en charge. Les fournisseurs de matériel testent la solution sur leur matériel et commentent s’ils sont pris en charge ou non. Par exemple, au moment de la rédaction de cet article, les serveurs tels que R730/R730xd/R630 qui possèdent plus de 8 emplacements de lecteur peuvent prendre en charge les y et sont compatibles avec espaces de stockage direct. Dell ne prend en charge que les HBA330 avec espaces de stockage direct. R620 ne prend pas en charge les SES et n’est pas compatible avec espaces de stockage direct.

Pour plus d’informations sur la prise en charge du matériel, accédez au site Web suivant : Catalogue Windows Server
 
## <a name="how-does-storage-spaces-direct-make-use-of-ses"></a>Comment espaces de stockage direct utilise-t-il ?

Espaces de stockage direct utilise le mappage des services de boîtier SCSI pour s’assurer que les sections de données et les métadonnées sont réparties sur les domaines d’erreur de manière résiliente. Si le matériel ne prend pas en charge SES, il n’y a pas de mappage des boîtiers, et le placement des données n’est pas résilient.
 
## <a name="what-command-can-you-use-to-check-the-physical-extent-for-a-virtual-disk"></a>Quelle commande pouvez-vous utiliser pour vérifier l’étendue physique d’un disque virtuel ?
  
Celui-là:

```powershell
get-virtualdisk -friendlyname “xyz” | get-physicalextent
```
