---
title: Fonctionnement du cache dans les espaces de stockage direct
ms.assetid: 69b1adc0-ee64-4eed-9732-0fb216777992
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: dongill
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 07/18/2017
ms.localizationpriority: medium
ms.openlocfilehash: 62fa33d08af25c424c786c10191fe6ae2b3d02bc
ms.sourcegitcommit: dfd25348ea3587e09ea8c2224237a3e8078422ae
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/16/2018
ms.locfileid: "4678618"
---
# Fonctionnement du cache dans les espaces de stockage direct

>S’applique à: Windows Server 2019, Windows Server 2016

Les [espaces de stockage direct](storage-spaces-direct-overview.md) tirent profit d'un cache intégré côté serveur pour optimiser les performances de stockage. C'est un cache volumineux, persistant et en temps réel pour la lecture *et* l'écriture. Le cache est configuré automatiquement lorsque les espaces de stockage direct sont activés. Dans la plupart des cas, aucune gestion manuelle n'est requise.
Le fonctionnement du cache dépend des types de lecteurs présents.

La vidéo suivante donne des informations détaillées sur le fonctionnement de la mise en cache des espaces de stockage direct, ainsi que d'autres considérations de conception.

<strong>Considérations en matière de conception d'espaces de stockage direct</strong><br>(20minutes)<br>
<iframe src="https://channel9.msdn.com/Blogs/windowsserver/Design-Considerations-for-Storage-Spaces-Direct/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## Types de lecteurs et options de déploiement

Les espaces de stockage direct prennent actuellement en charge trois types de dispositifs de stockage:

<table>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/NVMe-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            Mémoire non volatile NVMe (Non-Volatile Memory Express)
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/SSD-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            Disque SSD (Solid-State Drive) SATA/SAS
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/HDD-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            Disque dur HDD (Hard Disk Drive)
        </td>
    </tr>
</table>

Ces formats peuvent être combinés de six façons, que nous regroupons dans les deux catégories «100%Flash» et «hybride».

### Possibilités de déploiement 100% Flash

Les déploiements 100% Flash visent à optimiser les performances de stockage, et n'incluent pas de disques durs rotatifs (HDD).

![All-Flash-Deployment-Possibilities](media/understand-the-cache/All-Flash-Deployment-Possibilities.png)

### Possibilités de déploiement hybride

Les déploiements hybrides visent à équilibrer performances et capacité de stockage, ou à optimiser la capacité. Ils comprennent des disques durs rotatifs (HDD).

![Possibilités de déploiement hybride](media/understand-the-cache/Hybrid-Deployment-Possibilities.png)

## Les lecteurs de cache sont sélectionnés automatiquement

Dans les déploiements comprenant plusieurs types de lecteurs, les espaces de stockage direct utilisent automatiquement tous les lecteurs les plus «rapides» pour la mise en cache. Les lecteurs restants sont utilisés pour la capacité.

Les types de lecteurs les plus «rapides» sont déterminés selon la hiérarchie suivante.

![Hiérarchie des types de lecteurs](media/understand-the-cache/Drive-Type-Hierarchy.png)

Par exemple, si vous avez des lecteurs NVMe et SSD, les NVMe seront utilisés pour la mise en cache des SSD.

Si vous avez des lecteurs SSD et HDD, les SSD seront utilisés pour la mise en cache des HDD.

   >[!NOTE]
   > Les lecteurs de cache ne sont pas compris dans la capacité de stockage utilisable. Toutes les données stockées en cache sont également stockées ailleurs, même si cette opération peut se faire lors d'une étape ultérieure. Autrement dit, la capacité de stockage brute totale de votre déploiement correspond à la somme de vos lecteurs de capacité uniquement.

Lorsque les lecteurs sont tous du même type, aucun cache n'est configuré automatiquement. Vous avez la possibilité de configurer des lecteurs plus endurants pour la mise en cache de lecteurs moins endurants du même type. Pour en savoir plus, consultez la section [Configuration manuelle](#manual).

   >[!TIP]
   > Dans les déploiements 100%NVMe ou 100%SSD, en particulier à très petite échelle, vous pouvez améliorer considérablement l'efficacité du stockage en ne «gaspillant» aucun lecteur pour la mise en cache.

## Le comportement du cache est défini de manière automatique

Le comportement du cache est déterminé automatiquement en fonction du ou des types de lecteurs utilisés. Si vous créez un cache pour des disques SSD (par exemple, avec des NVMe), seules les écritures sont mises en cache. Si vous créez un cache pour des disques HDD (par exemple, avec des SSD), les écritures et les lectures sont mises en cache.

![Comportement en lecture et en écriture du cache](media/understand-the-cache/Cache-Read-Write-Behavior.png)

### Mise en cache en écriture seulement pour les déploiements 100% Flash

Si vous créez un cache pour des disques SSD (NVMe ou SSD), seules les écritures sont mises en cache. Avec cette méthode, les lecteurs de capacité s'usent moins rapidement. En effet, vous pouvez regrouper de nombreuses écritures et réécritures dans le cache, et ne les déstocker que lorsque c'est nécessaire. Le trafic cumulatif sur les lecteurs de capacité est ainsi réduit, ce qui permet de prolonger leur durée de vie. Pour cette raison, nous vous recommandons de sélectionner des lecteurs [très endurants et optimisés pour l'écriture](http://whatis.techtarget.com/definition/DWPD-device-drive-writes-per-day) pour le cache. Vous pouvez raisonnablement choisir des lecteurs de capacité moins endurants en écriture.

Les lectures ne sont pas mises en cache, étant donné qu'elles n'affectent pas la durée de vie des lecteurs Flash de manière significative, et que les disques SSD offrent généralement une faible latence dans ce domaine. Aussi, elles arrivent directement des lecteurs de capacité (sauf quand les données ont été écrites trop récemment et qu'elles n'ont pas encore été déstockées du cache). Le cache est ainsi dédié intégralement aux écritures pour une efficacité optimale.

Par conséquent, les caractéristiques d'écriture, comme la latence, sont dictées par les lecteurs de cache, alors que les caractéristiques de lecture sont dictées par les lecteurs de capacité. Dans les deux cas, elles sont cohérentes, prévisibles et uniformes.

### Mise en cache en lecture/écriture pour les déploiements hybrides

Si vous créez un cache pour des disques HDD, les écritures *et* les lectures sont mises en cache. Dans les deux cas, la latence est équivalente à celle des lecteurs Flash (souvent env. 10x meilleure). Le cache de lecture stocke les données lues récemment et fréquemment pour un accès rapide et pour réduire au maximum le trafic aléatoire vers les disques durs. (En raison des délais de recherche et de rotation, la latence et le temps perdu causés par l'accès aléatoire à un disque dur sont considérables.) Les écritures sont mises en cache pour absorber les pics de trafic, et comme précédemment, vous pouvez regrouper les écritures et réécritures afin de réduire au maximum le trafic cumulatif sur les lecteurs de capacité.

Les espaces de stockage direct font appel à un algorithme qui annule l'aspect aléatoire des écritures avant de les déstocker du cache. Cela permet d'émuler un schéma d'E/S d'apparence séquentielle au niveau du lecteur, même quand les E/S réelles provenant de la charge de travail (par exemple, des machines virtuelles) sont aléatoires. Les E/S par seconde et le débit au niveau des disques durs sont ainsi optimisés.

### Mise en cache dans des déploiements comprenant les trois types de lecteurs

Lorsque les trois types de lecteurs sont présents, les lecteurs NVMe fournissent le cache pour les disques SSD et HDD. Le comportement est celui décrit ci-dessus: seules les écritures sont mises en cache pour les SSD. Pour les HDD, les lectures et les écritures sont mises en cache. Le fardeau de la mise en cache pour les HDD est réparti équitablement entre les lecteurs de cache. 

## Récapitulatif

Ce tableau récapitule les lecteurs utilisés pour la mise en cache et la capacité, et rappelle les comportements de cache associés à chaque déploiement.

| Déploiement       | Lecteurs de cache                        | Lecteurs de capacité | Comportement du cache (par défaut)                  |
|------------------|-------------------------------------|-----------------|-------------------------------------------|
| Uniquement des disques NVMe         | Aucun (configuration manuelle possible, mais facultative) | NVMe            | Écriture seulement (si configuré)                |
| Disques SSD uniquement          | Aucun (configuration manuelle possible, mais facultative) | SSD             | Écriture seulement (si configuré)                |
| NVMe + SSD       | NVMe                                | SSD             | Écriture seulement                                |
| NVMe + HDD       | NVMe                                | HDD             | Lecture + Écriture                              |
| SSD + HDD        | SSD                                 | HDD             | Lecture + Écriture                              |
| NVMe + SSD + HDD | NVMe                                | SSD + HDD       | Lecture + Écriture pour les HDD, Écriture seulement pour les SSD  |

## Architecture côté serveur

Le cache est mis en œuvre côté lecteur: les différents lecteurs formant le cache au niveau d'un serveur sont reliés à un ou plusieurs lecteurs de capacité au sein de ce même serveur.

Étant donné que le cache est situé sous le reste de la pile de stockage à définition logicielle Windows, les concepts d'espaces de stockage et de tolérance de panne ne sont pas implémentés, ni nécessaires. Vous pouvez considérer cela comme des lecteurs «hybrides» (mi-Flash, mi-disque) qui sont ensuite présentés à Windows. Comme avec un véritable lecteur hybride, le mouvement en temps réel des données à chaud et à froid entre les portions plus rapides et plus lentes du support physique est presque invisible de l'extérieur.

Étant donné que la résilience dans les espaces de stockage direct relève au moins du niveau serveur (à savoir que les copies des données sont toujours écrites sur différents serveurs, avec au moins une copie par serveur), les données en cache bénéficient de la même résilience que les données qui ne sont pas en cache.

![Architecture de cache côté serveur](media/understand-the-cache/Cache-Server-Side-Architecture.png)

Par exemple, lorsque vous utilisez une mise en miroir triple, trois copies des données sont écrites sur différents serveurs quand elles arrivent en cache. Indépendamment du fait qu'elles soient ou non déstockées du cache, il existera toujours trois copies.

## Les mappages de lecteurs sont dynamiques

Le mappage entre les lecteurs de cache et de capacité peut être effectué selon n'importe quel ratio, de 1:1 jusqu'à 1:12 et au-delà. Il s'ajuste automatiquement lors de l'ajout ou du retrait de lecteurs, par exemple quand vous agrandissez le système ou en cas de panne. Cela signifie que vous pouvez ajouter des lecteurs de cache ou de capacité de manière indépendante, quand vous le souhaitez.

![Mappage dynamique](media/understand-the-cache/Dynamic-Binding.gif)

Nous vous recommandons de choisir un nombre de lecteurs de capacité qui soit un multiple du nombre de lecteurs de cache, pour des raisons de symétrie. Par exemple, si vous avez quatre lecteurs de cache, vous obtiendrez des performances plus équilibrées avec huit lecteurs de capacité plutôt qu'avec sept ou neuf.

## Gestion des pannes au niveau des lecteurs de cache

En cas de panne d'un lecteur de cache, les écritures qui n'ont pas encore été déstockées de ce dernier sont perdues *pour le serveur local*, autrement dit il ne reste que les autres copies (sur les autres serveurs). Tout comme dans n'importe quel scénario de panne de lecteur, les espaces de stockage récupèrent automatiquement les données en consultant les copies survivantes.

Pendant une brève période, les lecteurs de capacité rattachés au lecteur de cache perdu seront considérés comme défectueux. Une fois que le mappage de cache aura été renouvelé (automatiquement) et que les données auront été réparées (automatiquement), ces lecteurs seront de nouveau considérés comme intègres.

C'est pour cette raison qu'il faut au moins deux lecteurs de cache par serveur pour préserver les performances.

![Gestion des pannes](media/understand-the-cache/Handling-Failure.gif)

Vous pouvez remplacer un lecteur de cache tout comme n'importe quel autre lecteur.

   >[!NOTE]
   > Il vous faudra peut-être couper l'alimentation pour remplacer en toute sécurité un disque NVMe de type carte d'extension ou M.2.

## Relation avec les autres caches

Il existe plusieurs autres caches non reliés dans la pile de stockage à définition logicielle Windows. Par exemple, le cache en écriture différée des espaces de stockage et le cache de lecture en mémoire pour le volume partagé de cluster.

Avec les espaces de stockage direct, le comportement par défaut du cache en écriture différée des espaces de stockage ne doit pas être modifié. Par exemple, vous ne pouvez pas utiliser de paramètres tels que **-WriteCacheSize** sur l'applet de commande **New-Volume**.

Vous pouvez utiliser ou non le cache de volume partagé de cluster, au choix. Il est désactivé par défaut dans les espaces de stockage direct, mais il n'entre pas en conflit avec le nouveau cache décrit dans cette rubrique. Dans certains cas, il peut grandement améliorer les performances. Pour plus d'informations, consultez le message de blog [How to Enable CSV Cache](https://blogs.msdn.microsoft.com/clustering/2013/07/19/how-to-enable-csv-cache/) (Comment activer le cache de volume partagé de cluster).

## <a name="manual"></a> Configuration manuelle

Pour la plupart des déploiements, la configuration manuelle n'est pas requise. Si vous en avez besoin, lisez ce qui suit.

### Spécifier le modèle du lecteur de cache

Dans les déploiements où tous les lecteurs sont de même type (par exemple, uniquement NVMe ou uniquement SSD), aucun cache n'est configuré, car Windows ne distingue pas automatiquement les caractéristiques telles que l'endurance en écriture sur des lecteurs de même type.

Pour utiliser les lecteurs les plus endurants pour la mise en cache et les lecteurs les moins endurants pour la capacité, vous pouvez spécifier le modèle de lecteur à utiliser avec le paramètre **-CacheDeviceModel** de l'applet de commande **Enable-ClusterS2D**. Une fois les espaces de stockage direct activés, tous les lecteurs de ce modèle seront utilisés pour le cache.

   >[!TIP]
   > Assurez-vous d'utiliser exactement le modèle spécifié en sortie de **Get-PhysicalDisk**.

####  Exemple

```
PS C:\> Get-PhysicalDisk | Group Model -NoElement

Count Name
----- ----
    8 FABRIKAM NVME-1710
   16 CONTOSO NVME-1520

PS C:\> Enable-ClusterS2D -CacheDeviceModel "FABRIKAM NVME-1710"
```

Vous pouvez vérifier que les lecteurs que vous avez choisis sont utilisés pour le cache en exécutant **Get-PhysicalDisk** dans PowerShell et en vérifiant que la propriété **Usage** indique bien **"Journal"**.

### Possibilités de déploiement manuel

La configuration manuelle offre les possibilités de déploiement suivantes:

![Possibilités de déploiement exotiques](media/understand-the-cache/Exotic-Deployment-Possibilities.png)

### Définir le comportement du cache

Vous pouvez changer le comportement par défaut du cache. Par exemple, vous pouvez mettre en cache les lectures, même dans un déploiement 100% Flash. Nous vous déconseillons de modifier ce comportement à moins d'être certain que le comportement par défaut ne convient pas à votre charge de travail.

Pour modifier le comportement, utilisez l'applet de commande **Set-ClusterS2D** et ses paramètres **-CacheModeSSD** et **-CacheModeHDD**. Le paramètre **CacheModeSSD** définit le comportement d'un cache associé à des disques SSD. Le paramètre **CacheModeHDD** définit le comportement d'un cache associé à des disques durs. Vous pouvez le faire à tout moment une fois les espaces de stockage activés.

Vous pouvez utiliser **Get-ClusterS2D** pour vérifier le comportement.

#### Exemple

```
PS C:\> Get-ClusterS2D

CacheModeHDD : ReadWrite
CacheModeSSD : WriteOnly
...

PS C:\> Set-ClusterS2D -CacheModeSSD ReadWrite

PS C:\> Get-ClusterS2D

CacheModeHDD : ReadWrite
CacheModeSSD : ReadWrite
...
```

## Définir la taille du cache

La taille du cache doit être définie en fonction des besoins de votre charge de travail ou de vos applications (les données lues ou écrites activement à un instantT).

C'est important notamment pour les déploiements hybrides avec des disques durs. Si les plages de travail actives sont plus volumineuses que votre cache ou si elles dérivent trop vite, le nombre de lectures manquées risque d'augmenter et il faudra déstocker les écritures plus agressivement, ce qui nuira aux performances globales.

Vous pouvez utiliser l'utilitaire intégré Analyseur de performances (PerfMon.exe) dans Windows pour inspecter le taux d'absences dans le cache. Plus particulièrement, vous pouvez comparer la valeur **Cache Miss Reads/sec** au niveau du décompte **Cluster Storage Hybrid Disk** et le nombre total d'E/S par seconde en lecture de votre déploiement. Chaque «lecteur hybride» correspond à un lecteur de capacité.

Par exemple, deux lecteurs de cache pour quatre lecteurs de capacité vous donneront quatre instances de «lecteurs hybrides» par serveur.

![Analyse des performances](media/understand-the-cache/PerfMon.png)

Il n'y a pas de règle universelle, mais si vous constatez qu'il manque trop de lectures au niveau du cache, ce dernier est probablement trop petit et vous devez envisager d'ajouter des lecteurs de cache. Vous pouvez ajouter des lecteurs de cache ou de capacité indépendamment les uns des autres, à tout moment.

## Voir aussi

- [Choix des lecteurs et des types de résilience](choosing-drives.md)
- [Tolérance de panne et efficacité du stockage](storage-spaces-fault-tolerance.md)
- [Configuration matérielle requise pour les espaces de stockage direct](storage-spaces-direct-hardware-requirements.md)
