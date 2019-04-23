---
title: Performances de la mémoire Hyper-V
description: Considérations relatives à la mémoire dans Hyper-V d’optimisation des performances
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 63a1b654b8ac52725cc5dd87c8b245f9dfaf40f0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848070"
---
# <a name="hyper-v-memory-performance"></a>Performances de la mémoire Hyper-V


L’hyperviseur virtualise la mémoire physique de l’invité pour isoler les ordinateurs virtuels entre eux et à fournir un espace de mémoire contiguës, de base zéro pour chaque système d’exploitation invité, comme sur les systèmes non virtualisés.

## <a name="correct-memory-sizing-for-child-partitions"></a>Taille de mémoire pour les partitions enfants suffisante

Vous devez déterminer la taille de mémoire de la machine virtuelle comme vous le faites habituellement pour les applications serveur sur un ordinateur physique. Vous devez dimensionner pour permettre de gérer la charge attendue en ordinaire et aux heures de pointe, car la quantité de mémoire insuffisante peut augmenter considérablement le temps de réponse et l’utilisation du processeur ou d’e/s.

Vous pouvez activer la mémoire dynamique autoriser Windows à dynamiquement la taille de mémoire de la machine virtuelle. Avec la mémoire dynamique, si des applications dans la machine virtuelle rencontrez des problèmes effectue les allocations de mémoire soudaine volumineux, vous pouvez augmenter la taille de fichier d’échange pour la machine virtuelle pour vous assurer de stockage temporaire pendant que la mémoire dynamique répond à la pression de mémoire.

Pour plus d’informations sur la mémoire dynamique, consultez [Hyper-V Dynamic Memory Overview]( https://go.microsoft.com/fwlink/?linkid=834434) et [Guide de Configuration de mémoire dynamique Hyper-V](https://go.microsoft.com/fwlink/?linkid=834435).

Lorsque vous exécutez Windows dans la partition enfant, vous pouvez utiliser les compteurs de performances suivants dans une partition enfant pour déterminer si la partition enfant subit une sollicitation de la mémoire et est susceptible de s’exécuter mieux avec une taille de mémoire de machine virtuelle plus élevée.

| Compteur de performances                                                         | Valeur de seuil suggéré                                                                                                                                                           |
|-----------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Mémoire : octets de réserve du Cache en attente                                        | Somme des octets de réserve du Cache de mise en veille et gratuit et zéro Page liste octets doit être 200 Mo ou plus d’informations sur les systèmes avec 1 Go et 300 Mo ou plus d’informations sur les systèmes avec 2 Go de RAM ou plus visible. |
| Mémoire – gratuit & zéro octets de la liste des pages                                        | Somme des octets de réserve du Cache de mise en veille et gratuit et zéro Page liste octets doit être 200 Mo ou plus d’informations sur les systèmes avec 1 Go et 300 Mo ou plus d’informations sur les systèmes avec 2 Go de RAM ou plus visible. |
| Mémoire : Pages en entrée/s                                                    | Moyenne sur une période de 1 heure est inférieure à 10.                                                                                                                                       | 

## <a name="correct-memory-sizing-for-root-partition"></a>Dimensionnement correcte de la mémoire pour la partition racine

La partition racine doit avoir suffisamment de mémoire pour fournir des services tels que la virtualisation d’e/s, instantané de machine virtuelle et de gestion pour prendre en charge les partitions enfants.

Hyper-V dans Windows Server 2016 surveille l’intégrité de l’exécution gestion système de la partition racine d’exploitation pour déterminer la quantité de mémoire pouvant être allouée en toute sécurité à des partitions enfants, tout en garantissant des performances élevées et la fiabilité de la partition racine.

## <a name="see-also"></a>Voir aussi

-   [Terminologie Hyper-V](terminology.md)

-   [Architecture Hyper-V](architecture.md)

-   [Configuration de serveur Hyper-V](configuration.md)

-   [Performances du processeur de Hyper-V](processor-performance.md)

-   [Stockage Hyper-V les performances d’e/s](storage-io-performance.md)

-   [Réseau de Hyper-V les performances d’e/s](network-io-performance.md)

-   [Détecter les goulots d’étranglement dans un environnement virtualisé](detecting-virtualized-environment-bottlenecks.md)

-   [Machines virtuelles Linux](linux-virtual-machine-considerations.md)
