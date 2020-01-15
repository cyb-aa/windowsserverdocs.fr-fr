---
title: Réglage des performances Bureau à distance les hôtes de virtualisation
description: Réglage des performances pour les hôtes de virtualisation Bureau à distance
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: HammadBu; VladmiS; denisgun
author: phstee
ms.date: 10/22/2019
ms.openlocfilehash: 1b66f6404df5debee2a4c52ffc9166c8eabb9f81
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75947126"
---
# <a name="performance-tuning-remote-desktop-virtualization-hosts"></a>Réglage des performances Bureau à distance les hôtes de virtualisation

Serveur hôte de virtualisation des services Bureau à distance (ordinateur hôte de virtualisation des services Bureau à distance) est un service de rôle qui prend en charge les scénarios d’infrastructure VDI (Virtual Desktop Infrastructure) et permet à plusieurs utilisateurs d’exécuter des applications Windows sur des ordinateurs virtuels hébergés sur un serveur exécutant Windows Server et Hyper-V.

Windows Server prend en charge deux types de bureaux virtuels : les bureaux virtuels personnels et les bureaux virtuels mis en pool.

## <a name="general-considerations"></a>Observations générales

### <a name="storage"></a>Stockage

Le stockage est le goulot d’étranglement des performances le plus probable, et il est important de dimensionner votre stockage pour gérer correctement la charge d’e/s générée par les modifications de l’état de l’ordinateur virtuel. Si un pilote ou une simulation n’est pas possible, il est judicieux de configurer une seule pile de disques pour quatre machines virtuelles actives. Utilisez des configurations de disque qui présentent de bonnes performances en écriture (par exemple, RAID 1 + 0).

Le cas échéant, utilisez la déduplication de disque et la mise en cache pour réduire la charge de lecture sur le disque et permettre à votre solution de stockage d’accélérer les performances en mettant en cache une partie importante de l’image.

### <a name="data-deduplication-and-vdi"></a>Déduplication des données et VDI

Introduite dans Windows Server 2012 R2, la déduplication des données prend en charge l’optimisation des fichiers ouverts. Pour pouvoir utiliser des machines virtuelles exécutées sur un volume dédupliqué, les fichiers de l’ordinateur virtuel doivent être stockés sur un hôte distinct de l’hôte Hyper-V. Si Hyper-V et la déduplication s’exécutent sur le même ordinateur, les deux fonctionnalités sont en concurrence pour les ressources système et ont un impact négatif sur les performances globales.

Le volume doit également être configuré pour utiliser le type d’optimisation de déduplication « VDI (Virtual Desktop Infrastructure) ». Vous pouvez configurer ce paramètre à l’aide de Gestionnaire de serveur (**services de fichiers et de stockage** -&gt; **volumes** -&gt; **paramètres de déduplication**) ou à l’aide de la commande Windows PowerShell suivante :

``` syntax
Enable-DedupVolume <volume> -UsageType HyperV
```

> [!NOTE]
> L’optimisation de la déduplication des données des fichiers ouverts est prise en charge uniquement pour les scénarios VDI avec Hyper-V utilisant un stockage distant sur SMB 3,0.

### <a name="memory"></a>Memory

L’utilisation de la mémoire du serveur est motivée par trois facteurs principaux :

- Surcharge du système d’exploitation

- Surcharge du service Hyper-V par ordinateur virtuel

- Mémoire allouée à chaque machine virtuelle

Pour une charge de travail de travail de base de connaissances classique, les machines virtuelles invitées exécutant x86 Window 8 ou Windows 8.1 doivent disposer d’environ 512 Mo de mémoire comme ligne de base. Toutefois, Mémoire dynamique augmentera probablement la mémoire de la machine virtuelle invitée à environ 800 Mo, en fonction de la charge de travail. Pour x64, nous voyons environ 800 Mo de démarrage, ce qui devient de 1024 Mo.

Par conséquent, il est important de fournir suffisamment de mémoire serveur pour satisfaire la mémoire requise par le nombre attendu de machines virtuelles invitées, ainsi que d’une quantité suffisante de mémoire pour le serveur.

### <a name="cpu"></a>CPU

Lorsque vous planifiez la capacité du serveur pour un serveur hôte de virtualisation des services Bureau à distance, le nombre d’ordinateurs virtuels par cœur physique dépend de la nature de la charge de travail. Comme point de départ, il est raisonnable de planifier 12 machines virtuelles par cœur physique, puis d’exécuter les scénarios appropriés pour valider les performances et la densité. Une densité plus élevée peut être réalisable en fonction des caractéristiques de la charge de travail.

Nous vous recommandons d’activer l’Hyper-Threading, mais de calculer le taux de surabonnement en fonction du nombre de cœurs physiques et non du nombre de processeurs logiques. Cela garantit le niveau de performance attendu sur une base par UC.

## <a name="performance-optimizations"></a>Optimisation des performances

### <a name="dynamic-memory"></a>Mémoire dynamique

Mémoire dynamique permet une utilisation plus efficace des ressources mémoire du serveur exécutant Hyper-V en équilibrant la répartition de la mémoire entre les machines virtuelles en cours d’exécution. La mémoire peut être réallouée dynamiquement entre les machines virtuelles en réponse à leurs charges de travail variables.

Mémoire dynamique vous permet d’augmenter la densité des machines virtuelles avec les ressources que vous avez déjà sans sacrifier les performances ou l’évolutivité. Le résultat est une utilisation plus efficace des ressources matérielles serveur onéreuses, ce qui peut être plus facile à gérer et à réduire les coûts.

Sur les systèmes d’exploitation invités exécutant Windows 8 et versions ultérieures avec des processeurs virtuels qui s’étendent sur plusieurs processeurs logiques, considérez le compromis entre l’exécution avec Mémoire dynamique pour réduire l’utilisation de la mémoire et la désactivation des Mémoire dynamique pour améliorer les performances. d’une application qui prend en charge la topologie de l’ordinateur. Une telle application peut exploiter les informations de topologie pour prendre des décisions de planification et d’allocation de mémoire.

### <a name="tiered-storage"></a>Stockage à plusieurs niveaux

L’hôte de virtualisation des services Bureau à distance prend en charge le stockage hiérarchisé pour les pools de bureaux virtuels. L’ordinateur physique qui est partagé par tous les bureaux virtuels mis en pool au sein d’un regroupement peut utiliser une solution de stockage de petite taille et hautes performances, telle qu’un disque SSD en miroir. Les bureaux virtuels mis en pool peuvent être placés sur un stockage traditionnel moins onéreux, tel que RAID 1 + 0.

L’ordinateur physique doit être placé sur un disque SSD, car la plupart des e/s lues à partir de bureaux virtuels mis en pool sont dirigés vers le système d’exploitation de gestion. Par conséquent, le stockage utilisé par l’ordinateur physique doit supporter des e/s de lecture plus élevées par seconde.

Cette configuration de déploiement garantit des performances rentables lorsque les performances sont nécessaires. L’SSD offre des performances supérieures sur un disque de plus petite taille (environ 20 Go par collection, en fonction de la configuration). Le stockage traditionnel pour les bureaux virtuels mis en pool (RAID 1 + 0) utilise environ 3 Go par machine virtuelle.

### <a name="csv-cache"></a>Cache de volume partagé de cluster

Le clustering de basculement dans Windows Server 2012 et versions ultérieures fournit la mise en cache sur les volumes partagés de cluster (CSV). Cela est extrêmement bénéfique pour les collections de bureaux virtuels mis en pool où la majorité des e/s de lecture proviennent du système d’exploitation de gestion. Le cache de volume partagé de cluster fournit des performances supérieures par plusieurs ordres de grandeur, car il met en cache des blocs qui sont lus plusieurs fois et les remet à partir de la mémoire système, ce qui réduit les e/s. Pour plus d’informations sur le cache de volume partagé de cluster, consultez [la rubrique activation du cache de](https://blogs.msdn.com/b/clustering/archive/2012/03/22/10286676.aspx)volume partagé de cluster.

### <a name="pooled-virtual-desktops"></a>Bureaux virtuels mis en pool

Par défaut, les bureaux virtuels mis en pool sont restaurés à l’état initial après la déconnexion d’un utilisateur, si bien que toute modification apportée au système d’exploitation Windows depuis la dernière connexion de l’utilisateur est abandonnée.

Bien qu’il soit possible de désactiver la restauration, il s’agit toujours d’une condition temporaire, car généralement, une collection de bureaux virtuels mis en pool est recréée en raison de diverses mises à jour apportées au modèle de bureau virtuel.

Il est logique de désactiver les fonctionnalités et services Windows qui dépendent de l’état persistant. En outre, il est logique de désactiver les services principalement pour les scénarios n’appartenant pas à l’entreprise.

Chaque service spécifique doit être évalué de manière appropriée avant tout déploiement global. Voici quelques éléments à prendre en compte :

| Service                                      | Pourquoi ?                                                                                                                                                                                                      |
|----------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Mise à jour automatique                                  | Les bureaux virtuels mis en pool sont mis à jour en recréant le modèle de bureau virtuel.                                                                                                                          |
| Fichiers hors connexion                                | Les bureaux virtuels sont toujours en ligne et connectés à partir d’un point de vue réseau.                                                                                                                         |
| Défragmentation en arrière-plan                            | Les modifications du système de fichiers sont ignorées lorsqu’un utilisateur se déconnecte (en raison d’une restauration à l’état initial ou de la recréation du modèle de bureau virtuel, ce qui entraîne la recréation de tous les bureaux virtuels mis en pool). |
| Mettre en veille prolongée ou mettre en veille                           | Aucun concept de ce type pour l’infrastructure VDI                                                                                                                                                                                   |
| Vidage de la mémoire de vérification des bogues                        | Ce concept n’existe pas pour les bureaux virtuels mis en pool. Un bureau virtuel mis en pool de vérification des bogues démarre à partir de l’état initial.                                                                                       |
| Configuration automatique WLAN                              | Il n’y a pas d’interface de périphérique Wi-Fi pour VDI                                                                                                                                                                 |
| Service de partage réseau du lecteur Windows Media | Service centré sur les consommateurs                                                                                                                                                                                  |
| Fournisseur du groupe résidentiel                          | Service centré sur les consommateurs                                                                                                                                                                                  |
| Partage de connexion Internet                  | Service centré sur les consommateurs                                                                                                                                                                                  |
| Services étendus Media Center               | Service centré sur les consommateurs                                                                                                                                                                                  |
> [!NOTE]
> Cette liste n’est pas censée être une liste complète, car toutes les modifications affectent les objectifs et les scénarios prévus. Pour plus d’informations, reportez-vous aux [pressages, obtenez-le maintenant, le script d’optimisation de l’infrastructure VDI de Windows 8, avec l’aimable autorisation ingénieurs PFE !](https://blogs.technet.com/b/jeff_stokes/archive/2013/04/09/hot-off-the-presses-get-it-now-the-windows-8-vdi-optimization-script-courtesy-of-pfe.aspx).


> [!NOTE]
> SuperFetch dans Windows 8 est activé par défaut. Elle prend en charge l’infrastructure VDI et ne doit pas être désactivée. SuperFetch permet de réduire davantage la consommation de mémoire via le partage de pages mémoire, ce qui est bénéfique pour l’infrastructure VDI. Les bureaux virtuels mis en pool exécutant Windows 7, SuperFetch doivent être désactivés, mais pour les bureaux virtuels personnels exécutant Windows 7, il doit être conservé.
