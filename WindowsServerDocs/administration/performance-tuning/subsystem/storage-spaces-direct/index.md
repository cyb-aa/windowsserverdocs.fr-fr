---
title: Optimisation des niveaux de performance pour les espaces de stockage directs
description: La fonctionnalité Espaces de stockage directs (Storage Spaces Direct) adapte automatiquement son niveau de performance en fonction de la configuration du cache du matériel que vous utilisez, comme décrit dans cette rubrique.
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.assetid: 15a519fa-37cc-4d84-a9fe-097d33bb71ea
author: phstee
ms.author: Vshankar; DanLo; clausjor; StevenEk
ms.date: 4/14/2017
ms.openlocfilehash: 280d0e298afe5c9628fe73872e0983f819f2a3b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891300"
---
# <a name="performance-tuning-for-storage-spaces-direct"></a>Optimisation des niveaux de performance pour les espaces de stockage directs

Les espaces de stockage directs (Storage Spaces Direct), une solution de stockage définie par logiciel basée sur Windows Server, optimisent automatiquement leurs niveaux de performance, évitant ainsi de spécifier le nombre de colonnes manuellement, la configuration du cache du matériel utilisé et d’autres facteurs devant être définis manuellement avec les solutions de stockage SAS partagé. Pour obtenir davantage d’informations, consultez la section [Espaces de stockage directs dans Windows Server 2016](../../../../storage/storage-spaces/storage-spaces-direct-overview.md).

Le cache de bus de stockage logiciel des espaces de stockage directs est automatiquement configuré en fonction des types de stockage présents dans le système. Trois types de stockage sont reconnus : **HDD**, **SSD** et **NVMe**. Le cache utilise la solution de stockage la plus rapide pour la mise en cache en lecture et/ou en écriture, selon le cas, et utilise la solution de stockage la plus lente pour le stockage persistant des données.

Le tableau suivant propose un récapitulatif des réglages par défaut :

| Types de stockage | Configuration du cache |
| --- | --- |
| N’importe quel type unique | S’il n’y a qu’un seul type de stockage présent, le cache de bus de stockage logiciel n’est pas configuré. |
| SSD+HDD ou NVMe+HDD | Le stockage le plus rapide est configuré en tant que couche de cache et met en cache les lectures et les écritures. |
| SSD+SSD ou NVMe+NVMe | Ces options fast + fast sont conçues pour les combinaisons de stockage d’endurance plus élevé et plus faible, par exemple un disque SSD flash NAND à 10 écritures par jour (DWPD) pour le cache et un disque SSD flash 1,5 DWPD NAND pour la capacité. Elles sont activées en donnant aux espaces de stockage directs un ensemble de chaînes de modèle avec lesquelles identifier les périphériques de cache. Pour plus d’informations, consultez l’applet de commande de référence [Enable-StorageSpacesDirect](https://technet.microsoft.com/library/mt589697.aspx) (`CacheDeviceModel`). <br><br>Dans un système fast + fast, seules les écritures sont mises en cache. Les lectures ne sont pas mises en cache. |

Notez que la mise en cache par défaut sur un périphérique SSD ou NVMe utilise uniquement la mise en cache en écriture. En effet, étant donné que le périphérique de capacité est rapide, le transfert de contenu en lecture vers les périphériques de cache n’a qu’une valeur limitée. Ce n’est pas valable dans tous les cas, cependant il convient d’être vigilant, car l’activation du cache de lecture peut consommer inutilement l’endurance du périphérique de cache sans augmenter son niveau de performance. Voici quelques exemples :

* **NVme+SSD**L’activation du cache de lecture permettra aux IO de lecture de tirer parti de la connectivité PCIe et/ou des performances IOPS supérieures des périphériques NVMe par rapport au SSD agrégé. <br>Cela _peut_être vrai pour les scénarios orientés bande passante en raison des capacités de bande passante relative des périphériques NVMe par rapport au HBA se connectant au SSD. Cela _peut ne pas_ être vrai pour les scénarios orientés IOPS dans lesquels les coûts de processeur d’IOPS peuvent limiter les systèmes avant que les niveaux de performance accrus puissent être atteints.
* **NVMe + NVMe** De même, si la capacité de lecture du cache NVMe est supérieure à la capacité combinée NVMe, l’activation du cache de lecture peut s’avérer utile. <br>Les scénarios dans lesquels l’activation du cache de lecture serait bénéfique dans ces configurations devraient être inhabituels.

Pour afficher et modifier la configuration du cache, utilisez les applets de commande [Get-ClusterStorageSpacesDirect](https://technet.microsoft.com/library/mt634616.aspx) et [Set-ClusterStorageSpacesDirect](https://technet.microsoft.com/library/mt763265.aspx). Les propriétés `CacheModeHDD` et `CacheModeSSD` définissent le fonctionnement du cache sur les supports de capacité du type indiqué.

## <a name="see-also"></a>Voir également

- [Comprendre les espaces de stockage directs](../../../../storage/storage-spaces/understand-storage-spaces-direct.md)
- [Planification des espaces de stockage directs](../../../../storage/storage-spaces/plan-storage-spaces-direct.md)
- [Optimisation des niveaux de performance des serveurs de fichiers](../../role/file-server/index.md)
- [Guide pour la conception d’un stockage à définition logicielle](https://technet.microsoft.com/library/mt243829.aspx) (pour les espaces de stockage sous Windows Server 2012 R2 et SAS partagés)
