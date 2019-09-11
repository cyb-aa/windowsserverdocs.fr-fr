---
title: Optimisation des performances pour les sous-systèmes Gestion de la mémoire et du cache
description: Optimisation des performances pour les sous-systèmes Gestion de la mémoire et du cache
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: Pavel; ATales
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 628a73f66e15940f184a076ad72fc24ad75b0e6c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866421"
---
# <a name="performance-tuning-cache-and-memory-manager"></a>Optimisation des performances pour Gestion de la mémoire et du cache

Par défaut, Windows met en cache les données de fichier lues à partir de disques et écrites sur des disques. Cela implique que les opérations de lecture lisent les données de fichier depuis une zone de la mémoire système, appelée cache de fichiers système, plutôt que depuis le disque physique. En conséquence, les opérations d’écriture écrivent les données de fichier dans le cache de fichiers système plutôt que sur le disque, et ce type de cache est appelé cache de réinscription. La mise en cache est gérée par objet de fichier. La mise en cache se produit sous la direction du gestionnaire de cache, qui fonctionne en permanence pendant l’exécution de Windows.

Les données de fichier dans le cache de fichiers système sont écrites sur le disque à des intervalles déterminés par le système d’exploitation. Les pages vides restent soit dans l’ensemble de travail du cache système (lorsque FILE\_FLAG\_RANDOM\_ACCESS est défini et que le descripteur de fichier n’a pas été fermé), soit dans la liste de veille où ils font partie de la mémoire disponible.

La politique consistant à retarder l’écriture des données dans le fichier et à les conserver dans le cache jusqu’à ce que celui-ci soit vidé est appelée écriture différée et est déclenchée par le gestionnaire de cache à un intervalle de temps déterminé. Le moment auquel un bloc de données de fichier est vidé dépend en partie de la durée pendant laquelle il a été stocké dans le cache et de la période écoulée depuis le dernier accès aux données lors d’une opération de lecture. Cela garantit que les données de fichier fréquemment lues resteront accessibles dans le cache de fichiers système pendant le temps maximal imparti.

Ce processus de mise en cache de données de fichier est illustré dans la figure suivante :

![mise en cache des données de fichier](../../media/perftune-guide-file-data-caching.png)

Comme illustré par les flèches en trait plein dans la figure précédente, une région de 256 Ko de données est lue dans un emplacement de cache de 256 Ko dans l’espace d’adressage système lors de la première demande du gestionnaire de cache lors d’une opération de lecture de fichier. Un processus en mode utilisateur copie ensuite les données de cet emplacement dans son propre espace d’adressage. Lorsque le processus a terminé son accès aux données, il enregistre les données modifiées dans le même emplacement dans la mémoire cache du système, comme indiqué par la flèche en pointillé entre l’espace d’adressage du processus et la mémoire cache du système. Lorsque le gestionnaire de cache a déterminé que les données ne seront plus nécessaires pendant un certain temps, il réécrit les données modifiées dans le fichier sur le disque, comme indiqué par la flèche en pointillé entre le cache système et le disque.

**Dans cette section :**

-   [Problèmes de performances potentiel de Gestion de la mémoire et du cache](troubleshoot.md)

-   [Améliorations de Gestion de la mémoire et du cache dans Windows Server 2016](improvements-in-2016.md)
