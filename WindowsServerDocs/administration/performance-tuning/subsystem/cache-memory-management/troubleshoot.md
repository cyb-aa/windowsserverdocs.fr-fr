---
title: Résolution des problèmes de performances de gestionnaire de mémoire et de Cache
description: Résolution des problèmes de performances de gestionnaire de mémoire sur Windows Server 16 et de Cache
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Pavel; ATales
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 66c7e2a6b264a837c65df927b271fadd2672fa24
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835790"
---
# <a name="troubleshoot-cache-and-memory-manager-performance-issues"></a>Résolution des problèmes de performances de gestionnaire de mémoire et de Cache

Avant Windows Server 2012, deux problèmes potentiels principales a provoqué le cache de fichiers système de croître jusqu'à ce que la mémoire disponible a été presque épuisée sous certaines charges de travail. Lorsque cette résultats de la situation dans le système qui est lente, vous pouvez déterminer si le serveur est accessible sur un de ces problèmes.


## <a name="counters-to-monitor"></a>Compteurs à surveiller

-   Mémoire\\Long-Term Standby Cache durée de vie moyenne (s) &lt; 1 800 secondes

-   Mémoire\\mégaoctets disponibles est faible

-   Mémoire\\octets résidants dans le Cache système

Si mémoire\\mégaoctets disponibles est faible ou en même temps mémoire\\octets résidants de Cache système consomme une partie significative de la mémoire physique, vous pouvez utiliser [RAMMAP](https://technet.microsoft.com/sysinternals/ff700229.aspx) pour savoir quelles le cache est utilisé pour.

## <a name="system-file-cache-contains-ntfs-metafile-data-structures"></a>Cache du système de fichier contient des structures de données NTFS métafichier


Ce problème est indiqué par un très grand nombre de pages de métafichier actives dans RAMMAP de sortie, comme indiqué dans l’illustration suivante. Ce problème peut constatée sur les serveurs occupés avec des millions de fichiers ouverts, ce qui en résulte dans la mise en cache les données de métafichier NTFS ne pas commercialisées à partir du cache.

![vue de rammap](../../media/perftune-guide-rammap.png)

Le problème utilisé pour être atténués par *DynCache* outil. Dans Windows Server 2012 +, l’architecture a été repensée et ce problème n’existe plus.

## <a name="system-file-cache-contains-memory-mapped-files"></a>Contient des fichiers mappés en mémoire cache du système de fichiers


Ce problème est indiqué par un très grand nombre de pages de fichier mappé actives dans la sortie RAMMAP. Cela indique généralement que certaines applications sur le serveur ouvre un grand nombre de fichiers volumineux à l’aide de [CreateFile](https://msdn.microsoft.com/library/windows/desktop/aa363858.aspx) API avec le fichier\_indicateur\_RANDOM\_accès indicateur est défini.

Ce problème est décrit en détail dans l’article [2549369](https://support.microsoft.com/default.aspx?scid=kb;en-US;2549369). FICHIER\_indicateur\_RANDOM\_indicateur d’accès est un indicateur pour le Gestionnaire de Cache pour conserver les vues du fichier mappés en mémoire tant que possible (jusqu'à ce que le Gestionnaire de mémoire ne signaler la condition de mémoire insuffisante). En même temps, cet indicateur ordonne à désactiver la lecture anticipée de données de fichier du Gestionnaire de Cache.

Cette situation a été atténuée dans une certaine mesure, en travaillant ensemble découpage améliorations dans Windows Server 2012 +, mais le problème lui-même doit être traité principalement par le fournisseur de l’application en n’utilisant ne pas de fichier\_indicateur\_RANDOM\_Accès. Une autre solution pour le fournisseur de l’application peut consister à utiliser la priorité de mémoire insuffisante lors de l’accès aux fichiers. Vous pouvez le faire à l’aide de la [SetThreadInformation](https://msdn.microsoft.com/library/windows/desktop/hh448390.aspx) API. Les pages qui sont accessibles à la priorité de mémoire insuffisante sont supprimés de la plage de façon plus agressive de travail.

Gestionnaire de cache, à compter de Windows Server 2016 davantage atténue cette menace en ignorant FILE_FLAG_RANDOM_ACCESS lors de la prise de décisions de filtrage, donc il est traité comme n’importe quel autre fichier ouvert sans l’indicateur FILE_FLAG_RANDOM_ACCESS (Gestionnaire de Cache toujours respecte cette indicateur pour désactiver la lecture anticipée de données de fichier). Vous pouvez toujours provoquer un encombrement du cache système si vous avez grand nombre de fichiers ouverts avec cet indicateur et accessibles de manière véritablement aléatoire. Il est vivement recommandé que FILE_FLAG_RANDOM_ACCESS ne pas utilisée par les applications.
