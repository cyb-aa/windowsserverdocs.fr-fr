---
title: "Réduire un volume de base"
description: "Cet article explique comment réduire un volume de base"
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e54632b78fd67a65b51147323565130881d8d81b
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="shrink-a-basic-volume"></a>Réduire un volume de base

> **S’applique à:** Windows10, Windows8.1, WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012

Vous pouvez diminuer l’espace utilisé par les partitions principales et les lecteurs logiques en les réduisant dans un espace contigu et adjacent sur le même disque. Par exemple, si vous estimez avoir besoin d’une partition supplémentaire mais que vous n’avez pas d’autre disque, vous pouvez réduire la partition existante à la fin du volume pour créer un nouvel espace non alloué qui servira par la suite à créer la nouvelle partition. L’opération de réduction peut être bloquée par la présence de certains types de fichiers. Pour plus d’informations, voir [Autres éléments à prendre en considération](#addcon). 

Lorsque vous réduisez une partition, tous les fichiers ordinaires sont automatiquement déplacés pour créer le nouvel espace non alloué sur le disque. Il est inutile de reformater le disque pour réduire la partition.

> [!CAUTION]
> Si la partition est une partition brute (autrement dit, sans système de fichiers) qui contient des données (par exemple, un fichier de base de données), la réduction de la partition peut détruire les données.

## <a name="shrinking-a-basic-volume"></a>Réduction d’un volume de base

-   [Utilisation de l’interface Windows](#BKMK_WINUI)
-   [Utilisation d’une ligne de commande](#BKMK_CMD)

> [!NOTE]
> Vous devez au moins être membre du groupe **Opérateurs de sauvegarde** ou **Administrateurs** pour effectuer ces étapes.

<a id="BKMK_WINUI"></a>
#### <a name="to-shrink-a-basic-volume-using-the-windows-interface"></a>Pour réduire un volume de base à l’aide de l’interface Windows

1.  Dans le Gestionnaire de disque, cliquez avec le bouton droit sur le volume que vous souhaitez réduire.

2.  Cliquez sur **Réduire le volume**.

3.  Suivez les instructions à l’écran.

<br />

> [!NOTE]
> Vous pouvez uniquement réduire les volumes de base n’ayant pas de système de fichiers ou utilisant le système de fichiers NTFS.

<a id="BKMK_CMD"></a>
#### <a name="to-shrink-a-basic-volume-using-a-command-line"></a>Pour réduire un volume de base à l’aide d’une ligne de commande

1.  Ouvrez une invite de commande et tapez `diskpart`.

2.  À l’invite **DISKPART**, tapez `list volume`. Notez le numéro du volume simple à réduire.

3.  À l’invite **DISKPART**, tapez `select volume <volumenumber>`. Sélectionnez le *numéro_de_volume* du volume simple que vous voulez réduire.

4.  À l’invite **DISKPART**, tapez `shrink [desired=<desiredsize>] [minimum=<minimumsize>]`. Réduit le volume sélectionné à la valeur *desiredsize* (taille requise) en mégaoctets (Mo) si possible, ou *minimumsize* (taille minimale) si la valeur *desiredsize* est trop importante.

<br />

| Valeur | Description|
|---|---|
| <p>**list volume**</p> | <p>Affiche une liste des volumes de base et dynamiques sur tous les disques.</p>|
| <p>**select volume**</p> | <p>Sélectionne le volume spécifié, où <em>numéro_de_volume</em> est le numéro du volume et place le focus sur celui-ci. Si aucun volume n’est spécifié, la commande **select** répertorie le volume actuel avec le focus. Vous pouvez spécifier le volume par numéro, lettre de lecteur ou chemin d’accès de dossier de point de montage. Sur un disque de base, la sélection d’un volume positionne également le focus sur la partition correspondante.</p> |
| <p>**shrink**</p> | <p>Réduit le volume sur lequel se trouve le focus pour créer un espace non alloué. Cela n’entraîne aucune perte de données. Si la partition inclut les fichiers ne pouvant être déplacés (par exemple, le fichier de pagination ou la zone de stockage de clichés instantanés), le volume est réduit jusqu’à la position des fichiers non déplaçables. |
| <p>**desired=** <em>taille_requise</em></p> | <p>La quantité d’espace, en mégaoctets (Mo), à récupérer pour la partition actuelle.</p> |
| <p>**minimum=** <em>taille_minimale</em></p> | <p>La quantité d’espace minimale, en mégaoctets (Mo), à récupérer pour la partition actuelle. Si vous ne spécifiez pas une taille requise ou minimale, la commande récupère la quantité maximale d’espace possible.</p> 

<a id="addcon"></a>

## <a name="additional-considerations"></a>Autres éléments à prendre en considération

-   Lorsque vous réduisez une partition, certains fichiers (par exemple, le fichier de pagination ou la zone de stockage de clichés instantanés) ne peuvent pas être déplacés automatiquement et vous ne pouvez pas réduire l’espace alloué au-delà du point où se trouvent ces fichiers. Si l’opération de réduction échoue, recherchez l’événement 259 dans le journal des applications. Cet événement identifie le fichier non déplaçable. Si vous connaissez les clusters associés au fichier qui bloque l’opération de réduction, vous pouvez également utiliser la commande **fsutil** à une invite de commandes (tapez **fsutil volume querycluster /?** pour l’utiliser). Lorsque vous utilisez le paramètre **querycluster**, la sortie de commande identifie le fichier non déplaçable qui bloque l’opération de réduction.
Dans certains cas, vous devrez déplacer temporairement le fichier. Par exemple, si vous devez réduire davantage la partition, vous pouvez utiliser le Panneau de configuration pour déplacer le fichier de pagination ou les clichés instantanés enregistrés sur un autre disque, supprimer les copies des clichés instantanés stockés, réduire le volume, puis replacer le fichier de pagination sur le disque. Si le nombre de clusters défectueux détectés par le remappage dynamique de clusters défectueux est trop élevé, il ne sera pas possible de réduire la partition. Si cela se produit, vous devrez envisager de déplacer les données et de remplacer le disque.

-  N’utilisez pas une copie au niveau du bloc pour transférer les données. Cela copiera également la table de secteurs défectueux et le nouveau disque traitera les mêmes secteurs comme étant défectueux, alors que cela nֹ’est pas le cas.

-   Vous pouvez réduire les partitions principales et les lecteurs logiques sur des partitions brutes (sans système de fichiers), tout comme des partitions utilisant le système de fichiers NTFS.

## <a name="see-also"></a>Articles associés

-   [Gérer les volumes de base](manage-basic-volumes.md)