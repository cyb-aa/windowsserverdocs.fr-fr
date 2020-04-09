---
title: Résoudre les problèmes de performances du gestionnaire de mémoire et du cache
description: Résoudre les problèmes de performances du gestionnaire de cache et de mémoire sur Windows Server 16
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: pavel; atales
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 0b01808564cfaf1eaedf30a66c774e2228205847
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851652"
---
# <a name="troubleshoot-cache-and-memory-manager-performance-issues"></a>Résoudre les problèmes de performances du gestionnaire de mémoire et du cache

Avant Windows Server 2012, deux principaux problèmes potentiels ont provoqué l’augmentation du cache des fichiers système jusqu’à ce que la mémoire disponible soit presque épuisée dans certaines charges de travail. Lorsque cette situation entraîne un ralentissement du système, vous pouvez déterminer si le serveur est confronté à l’un de ces problèmes.


## <a name="counters-to-monitor"></a>Compteurs à surveiller

-   Mémoire\\durée de vie moyenne du cache d’attente moyenne à long terme &lt; 1800 secondes

-   La mémoire\\mégaoctets disponibles est faible

-   Mémoire\\octets résidants dans le cache système

Si la mémoire\\mégaoctets disponibles est faible et que la mémoire\\octets résidants dans le cache système consomme une partie importante de la mémoire physique, vous pouvez utiliser [RAMMAP](https://technet.microsoft.com/sysinternals/ff700229.aspx) pour connaître l’utilisation du cache.

## <a name="system-file-cache-contains-ntfs-metafile-data-structures"></a>Le cache des fichiers système contient des structures de données de métafichier NTFS


Ce problème est indiqué par un très grand nombre de pages de métafichier actives dans la sortie RAMMAP, comme illustré dans la figure suivante. Ce problème peut avoir été observé sur des serveurs occupés avec des millions de fichiers en cours d’accès, provoquant ainsi la mise en cache des données de métafichier NTFS qui n’ont pas été libérées à partir du cache.

![vue rammap](../../media/perftune-guide-rammap.png)

Le problème a été utilisé pour être atténué par l’outil *DynCache* . Dans Windows Server 2012, l’architecture a été repensée et ce problème ne devrait plus exister.

## <a name="system-file-cache-contains-memory-mapped-files"></a>Le cache des fichiers système contient des fichiers mappés en mémoire


Ce problème est indiqué par un très grand nombre de pages de fichiers mappées actives dans la sortie RAMMAP. Cela indique généralement qu’une application sur le serveur ouvre un grand nombre de fichiers volumineux à l’aide de l’API [CreateFile](https://msdn.microsoft.com/library/windows/desktop/aa363858.aspx) avec l’indicateur de\_de fichier\_drapeau d’accès aléatoire\_défini.

Ce problème est décrit en détail dans l’article [2549369](https://support.microsoft.com/default.aspx?scid=kb;en-US;2549369)de la base de connaissances. INDICATEUR de\_de fichier\_indicateur d’accès aux\_ALÉATOIREs est un Conseil pour que le gestionnaire de cache conserve les vues mappées du fichier dans la mémoire aussi longtemps que possible (jusqu’à ce que le gestionnaire de mémoire ne signale pas une condition de mémoire insuffisante). En même temps, cet indicateur demande au gestionnaire de cache de désactiver la prérécupération des données de fichier.

Cette situation a été atténuée dans une certaine mesure par les améliorations de la suppression de la plage de travail dans Windows Server 2012 +, mais le problème lui-même doit être principalement traité par le fournisseur de l’application en n’utilisant pas d’indicateur de\_de fichier\_l’accès aléatoire aux\_. Une autre solution pour le fournisseur de l’application peut consister à utiliser une priorité de mémoire faible lors de l’accès aux fichiers. Cela est possible à l’aide de l’API [SetThreadInformation](https://msdn.microsoft.com/library/windows/desktop/hh448390.aspx) . Les pages qui sont accessibles à une priorité de mémoire faible sont supprimées de la plage de travail de façon plus agressive.

Le gestionnaire de cache, à partir de Windows Server 2016 atténue davantage cela en ignorant FILE_FLAG_RANDOM_ACCESS lors de la prise de décisions de suppression. il est donc traité comme tout autre fichier ouvert sans l’indicateur FILE_FLAG_RANDOM_ACCESS (le gestionnaire de cache honore toujours cet indicateur pour désactiver la prérécupération des données de fichier). Vous pouvez toujours provoquer une augmentation du cache du système si vous disposez d’un grand nombre de fichiers ouverts avec cet indicateur et que vous y accédez de façon aléatoire. Il est vivement recommandé de ne pas utiliser FILE_FLAG_RANDOM_ACCESS par les applications.
