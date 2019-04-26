---
title: Hôtes de virtualisation des services Bureau à distance de réglage des performances
description: Réglage des performances pour les hôtes de virtualisation des services Bureau à distance
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: HammadBu; VladmiS
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 7ef1eee97e40b9d3d131cef01722a0d42660b609
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838060"
---
# <a name="performance-tuning-remote-desktop-virtualization-hosts"></a>Hôtes de virtualisation des services Bureau à distance de réglage des performances


Hôte de virtualisation Bureau à distance (hôte de virtualisation Bureau à distance) est un service de rôle qui prend en charge les scénarios d’Infrastructure VDI (Virtual Desktop) et permet à plusieurs utilisateurs simultanés d’exécuter des applications Windows dans les machines virtuelles qui sont hébergées sur un serveur en cours d’exécution Windows Server 2016 et Hyper-V.

Windows Server 2016 prend en charge deux types de bureaux virtuels, des bureaux virtuels personnels et des bureaux virtuels.

**Dans cette rubrique :**

-   [Considérations générales](#general)

-   [Optimisations des performances](#perfopt)

## <a href="" id="general"></a>Considérations générales


### <a name="storage"></a>Stockage

Le stockage est le plus probable goulot d’étranglement de performances, et il est important de dimensionner votre stockage pour gérer correctement la charge d’e/s qui est générée par les modifications d’état de machine virtuelle. Si un pilote ou une simulation n’est pas possible, une bonne indication consiste à configurer une pile de disques pour les quatre ordinateurs virtuels actifs. Utilisez les configurations de disque qui ont de bonnes performances en écriture (telles que RAID 1 + 0).

Lorsque cela est approprié, utilisez la déduplication de disque et la mise en cache afin de réduire la charge de lecture sur le disque et pour activer votre solution de stockage améliorer les performances en mettant en cache une partie significative de l’image.

### <a name="data-deduplication-and-vdi"></a>VDI et la déduplication des données

Introduite dans Windows Server 2012 R2, la déduplication des données prend en charge l’optimisation des fichiers ouverts. Pour pouvoir utiliser des machines virtuelles s’exécutant sur un volume dédupliqué, les fichiers de machine virtuelle doivent être stockés sur un hôte distinct à partir de l’hôte Hyper-V. Si Hyper-V et la déduplication s’exécutent sur le même ordinateur, les deux fonctionnalités seront sont en concurrence pour les ressources système et un impact négatif sur les performances globales.

Le volume doit également être configuré pour utiliser le « Virtual Desktop Infrastructure (VDI) ? type d’optimisation de la déduplication. Vous pouvez le configurer à l’aide du Gestionnaire de serveur (**File and Storage Services**  - &gt; **Volumes**  - &gt; **paramètres de déduplication**) ou de commande à l’aide de la commande Windows PowerShell suivante :

``` syntax
Enable-DedupVolume <volume> -UsageType HyperV
```

**Remarque**    optimisation de la déduplication des données des fichiers ouverts est prise en charge uniquement pour les scénarios VDI avec Hyper-V à l’aide du stockage distant sur SMB 3.0.


### <a name="memory"></a>Mémoire

Utilisation mémoire du serveur est pilotée par les trois facteurs principaux :

-   Surcharge du système d’exploitation

-   Service de Hyper-V surcharge par machine virtuelle

-   Mémoire allouée à chaque machine virtuelle

Pour une charge de travail travailleur de connaissances typique, invité virtual machines en cours d’exécution x86 Windows 8 ou Windows 8.1 convient ~ 512 Mo de mémoire en tant que la ligne de base. Toutefois, la mémoire dynamique augmentera probablement mémoire de l’ordinateur virtuel invité à peu près 800 Mo, selon la charge de travail. Pour x64, nous voyons sur 800 Mo à partir de, augmenter jusqu'à 1 024 Mo.

Par conséquent, il est important de fournir suffisamment de mémoire serveur afin de satisfaire la mémoire requise par le nombre de machines virtuelles invitées attendu, par ailleurs une quantité suffisante de mémoire pour le serveur.

### <a name="cpu"></a>Processeur

Lorsque vous planifiez la capacité du serveur pour un serveur hôte de virtualisation Bureau à distance, le nombre d’ordinateurs virtuels par cœur physique dépendra de la nature de la charge de travail. En tant que point de départ, il est raisonnable planifier des machines virtuelles 12 par cœur physique, puis exécutez les scénarios appropriés pour valider les performances et la densité. Densité plus élevée peut être obtenue selon les spécificités de la charge de travail.

Nous vous recommandons d’activer hyper-threading, mais veillez à calculer le taux de surabonnement selon le nombre de cœurs physiques et non le nombre de processeurs logiques. Cela garantit le niveau de performances par UC attendu.

### <a name="virtual-gpu"></a>Processeur graphique virtuel

Microsoft RemoteFX pour l’hôte de virtualisation Bureau à distance offre une expérience de graphiques riches pour encoder d’Infrastructure VDI (Virtual Desktop) via la communication à distance côté hôte, un pipeline de rendu / capture / encodage, très efficaces basées sur GPU, la limitation basée sur le client activité et un processeur graphique virtuel compatible DirectX. RemoteFX pour l’hôte de virtualisation Bureau à distance met à niveau le processeur graphique virtuel à partir de DirectX9 DirectX11. Il améliore également l’expérience utilisateur en prenant en charge de plusieurs moniteurs aux résolutions plus élevées.

L’expérience de RemoteFX DirectX11 est disponible sans un matériel GPU, via un pilote émulés. Bien que ce logiciel GPU fournit une bonne expérience, l’unité de traitement graphique virtuelle de RemoteFX (VGPU) ajoute une expérience de l’accélération matérielle aux bureaux virtuels.

Pour tirer parti de l’expérience de VGPU RemoteFX sur un serveur exécutant Windows Server 2016, vous avez besoin d’un pilote GPU (par exemple, DirectX11.1 ou WDDM 1.2) sur le serveur hôte. Pour plus d’informations sur les offres GPU à utiliser avec RemoteFX pour l’hôte de virtualisation Bureau à distance, contactez votre fournisseur GPU.

Si vous utilisez le GPU virtuel RemoteFX dans votre déploiement VDI, la capacité de déploiement peut varier en fonction des scénarios d’utilisation et la configuration matérielle. Lorsque vous planifiez votre déploiement, procédez comme suit :

-   Nombre de processeurs graphiques sur votre système

-   Capacité de mémoire vidéo sur les GPU

-   Ressources de processeur et de matériel sur votre système

### <a name="remotefx-server-system-memory"></a>Mémoire du système serveur RemoteFX

Pour chaque ordinateur de bureau virtuel activé avec un processeur graphique virtuel, RemoteFX utilise la mémoire système dans le système d’exploitation invité et sur le serveur compatibles RemoteFX. L’hyperviseur garantit la disponibilité de la mémoire système pour un système d’exploitation. Sur le serveur, chaque bureau virtuel virtuel prenant en charge GPU doit publier ses besoins en mémoire système à l’hyperviseur. Lors du démarrage de bureau virtuel prenant en charge GPU virtuel, l’hyperviseur réserve de mémoire système supplémentaire sur le serveur prenant en charge de RemoteFX pour le bureau virtuel prenant en charge le processeur graphique.

La mémoire requise pour le serveur compatibles RemoteFX est dynamique, car la quantité de mémoire consommée sur le serveur compatibles RemoteFX est dépend du nombre de moniteurs qui sont associés les bureaux virtuels prenant en charge de VGPU et la résolution maximale pour Ces moniteurs.

### <a name="remotefx-server-gpu-video-memory"></a>Serveur RemoteFX mémoire vidéo GPU

Chaque bureau virtuel prenant en charge GPU virtuel utilise la mémoire vidéo dans le matériel GPU sur le serveur hôte pour afficher le bureau. En plus de rendu, la mémoire vidéo est utilisée par un codec pour compresser l’écran affiché. La quantité de mémoire nécessaire est directement basée sur la quantité de moniteurs qui sont configurées sur l’ordinateur virtuel.

La mémoire vidéo qui est réservée varie en fonction du nombre d’analyses et de la résolution d’écran système. Certains utilisateurs peuvent nécessiter une résolution d’écran plus élevée pour des tâches spécifiques. Il existe une plus grande évolutivité avec les paramètres de résolution inférieure si tous les autres paramètres restent constants.

### <a name="remotefx-processor"></a>Processeur de RemoteFX

L’hyperviseur planifie le serveur compatibles RemoteFX et les virtuels bureaux virtuels compatibles GPU sur l’UC. Contrairement à la mémoire système, il n’est pas les informations relatives à des ressources supplémentaires que RemoteFX a besoin de partager avec l’hyperviseur. Surcharge l’UC supplémentaire qui permet de bureau virtuel prenant en charge GPU virtuel RemoteFX est liée à l’exécution du pilote de processeur graphique virtuel et une pile de protocole Bureau à distance en mode utilisateur.

Sur le serveur compatibles RemoteFX, la surcharge est augmentée, étant donné que le système exécute un traitement supplémentaire (rdvgm.exe) par un bureau virtuel virtuel compatibles GPU. Ce processus utilise le pilote de périphérique graphique pour exécuter des commandes sur le GPU. Le codec utilise également les unités centrales pour compresser les données d’écran qui doivent être envoyé au client.

Plus de processeurs virtuels signifient une meilleure expérience utilisateur. Nous vous recommandons d’allouer au moins deux processeurs virtuels par un bureau virtuel virtuel compatibles GPU. Nous vous recommandons également de l’utilisation de la x64 architecture pour des bureaux virtuels virtuels compatibles GPU, car les performances sur x64 machines virtuelles est mieux comparé à x86 machines virtuelles.

### <a name="remotefx-gpu-processing-power"></a>Puissance de traitement de RemoteFX GPU

Pour chaque compatibles GPU virtuel bureau virtuel, il existe un processus de DirectX correspondant s’exécutant sur le serveur compatibles RemoteFX. Ce processus relit toutes les commandes de graphiques qu’il reçoit à partir du bureau virtuel RemoteFX sur le GPU physique. Pour le GPU physique, il est équivalent à l’exécution simultanée de plusieurs applications DirectX.

En règle générale, les pilotes et périphériques graphiques sont paramétrées pour exécuter plusieurs applications sur le bureau. RemoteFX s’étend sur les GPU à utiliser de manière unique. Pour mesurer les performances de la GPU sur un serveur RemoteFX, les compteurs de performances ont été ajoutées pour mesurer la réponse GPU pour les demandes de RemoteFX.

Généralement quand une ressource GPU est faible sur les ressources, lire et écrire des opérations dans le take GPU beaucoup de temps pour terminer. À l’aide des compteurs de performances, les administrateurs peuvent mesures préventives, éliminant ainsi l’éventualité de temps d’arrêt pour leurs utilisateurs finaux.

Les compteurs de performances suivants sont disponibles sur le serveur RemoteFX pour mesurer les performances de processeur graphique virtuel :

**Graphique de RemoteFX**

-   **Cadres ignorés/seconde - manque de ressources Client** nombre de trames ignorés par seconde en raison de ressources insuffisantes client

-   **Taux de Compression de graphiques** rapport entre le nombre d’octets codé au nombre d’octets en entrée

**Racine de RemoteFX gestion de GPU**

-   **Ressources : TdR dans Server GPU** nombre Total de fois où la fonctionnalité TDR arrive à expiration dans le GPU sur le serveur

-   **Ressources : Les machines virtuelles en cours d’exécution RemoteFX** nombre Total de machines virtuelles qui ont de la carte vidéo 3D RemoteFX installé

-   **VRAM : Mo disponible par GPU** quantité de mémoire vidéo dédiée qui n’est pas utilisée

-   **VRAM : % Réservée par GPU** pourcentage de mémoire vidéo dédiée qui a été réservée pour RemoteFX

**Logiciel de RemoteFX**

-   **Taux de capture pour analyse** \[1-4\] affiche la fréquence de capture de RemoteFX pour les moniteurs 1-4

-   **Taux de compression** déconseillées dans Windows 8 et remplacé par **taux de Compression de graphiques**

-   **Retardé images par seconde** nombre de frames par seconde où les données de graphique n’a pas envoyées au sein d’un certain laps de temps

-   **Temps de réponse GPU de Capture** latence mesurée au sein de la Capture de RemoteFX (en microsecondes) pour effectuer des opérations de GPU

-   **Temps de réponse GPU de rendu** latence mesurée au sein de RemoteFX rendu (en microsecondes) pour effectuer des opérations de GPU

-   **Octets de sortie** octets de sortie du nombre Total de RemoteFX

-   **En attente pour le compte client/s** déconseillées dans Windows 8 et remplacé par **cadres ignorés/seconde - manque de ressources Client**

**Gestion de vGPU RemoteFX**

-   **Ressources : TDR local aux machines virtuelles** nombre Total de TDR qui se sont produites dans cette machine virtuelle (TDR que le serveur propagé vers les machines virtuelles ne sont pas inclus)

-   **Ressources : TDR propagées par serveur** nombre Total de TDR qui s’est produite sur le serveur et qui ont été propagées à la machine virtuelle

**Performances de vGPU RemoteFX machine virtuelle**

-   **Données : Appelé par seconde présente** nombre Total (en secondes) d’opérations présentes à restituer sur le bureau de l’ordinateur virtuel par seconde

-   **Données : Sortants/s présente** nombre Total d’opérations présentes envoyées par l’ordinateur virtuel au serveur GPU par seconde

-   **Données : Octets lus/s** nombre Total d’octets lus à partir du serveur compatibles RemoteFX par seconde

-   **Données : Octets envoyés/s** nombre Total d’octets envoyés sur le serveur compatibles RemoteFX GPU par seconde

-   **DMA : Latence (s) de moyenne de tampons de communication** durée moyenne (en secondes) passé dans les mémoires tampons de communication

-   **DMA : Latence de mémoire tampon DMA (s)** laps de temps (en secondes) à partir de laquelle le DMA est soumis avant la fin

-   **DMA : Longueur de file d’attente** longueur de file d’attente DMA pour une carte vidéo RemoteFX 3D

-   **Ressources : Délais d’expiration de la fonctionnalité TDR par GPU** nombre des TDR des délais d’attente qui se sont produites par des GPU sur la machine virtuelle

-   **Ressources : Délais d’expiration de la fonctionnalité TDR par moteur GPU** du moteur de délais d’expiration de nombre des TDR qui se sont produites par des GPU sur la machine virtuelle

Outre les compteurs de performances du GPU RemoteFX virtuels, vous pouvez également mesurer l’utilisation du GPU à l’aide de Process Explorer, qui affiche l’utilisation de la mémoire vidéo et l’utilisation du GPU.

## <a href="" id="perfopt"></a>Optimisations des performances


### <a name="dynamic-memory"></a>Mémoire dynamique

Mémoire dynamique permet plus efficacement l’utilisation des ressources mémoire du serveur exécutant Hyper-V en équilibrant la mémoire est répartie entre les machines virtuelles en cours d’exécution. Mémoire peut être réaffectée de manière dynamique entre les machines virtuelles en réponse à leurs charges de travail fluctuantes.

Mémoire dynamique vous permet d’augmenter la densité de la machine virtuelle avec les ressources que vous avez déjà sans sacrifier les performances ou l’évolutivité. Le résultat est une utilisation plus efficace des ressources matérielles du serveur, ce qui peut mener à faciliter la gestion et réduire les coûts.

Sur les systèmes d’exploitation invités exécutant Windows 8 et ci-dessus avec des processeurs virtuels qui s’étendent sur plusieurs processeurs logiques, un compromis entre en cours d’exécution avec la mémoire dynamique pour aider à réduire l’utilisation de la mémoire et la désactivation de la mémoire dynamique pour améliorer les performances d’une application qui est la topologie ordinateurs prenant en charge. Ce type d’application peut exploiter les informations de topologie pour les décisions de planification et de la mémoire d’allocation.

### <a name="tiered-storage"></a>Stockage à plusieurs niveaux

Hôte de virtualisation Bureau à distance prend en charge le stockage hiérarchisé pour les pools de bureaux virtuels. L’ordinateur physique qui est partagé par tous les bureaux virtuels au sein d’une collection peut utiliser une solution de stockage de petite taille, hautes performances, comme un miroir disque SSD (SSD). Les bureaux virtuels peuvent être placés sur un stockage moins coûteux, traditionnel telles que RAID 1 + 0.

L’ordinateur physique doit être placé sur un disque SSD est, car la plupart de la lecture-je/du système d’exploitation à partir de bureaux virtuels sont dans le système d’exploitation de gestion. Par conséquent, le stockage est utilisé par l’ordinateur physique doit prendre en charge beaucoup plus élevée lus e/s par seconde.

Cette configuration de déploiement garantit des performances rentable là où les performances sont nécessaire. Le disque SSD fournit de meilleures performances sur un disque de taille plus petit (environ 20 Go par collection, selon la configuration). Stockage classique pour les bureaux virtuels (RAID 1 + 0) utilise environ 3 Go par machine virtuelle.

### <a name="csv-cache"></a>Cache de volume partagé de cluster

Le Clustering de basculement dans Windows Server 2012 et versions ultérieures fournit la mise en cache sur Cluster Shared Volumes (CSV). Cela est extrêmement avantageux pour les collections de bureaux virtuels où la majorité de la lecture e/s proviennent du système d’exploitation de gestion. Le cache de volume partagé de cluster fournit de meilleures performances de plusieurs ordres de grandeur, car elle met en cache des blocs qui sont lus plusieurs fois et les remet à partir de la mémoire système, ce qui réduit les e/s. Pour plus d’informations sur le cache de volume partagé de cluster, consultez [Guide to Enable CSV Cache](http://blogs.msdn.com/b/clustering/archive/2012/03/22/10286676.aspx).

### <a name="pooled-virtual-desktops"></a>Bureaux virtuels

Par défaut, bureaux virtuels sont restaurées à l’état initial une fois un utilisateur se déconnecte, par conséquent, toutes les modifications apportées au système d’exploitation Windows, dans la mesure où la dernière connexion de l’utilisateur sont abandonnées.

Bien qu’il soit possible de désactiver la restauration, il est toujours une condition temporaire, car il est généralement une collection de bureaux virtuels est recréée en raison des différentes mises à jour pour le modèle de bureau virtuel.

Il est judicieux de désactiver les fonctionnalités de Windows et les services qui dépendent d’un état persistant. En outre, il est judicieux de désactiver les services qui sont principalement destinées aux scénarios de non-enterprise.

Chaque service spécifique doit être convenablement évalué avant les déploiements à grande échelle. Voici quelques points initiaux à prendre en compte :

| Service                                      | Pourquoi ?                                                                                                                                                                                                      |
|----------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Mise à jour automatique                                  | Bureaux virtuels sont mis à jour en recréant le modèle de bureau virtuel.                                                                                                                          |
| Fichiers hors connexion                                | Bureaux virtuels sont toujours en ligne et connecté à partir d’un point de vue mise en réseau.                                                                                                                         |
| Défragmentation en arrière-plan                            | Les modifications de système de fichiers sont ignorées après qu’un utilisateur se déconnecte (en raison d’une restauration à l’état initial ou à recréer le modèle de bureau virtuel, ce qui entraîne la recréation de tous les bureaux virtuels). |
| Mise en veille ou veille prolongée                           | Pas de concept pour l’infrastructure VDI                                                                                                                                                                                   |
| Vidage de mémoire de vérification de bogue                        | Pas de concept pour les bureaux virtuels. Un bureau virtuel mis en pool de vérification des bogues démarre à partir de l’état initial.                                                                                       |
| Configuration automatique WLAN                              | Il n’existe aucune interface de périphérique Wi-Fi pour l’infrastructure VDI                                                                                                                                                                 |
| Service de partage de réseau de Windows Media Player | Services centrés sur consommateur                                                                                                                                                                                  |
| Fournisseur de groupe résidentiel                          | Services centrés sur consommateur                                                                                                                                                                                  |
| Partage de connexion Internet                  | Services centrés sur consommateur                                                                                                                                                                                  |
| Media Center services étendus               | Services centrés sur consommateur                                                                                                                                                                                  |

 

**Remarque**    cette liste n’est pas destinée à être une liste complète, car toutes les modifications affectent les objectifs prévues et les scénarios. Pour plus d’informations, consultez [chaud désactiver les activations, obtenez-le maintenant, le script de l’optimisation d’infrastructure VDI de Windows 8, courtoisie d’ingénieurs PFE !](http://blogs.technet.com/b/jeff_stokes/archive/2013/04/09/hot-off-the-presses-get-it-now-the-windows-8-vdi-optimization-script-courtesy-of-pfe.aspx).

 

**Remarque**    SuperFetch dans Windows 8 est activé par défaut. Il prend en charge VDI et ne doit pas être désactivé. SuperFetch permet de réduire la consommation de mémoire via le partage de page mémoire, ce qui est utile pour l’infrastructure VDI. Bureaux virtuels qui exécutent Windows 7, SuperFetch doit être désactivé, mais pour les bureaux virtuels personnels qui exécutent Windows 7, il doit être conservé sur.

 
