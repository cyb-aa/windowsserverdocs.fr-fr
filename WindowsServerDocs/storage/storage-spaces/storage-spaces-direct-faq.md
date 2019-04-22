---
title: Espaces de stockage Direct - Forum aux questions
description: Découvrez qu’en est-il des espaces de stockage Direct
keywords: Espaces de stockage
ms.prod: windows-server-threshold
ms.author: kaushik
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: b17aa7ddc783e95fbcc19fe3913192d245133c7f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818910"
---
# <a name="storage-spaces-direct---frequently-asked-questions-faq"></a>Espaces de stockage Direct - Forum aux questions (FAQ)

Cet article répertorie certaines courants et des questions fréquemment posées relatives à [espaces de stockage Direct](storage-spaces-direct-overview.md).

## <a name="when-you-use-storage-spaces-direct-with-3-nodes-can-you-get-both-performance-and-capacity-tiers"></a>Lorsque vous utilisez espaces de stockage Direct avec 3 nœuds, obtenir des performances et les niveaux de capacité ?

Oui, vous pouvez obtenir une performance et le niveau de capacité dans une configuration d’espaces de stockage Direct 2 ou 3 nœuds. Toutefois, il se peut que vous devez vous assurer que vous avez 2 appareils de capacité. Cela signifie que vous devez utiliser les trois types d’appareils : NVME, SSD et HDD.
 
## <a name="refs-file-system-provides-real-time-tiaring-with-storage-spaces-direct-does-refs-provides-the-same-functionality-with-shared-storage-spaces-in-2016"></a>Système de fichiers Refs fournit tiaring en temps réel avec les espaces de stockage Direct. REFS fournit les mêmes fonctionnalités avec des espaces de stockage partagé en 2016 ?

Non, vous n’obtiendrez pas en temps réel la hiérarchisation avec les espaces de stockage partagé avec 2016. Il s’agit uniquement d’espaces de stockage Direct. 
 
## <a name="can-i-use-an-ntfs-file-system-with-storage-spaces-direct"></a>Puis-je utiliser un système de fichiers NTFS avec des espaces de stockage Direct ?
  
Oui, vous pouvez utiliser le système de fichiers NTFS avec des espaces de stockage Direct. Toutefois, il est recommandé de références. NTFS ne fournit pas la hiérarchisation en temps réel. 
 
## <a name="i-have-configured-2-node-storage-spaces-direct-clusters-where-the-virtual-disk-is-configured-as-2-way-mirror-resiliency-if-i-add-a-new-fault-domain-will-the-resiliency-of-the-existing-virtual-disk-change"></a>J’ai configuré des espaces de stockage Direct clusters de 2 nœuds, où le disque virtuel est configuré en tant que la résilience en miroir bidirectionnel 2. Si j’ajoute un nouveau domaine d’erreur, va changer la résilience du disque virtuel existant ?

Une fois que vous avez ajouté le nouveau domaine d’erreur, les nouveaux disques virtuels que vous créez passera à 3 voies miroir. Toutefois, le disque virtuel existant restera un disque en miroir à 2 facteurs. Vous pouvez copier les données vers les nouveaux disques virtuels à partir des volumes existants pour bénéficier des avantages de la résilience de nouveau.
 
## <a name="the-storage-spaces-direct-was-created-using-the-autoconfig0-switch-and-the-pool-created-manually-when-i-try-to-query-the-storage-spaces-direct-pool-to-create-a-new-volume-i-get-a-message-that-says-enable-clusters2d-again-what-should-i-do"></a>Les espaces de stockage Direct créé à l’aide de l’autoconfig:0 commutateur et le pool créé manuellement. Lorsque je tente d’interroger le pool d’espaces de stockage Direct pour créer un nouveau volume, j’obtiens un message indiquant « Enable-ClusterS2D à nouveau. » Que dois-je faire ?

Par défaut, lorsque vous configurez des espaces de stockage Direct à l’aide de l’applet de commande enable-S2D, l’applet de commande fait tout pour vous. Il crée le pool et les niveaux. Lorsque vous utilisez autoconfig:0, tous les éléments doivent être effectuées manuellement. Si vous avez créé uniquement le pool, le niveau n’est pas nécessairement créé. Vous recevrez un message d’erreur « Enable-ClusterS2D à nouveau » si vous avez ne pas créé à tous les niveaux ou créé ne pas les niveaux d’une manière correspondant pour les périphériques connectés. Nous recommandons que vous n’utilisez pas le commutateur de configuration automatique de réseau dans un environnement de production. 
 
## <a name="is-it-possible-to-add-a-spinning-disk-hdd-to-the-storage-spaces-direct-pool-after-you-have-created-storage-spaces-direct-with-ssd-devices"></a>Il est possible d’ajouter un disque de rotation (HDD) pour le pool d’espaces de stockage Direct, après avoir créé des espaces de stockage Direct avec les périphériques SSD ?

Non. Par défaut, si vous utilisez le type d’appareil unique pour créer le pool, il est inutile de configurer les disques de cache et tous les disques à utiliser pour la capacité. Vous pouvez ajouter des disques NVME à la configuration et les disques NVME doit être configurés pour le cache.
 
## <a name="i-have-configured-a-2-rack-fault-domain-rack-1-has-2-fault-domains-rack-2-has-1-fault-domain-each-server-has-4-capacity-100-gb-devices-can-i-use-all-1200-gb-of-space-from-the-pool"></a>J’ai configuré un domaine d’erreur 2-rack : RACK 1 a 2 domaines d’erreur, RACK 2 1 domaine d’erreur. Chaque serveur a 4 appareils de 100 Go de capacité. Puis-je utiliser toutes les 1 200 Go espace à partir du pool ?

Non, vous pouvez utiliser uniquement de 800 Go. Dans un domaine d’erreur rack, il se peut que vous devez vous assurer que vous disposez d’une configuration en miroir bidirectionnel 2 afin que chaque chuck et son land en double dans un rack différent.
 
## <a name="what-should-the-cache-size-be-when-i-am-configuring-storage-spaces-direct"></a>Quelle doit être la taille du cache quand je configure espaces de stockage Direct ?

Le cache doit être calibré pour contenir la plage de travail (les données qui sont activement lues ou écrites à un moment donné) de vos applications et les charges de travail.

## <a name="how-can-i-determine-the-size-of-cache-that-is-being-used-by-storage-spaces-direct"></a>Comment puis-je déterminer la taille du cache qui est utilisé par les espaces de stockage Direct ?

Utilisez l’utilitaire intégré, PerfMon pour inspecter les absences dans le cache. Passez en revue le cache absence dans lectures par seconde à partir du compteur de disque de Cluster stockage hybride. N’oubliez pas que s’il manquent trop grand nombre de lectures du cache, votre cache est de taille insuffisante souhaité pour le développer. 
 
## <a name="is-there-a-calculator-that-shows-the-exact-size-of-the-disks-that-are-being-set-aside-for-cache-capacity-and-resiliency-that-would-enable-me-to-plan-better"></a>Y a-t-il une calculatrice qui affiche la taille exacte des disques qui sont en cours réserver pour le cache, la capacité et la résilience qui me permet de mieux planifier ?

Vous pouvez utiliser la calculatrice d’espaces de stockage pour vous aider à la planification de votre. Il est disponible à l’adresse http://aka.ms/s2dcalc.
 
## <a name="what-is-the-best-configuration-that-you-would-recommend-when-configuring-6-servers-and-3-racks"></a>Qu’est la meilleure configuration que vous recommandiez lors de la configuration des 6 serveurs et 3 racks ?

Utilisez 2 serveurs sur chacun des racks pour obtenir la résilience de disque virtuel d’un miroir à 3 voies. N’oubliez pas que la configuration de rack fonctionnera correctement que si vous fournissez la configuration pour le système d’exploitation de la manière, qu'il est placé sur le rack. 
 
## <a name="can-i-enable-maintenance-mode-for-a-specific-disk-on-a-specific-server-in-storage-spaces-direct-cluster"></a>Puis-je activer le mode de maintenance pour un disque spécifique sur un serveur spécifique dans le cluster d’espaces de stockage Direct ?

Oui, vous pouvez activer le mode de maintenance stockage sur un disque spécifique et un domaine d’erreur spécifique. La commande Enable-StorageMaintenanceMode est appelée automatiquement lorsque vous placez un nœud. Vous pouvez l’activer pour un disque spécifique en exécutant la commande suivante :

```powershell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Enable-StorageMaintenanceMode
```

## <a name="is-storage-spaces-direct-supported-on-my-hardware"></a>Espaces de stockage Direct en charge mon matériel ?

Nous vous conseillons de contacter votre fournisseur de matériel pour vérifier la prise en charge. Fournisseurs de matériel testent la solution sur leur matériel et un commentaire si elle est pris en charge ou non. Par exemple, au moment de cette écriture, de serveurs, tels que R730 / R730xd / R630 ayant plus de 8 emplacements de lecteur peut prendre en charge des SES et sont compatibles avec les espaces de stockage Direct. Dell prend en charge uniquement la HBA330 avec les espaces de stockage Direct. R620 ne prend pas en charge SES et n’est pas compatible avec les espaces de stockage Direct.

Pour plus de matériel prend en charge plus d’informations, consultez le site Web suivant : Catalogue Windows Server
 
## <a name="how-does-storage-spaces-direct-make-use-of-ses"></a>Manière dont les espaces de stockage Direct rendent utiliser d’es ?

Espaces de stockage Direct utilise le mappage de Services SES (SCSI Enclosure) pour vous assurer que les sections de données et les métadonnées est réparti entre les domaines d’erreur de manière résiliente. Si le matériel ne prend pas en charge SES, il n’existe aucun mappage des boîtiers, et le placement des données n’est pas résilient.
 
## <a name="what-command-can-you-use-to-check-the-physical-extent-for-a-virtual-disk"></a>Quelle commande peut utiliser pour vérifier l’extension physique d’un disque virtuel ?
  
Celui-là :

```powershell
get-virtualdisk -friendlyname “xyz” | get-physicalextent
```
