---
title: Minroot
description: Configuration des contrôles de ressources du processeur hôte
keywords: Windows 10, Hyper-V
author: allenma
ms.date: 12/15/2017
ms.topic: article
ms.prod: windows-10-hyperv
ms.service: windows-10-hyperv
ms.assetid: ''
ms.openlocfilehash: e1269c11df32c8ce95cc7455d47d7170e9d0b1c8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844330"
---
# <a name="hyper-v-host-cpu-resource-management"></a>Gestion des ressources du processeur hôte Hyper-V

Les contrôles de ressources du processeur hôte Hyper-V introduit dans Windows Server 2016 ou plus tard permettent aux administrateurs d’Hyper-V pour mieux gérer et allouer des ressources du processeur entre « root », ou partition de gestion, les machines virtuelles invitées serveur hôte. À l’aide de ces contrôles, les administrateurs peuvent dédier un sous-ensemble des processeurs d’un système hôte pour la partition racine. Cela peut isoler le travail effectué dans un hôte Hyper-V à partir de charges de travail en cours d’exécution dans les machines virtuelles invitées en les exécutant sur des sous-ensembles distincts des processeurs système.

Pour plus d’informations sur le matériel pour les ordinateurs hôtes Hyper-V, consultez [requise pour Windows 10 Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/hyper-v-requirements).

## <a name="background"></a>Arrière-plan

Avant que le paramètre de contrôle pour Hyper-V héberge les ressources de processeur, il est utile passer en revue les principes fondamentaux de l’architecture de Hyper-V.  
Vous trouverez un résumé général dans le [Architecture Hyper-V](https://docs.microsoft.com/windows-server/administration/performance-tuning/role/hyper-v-server/architecture) section.
Il s’agit des concepts importants pour cet article :

* Hyper-V crée et gère les partitions de la machine virtuelle, sur le calcul des ressources sont alloués et partagés, sous contrôle de l’hyperviseur.  Les partitions offrent des limites d’isolation renforcée entre toutes les machines de virtuelles invitées et entre les machines virtuelles invitées et la partition racine.

* La partition racine est lui-même une partition de la machine virtuelle, bien qu’il ait des propriétés uniques et bien supérieure privilèges que les machines virtuelles invitées.  La partition racine fournit les services de gestion qui contrôlent toutes les machines virtuelles d’invité fournit la prise en charge de l’appareil virtuel pour les invités et gère toutes les e/s de périphérique pour les machines virtuelles invitées.  Microsoft vous recommande fortement de ne pas en cours d’exécution de charges de travail d’application dans une partition de l’hôte.

* Chaque processeur virtuel (VP) de la partition racine est mappé 1:1 pour un processeur logique sous-jacent (LP).  Un hôte VP s’exécute toujours sur le même LP sous-jacente : il n’existe aucune migration de VPs la partition racine.  

* Par défaut, les LPs sur lequel exécuter hôte VPs peuvent également exécuter des VPs d’invité.

* Un vice-président invité peut être planifié par l’hyperviseur pour s’exécuter sur n’importe quel processeur logique disponible.  Alors que le Planificateur de l’hyperviseur prend soin d’envisager localité de cache temporelle, topologie NUMA et beaucoup d’autres facteurs lors de la planification de vice-président des invités, au final le vice-président peut être planifiée sur n’importe quel hôte LP.

## <a name="the-minimum-root-or-minroot-configuration"></a>La Configuration de « Minroot » ou racine minimale

Des versions antérieures d’Hyper-V avaient une limite maximale architecturale des 64 VPs par partition.  Cela est appliquée aux partitions de la racine et l’invité.  Comme les systèmes avec plus de 64 processeurs logiques est apparu sur les serveurs haut de gamme, Hyper-V a également évolué ses limites de mise à l’échelle d’hôte pour prendre en charge ces grands systèmes, à un moment prise en charge d’un hôte avec 320 jusqu'à LPs.  Toutefois, VP par limite de partition à ce moment-là avec rupture du 64 présenté plusieurs défis et introduit les complexités qui a effectué la prise en charge plus de 64 VPs par partition prohibitive.  Pour résoudre ce problème, Hyper-V limité le nombre de VPs donné à la partition racine à 64, même si la machine sous-jacente était le nombre de processeurs logique plus disponible.  L’hyperviseur voulez-vous continuer à utiliser des LPs disponibles toutes les pour invité VPs en cours d’exécution, mais artificiellement affecté la partition racine à 64.  Cette configuration est devenue appelée le « minimale racine », ou la configuration de « minroot ».  Tests de performances confirmé que, même sur les systèmes à grande échelle avec plus de 64 LPs, la racine n’a pas besoin plus de 64 VPs racine pour fournir une prise en charge suffisante pour un grand nombre de machines virtuelles invitées et invité VPs – en fait, beaucoup moins de 64 racine VPs était souvent adéquate , bien entendu selon le nombre et la taille des ordinateurs virtuels, les charges de travail spécifiques en cours d’exécution, etc.

Ce concept de « minroot » continue à être utilisé dès aujourd'hui.  En fait, même si Windows Server 2016 Hyper-V augmente sa limite maximale prise en charge architecture hôte LPs 512 LPs, la partition racine sera toujours limitée à un maximum de 320 LPs.

## <a name="using-minroot-to-constrain-and-isolate-host-compute-resources"></a>À l’aide de Minroot pour limiter et isoler les ressources de calcul d’hôte
Avec le seuil par défaut élevée de 320 LPs dans Windows Server 2016 Hyper-V, la configuration minroot est uniquement utilisée sur les très grands systèmes serveur.  Toutefois, cette fonctionnalité peut être configurée pour un quantité seuil inférieur par l’administrateur de l’hôte Hyper-V et donc mises à profit pour limiter considérablement la quantité de ressources du processeur hôte disponibles pour la partition racine.  Le nombre spécifique de LPs racine à utiliser doit bien sûr être choisi avec soin pour prendre en charge les demandes maximales de machines virtuelles et des charges de travail allouées à l’hôte.  Toutefois, des valeurs raisonnables pour le nombre d’hôtes LPs peuvent être déterminé grâce à une évaluation minutieuse et surveillance des charges de travail de production et validées dans des environnements hors production avant de déploiements à grande échelle.

## <a name="enabling-and-configuring-minroot"></a>Activation et configuration Minroot

La configuration de minroot est contrôlée par le biais de nouvelles entrées BCD hyperviseur. Pour activer minroot, à partir d’une invite de commandes avec des privilèges d’administrateur :

```
    bcdedit /set hypervisorrootproc n
```
Où n est le nombre de racine VPs. 

Le système doit être redémarré et que le nouveau nombre de processeurs de la racine est conservé pour la durée de vie de l’initialisation du système d’exploitation.  La configuration de minroot ne peut pas être modifiée dynamiquement lors de l’exécution.

S’il existe plusieurs nœuds NUMA, chaque nœud obtiendra `n/NumaNodeCount` processeurs.

Notez que plusieurs nœuds NUMA, vous devez vous assurer que les topologie de la machine virtuelle sont qu’il n’y suffisamment LPs gratuites (c'est-à-dire LPs sans racine VPs) sur chaque nœud NUMA pour exécuter le nœud NUMA de le correspondantes de la machine virtuelle VPs.

## <a name="verifying-the-minroot-configuration"></a>Vérification de la Configuration Minroot

Vous pouvez vérifier la configuration de minroot de l’ordinateur hôte à l’aide du Gestionnaire des tâches, comme indiqué ci-dessous.

![](./media/minroot-taskman.png)

Lorsque Minroot est active, le Gestionnaire des tâches affiche le nombre de processeurs logiques actuellement alloué à l’hôte, en plus du nombre total de processeurs logiques dans le système.
 
