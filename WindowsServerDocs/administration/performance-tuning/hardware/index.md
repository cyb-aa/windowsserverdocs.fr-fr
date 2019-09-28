---
title: Considérations relatives aux performances du matériel du serveur
description: Considérations relatives aux performances du matériel du serveur pour Windows Server 2016
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: phstee
author: phstee
ms.date: 01/08/2018
ms.openlocfilehash: 9c012711dff3746587b4a04b31d9c23ebb7de4cd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370550"
---
# <a name="server-hardware-performance-considerations"></a>Considérations relatives aux performances du matériel du serveur

La section suivante répertorie les éléments importants à prendre en compte lors du choix du matériel du serveur. Le respect de ces consignes peut aider à éliminer les goulots d’étranglement susceptibles de nuire aux performances du serveur.

## <a name="processor-recommendations"></a>Recommandations concernant le processeur

Choisissez des processeurs 64 bits pour les serveurs. Les processeurs 64 bits offrent sensiblement plus d’espace d’adressage. Ils sont par ailleurs requis pour Windows Server 2016. S’il n’existe pas d’édition 32 bits de Windows Server 2016, les applications 32 bits s’exécutent sur ce système d’exploitation.

Pour augmenter la puissance de calcul d’un serveur, vous pouvez utiliser un processeur doté soit de cœurs cadencés à une fréquence supérieure, ou de davantage de cœurs. Si le processeur est la cause limitant les capacités du système, un doublement de la fréquence des cœurs est généralement plus avantageux que le doublement de leur nombre.

Outre le fait que des cœurs multiples ne produisent pas de mise à l’échelle parfaitement linéaire, le facteur d’échelle peut même être moindre en cas d’activation de la technologie Hyper-Threading qui repose sur un partage des ressources d’un même cœur physique.


>[!Important]
> Adaptez l’échelle la mémoire et du sous-système d’E/S de façon à ce qu’ils concordent avec performances du processeur, et inversement.

Ne comparez pas les fréquences de processeur entre fabricants et générations de processeurs, car une telle comparaison peut constituer un indicateur trompeur de la vitesse.

Pour l’Hyper-V, assurez-vous que le processeur prend en charge la fonctionnalité de traduction d’adresse de second niveau (SLAT). Intel implémente celle-ci à l’aide de tables de pages étendues (EPT), et AMD à l’aide de tables de pages imbriquées (NPT). Vous pouvez vérifier la présence de cette fonctionnalité en exécutant SystemInfo.exe sur votre serveur.

## <a name="cache-recommendations"></a>Recommandations concernant le cache

Choisissez des caches de processeur L2 ou L3 volumineux. Les architectures récentes, telles que Haswell ou Skylake, offrent un cache de dernier niveau (LLC) unifié ou un cache L4. Des caches plus volumineux offrent généralement de meilleures performances et jouent souvent un rôle plus important que la fréquence brute du processeur.

## <a name="memory-ram-and-paging-storage-recommendations"></a>Recommandations concernant la mémoire vive (RAM) et le stockage de pagination

>[!Note] 
> Suite à l’installation de Windows Server 2016, certains systèmes peuvent présenter des performances de stockage réduites par rapport à Windows Server 2012 R2. Dans le cadre du développement de Windows Server 2016, un certain nombre de modifications ont été apportées pour améliorer la sécurité et la fiabilité de la plateforme. Certaines de ces modifications, telle l’activation par défaut de Windows Defender, allongent les chemins d’E/S. Elles peuvent avoir pour effet de réduire les performances d’E/S de charges de travail et de modèles spécifiques. Microsoft déconseille de désactiver Windows Defender car il s’agit d’une couche de protection importante pour vos systèmes. 

Si vous avez besoin de davantage de mémoire, augmentez la RAM.
Lorsque l’ordinateur vient à manquer de mémoire, Windows utilise l’espace disque pour compléter la RAM du système selon une procédure appelée pagination. Un excès de pagination entraîne une dégradation des performances globales du système.
Vous pouvez optimiser la pagination en suivant les instructions ci-après pour le placement des fichiers d’échange :
- Isolez le fichier d’échange (ou de pagination) sur un dispositif de stockage dédié, ou assurez-vous au moins qu’il ne partage pas le dispositif de stockage avec d’autres fichiers fréquemment consultés. Par exemple, placez le fichier d’échange et les fichiers du système d’exploitation sur des disques durs physiques distincts.

- Placez le fichier d’échange sur un lecteur sans tolérance de panne. Une défaillance de disque peut entraîner une panne du système. Si vous placez le fichier de pagination sur un lecteur à tolérance de panne, n’oubliez pas qu’un tel système est souvent plus lent pour l’écriture des données parce qu’il écrit celles-ci dans plusieurs emplacements.

- Si vous avez besoin de davantage de bande passante de disque, utilisez plusieurs disques ou une baie de disques. Ne placez pas plusieurs fichiers de pagination sur différentes partitions du même lecteur de disque physique.

## <a name="peripheral-bus-recommendations"></a>Recommandations concernant le bus périphérique
Dans Windows Server 2016, le stockage principal et les interfaces réseau doivent être de type PCI Express (PCIe). Il est donc recommandé d’utiliser des serveurs dotés de bus PCIe. Afin d’éviter les limitations de vitesse de bus, utilisez les emplacements PCIe x8 et supérieurs pour les adaptateurs Ethernet d’une capacité supérieure à 10 Go.

## <a name="disk-recommendations"></a>Recommandations concernant les disques
Choisissez des disques dont les vitesses de rotation sont plus élevées afin de réduire les temps de service de demandes aléatoires (les disques à 15 000 tours/min offrent un gain moyen d’environ 2 ms par rapport aux disques à 7 200 tours/min) et d’augmenter la bande passante des demandes séquentielles. Certains aspects de coût, de puissance et autres doivent cependant être pris en considération en ce qui concerne les disques à vitesse de rotation élevée.

Des disques à usage professionnel de 2,5 pouces peuvent traiter un nombre sensiblement plus important de demandes aléatoires par seconde que des disques équivalents de 3,5 pouces.

Stockez les données fréquemment utilisées, en particulier celles qui font l’objet d’un accès séquentiel, au début du disque, soit sur les pistes les plus éloignées de l’axe de rotation (donc les plus rapides).

La consolidation de petits disques en un moindre nombre de lecteurs haute densité peut réduire les performances de stockage globales. Une réduction du nombre de broches entraîne une moindre simultanéité du service des demandes et, par conséquent, un débit potentiellement inférieur et des temps de réponse plus longs (selon l’intensité de la charge de travail).

L’usage de disques SSD et de disques flash à grande vitesse est indiqué essentiellement pour des disques aux taux d’E/S élevés ou dont les E/S sont sensibles à la latence. Ils conviennent particulièrement bien pour des disques de démarrage, car ils peuvent accélérer considérablement le processus.

Les disques SSD NVMe offrent des performances supérieures grâce à des profondeurs plus importantes de file d’attente de commandes, à un traitement plus efficace des interruptions et à une efficacité accrue des commandes de 4 Ko. Cela est particulièrement bénéfique quand un grand nombre d’E/S simultanées sont nécessaires.


## <a name="network-and-storage-adapter-recommendations"></a>Recommandations concernant les adaptateurs réseau et du stockage

La section suivante énumère les caractéristiques des adaptateurs réseau et du stockage recommandées pour des serveurs hautes performances. Le respect de ces recommandations peut vous aider à éviter des goulots d’étranglement de votre matériel de réseau ou de stockage lorsque celui-ci est soumis à une charge importante.

### <a name="certified-adapter-usage"></a>Utilisation d’un adaptateur certifié
Utilisez un adaptateur ayant passé avec succès la batterie de tests de certification de matériel Windows.

### <a name="64-bit-capability"></a>Capacité 64 bits
Les adaptateurs compatibles 64 bits peuvent effectuer des opérations d’accès direct à la mémoire (DMA) en lien avec des emplacements de mémoire physique élevée (plus de 4 Go). Si le pilote ne prend pas en charge un DMA supérieur à 4 Go, le système met deux fois en mémoire tampon les E/S en lien avec un espace d’adressage physique inférieur à 4 Go.

### <a name="copper-and-fiber-adapters"></a>Adaptateurs en cuivre et fibre
Les adaptateurs en cuivre offrent généralement les mêmes performances que leurs équivalents en fibre, et l’on trouve tant le cuivre que la fibre sur certains adaptateurs Fibre Channel. Des adaptateurs en cuivre sont plus indiqués pour certains environnements, et des adaptateurs en fibre pour d’autres.

### <a name="dual--or-quad-port-adapters"></a>Adaptateurs à deux ou quatre ports
Des adaptateurs multiports sont utiles pour des serveurs dotés d’un nombre limité d’emplacements PCI.

En réponse aux limitations SCSI relatives au nombre de disques pouvant être connectés à un bus SCSI, certains adaptateurs sont dotés de deux ou quatre bus SCSI. Les adaptateurs Fibre Channel ne sont généralement pas limités quant au nombre de disques pouvant être connectés, sauf s’ils se trouvent derrière une interface SCSI.

Le nombre de connexions des adaptateurs SAS (Serial Attached SCSI) et SATA (Serial ATA) est également limité en raison de la nature sérielle des protocoles, mais il est possible de connecter davantage de disques en utilisant des commutateurs.

Les adaptateurs réseau offrent cette fonctionnalité pour l’équilibrage de charge ou le basculement. À charge de travail égale, l’utilisation de deux adaptateurs réseau à port unique produit généralement de meilleures performances que l’utilisation d’un seul adaptateur réseau à deux ports.

La limitation du bus PCI peut être un facteur majeur de limitation des performances des adaptateurs multiports. Il est donc important de les placer dans un emplacement PCIe hautes performances offrant une bande passante suffisante.

### <a name="interrupt-moderation"></a>Modération des interruptions
Certains adaptateurs peuvent modérer la fréquence à laquelle ils interrompent les processeurs hôtes pour indiquer une activité ou l’achèvement de celle-ci. Une modération des interruptions peut souvent entraîner une réduction de la charge du processeur sur l’hôte mais, si cette modération n’est pas effectuée de manière intelligente, les économies de temps processeur risquent d’augmenter la latence.

### <a name="receive-side-scaling-rss-support"></a>Prise en charge du partage du trafic entrant (RSS)
Le partage du trafic entrant permet de mettre à l’échelle le traitement de la réception de paquets en fonction du nombre de processeurs disponibles. Cela est particulièrement important avec une connexion Ethernet 10 Go ou plus rapide.

### <a name="offload-capability-and-other-advanced-features-such-as-message-signaled-interrupt-msi-x"></a>Capacité de déchargement et autres fonctionnalités avancées telles que les interruptions signalées par message (MSI)-X
Les adaptateurs compatibles avec le déchargement offrent des économies de temps processeur qui améliorent les performances.

### <a name="dynamic-interrupt-and-deferred-procedure-call-dpc-redirection"></a>Interruption dynamique et redirection d’appel de procédure différé (DPC)
Dans Windows Server 2016, les E/S Numa permettent aux adaptateurs du stockage PCIe de rediriger de manière dynamique les interruptions et les DPC, et peuvent aider tout système multiprocesseur en améliorant le partitionnement de la charge de travail, les taux de réussite du cache et l’utilisation de l’interconnexion matérielle intégrée pour les charges de travail nécessitant des E/S intensives.

## <a name="see-also"></a>Voir aussi
- [Considérations relatives à la puissance du matériel du serveur](power.md)
- [Réglage de la puissance et des performances](power/power-performance-tuning.md)
- [Réglage de la gestion de la puissance du processeur](power/processor-power-management-tuning.md)
- [Paramètres de planification équilibrée recommandés](power/recommended-balanced-plan-parameters.md)
