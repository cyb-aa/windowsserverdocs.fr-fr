---
title: Performances de la mémoire Hyper-V
description: Considérations relatives à la mémoire dans le réglage des performances Hyper-V
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: asmahi; sandysp; jopoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 5f683b85657b8dd263e93380b71c646ad677950c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851722"
---
# <a name="hyper-v-memory-performance"></a>Performances de la mémoire Hyper-V


L’hyperviseur virtualise la mémoire physique de l’invité pour isoler les machines virtuelles les unes des autres et pour fournir un espace mémoire contigu de base zéro pour chaque système d’exploitation invité, tout comme sur les systèmes non virtualisés.

## <a name="correct-memory-sizing-for-child-partitions"></a>Corriger le dimensionnement de la mémoire pour les partitions enfants

Vous devez dimensionner la mémoire de l’ordinateur virtuel comme vous le feriez habituellement pour les applications serveur sur un ordinateur physique. Vous devez le dimensionner pour gérer raisonnablement la charge prévue à des heures de pointe et normales, car une mémoire insuffisante peut augmenter considérablement les temps de réponse et l’utilisation du processeur ou des e/s.

Vous pouvez activer Mémoire dynamique pour permettre à Windows de dimensionner dynamiquement la mémoire de l’ordinateur virtuel. Avec Mémoire dynamique, si les applications de l’ordinateur virtuel rencontrent des problèmes d’allocations de mémoire soudaine, vous pouvez augmenter la taille du fichier d’échange de la machine virtuelle pour garantir une sauvegarde temporaire pendant que Mémoire dynamique répond à la sollicitation de la mémoire.

Pour plus d’informations sur la Mémoire dynamique, consultez [vue d’ensemble d’Hyper-v mémoire dynamique]( https://go.microsoft.com/fwlink/?linkid=834434) et guide de configuration de la [mémoire dynamique hyper](https://go.microsoft.com/fwlink/?linkid=834435)-v.

Quand vous exécutez Windows dans la partition enfant, vous pouvez utiliser les compteurs de performances suivants dans une partition enfant pour déterminer si la partition enfant subit une sollicitation de la mémoire et est susceptible d’être plus efficace avec une plus grande taille de mémoire de l’ordinateur virtuel.

| Compteur de performance                                                         | Valeur de seuil suggérée                                                                                                                                                           |
|-----------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Mémoire – octets de réserve du cache en attente                                        | La somme des octets de réserve de cache en attente et des octets de liste de pages libre et zéro doit être de 200 Mo ou plus sur les systèmes avec 1 Go et 300 Mo ou plus sur les systèmes avec au moins 2 Go de RAM visible. |
| Mémoire – octets libres & de la liste des pages de zéros                                        | La somme des octets de réserve de cache en attente et des octets de liste de pages libre et zéro doit être de 200 Mo ou plus sur les systèmes avec 1 Go et 300 Mo ou plus sur les systèmes avec au moins 2 Go de RAM visible. |
| Mémoire-entrées de pages/s                                                    | La moyenne sur une période de 1 heure est inférieure à 10.                                                                                                                                       | 

## <a name="correct-memory-sizing-for-root-partition"></a>Corriger le dimensionnement de la mémoire pour la partition racine

La partition racine doit disposer de suffisamment de mémoire pour fournir des services tels que la virtualisation des e/s, la capture instantanée de l’ordinateur virtuel et la gestion pour prendre en charge les partitions enfants.

Hyper-V dans Windows Server 2016 surveille l’intégrité du runtime du système d’exploitation de gestion de la partition racine afin de déterminer la quantité de mémoire pouvant être allouée en toute sécurité aux partitions enfants, tout en garantissant des performances et une fiabilité élevées de la partition racine.

## <a name="see-also"></a>Voir aussi

-   [Terminologie Hyper-V](terminology.md)

-   [Architecture Hyper-V](architecture.md)

-   [Configuration du serveur Hyper-V](configuration.md)

-   [Performances du processeur Hyper-V](processor-performance.md)

-   [Performances E/S du stockage Hyper-V](storage-io-performance.md)

-   [Performances E/S du réseau Hyper-V](network-io-performance.md)

-   [Détecter les goulots d’étranglement dans un environnement virtualisé](detecting-virtualized-environment-bottlenecks.md)

-   [Ordinateurs virtuels Linux](linux-virtual-machine-considerations.md)
