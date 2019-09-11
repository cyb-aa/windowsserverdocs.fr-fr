---
title: Minroot
description: Configuration des contrôles de ressources processeur de l’ordinateur hôte
keywords: Windows 10, Hyper-V
author: allenma
ms.date: 12/15/2017
ms.topic: article
ms.prod: windows-10-hyperv
ms.service: windows-10-hyperv
ms.assetid: ''
ms.openlocfilehash: 92de899a39aed05e2f598fcb3aae3fbae3f1cb67
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872039"
---
# <a name="hyper-v-host-cpu-resource-management"></a>Gestion des ressources de l’UC de l’hôte Hyper-V

Les contrôles de ressources processeur de l’ordinateur hôte Hyper-V, introduits dans Windows Server 2016 ou version ultérieure, permettent aux administrateurs Hyper-V de mieux gérer et allouer les ressources processeur du serveur hôte entre la « racine », la partition de gestion et les machines virtuelles invitées. À l’aide de ces contrôles, les administrateurs peuvent dédier un sous-ensemble des processeurs d’un système hôte à la partition racine. Cela peut séparer le travail effectué dans un hôte Hyper-V des charges de travail en cours d’exécution sur les machines virtuelles invitées en les exécutant sur des sous-ensembles distincts des processeurs du système.

Pour plus d’informations sur le matériel pour les ordinateurs hôtes Hyper-V, voir [Configuration système requise pour Hyper-v dans Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/hyper-v-requirements).

## <a name="background"></a>Présentation

Avant de définir des contrôles pour les ressources processeur de l’ordinateur hôte Hyper-V, il est utile de passer en revue les principes de base de l’architecture Hyper-V.  
Vous trouverez un résumé général dans la section [architecture Hyper-V](https://docs.microsoft.com/windows-server/administration/performance-tuning/role/hyper-v-server/architecture) .
Il s’agit de concepts importants pour cet article :

* Hyper-V crée et gère des partitions d’ordinateur virtuel, sur lesquelles des ressources de calcul sont allouées et partagées, sous le contrôle de l’hyperviseur.  Les partitions fournissent de fortes limites d’isolation entre toutes les machines virtuelles invitées et entre les machines virtuelles invitées et la partition racine.

* La partition racine est elle-même une partition d’ordinateur virtuel, bien qu’elle possède des propriétés uniques et des privilèges bien supérieurs à ceux des ordinateurs virtuels invités.  La partition racine fournit les services de gestion qui contrôlent toutes les machines virtuelles invitées, fournit la prise en charge des appareils virtuels pour les invités et gère toutes les e/s de périphérique pour les machines virtuelles invitées.  Microsoft recommande vivement de ne pas exécuter de charges de travail d’application dans une partition hôte.

* Chaque processeur virtuel (VP) de la partition racine est mappé 1:1 à un processeur logique sous-jacent (LP).  Un VP hôte s’exécutera toujours sur la même LP sous-jacente : il n’existe aucune migration de l’VPs de la partition racine.  

* Par défaut, le niveau de VPs sur lequel s’exécute l’ordinateur hôte peut également exécuter VPs invité.

* Un vice-président peut être planifié par l’hyperviseur pour s’exécuter sur n’importe quel processeur logique disponible.  Alors que le planificateur d’hyperviseur s’occupe de la localité du cache temporel, de la topologie NUMA et de nombreux autres facteurs lors de la planification d’un vice-président, le VP peut être planifié sur n’importe quel hôte LP.

## <a name="the-minimum-root-or-minroot-configuration"></a>La racine minimale ou la configuration « Minroot »

Les versions antérieures d’Hyper-V avaient une limite maximale architecturale de 64 VPs par partition.  Cela s’applique aux partitions racine et invité.  À mesure que des systèmes avec plus de 64 processeurs logiques apparaissaient sur des serveurs haut de gamme, Hyper-V a également évolué ses limites de mise à l’échelle pour prendre en charge ces systèmes de grande taille, à un moment donné prenant en charge un hôte avec une capacité allant jusqu’à 320 MHz.  Toutefois, le fait de rompre la limite de 64 VP par partition à ce moment présent présentait plusieurs défis et présentait des complexités qui ont rendu la prise en charge de plus de 64 VPs par partition.  Pour résoudre ce cas, Hyper-V limite le nombre de VPs donné à la partition racine à 64, même si un grand nombre de processeurs logiques sont disponibles sur l’ordinateur sous-jacent.  L’hyperviseur continue à utiliser toutes les fonctionnalités de niveau de service disponibles pour l’exécution de VPs invité, mais a relimité artificiellement la partition racine à 64.  Cette configuration est devenue la configuration « racine minimale » ou « minroot ».  Les tests de performances ont confirmé que, même sur des systèmes à grande échelle avec plus de 64 de 1 à 2, la racine n’a pas besoin de plus de 64 racine VPs pour fournir une prise en charge suffisante à un grand nombre de machines virtuelles invitées et de VPs invités. en fait, beaucoup moins de 64 VPs racine étaient souvent suffisantes , en fonction du nombre et de la taille des machines virtuelles invitées, des charges de travail spécifiques exécutées, etc.

Ce concept « minroot » continue à être utilisé aujourd’hui.  En fait, même si Windows Server 2016 Hyper-V a augmenté sa limite de prise en charge architecturale maximale pour l’ordinateur hôte de taille égale à 512, la partition racine sera toujours limitée à un maximum de 320.

## <a name="using-minroot-to-constrain-and-isolate-host-compute-resources"></a>Utilisation de Minroot pour limiter et isoler les ressources de calcul de l’hôte
Avec le seuil par défaut élevé de 320 sur Windows Server 2016 Hyper-V, la configuration minroot sera utilisée uniquement sur les systèmes serveurs les plus volumineux.  Toutefois, cette fonctionnalité peut être configurée à un seuil beaucoup plus bas par l’administrateur de l’hôte Hyper-V, et donc être utilisée pour limiter considérablement la quantité de ressources processeur de l’ordinateur hôte disponibles pour la partition racine.  Le nombre spécifique de la racine de la partie à utiliser doit bien sûr être choisi avec soin pour prendre en charge les demandes maximales des machines virtuelles et des charges de travail allouées à l’hôte.  Toutefois, il est possible de déterminer des valeurs raisonnables pour le nombre de bits de l’hôte par le biais d’une évaluation et d’une surveillance rigoureuses des charges de travail de production, et d’une validation dans des environnements hors production avant un déploiement étendu.

## <a name="enabling-and-configuring-minroot"></a>Activation et configuration de Minroot

La configuration minroot est contrôlée via les entrées d’hyperviseur BCD. Pour activer minroot, à partir d’une invite cmd avec des privilèges d’administrateur :

```
    bcdedit /set hypervisorrootproc n
```
Où n est le nombre de VPs racines. 

Le système doit être redémarré et le nouveau nombre de processeurs racine sera conservé pendant toute la durée du démarrage du système d’exploitation.  La configuration minroot ne peut pas être modifiée de manière dynamique au moment de l’exécution.

S’il y a plusieurs nœuds NUMA, chaque nœud `n/NumaNodeCount` obtiendra des processeurs.

Notez qu’avec plusieurs nœuds NUMA, vous devez vous assurer que la topologie de la machine virtuelle est telle qu’il y a suffisamment de vinyle libre (par exemple, au niveau de la VPs racine) sur chaque nœud NUMA pour exécuter le nœud NUMA de la machine virtuelle correspondant VPs.

## <a name="verifying-the-minroot-configuration"></a>Vérification de la configuration de Minroot

Vous pouvez vérifier la configuration minroot de l’ordinateur hôte à l’aide du gestionnaire des tâches, comme indiqué ci-dessous.

![](./media/minroot-taskman.png)

Lorsque Minroot est actif, le gestionnaire des tâches affiche le nombre de processeurs logiques actuellement alloués à l’hôte, en plus du nombre total de processeurs logiques dans le système.
 
