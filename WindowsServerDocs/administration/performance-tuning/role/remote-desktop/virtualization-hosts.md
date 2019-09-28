---
title: Réglage des performances Bureau à distance les hôtes de virtualisation
description: Réglage des performances pour les hôtes de virtualisation Bureau à distance
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: HammadBu; VladmiS
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 6aad1560fa9f9429af94426487d9a33369137ded
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370030"
---
# <a name="performance-tuning-remote-desktop-virtualization-hosts"></a>Réglage des performances Bureau à distance les hôtes de virtualisation


Serveur hôte de virtualisation des services Bureau à distance (ordinateur hôte de virtualisation des services Bureau à distance) est un service de rôle qui prend en charge les scénarios d’infrastructure VDI (Virtual Desktop Infrastructure) et permet à plusieurs utilisateurs simultanés d’exécuter des applications Windows sur des ordinateurs virtuels hébergés sur un serveur exécutant Windows Server 2016 et Hyper-V.

Windows Server 2016 prend en charge deux types de bureaux virtuels, de bureaux virtuels personnels et de bureaux virtuels mis en pool.

**Dans cette rubrique :**

-   [Considérations générales](#general-considerations)

-   [Optimisations des performances](#performance-optimizations)

## <a name="general-considerations"></a>Considérations générales


### <a name="storage"></a>Stockage

Le stockage est le goulot d’étranglement des performances le plus probable, et il est important de dimensionner votre stockage pour gérer correctement la charge d’e/s générée par les modifications de l’état de l’ordinateur virtuel. Si un pilote ou une simulation n’est pas possible, il est judicieux de configurer une seule pile de disques pour quatre machines virtuelles actives. Utilisez des configurations de disque qui présentent de bonnes performances en écriture (par exemple, RAID 1 + 0).

Le cas échéant, utilisez la déduplication de disque et la mise en cache pour réduire la charge de lecture sur le disque et permettre à votre solution de stockage d’accélérer les performances en mettant en cache une partie importante de l’image.

### <a name="data-deduplication-and-vdi"></a>Déduplication des données et VDI

Introduite dans Windows Server 2012 R2, la déduplication des données prend en charge l’optimisation des fichiers ouverts. Pour pouvoir utiliser des machines virtuelles exécutées sur un volume dédupliqué, les fichiers de l’ordinateur virtuel doivent être stockés sur un hôte distinct de l’hôte Hyper-V. Si Hyper-V et la déduplication s’exécutent sur le même ordinateur, les deux fonctionnalités sont en concurrence pour les ressources système et ont un impact négatif sur les performances globales.

Le volume doit également être configuré pour utiliser le type d’optimisation de déduplication « VDI (Virtual Desktop Infrastructure) ». Vous pouvez configurer ce paramètre à l’aide de gestionnaire de serveur ( **paramètres de déduplication**&gt; des **volumes**  -  - &gt; **des services de fichiers et de stockage** ) ou à l’aide de la commande Windows PowerShell suivante :

``` syntax
Enable-DedupVolume <volume> -UsageType HyperV
```

> [!NOTE]
> L’optimisation de la déduplication des données des fichiers ouverts est prise en charge uniquement pour les scénarios VDI avec Hyper-V utilisant un stockage distant sur SMB 3,0.

### <a name="memory"></a>Mémoire

L’utilisation de la mémoire du serveur est motivée par trois facteurs principaux :

-   Surcharge du système d’exploitation

-   Surcharge du service Hyper-V par ordinateur virtuel

-   Mémoire allouée à chaque machine virtuelle

Pour une charge de travail de travail de base de connaissances classique, les machines virtuelles invitées exécutant x86 Window 8 ou Windows 8.1 doivent disposer d’environ 512 Mo de mémoire comme ligne de base. Toutefois, Mémoire dynamique augmentera probablement la mémoire de la machine virtuelle invitée à environ 800 Mo, en fonction de la charge de travail. Pour x64, nous voyons environ 800 Mo de démarrage, ce qui devient de 1024 Mo.

Par conséquent, il est important de fournir suffisamment de mémoire serveur pour satisfaire la mémoire requise par le nombre attendu de machines virtuelles invitées, ainsi que d’une quantité suffisante de mémoire pour le serveur.

### <a name="cpu"></a>Processeur

Lorsque vous planifiez la capacité du serveur pour un serveur hôte de virtualisation des services Bureau à distance, le nombre d’ordinateurs virtuels par cœur physique dépend de la nature de la charge de travail. Comme point de départ, il est raisonnable de planifier 12 machines virtuelles par cœur physique, puis d’exécuter les scénarios appropriés pour valider les performances et la densité. Une densité plus élevée peut être réalisable en fonction des caractéristiques de la charge de travail.

Nous vous recommandons d’activer l’Hyper-Threading, mais de calculer le taux de surabonnement en fonction du nombre de cœurs physiques et non du nombre de processeurs logiques. Cela garantit le niveau de performance attendu sur une base par UC.

### <a name="virtual-gpu"></a>GPU virtuel

Microsoft RemoteFX pour l’hôte de virtualisation des services Bureau à distance offre une expérience graphique riche pour l’infrastructure VDI (Virtual Desktop Infrastructure) via la communication à distance côté hôte, un pipeline de capture et de capture de rendu, un encodage basé sur GPU très efficace, une limitation basée sur le client et un GPU virtuel compatible DirectX. RemoteFX pour hôte de virtualisation des services Bureau à distance met à niveau le GPU virtuel de DirectX9 vers DirectX11. Il améliore également l’expérience utilisateur en prenant en charge davantage d’analyses à des résolutions supérieures.

L’expérience DirectX11 RemoteFX est disponible sans GPU matériel, par le biais d’un pilote émulé par logiciel. Bien que ce logiciel GPU offre une bonne expérience, l’unité de traitement graphique (GPU) virtuelle RemoteFX ajoute une expérience d’accélération matérielle aux bureaux virtuels.

Pour tirer parti de l’expérience du processeur graphique virtuel RemoteFX sur un serveur exécutant Windows Server 2016, vous avez besoin d’un pilote GPU (tel que DirectX 11.1 ou WDDM 1,2) sur le serveur hôte. Pour plus d’informations sur les offres GPU à utiliser avec RemoteFX pour hôte de virtualisation des services Bureau à distance, contactez votre fournisseur GPU.

Si vous utilisez le GPU virtuel RemoteFX dans votre déploiement VDI, la capacité de déploiement varie en fonction des scénarios d’utilisation et de la configuration matérielle. Lorsque vous planifiez votre déploiement, tenez compte des éléments suivants :

-   Nombre de GPU sur votre système

-   Capacité de la mémoire vidéo sur les GPU

-   Ressources du processeur et du matériel sur votre système

### <a name="remotefx-server-system-memory"></a>Mémoire système du serveur RemoteFX

Pour chaque bureau virtuel activé avec un GPU virtuel, RemoteFX utilise la mémoire système dans le système d’exploitation invité et dans le serveur prenant en charge RemoteFX. L’hyperviseur garantit la disponibilité de la mémoire système pour un système d’exploitation invité. Sur le serveur, chaque bureau virtuel prenant en charge le GPU virtuel doit publier ses besoins en mémoire système sur l’hyperviseur. Lorsque le bureau virtuel compatible avec le GPU virtuel démarre, l’hyperviseur réserve de la mémoire système supplémentaire dans le serveur prenant en charge RemoteFX pour le bureau virtuel compatible avec les PROCESSEURs virtuels.

La mémoire requise pour le serveur RemoteFX est dynamique, car la quantité de mémoire consommée sur le serveur RemoteFX dépend du nombre d’analyses associées aux bureaux virtuels compatibles avec les PROCESSEURs virtuels et de la résolution maximale pour Ces analyses.

### <a name="remotefx-server-gpu-video-memory"></a>Mémoire vidéo du GPU du serveur RemoteFX

Chaque bureau virtuel prenant en charge le GPU virtuel utilise la mémoire vidéo dans le matériel du GPU sur le serveur hôte pour afficher le bureau. En plus du rendu, la mémoire vidéo est utilisée par un codec pour compresser l’écran rendu. La quantité de mémoire nécessaire dépend directement de la quantité d’analyses approvisionnées sur la machine virtuelle.

La mémoire vidéo réservée varie selon le nombre d’écrans et la résolution de l’écran système. Certains utilisateurs peuvent nécessiter une résolution d’écran plus élevée pour des tâches spécifiques. Il existe une plus grande évolutivité avec des paramètres de résolution inférieurs si tous les autres paramètres restent constants.

### <a name="remotefx-processor"></a>Processeur RemoteFX

L’hyperviseur planifie le serveur prenant en charge RemoteFX et les bureaux virtuels compatibles GPU virtuels sur le processeur. Contrairement à la mémoire système, il n’y a pas d’informations relatives aux ressources supplémentaires que RemoteFX doit partager avec l’hyperviseur. La surcharge d’UC supplémentaire introduite par RemoteFX dans le bureau virtuel prenant en charge le GPU virtuel est liée à l’exécution du pilote GPU virtuel et à la pile protocole RDP (Remote Desktop Protocol) en mode utilisateur.

Sur le serveur prenant en charge RemoteFX, la surcharge est augmentée car le système exécute un processus supplémentaire (rdvgm. exe) par bureau virtuel prenant en charge le GPU virtuel. Ce processus utilise le pilote de périphérique graphique pour exécuter des commandes sur le GPU. Le codec utilise également les processeurs pour compresser les données d’écran qui doivent être renvoyées au client.

Un plus grand nombre de processeurs virtuels signifie une meilleure expérience utilisateur. Nous vous recommandons d’allouer au moins deux processeurs virtuels par bureau virtuel compatible avec le GPU virtuel. Nous vous recommandons également d’utiliser l’architecture x64 pour les bureaux virtuels compatibles GPU, car les performances sur les machines virtuelles x64 sont meilleures par rapport aux machines virtuelles x86.

### <a name="remotefx-gpu-processing-power"></a>Puissance de traitement GPU RemoteFX

Pour chaque bureau virtuel compatible GPU, il existe un processus DirectX correspondant en cours d’exécution sur le serveur prenant en charge RemoteFX. Ce processus relit toutes les commandes graphiques qu’il reçoit du bureau virtuel RemoteFX sur le GPU physique. Pour le GPU physique, il est équivalent à l’exécution simultanée de plusieurs applications DirectX.

En règle générale, les périphériques et les pilotes graphiques sont réglés pour exécuter quelques applications sur le bureau. RemoteFX étire les GPU à utiliser de manière unique. Pour mesurer la manière dont le GPU s’exécute sur un serveur RemoteFX, des compteurs de performances ont été ajoutés pour mesurer la réponse GPU aux demandes RemoteFX.

Généralement, lorsqu’une ressource GPU manque de ressources, les opérations de lecture et d’écriture sur le GPU prennent beaucoup de temps. En utilisant des compteurs de performances, les administrateurs peuvent prendre des mesures préventives, éliminant ainsi le risque de temps d’arrêt pour leurs utilisateurs finaux.

Les compteurs de performances suivants sont disponibles sur le serveur RemoteFX pour mesurer les performances du GPU virtuel :

**Graphiques RemoteFX**

-   **Trames ignorées/seconde-ressources client insuffisantes** Nombre de trames ignorées par seconde en raison de ressources client insuffisantes

-   **Taux de compression des graphiques** Rapport entre le nombre d’octets encodés et le nombre d’octets entrés

**Gestion de GPU racine RemoteFX**

-   **Situées TDRS dans les GPU** du serveur nombre total de fois où le TDR expire dans le GPU sur le serveur

-   **Situées Machines virtuelles exécutant** RemoteFX nombre total d’ordinateurs virtuels sur lesquels la carte vidéo RemoteFX 3D est installée

-   **RAM Mo disponibles par processeur** graphique, quantité de mémoire vidéo dédiée non utilisée

-   **RAM % Réservé par GPU @ no__t-0% de la mémoire vidéo dédiée qui a été réservée pour RemoteFX

**Logiciel RemoteFX**

-   **Taux de capture pour l’analyse** \[1-4\] affiche le taux de capture RemoteFX pour les moniteurs 1-4

-   **Taux de compression** Déconseillé dans Windows 8 et remplacé par le **ratio de compression graphique**

-   **Images différées/s** Nombre d’images par seconde où les données graphiques n’ont pas été envoyées dans un laps de temps donné

-   **Temps de réponse GPU de capture** Latence mesurée au sein de la capture RemoteFX (en microsecondes) pour l’exécution des opérations GPU

-   **Temps de réponse GPU à partir du rendu** Latence mesurée dans RemoteFX Render (en microsecondes) pour l’exécution des opérations GPU

-   **Octets de sortie** Nombre total d’octets de sortie RemoteFX

-   En **attente du nombre de clients/s** Déconseillé dans Windows 8 et remplacé par des **frames ignorés/seconde-ressources client insuffisantes**

**Gestion du processeur graphique virtuel RemoteFX**

-   **Situées TDRS local aux machines** virtuelles nombre total de TDRS qui se sont produits dans cet ordinateur virtuel (TDRS que le serveur a propagé aux machines virtuelles n’est pas inclus)

-   **Situées TDRS propagée par le** serveur nombre total de TDRS qui se sont produits sur le serveur et qui ont été propagés à la machine virtuelle

**Performances du processeur graphique virtuel RemoteFX**

-   **Métadonnée L’appel de présente/** s nombre total (en secondes) d’opérations présentes à rendre sur le Bureau de l’ordinateur virtuel par seconde

-   **Métadonnée Nombre total de messages** entrants/s nombre total d’opérations présentes envoyées par l’ordinateur virtuel au GPU du serveur par seconde

-   **Métadonnée Octets lus/s** nombre total d’octets lus à partir du serveur RemoteFX par seconde

-   **Métadonnée Octets envoyés/s** nombre total d’octets envoyés au GPU de serveur prenant en charge RemoteFX par seconde

-   **CANAL Latence moyenne des tampons de communication (s** ) durée moyenne (en secondes) passée dans les tampons de communication

-   **CANAL Latence de la mémoire tampon DMA (** s) (en secondes) à partir du moment où le DMA est soumis jusqu’à la fin de l’opération

-   **CANAL Longueur de** file d’attente DMA longueur de file d’attente pour une carte vidéo 3D RemoteFX

-   **Situées Délais d’expiration TDR par** GPU nombre de dépassements de délai TDR qui se sont produits par GPU sur l’ordinateur virtuel

-   **Situées Délais d’expiration TDR par moteur** GPU nombre de dépassements de délai d’attente TDR qui se sont produits par moteur GPU sur l’ordinateur virtuel

En plus des compteurs de performances du GPU virtuel RemoteFX, vous pouvez également mesurer l’utilisation du GPU à l’aide de Process Explorer, qui montre l’utilisation de la mémoire vidéo et l’utilisation du GPU.

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

Le clustering de basculement dans Windows Server 2012 et versions ultérieures fournit la mise en cache sur les volumes partagés de cluster (CSV). Cela est extrêmement bénéfique pour les collections de bureaux virtuels mis en pool où la majorité des e/s de lecture proviennent du système d’exploitation de gestion. Le cache de volume partagé de cluster fournit des performances supérieures par plusieurs ordres de grandeur, car il met en cache des blocs qui sont lus plusieurs fois et les remet à partir de la mémoire système, ce qui réduit les e/s. Pour plus d’informations sur le cache de volume partagé de cluster, consultez [la rubrique activation du cache de](http://blogs.msdn.com/b/clustering/archive/2012/03/22/10286676.aspx)volume partagé de cluster.

### <a name="pooled-virtual-desktops"></a>Bureaux virtuels mis en pool

Par défaut, les bureaux virtuels mis en pool sont restaurés à l’état initial après la déconnexion d’un utilisateur, si bien que toute modification apportée au système d’exploitation Windows depuis la dernière connexion de l’utilisateur est abandonnée.

Bien qu’il soit possible de désactiver la restauration, il s’agit toujours d’une condition temporaire, car généralement, une collection de bureaux virtuels mis en pool est recréée en raison de diverses mises à jour apportées au modèle de bureau virtuel.

Il est logique de désactiver les fonctionnalités et services Windows qui dépendent de l’état persistant. En outre, il est logique de désactiver les services principalement pour les scénarios n’appartenant pas à l’entreprise.

Chaque service spécifique doit être évalué de manière appropriée avant tout déploiement global. Voici quelques éléments à prendre en compte :

| de diffusion en continu                                      | Pourquoi ?                                                                                                                                                                                                      |
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
> Cette liste n’est pas censée être une liste complète, car toutes les modifications affectent les objectifs et les scénarios prévus. Pour plus d’informations, reportez-vous aux [pressages, obtenez-le maintenant, le script d’optimisation de l’infrastructure VDI de Windows 8, avec l’aimable autorisation ingénieurs PFE !](http://blogs.technet.com/b/jeff_stokes/archive/2013/04/09/hot-off-the-presses-get-it-now-the-windows-8-vdi-optimization-script-courtesy-of-pfe.aspx).

 
> [!NOTE]
> SuperFetch dans Windows 8 est activé par défaut. Elle prend en charge l’infrastructure VDI et ne doit pas être désactivée. SuperFetch permet de réduire davantage la consommation de mémoire via le partage de pages mémoire, ce qui est bénéfique pour l’infrastructure VDI. Les bureaux virtuels mis en pool exécutant Windows 7, SuperFetch doivent être désactivés, mais pour les bureaux virtuels personnels exécutant Windows 7, il doit être conservé.

 
