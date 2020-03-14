---
ms.assetid: 342173ca-4e10-44f4-b2c9-02a6c26f7a4a
title: Planification des volumes dans les espaces de stockage direct
ms.prod: windows-server
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 06/28/2019
ms.localizationpriority: medium
ms.openlocfilehash: 52c600068d5dd447ff9faa7c40788664e222a83a
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79322781"
---
# <a name="planning-volumes-in-storage-spaces-direct"></a>Planification des volumes dans les espaces de stockage direct

> S’applique à : Windows Server 2019, Windows Server 2016

Cette rubrique fournit des recommandations sur la manière de planifier vos volumes dans les espaces de stockage direct de manière à répondre aux besoins de vos charges de travail en matière de performances et de capacités, y compris en ce qui concerne le choix de leur système de fichiers, de leur type de résilience et de leur taille.

## <a name="review-what-are-volumes"></a>Révision : Définition des volumes

Les volumes sont l’endroit où vous placez les fichiers nécessaires à vos charges de travail, tels que les fichiers VHD ou VHDX pour les machines virtuelles Hyper-V. Les volumes utilisent les disques du pool de stockage de manière combinée afin d'optimiser la tolérance de panne, l'évolutivité et les performances de votre déploiement d'espaces de stockage direct.

   >[!NOTE]
   > Dans toute la documentation relative aux espaces de stockage direct, nous utilisons le terme « volume » pour désigner à la fois le volume et le disque virtuel sous-jacent, y compris les fonctionnalités offertes par d'autres fonctions Windows intégrées comme les volumes partagés de cluster (Cluster Shared Volumes, CSV) et ReFS. Il n'est pas nécessaire de comprendre toutes les subtilités au niveau de l'implémentation pour planifier et déployer avec succès des espaces de stockage direct.

![what-are-volumes](media/plan-volumes/what-are-volumes.png)

Tous les volumes sont accessibles simultanément à l'ensemble des serveurs du cluster. Une fois créés, ils s’affichent sur **C:\ClusterStorage\\** sur tous les serveurs.

![csv-folder-screenshot](media/plan-volumes/csv-folder-screenshot.png)

## <a name="choosing-how-many-volumes-to-create"></a>Choix du nombre de volumes à créer

Nous vous recommandons de choisir comme nombre de volumes un multiple du nombre de serveurs présents dans votre cluster. Par exemple, si vous avez 4 serveurs, vous obtiendrez des performances plus cohérentes avec 4 volumes au total que 3 ou 5. Cela permet au cluster de répartir la « propriété » des volumes (un serveur gère l'orchestration des métadonnées pour chaque volume) équitablement entre les serveurs.

Nous vous recommandons de limiter le nombre total de volumes à :

| Windows Server 2016          | Windows Server 2019          |
|------------------------------|------------------------------|
| Jusqu’à 32 volumes par cluster | Jusqu’à 64 volumes par cluster |

## <a name="choosing-the-filesystem"></a>Choix du système de fichiers

Nous vous recommandons d’utiliser le nouveau [système de fichiers ReFS (Resilient File System)](../refs/refs-overview.md) pour les espaces de stockage direct. ReFS est le premier système de fichiers conçu spécialement pour la virtualisation et offre de nombreux avantages, notamment une accélération considérable des performances et une protection intégrée contre la corruption des données. Il prend en charge presque toutes les principales fonctionnalités NTFS, notamment la déduplication des données dans Windows Server, la version 1709 et les versions ultérieures. Pour plus d’informations, consultez le [tableau de comparaison des fonctionnalités](../refs/refs-overview.md#feature-comparison) ReFS.

Si votre charge de travail requiert une fonction que ReFS ne prend pas encore en charge, vous pouvez utiliser le système de fichiers NTFS à la place.

   > [!TIP]
   > Des volumes utilisant des systèmes de fichiers différents peuvent coexister au sein d’un même cluster.

## <a name="choosing-the-resiliency-type"></a>Choix du type de résilience

Les volumes utilisés par les espaces de stockage direct fournissent une résilience qui vise à protéger contre les problèmes matériels tels que les pannes de disque ou de serveur, et qui assure une disponibilité continue pendant toutes les opérations de maintenance des serveurs, notamment les mises à jour logicielles.

   > [!NOTE]
   > Le type de résilience que vous choisissez ne dépend pas du type de disque que vous possédez.

### <a name="with-two-servers"></a>Avec deux serveurs

Avec deux serveurs dans le cluster, vous pouvez utiliser la mise en miroir bidirectionnelle. Si vous exécutez Windows Server 2019, vous pouvez également utiliser la résilience imbriquée.

La mise en miroir bidirectionnelle conserve deux copies de toutes les données, une copie sur les lecteurs de chaque serveur. Son efficacité de stockage est de 50% : pour écrire 1 to de données, vous devez disposer d’au moins 2 to de capacité de stockage physique dans le pool de stockage. La mise en miroir bidirectionnelle peut tolérer en toute sécurité une défaillance matérielle à la fois (un serveur ou un lecteur).

![two-way-mirror](media/plan-volumes/two-way-mirror.png)

La résilience imbriquée (disponible uniquement sur Windows Server 2019) assure la résilience des données entre les serveurs avec mise en miroir bidirectionnelle, puis ajoute la résilience au sein d’un serveur avec une mise en miroir bidirectionnelle ou une parité à accélération miroir. L’imbrication assure la résilience des données même lorsqu’un serveur est redémarré ou non disponible. Son efficacité de stockage est de 25% avec la mise en miroir bidirectionnelle imbriquée et environ 35-40% pour la parité imbriquée avec accélération en miroir. La résilience imbriquée peut tolérer en toute sécurité deux défaillances matérielles à la fois (deux lecteurs, ou un serveur et un lecteur sur le serveur restant). En raison de cette résilience des données ajoutée, nous vous recommandons d’utiliser la résilience imbriquée sur les déploiements de production de clusters à deux serveurs, si vous utilisez Windows Server 2019. Pour plus d’informations, consultez [résilience imbriquée](nested-resiliency.md).

![Parité imbriquée avec accélération miroir](media/nested-resiliency/nested-mirror-accelerated-parity.png)

### <a name="with-three-servers"></a>Avec trois serveurs

Avec trois serveurs, il est préférable d'utiliser une mise en miroir triple afin d'améliorer la tolérance de panne et les performances. La mise en miroir triple conserve trois copies de toutes les données, une copie sur les disques de chaque serveur. Son efficacité de stockage est de 33,3 %. Pour écrire 1 To de données, vous devez donc disposer d’au moins 3 To de capacité de stockage physique dans le pool de stockage. La mise en miroir triple tolère [au moins deux problèmes matériels (disque ou serveur) à la fois](storage-spaces-fault-tolerance.md#examples). Si deux nœuds ne sont plus disponibles, le pool de stockage perd le quorum, car 2/3 des disques ne sont pas disponibles et les disques virtuels ne sont pas accessibles. Toutefois, un nœud peut être en panne et un ou plusieurs disques sur un autre nœud peuvent échouer et les disques virtuels restent en ligne. Par exemple, si vous redémarrez un serveur au moment où un autre disque ou serveur tombe soudainement en panne, toutes les données restent en sécurité et demeurent accessibles en continu.

![three-way-mirror](media/plan-volumes/three-way-mirror.png)

### <a name="with-four-or-more-servers"></a>Avec quatre serveurs ou plus

Avec quatre serveurs ou plus, vous pouvez choisir pour chaque volume d’utiliser la mise en miroir triple, la parité double (souvent appelée « codage d’effacement ») ou mélanger les deux avec une parité à accélération miroir.

La double parité offre la même tolérance de panne que la mise en miroir triple, mais avec une meilleure efficacité de stockage. Avec quatre serveurs, son efficacité de stockage est de 50.0% : pour stocker 2 to de données, vous avez besoin de 4 to de capacité de stockage physique dans le pool de stockage. Cette efficacité passe à 66,7 % avec sept serveurs, et peut aller jusqu'à 80 %. La contrepartie est que le codage de parité est plus gourmand en ressources système, ce qui peut limiter les performances.

![dual-parity](media/plan-volumes/dual-parity.png)

Le choix du type de résilience à utiliser dépend des besoins de votre charge de travail. Voici un tableau résumant les charges de travail adaptées à chaque type de résilience, ainsi que les performances et l’efficacité de stockage de chaque type de résilience.

| Type de résilience | Efficacité de la capacité | Vitesse | Charges de travail |
| ------------------- | ----------------------  | --------- | ------------- |
| **Miroir**         | ![Efficacité de stockage avec 33%](media/plan-volumes/3-way-mirror-storage-efficiency.png)<br>Miroir triple : 33% <br>Miroir bidirectionnel : 50%     |![Performances avec 100%](media/plan-volumes/three-way-mirror-perf.png)<br> Performances optimales  | Charges de travail virtualisées<br> Bases de données<br>Autres charges de travail hautes performances |
| **Parité accélérée grâce à la mise en miroir** |![Efficacité de stockage avec environ 50%](media/plan-volumes/mirror-accelerated-parity-storage-efficiency.png)<br> Dépend de la proportion de miroir et de parité | ![Performances qui indiquent environ 20%](media/plan-volumes/mirror-accelerated-parity-perf.png)<br>Beaucoup plus lent que le miroir, mais jusqu’à deux fois plus rapide qu’une double parité<br> Idéal pour les lectures et écritures séquentielles volumineuses | Archivage et sauvegarde<br> Infrastructure de bureau virtualisée     |
| **Double parité**               | ![Efficacité de stockage avec environ 80%](media/plan-volumes/dual-parity-storage-efficiency.png)<br>4 serveurs : 50% <br>16 serveurs : jusqu’à 80% | ![Performances qui indiquent environ 10%](media/plan-volumes/dual-parity-perf.png)<br>Latence d’e/s la plus élevée & l’utilisation du processeur sur les écritures<br> Idéal pour les lectures et écritures séquentielles volumineuses | Archivage et sauvegarde<br> Infrastructure de bureau virtualisée  |

#### <a name="when-performance-matters-most"></a>Lorsque la performance est le critère principal

Les charges de travail avec des exigences strictes en matière de latence ou qui nécessitent un grand nombre d'E/S par seconde (IOPS) aléatoires mixtes (bases de données SQL Server ou machines virtuelles Hyper-V sensibles aux performances, par exemple) doivent s'exécuter sur des volumes qui utilisent une mise en miroir afin d'optimiser les performances.

   >[!TIP]
   > La mise en miroir est plus rapide que n’importe quel autre type de résilience. Nous utilisons la mise en miroir pour quasiment tous nos exemples de performances.

#### <a name="when-capacity-matters-most"></a>Lorsque la capacité est le critère principal

Les charges de travail qui réalisent peu d'écritures, par exemple les entrepôts de données ou le stockage « à froid », doivent s'exécuter sur des volumes qui utilisent la double parité pour optimiser l’efficacité du stockage. Certaines autres charges de travail, telles que les serveurs de fichiers traditionnels, les infrastructures VDI (Virtual Desktop Infrastructure) ou autres qui ne génèrent pas beaucoup de trafic d’E/S aléatoire à dérive rapide et/ou qui ne nécessitent pas les meilleures performances peuvent également utiliser la double parité, à votre convenance. La parité augmente de façon inévitable l’utilisation du processeur et la latence des E/S, en particulier sur les écritures, par rapport à la mise en miroir.

#### <a name="when-data-is-written-in-bulk"></a>Lorsque les données sont écrites en bloc

Les charges de travail qui écrivent en grandes passes séquentielles, telles que les cibles d'archivage ou de sauvegarde, ont une autre option qui est une nouveauté introduite par Windows Server 2016 : un même volume peut combiner la mise en miroir et la double parité. Les écritures sont dans un premier temps hébergées sur la partie en miroir et sont progressivement déplacées, plus tard, dans la partie dédiée à la parité. Cela accélère l'ingestion et réduit l'utilisation des ressources lors de l’arrivée d'écritures volumineuses en permettant à l’encodage de parité gourmand en ressources système de se produire sur une durée plus longue. Lors du dimensionnement des zones, n'oubliez pas que la zone en miroir doit être de taille suffisante pour accueillir les écritures à effectuer simultanément (comme c’est le cas lors des sauvegardes quotidiennes). Par exemple, si vous devez traiter 100 Go par jour en une seule opération, prévoyez une mise en miroir de 150 Go à 200 Go, et une double parité pour le reste.

L’efficacité de stockage qui en résulte varie en fonction des proportions que vous choisissez. Voir [cette démonstration](https://www.youtube.com/watch?v=-LK2ViRGbWs&t=36m55s) pour obtenir des exemples.

   > [!TIP]
   > Si vous observez une baisse soudaine des performances d’écriture, en cours d’ingestion de données, cela peut indiquer que la portion miroir n’est pas assez grande ou que la parité avec accélération miroir n’est pas adaptée à votre cas d’utilisation. Par exemple, si les performances d’écriture descendent de 400 Mo/s à 40 Mo/s, envisagez d’étendre la partie miroir ou de basculer vers un miroir triple.

### <a name="about-deployments-with-nvme-ssd-and-hdd"></a>À propos des déploiements utilisant des disques NVMe, SSD et HDD

Dans les déploiements avec deux types de disques, les disques plus rapides assurent la mise en cache, tandis que les disques plus lents fournissent la capacité. Ceci est configuré automatiquement. Pour plus d’informations, voir [Fonctionnement du cache dans les espaces de stockage direct](understand-the-cache.md). Dans ces déploiements, tous les volumes résident sur le même type de disques, à savoir les disques de capacité.

Dans les déploiements utilisant les trois types de disques, seuls les disques les plus rapides (NVMe) assurent la mise en cache, tandis que les deux autres types de disques (SSD et HDD) fournissent la capacité. Pour chaque volume, vous pouvez choisir s'il doit résider entièrement sur le niveau SSD, entièrement sur le niveau HDD, ou s'il doit s'étendre sur les deux.

   > [!IMPORTANT]
   > Nous vous recommandons d’utiliser le niveau SSD pour placer vos charges de travail les plus sensibles aux performances sur du 100 % Flash.

## <a name="choosing-the-size-of-volumes"></a>Choix de la taille des volumes

Nous vous recommandons de limiter la taille de chaque volume à :

| Windows Server 2016 | Windows Server 2019 |
| ------------------- | ------------------- |
| Jusqu’à 32 to         | Jusqu’à 64 to         |

   > [!TIP]
   > Si vous utilisez une solution de sauvegarde qui s’appuie sur le service VSS et le fournisseur de logiciel Volsnap, comme c’est souvent le cas avec les charges de travail de serveur de fichiers, la limitation de la taille du volume à 10 to améliorera les performances et la fiabilité. Les solutions de sauvegarde qui utilisent l'API plus récente Hyper-V RCT et/ou le clonage de bloc sur ReFS et/ou les API de sauvegarde SQL natives fonctionnent bien jusqu'à 32 To et au-delà.

### <a name="footprint"></a>Encombrement

La taille d’un volume fait référence à sa capacité utile, à savoir la quantité de données qu'il peut stocker. Elle est spécifiée par le paramètre **-Size** de l'applet de commande **New-Volume** et s’affiche dans la propriété **Size** lorsque vous exécutez l'applet de commande **Get-Volume**.

La taille est différente de l'*encombrement* du volume, à savoir la capacité de stockage physique totale qu'il occupe sur le pool de stockage. L’encombrement du volume dépend de son type de résilience. Par exemple, les volumes qui utilisent une mise en miroir triple ont un encombrement de trois fois leur taille.

Les encombrements de vos volumes doivent tenir dans votre pool de stockage.

![size-versus-footprint](media/plan-volumes/size-versus-footprint.png)

### <a name="reserve-capacity"></a>Capacité de réserve

Le fait de conserver une certaine capacité non allouée dans le pool de stockage donne aux volumes un espace de réparation « sur place » après une panne de disques, ce qui améliore la sécurité des données ainsi que les performances. Lorsque la capacité est suffisante, une réparation parallèle immédiate, sur place, permet de restaurer les volumes dans un état de résilience totale avant même le remplacement des disques défaillants. Cela se fait de manière automatique.

Nous vous recommandons de réserver l’équivalent d'un disque de capacité par serveur, jusqu'à 4 disques. Vous pouvez réserver davantage de capacité, à votre convenance, mais cette recommandation minimale vous donne l'assurance qu'une réparation parallèle immédiate, sur place, pourra être effectuée si un disque quelconque tombe en panne.

![reserve](media/plan-volumes/reserve.png)

Par exemple, si vous disposez de 2 serveurs et que vous utilisez des disques de capacité de 1 To, mettez de côté l'équivalent de 2 x 1 = 2 To du pool en réserve. Si vous disposez de 3 serveurs et que vous utilisez des disques de capacité de 1 To, mettez de côté l'équivalent de 3 x 1 = 3 To en réserve. Si vous disposez de 4 serveurs ou plus et que vous utilisez des disques de capacité de 1 To, mettez de côté l'équivalent de 4 x 1 = 4 To en réserve.

   >[!NOTE]
   > Dans les clusters utilisant des disques des trois types (NVMe + SSD + HDD), nous vous recommandons de réserver l’équivalent d’un disque SSD plus un disque HDD par serveur, jusqu'à 4 disques de chaque type.

## <a name="example-capacity-planning"></a>Exemple : planification de la capacité

Prenons l'exemple d'un cluster à quatre serveurs. Chaque serveur possède un certain nombre de disques de cache plus seize disques de capacité de 2 To chacun.

```
4 servers x 16 drives each x 2 TB each = 128 TB
```

À partir de ces 128 To dont nous disposons dans le pool de stockage, nous réservons quatre disques, soit 8 To, afin que des réparations sur place soient possibles sans qu'il soit impératif de remplacer immédiatement tout disque défaillant. Nous disposons donc de 120 To de capacité de stockage physique dans le pool avec lesquels nous pouvons créer nos volumes.

```
128 TB – (4 x 2 TB) = 120 TB
```

Notre déploiement doit héberger un certain nombre de machines virtuelles Hyper-V très actives, mais nous avons également beaucoup de stockage à froid : d'anciens fichiers et d'anciennes sauvegardes qu'il nous faut conserver. Étant donné que nous avons quatre serveurs, nous allons créer quatre volumes.

Nous plaçons les machines virtuelles sur les deux premiers volumes, *Volume1* et *Volume2*. Nous choisissons ReFS comme système de fichiers (pour une création plus rapide et la disponibilité de points de contrôle) et une mise en miroir triple pour la résilience afin de maximiser les performances. Par ailleurs, nous plaçons le stockage à froid sur les deux autres volumes, *Volume 3* et *Volume 4*. Nous choisissons NTFS comme système de fichiers (pour la déduplication des données) et la double parité pour la résilience afin de maximiser la capacité.

Il n'est pas obligatoire que tous les volumes soient de la même taille, mais pour simplifier, disons que nous leur attribuons à tous une taille identique de 12 To.

*Volume1* et *Volume2* occuperont donc chacun 12 To x 33,3 % d’efficacité = 36 To de capacité de stockage physique.

De leur côté, *Volume3* and *Volume4* occuperont chacun 12 To x 50,0 % d’efficacité = 24 To de capacité de stockage physique.

```
36 TB + 36 TB + 24 TB + 24 TB = 120 TB
```

Nos quatre volumes correspondent exactement à la capacité de stockage physique disponible dans notre pool. Parfait !

![exemple](media/plan-volumes/example.png)

   >[!TIP]
   > Vous n’avez pas besoin de créer tous vos volumes en une seule fois. Vous avez toujours la possibilité d'étendre des volumes ou d'en créer de nouveaux par la suite.

Pour simplifier, cet exemple utilise des unités décimales (base 10), c'est-à-dire que 1 To = 1 000 000 000 000 d'octets. Toutefois, les quantités de stockage dans Windows sont exprimées en unités binaires (base 2). Par exemple, tout disque de 2 To affichera une capacité de 1,82 Tio dans Windows. De même, notre pool de stockage de 128 To affichera une capacité de 116,41 Tio. Ce comportement est normal.

## <a name="usage"></a>Utilisation

Voir [Création de volumes dans les espaces de stockage direct](create-volumes.md).

### <a name="see-also"></a>Voir aussi

- [Présentation de espaces de stockage direct](storage-spaces-direct-overview.md)
- [Choix des lecteurs pour espaces de stockage direct](choosing-drives.md)
- [Tolérance de panne et efficacité du stockage](storage-spaces-fault-tolerance.md)
