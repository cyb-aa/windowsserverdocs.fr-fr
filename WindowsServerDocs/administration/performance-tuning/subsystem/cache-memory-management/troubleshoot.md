---
title: Résoudre les problèmes de performances du gestionnaire de mémoire et du cache
description: Résoudre les problèmes de performances du gestionnaire de cache et de mémoire sur Windows Server 16
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Pavel; ATales
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 56871924311a945d62fef8a7ef7231889ba018cc
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866410"
---
# <a name="troubleshoot-cache-and-memory-manager-performance-issues"></a>Résoudre les problèmes de performances du gestionnaire de mémoire et du cache

Avant Windows Server 2012, deux principaux problèmes potentiels ont provoqué l’augmentation du cache des fichiers système jusqu’à ce que la mémoire disponible soit presque épuisée dans certaines charges de travail. Lorsque cette situation entraîne un ralentissement du système, vous pouvez déterminer si le serveur est confronté à l’un de ces problèmes.


## <a name="counters-to-monitor"></a>Compteurs à surveiller

-   Durée\\de vie moyenne du cache en attente de la mémoire &lt; moyenne (s) 1800 secondes

-   La\\mémoire disponible en mégaoctets est faible

-   Octets\\résidants dans le cache du système de mémoire

Si la\\mémoire disponible en mégaoctets est faible et que,\\dans le même temps, les octets résidants dans le cache du système de mémoire consomment une partie importante de la mémoire physique, vous pouvez utiliser [RAMMAP](https://technet.microsoft.com/sysinternals/ff700229.aspx) pour connaître l’utilisation du cache.

## <a name="system-file-cache-contains-ntfs-metafile-data-structures"></a>Le cache des fichiers système contient des structures de données de métafichier NTFS


Ce problème est indiqué par un très grand nombre de pages de métafichier actives dans la sortie RAMMAP, comme illustré dans la figure suivante. Ce problème peut avoir été observé sur des serveurs occupés avec des millions de fichiers en cours d’accès, provoquant ainsi la mise en cache des données de métafichier NTFS qui n’ont pas été libérées à partir du cache.

![vue rammap](../../media/perftune-guide-rammap.png)

Le problème a été utilisé pour être atténué par l’outil *DynCache* . Dans Windows Server 2012, l’architecture a été repensée et ce problème ne devrait plus exister.

## <a name="system-file-cache-contains-memory-mapped-files"></a>Le cache des fichiers système contient des fichiers mappés en mémoire


Ce problème est indiqué par un très grand nombre de pages de fichiers mappées actives dans la sortie RAMMAP. Cela indique généralement qu’une application sur le serveur ouvre un grand nombre de fichiers volumineux à l’aide de l'\_API\_ [CreateFile](https://msdn.microsoft.com/library/windows/desktop/aa363858.aspx) avec l’indicateur d’accès aléatoire\_de l’indicateur de fichier défini.

Ce problème est décrit en détail dans l’article [2549369](https://support.microsoft.com/default.aspx?scid=kb;en-US;2549369)de la base de connaissances. Indicateur de\_fichier\_l’indicateur d’accèsaléatoireestuneindicationpourquelegestionnairedecacheconservelesvuesmappéesdufichierdanslamémoireaussilongtempsquepossible(jusqu’àcequelegestionnairedemémoirenesignalepasuneconditiondemémoireinsuffisante).\_ En même temps, cet indicateur demande au gestionnaire de cache de désactiver la prérécupération des données de fichier.

Cette situation a été atténuée dans une certaine mesure par les améliorations de la suppression de la plage de travail dans Windows Server 2012 +, mais le problème lui-même doit être principalement traité\_par\_le fournisseur de l’application en n’utilisant pas l’indicateur de fichier aléatoire\_Accès. Une autre solution pour le fournisseur de l’application peut consister à utiliser une priorité de mémoire faible lors de l’accès aux fichiers. Cela est possible à l’aide de l’API [SetThreadInformation](https://msdn.microsoft.com/library/windows/desktop/hh448390.aspx) . Les pages qui sont accessibles à une priorité de mémoire faible sont supprimées de la plage de travail de façon plus agressive.

Le gestionnaire de cache, à partir de Windows Server 2016 atténue davantage cela en ignorant FILE_FLAG_RANDOM_ACCESS lors de la prise de décisions de suppression. il est donc traité comme tout autre fichier ouvert sans l’indicateur FILE_FLAG_RANDOM_ACCESS (le gestionnaire de cache honore ce problème indicateur de désactivation de la prérécupération des données de fichier). Vous pouvez toujours provoquer une augmentation du cache du système si vous disposez d’un grand nombre de fichiers ouverts avec cet indicateur et que vous y accédez de façon aléatoire. Il est vivement recommandé de ne pas utiliser FILE_FLAG_RANDOM_ACCESS par les applications.
