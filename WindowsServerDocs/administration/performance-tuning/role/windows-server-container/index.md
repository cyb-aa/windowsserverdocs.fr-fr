---
title: Réglage des performances des conteneurs Windows Server
description: Recommandations concernant le réglage des performances des conteneurs sur Windows Server 16
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: davso; ericam; yashi
author: akino
ms.date: 10/16/2017
ms.openlocfilehash: a4508e28e54562748422b198f703e23326d15720
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80851632"
---
# <a name="performance-tuning-windows-server-containers"></a>Réglage des performances des conteneurs Windows Server

## <a name="introduction"></a>Introduction
Windows Server 2016 est la première version de Windows à prendre en charge la technologie de conteneur intégrée dans le système d’exploitation. Dans Server 2016, deux types de conteneurs sont disponibles : les conteneurs Windows Server et les conteneurs Hyper-V. Chaque type de conteneur prend en charge la référence SKU Server Core ou Nano Server de Windows Server 2016. 

Ces configurations ont des implications différentes en termes de performances, que nous détaillons ci-dessous pour vous aider à comprendre ce qui vous convient. Nous détaillons également les configurations ayant un impact sur les performances, et décrivons les compromis possibles avec chacune de ces options.

### <a name="windows-server-container-and-hyper-v-containers"></a>Conteneurs Windows Server et Hyper-V

Les conteneurs Windows Server et Hyper-V offrent bon nombre d’avantages identiques en matière de portabilité et de cohérence, mais diffèrent sur le plan des garanties d’isolation et des caractéristiques des performances.

Les **conteneurs Windows Server** assurent l’isolation d’applications grâce à la technologie d’isolation des processus et des espaces de noms. Un conteneur Windows Server partage un noyau avec l’hôte de conteneur et tous les conteneurs exécutés sur l’hôte.

Les **conteneurs Hyper-V** étendent l’isolation fournie par les conteneurs Windows Server en exécutant chaque conteneur sur une machine virtuelle hautement optimisée. Dans cette configuration, le noyau de l’hôte de conteneur n’est pas partagé avec les conteneurs Hyper-V.

L’isolation supplémentaire fournie par les conteneurs Hyper-V est en grande partie réalisée par une couche d’isolation d’hyperviseur entre le conteneur et l’hôte du conteneur. Cela affecte la densité des conteneurs car, contrairement aux conteneurs Windows Server, le partage des fichiers système et des fichiers binaires est réduit, ce qui se traduit par une empreinte globale plus importante du stockage et de la mémoire. Une surcharge supplémentaire est en outre à prévoir dans certains chemins réseau, d’E/S de stockage et de processeur.

### <a name="nano-server-and-server-core"></a>Nano Server et Server Core

Les conteneurs Windows Server et Hyper-V prennent en charge Server Core et une nouvelle option d’installation disponible dans Windows Server 2016 : [Nano Server](https://technet.microsoft.com/windows-server-docs/compute/nano-server/getting-started-with-nano-server). 

Nano Server est un système d’exploitation de serveur administré à distance et optimisé pour les centres de données et clouds privés. Il est similaire à Windows Server en mode Server Core (Installation minimale), mais il est beaucoup plus petit, n’a aucune fonction d’ouverture de session locale et ne prend en charge que les agents, outils et applications 64 bits. Il prend occupe moins d’espace disque et démarre plus rapidement.

## <a name="container-start-up-time"></a>Temps de démarrage du conteneur
Le temps de démarrage du conteneur est une métrique clé dans bon nombre des scénarios où l’usage de conteneurs est le plus avantageux. Il est donc essentiel de comprendre comment optimiser le temps de démarrage du conteneurs. Vous trouverez ci-dessous quelques compromis de réglage destinés à vous aider à comprendre comment améliorer le temps de démarrage.

### <a name="first-logon"></a>Première ouverture de session

Microsoft fournit une image de base pour Nano Server et Server Core. L’image de base fournie pour Server Core a été optimisée en supprimant la surcharge de temps de démarrage associée à la première ouverture de session (OOBE). Ce n’est pas le cas de l’image de base pour Nano Server. Il est cependant possible d’éliminer ce coût des images basées sur Nano Server en validant au moins une couche dans l’image conteneur. Le conteneur suivant démarre à partir de l’image et n’engendre pas de coût de première ouverture de session.
### <a name="scratch-space-location"></a>Emplacement de l’espace de travail

Par défaut, les conteneurs utilisent un espace de travail temporaire sur le support de lecteur système de l’hôte du conteneur pour le stockage pendant la durée de vie du conteneur en cours d’exécution. Celui-ci fait office de lecteur système du conteneur, de sorte que de nombreuses écritures et lectures effectuées dans le cadre du fonctionnement du conteneur suivent ce chemin. Pour les systèmes hôtes où le lecteur système est installé sur un support magnétique de disque en rotation (disque dur) alors qu’un support de stockage plus rapide est disponible (disque dur ou SSD plus rapides), il est possible de déplacer l’espace de travail du conteneur vers un autre lecteur. Cela est réalisé à l’aide de la commande dockerd –g. Cette commande globale affecte tous les conteneurs en cours d’exécution sur le système.

### <a name="nested-hyper-v-containers"></a>Conteneurs Hyper-V imbriqués
Hyper-V pour Windows Server 2016 introduit la prise en charge d’hyperviseur imbriqué. Cela signifie qu’il est désormais possible d’exécuter une machine virtuelle à partir d’une machine virtuelle. Cela ouvre de nombreuses possibilités intéressantes, mais accentue l’incidence de l’hyperviseur sur les performances, car il existe deux niveaux d’hyperviseurs au dessus de l’hôte physique.

Pour des conteneurs, cela a un impact lors de l’exécution d’un conteneur Hyper-V à l’intérieur d’une machine virtuelle. Étant donné qu’un conteneur Hyper-V assure l’isolation à l’aide d’une couche d’hyperviseur entre lui et l’hôte du conteneur, lorsque celui-ci est une machine virtuelle basée sur Hyper-V, cela entraîne une surcharge de performances en termes de temps de démarrage du conteneur, d’E/S de stockage, d’E/S et de débit réseau, et de processeur.

## <a name="storage"></a>Stockage
### <a name="mounted-data-volumes"></a>Volumes de données montés

Un conteneur offre la possibilité d’utiliser le lecteur de son système hôte en tant qu’espace de travail. Cependant, l’espace de travail du conteneur a une durée de vie égale à celle du conteneur. Autrement dit, lors de l’arrêt du conteneur, l’espace de travail et toutes les données qui y sont associées disparaissent.

Cependant, il est souvent souhaitable que les données soient conservées indépendamment de la durée de vie du conteneur. Dans ce cas, nous prenons en charge le montage de volumes de données de l’hôte du conteneur dans le conteneur. Pour des conteneurs Windows Server, une surcharge négligeable du chemin d’E/S est associée aux volumes de données montés (les performances sont quasi natives). En revanche, lors du montage de volumes de données dans des conteneurs Hyper-V, une détérioration des performances d’E/S se produit dans ce chemin. De plus, cet impact est amplifié lors de l’exécution de conteneurs Hyper-V à l’intérieur de machines virtuelles.

### <a name="scratch-space"></a>Espace de travail

Par défaut, les conteneurs Windows Server et Hyper-V offrent tous deux un disque dur virtuel dynamique de 20 Go pour l’espace de travail du conteneur. Dans les deux cas, le système d’exploitation du conteneur occupe une partie de cet espace pour chaque conteneur démarré. Par conséquent, il est important de garder à l’esprit que chaque démarrage de conteneur a une incidence sur le stockage et que, selon la charge de travail, il peut entraîner l’écriture de jusqu’à 20 Go sur le support de stockage de sauvegarde. Les configurations de stockage du serveur doivent être conçues dans cet esprit
(pouvons-nous configurer la taille de l’espace de stockage ?).

## <a name="networking"></a>Mise en réseau
Les conteneurs Windows Server et Hyper-V offrent divers modes de mise en réseau pour répondre au mieux aux besoins des différentes configurations de réseau. Chacune de ces options présente des caractéristiques propres en termes de performance.

### <a name="windows-network-address-translation-winnat"></a>Traduction d’adresses réseau Windows (WinNAT)

Chaque conteneur reçoit une adresse IP avec préfixe IP interne privé (par exemple, 172.16.0.0/12). Le réacheminement/mappage de ports à partir de l’hôte de conteneur vers des points de terminaison de conteneur est pris en charge. Docker crée un réseau NAT par défaut lors de la première exécution de la commande dockerd.

Parmi ces trois modes, la configuration NAT est le chemin d’E/S réseau le plus coûteux, mais nécessitant le moins de configuration. 

Les conteneurs Windows Server utilisent une carte réseau virtuelle hôte pour s’attacher au commutateur virtuel. Les conteneurs Hyper-V utilisent une carte réseau de machine virtuelle synthétique (non exposée à la machine virtuelle d’utilitaire) pour s’attacher au commutateur virtuel. Lorsque des conteneurs communiquent avec le réseau externe, des paquets sont acheminés via WinNAT avec application de traductions d’adresses, ce qui entraîne une surcharge.

### <a name="transparent"></a>Mode transparent

Chaque point de terminaison de conteneur est directement connecté au réseau physique. Des adresses IP du réseau physique peuvent être attribuées de manière statique ou dynamique à l’aide d’un serveur DHCP externe.

Le mode transparent est l’option la moins coûteuse en termes de chemin d’E/S réseau, et les paquets externes sont directement transmis à la carte réseau virtuelle du conteneur, donnant ainsi un accès direct au réseau externe.

### <a name="l2-bridge"></a>L2 Bridge
Chaque point de terminaison de conteneur se trouve dans le même sous-réseau IP que l’hôte du conteneur. Les adresses IP doivent être attribuées de façon statique à partir du même préfixe que l’hôte de conteneur. Tous les points de terminaison de conteneur sur l’hôte ont la même adresse MAC en raison de la traduction d’adresses Layer-2.

Le mode pont L2 est plus performant que le mode WinNAT, car il fournit un accès direct au réseau externe, mais moins performant que le mode transparent parce qu’il ajoute une traduction d’adresses MAC.




