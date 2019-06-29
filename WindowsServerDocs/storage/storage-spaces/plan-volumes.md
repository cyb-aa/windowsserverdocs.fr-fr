---
ms.assetid: 342173ca-4e10-44f4-b2c9-02a6c26f7a4a
title: Planification des volumes dans les espaces de stockage direct
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 06/28/2019
ms.localizationpriority: medium
ms.openlocfilehash: a04a362b65af8f184037d26728a1c147ca8ef948
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469669"
---
# <a name="planning-volumes-in-storage-spaces-direct"></a>Planification des volumes dans les espaces de stockage direct

> S’applique à : Windows Server 2019, Windows Server 2016

Cette rubrique fournit des recommandations sur la manière de planifier vos volumes dans les espaces de stockage direct de manière à répondre aux besoins de vos charges de travail en matière de performances et de capacités, y compris en ce qui concerne le choix de leur système de fichiers, de leur type de résilience et de leur taille.

## <a name="review-what-are-volumes"></a>Révision : Quelles sont les volumes

Les volumes sont où vous placez les fichiers que nécessitent de vos charges de travail, telles que de VHD ou VHDX pour les ordinateurs virtuels Hyper-V. Les volumes utilisent les disques du pool de stockage de manière combinée afin d'optimiser la tolérance de panne, l'évolutivité et les performances de votre déploiement d'espaces de stockage direct.

   >[!NOTE]
   > Dans toute la documentation relative aux espaces de stockage direct, nous utilisons le terme « volume » pour désigner à la fois le volume et le disque virtuel sous-jacent, y compris les fonctionnalités offertes par d'autres fonctions Windows intégrées comme les volumes partagés de cluster (Cluster Shared Volumes, CSV) et ReFS. Il n'est pas nécessaire de comprendre toutes les subtilités au niveau de l'implémentation pour planifier et déployer avec succès des espaces de stockage direct.

![what-are-volumes](media/plan-volumes/what-are-volumes.png)

Tous les volumes sont accessibles simultanément à l'ensemble des serveurs du cluster. Une fois créé, ils apparaissent au **C:\ClusterStorage\\**  sur tous les serveurs.

![csv-folder-screenshot](media/plan-volumes/csv-folder-screenshot.png)

## <a name="choosing-how-many-volumes-to-create"></a>Choix du nombre de volumes à créer

Nous vous recommandons de choisir comme nombre de volumes un multiple du nombre de serveurs présents dans votre cluster. Par exemple, si vous avez 4 serveurs, vous rencontrerez des performances plus cohérentes et 4 volumes total que 3 ou 5. Cela permet au cluster de répartir la « propriété » des volumes (un serveur gère l'orchestration des métadonnées pour chaque volume) équitablement entre les serveurs.

Nous vous recommandons de limiter le nombre total de volumes à :

| Windows Server 2016          | Windows Server 2019          |
|------------------------------|------------------------------|
| Jusqu'à 32 volumes par cluster | Jusqu'à 64 volumes par cluster |

## <a name="choosing-the-filesystem"></a>Choix du système de fichiers

Nous vous recommandons d’utiliser le nouveau [système de fichiers ReFS (Resilient File System)](../refs/refs-overview.md) pour les espaces de stockage direct. ReFS est le premier système de fichiers conçu spécialement pour la virtualisation et offre de nombreux avantages, notamment une accélération considérable des performances et une protection intégrée contre la corruption des données. Il prend en charge presque toutes les principales fonctionnalités NTFS, notamment la déduplication des données dans Windows Server, version 1709 et versions ultérieure. Consultez les références [tableau comparatif des fonctionnalités](../refs/refs-overview.md#feature-comparison) pour plus d’informations.

Si votre charge de travail requiert une fonction que ReFS ne prend pas encore en charge, vous pouvez utiliser le système de fichiers NTFS à la place.

   > [!TIP]
   > Des volumes utilisant des systèmes de fichiers différents peuvent coexister au sein d’un même cluster.

## <a name="choosing-the-resiliency-type"></a>Choix du type de résilience

Les volumes utilisés par les espaces de stockage direct fournissent une résilience qui vise à protéger contre les problèmes matériels tels que les pannes de disque ou de serveur, et qui assure une disponibilité continue pendant toutes les opérations de maintenance des serveurs, notamment les mises à jour logicielles.

   > [!NOTE]
   > Le type de résilience que vous choisissez ne dépend pas du type de disque que vous possédez.

### <a name="with-two-servers"></a>Avec deux serveurs

Avec deux serveurs du cluster, vous pouvez utiliser la mise en miroir bidirectionnelle. Si vous exécutez Windows Server 2019, vous pouvez également utiliser la résilience imbriquée.

Mise en miroir bidirectionnel conserve deux copies de toutes les données, une copie sur les lecteurs dans chaque serveur. Son efficacité de stockage est de 50 %, pour écrire 1 To de données, vous devez au moins 2 To de capacité de stockage physique dans le pool de stockage. Mise en miroir bidirectionnelle peut en toute sécurité capable de tolérer une défaillance matérielle à la fois (un serveur ou lecteur).

![two-way-mirror](media/plan-volumes/two-way-mirror.png)

Résilience imbriquée (disponible uniquement sur Windows Server 2019) fournit la résilience des données entre les serveurs avec mise en miroir bidirectionnelle, puis ajoute la résilience au sein d’un serveur avec la mise en miroir bidirectionnel ou parité accélérée de miroir. Imbrication fournit la résilience des données même si un serveur est indisponible ou de redémarrage. Son efficacité de stockage est 25 avec mise en miroir bidirectionnelle imbriquée et environ 35 à 40 % pour la parité d’accélérée de miroir imbriquée. Résilience imbriquée peut tolérer en toute sécurité les deux défaillances de matériel à la fois (deux disques, ou un serveur et un lecteur sur le serveur restant). En raison de cette tolérance de données ajoutées, nous recommandons d’utiliser la résilience imbriquée sur des clusters de deux serveurs, les déploiements de production si vous exécutez Windows Server 2019. Pour plus d’informations, consultez [Nested résilience](nested-resiliency.md).

![Parité d’accélérée de miroir imbriquée](media/nested-resiliency/nested-mirror-accelerated-parity.png)

### <a name="with-three-servers"></a>Avec trois serveurs

Avec trois serveurs, il est préférable d'utiliser une mise en miroir triple afin d'améliorer la tolérance de panne et les performances. La mise en miroir triple conserve trois copies de toutes les données, une copie sur les disques de chaque serveur. Son efficacité de stockage est de 33,3 %. Pour écrire 1 To de données, vous devez donc disposer d’au moins 3 To de capacité de stockage physique dans le pool de stockage. La mise en miroir triple tolère [au moins deux problèmes matériels (disque ou serveur) à la fois](storage-spaces-fault-tolerance.md#examples). Si 2 nœuds deviennent indisponibles au pool de stockage perd le quorum, dans la mesure où les 2/3 des disques ne sont pas disponibles, et les disques virtuels seront inaccessibles. Toutefois, un nœud peut être hors service et un ou plusieurs disques sur un autre nœud peuvent échouer et les disques virtuels restent en ligne. Par exemple, si vous redémarrez un serveur au moment où un autre disque ou serveur tombe soudainement en panne, toutes les données restent en sécurité et demeurent accessibles en continu.

![three-way-mirror](media/plan-volumes/three-way-mirror.png)

### <a name="with-four-or-more-servers"></a>Avec quatre serveurs ou plus

Avec quatre ou plusieurs serveurs, vous pouvez choisir pour chaque volume s’il faut utiliser la mise en miroir triple, parité double (souvent appelée « codage d’effacement »), ou la combinaison des deux avec parité accélérée de miroir.

La double parité offre la même tolérance de panne que la mise en miroir triple, mais avec une meilleure efficacité de stockage. Avec quatre serveurs, son efficacité de stockage est 50.0%—to stocker 2 To de données, vous avez besoin de 4 To de capacité de stockage physique dans le pool de stockage. Cette efficacité passe à 66,7 % avec sept serveurs, et peut aller jusqu'à 80 %. La contrepartie est que le codage de parité est plus gourmand en ressources système, ce qui peut limiter les performances.

![dual-parity](media/plan-volumes/dual-parity.png)

Le choix du type de résilience à utiliser dépend des besoins de votre charge de travail. Voici un tableau résumant les charges de travail sont adaptées pour chaque type de résilience, ainsi que l’efficacité de stockage et de performances de chaque type de résilience.

| Type de résilience | Efficacité de la capacité | Vitesse | Charges de travail |
| ------------------- | ----------------------  | --------- | ------------- |
| **Miroir**         | ![Affichage de l’efficacité de stockage 33 %](media/plan-volumes/3-way-mirror-storage-efficiency.png)<br>Miroir triple : 33% <br>Deux-way-mise en miroir : 50%     |![Affichage des performances 100 %](media/plan-volumes/three-way-mirror-perf.png)<br> Meilleures performances  | Charges de travail virtualisées<br> Bases de données<br>Autres charges de travail hautes performances |
| **Parité accélérée grâce à la mise en miroir** |![Affichage d’environ 50 % de l’efficacité du stockage](media/plan-volumes/mirror-accelerated-parity-storage-efficiency.png)<br> Varie selon la proportion de mise en miroir et parité | ![Affichage d’environ 20 % des performances](media/plan-volumes/mirror-accelerated-parity-perf.png)<br>Beaucoup plus lent que mettre en miroir, mais jusqu'à deux fois plus vite avec double parité<br> Meilleur pour les lectures et écritures séquentielles de grande taille | Archivage et sauvegarde<br> Infrastructure de bureau virtuel     |
| **Double parité**               | ![Affichage d’environ 80 % de l’efficacité du stockage](media/plan-volumes/dual-parity-storage-efficiency.png)<br>4 serveurs : 50% <br>16 serveurs : jusqu'à 80 % | ![Affichage d’environ 10 % des performances](media/plan-volumes/dual-parity-perf.png)<br>Latence d’e/s la plus élevée et l’utilisation du processeur sur les écritures<br> Meilleur pour les lectures et écritures séquentielles de grande taille | Archivage et sauvegarde<br> Infrastructure de bureau virtuel  |

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
   > Si vous observez une brusque baisse des performances de l’écriture en cours via la réception de données, elle peut indiquer que la partie de la mise en miroir n’est pas assez grande ou de parité avec accélération miroir n’est pas bien adaptée à votre cas d’utilisation. Par exemple, si écrire des diminutions de performances à partir de 400 Mo/s à 40 Mo/s, envisagez de développer la partie de la mise en miroir ou le basculement vers le miroir triple.

### <a name="about-deployments-with-nvme-ssd-and-hdd"></a>À propos des déploiements utilisant des disques NVMe, SSD et HDD

Dans les déploiements avec deux types de disques, les disques plus rapides assurent la mise en cache, tandis que les disques plus lents fournissent la capacité. Ceci est configuré automatiquement. Pour plus d’informations, voir [Fonctionnement du cache dans les espaces de stockage direct](understand-the-cache.md). Dans ces déploiements, tous les volumes résident sur le même type de disques, à savoir les disques de capacité.

Dans les déploiements utilisant les trois types de disques, seuls les disques les plus rapides (NVMe) assurent la mise en cache, tandis que les deux autres types de disques (SSD et HDD) fournissent la capacité. Pour chaque volume, vous pouvez choisir s'il doit résider entièrement sur le niveau SSD, entièrement sur le niveau HDD, ou s'il doit s'étendre sur les deux.

   > [!IMPORTANT]
   > Nous vous recommandons d’utiliser le niveau SSD pour placer vos charges de travail les plus sensibles aux performances sur du 100 % Flash.

## <a name="choosing-the-size-of-volumes"></a>Choix de la taille des volumes

Nous vous recommandons de limiter la taille de chaque volume à :

| Windows Server 2016 | Windows Server 2019 |
| ------------------- | ------------------- |
| Jusqu'à 32 To         | Jusqu'à 64 To         |

   > [!TIP]
   > Si vous utilisez une solution de sauvegarde qui s’appuie sur le service de cliché instantané de Volume (VSS) et le fournisseur de logiciel Volsnap, comme cela est courant avec les charges de travail de fichiers serveur, limitant la taille du volume à 10 To améliore les performances et fiabilité. Les solutions de sauvegarde qui utilisent l'API plus récente Hyper-V RCT et/ou le clonage de bloc sur ReFS et/ou les API de sauvegarde SQL natives fonctionnent bien jusqu'à 32 To et au-delà.

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

## <a name="example-capacity-planning"></a>Exemple : Planification de la capacité

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

- [Vue d’ensemble Direct des espaces de stockage](storage-spaces-direct-overview.md)
- [Choix des lecteurs pour les espaces de stockage Direct](choosing-drives.md)
- [Tolérance de panne et efficacité du stockage](storage-spaces-fault-tolerance.md)
