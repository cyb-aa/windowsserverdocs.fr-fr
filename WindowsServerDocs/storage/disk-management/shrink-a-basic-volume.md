---
title: Réduire un volume de base
description: Cet article explique comment réduire un volume de base
ms.date: 06/07/2019
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 9073632a656f512bdb49ebe4eeefd4cd5f4eaadf
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66812532"
---
# <a name="shrink-a-basic-volume"></a>Réduire un volume de base

> **S’applique à :** Windows 10, Windows 8.1, Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez diminuer l’espace utilisé par les partitions principales et les lecteurs logiques en les réduisant dans un espace contigu et adjacent sur le même disque. Par exemple, si vous estimez avoir besoin d’une partition supplémentaire alors que vous n’avez pas d’autre disque, vous pouvez réduire la partition existante à la fin du volume pour créer un nouvel espace non alloué qui servira par la suite à créer la nouvelle partition. L’opération de réduction peut être bloquée par la présence de certains types de fichiers. Pour plus d’informations, consultez [Autres éléments à prendre en considération](#additional-considerations) 

Lorsque vous réduisez une partition, tous les fichiers ordinaires sont automatiquement déplacés pour créer le nouvel espace non alloué sur le disque. Il est inutile de reformater le disque pour réduire la partition.

> [!CAUTION]
> Si la partition est une partition brute (autrement dit, sans système de fichiers) qui contient des données (par exemple, un fichier de base de données), la réduction de la partition peut détruire les données.

## <a name="shrinking-a-basic-volume"></a>Réduction d’un volume de base

> [!NOTE]
> Vous devez au moins être membre du groupe **Opérateurs de sauvegarde** ou **Administrateurs** pour effectuer ces étapes.

#### <a name="to-shrink-a-basic-volume-using-the-windows-interface"></a>Pour réduire un volume de base à l’aide de l’interface Windows

1.  Dans le Gestionnaire de disque, cliquez avec le bouton droit sur le volume de base que vous souhaitez réduire.

2.  Cliquez sur **Réduire le volume**.

3.  Suivez les instructions à l’écran.


> [!NOTE]
> Vous pouvez uniquement réduire les volumes de base n’ayant pas de système de fichiers ou utilisant le système de fichiers NTFS.

#### <a name="to-shrink-a-basic-volume-using-a-command-line"></a>Pour réduire un volume de base à l’aide d’une ligne de commande

1.  Ouvrez une invite de commande et tapez `diskpart`.

2.  À l’invite **DISKPART**, tapez `list volume`. Notez le numéro du volume simple à réduire.

3.  À l’invite **DISKPART**, tapez `select volume <volumenumber>`. Sélectionnez le *volumenumber* du volume simple que vous voulez réduire.

4.  À l’invite **DISKPART**, tapez `shrink [desired=<desiredsize>] [minimum=<minimumsize>]`. Réduisez le volume sélectionné à la valeur *desiredsize* en mégaoctets (Mo) si possible, ou *minimumsize* si la valeur *desiredsize* est trop importante.

| Valeur             | Description |
| ---               | ----------- |
| **list volume** | Affiche une liste des volumes de base et dynamiques sur tous les disques. |
| **select volume** | Sélectionne le volume spécifié, où <em>volumenumber</em> est le numéro du volume et place le focus sur celui-ci. Si aucun volume n’est spécifié, la commande **select** répertorie le volume actuel avec le focus. Vous pouvez spécifier le volume par numéro, lettre de lecteur ou chemin d’accès de dossier de point de montage. Sur un disque de base, la sélection d’un volume positionne également le focus sur la partition correspondante. |
| **shrink** | Réduit le volume sur lequel se trouve le focus pour créer un espace non alloué. Cela n’entraîne aucune perte de données. Si la partition inclut les fichiers ne pouvant être déplacés (par exemple, le fichier de pagination ou la zone de stockage de clichés instantanés), le volume est réduit jusqu’à la position des fichiers non déplaçables. |
| **desired=** <em>desiredsize</em> | La quantité d’espace, en mégaoctets (Mo), à récupérer pour la partition actuelle. |
| **minimum=** <em>minimumsize</em> | La quantité d’espace minimale, en mégaoctets (Mo), à récupérer pour la partition actuelle. Si vous ne spécifiez pas une taille requise ou minimale, la commande récupère la quantité maximale d’espace possible. |

## <a name="additional-considerations"></a>Considérations supplémentaires

-   Lorsque vous réduisez une partition, certains fichiers (par exemple, le fichier de pagination ou la zone de stockage de clichés instantanés) ne peuvent pas être déplacés automatiquement et vous ne pouvez pas réduire l’espace alloué au-delà du point où se trouvent ces fichiers. Si l’opération de réduction échoue, recherchez l’événement 259 dans le journal des applications. Cet événement identifie le fichier non déplaçable. Si vous connaissez les clusters associés au fichier qui bloque l’opération de réduction, vous pouvez également utiliser la commande **fsutil** à une invite de commandes (tapez **fsutil volume querycluster /?** pour l’utiliser). Lorsque vous utilisez le paramètre **querycluster**, la sortie de commande identifie le fichier non déplaçable qui bloque l’opération de réduction.
Dans certains cas, vous devrez déplacer temporairement le fichier. Par exemple, si vous devez réduire davantage la partition, vous pouvez utiliser le Panneau de configuration pour déplacer le fichier de pagination ou les clichés instantanés enregistrés sur un autre disque, supprimer les copies des clichés instantanés stockés, réduire le volume, puis replacer le fichier de pagination sur le disque. Si le nombre de clusters défectueux détectés par le remappage dynamique de clusters défectueux est trop élevé, il ne sera pas possible de réduire la partition. Si cela se produit, vous devrez envisager de déplacer les données et de remplacer le disque.

-  N’utilisez pas une copie au niveau du bloc pour transférer les données. Cela copiera également la table de secteurs défectueux et le nouveau disque traitera les mêmes secteurs comme étant défectueux, alors que cela n’est pas le cas.

-   Vous pouvez réduire les partitions principales et les lecteurs logiques sur des partitions brutes (sans système de fichiers), tout comme des partitions utilisant le système de fichiers NTFS.

## <a name="see-also"></a>Voir aussi

-   [Gérer les volumes de base](manage-basic-volumes.md)