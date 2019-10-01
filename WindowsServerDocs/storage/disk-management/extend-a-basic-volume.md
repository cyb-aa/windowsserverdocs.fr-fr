---
title: Étendre un volume de base
description: Cet article décrit comment ajouter de l’espace sur les partitions principales et les lecteurs logiques afin d’étendre un volume de base
ms.date: 06/07/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: a98bd3553c3223716d70ed4329bd7e265e697b73
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402094"
---
# <a name="extend-a-basic-volume"></a>Étendre un volume de base

> **S’applique à :** Windows 10, Windows 8.1, Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez ajouter davantage d’espace à des partitions principales et des lecteurs logiques existants en les étendant dans un espace non alloué adjacent sur le même disque. Pour étendre un volume de base, celui-ci doit être brut (non formaté avec un système de fichiers) ou formaté avec le système de fichiers NTFS. Vous pouvez étendre un lecteur logique dans un espace libre contigu dans la partition étendue qui le contient. Si vous étendez un lecteur logique au-delà de l’espace libre disponible dans la partition étendue, la partition étendue s’agrandit pour contenir le lecteur logique.

Pour les lecteurs logiques et les volumes système ou de démarrage, vous pouvez étendre le volume dans un espace contigu uniquement et ce, uniquement si le disque peut être mis à niveau vers un disque dynamique. Pour les autres volumes, vous pouvez étendre le volume dans un espace non contigu, mais vous devrez convertir le disque en disque dynamique.

## <a name="extending-a-basic-volume"></a>Extension d’un volume de base

#### <a name="to-extend-a-basic-volume-using-the-windows-interface"></a>Pour étendre un volume de base à l’aide de l’interface Windows

1. Dans le Gestionnaire de disque, cliquez avec le bouton droit sur le volume que vous souhaitez étendre.

2. Cliquez sur **Étendre le volume**.

3. Suivez les instructions à l’écran.

#### <a name="to-extend-a-basic-volume-using-a-command-line"></a>Pour étendre un volume de base à l’aide d’une ligne de commande

1. Ouvrez une invite de commande et tapez `diskpart`.

2. À l’invite **DISKPART**, tapez `list volume`. Notez le volume de base que vous souhaitez étendre.

3. À l’invite **DISKPART**, tapez `select volume <volumenumber>`. Cette commande sélectionne le volume de base *, numéro_de_volume,* que vous voulez étendre dans un espace vide contigu sur le même disque.

4. À l’invite **DISKPART**, tapez `extend [size=<size>]`. Cette commande étend le volume sélectionné de *size* en mégaoctets (Mo).

| Valeur | Description |
| --- | --- |
| **list volume** | Affiche une liste des volumes de base et dynamiques sur tous les disques. |
| **select volume** | Sélectionne le volume spécifié, où <em>volumenumber</em> est le numéro du volume et place le focus sur celui-ci. Si aucun volume n’est spécifié, la commande **select** répertorie le volume actuel avec le focus. Vous pouvez spécifier le volume par numéro, lettre de lecteur ou chemin d’accès de dossier de point de montage. Sur un disque de base, la sélection d’un volume positionne également le focus sur la partition correspondante. |
| **extend** | <ul><li>Étend le volume sur lequel se trouve le focus dans l’espace non alloué contigu suivant. Pour les volumes de base, l’espace non alloué doit se trouver sur le même disque que la partition avec le focus et doit également suivre (être de décalage de secteur supérieur à) celle-ci. Un volume simple ou fractionné dynamique peut être étendu dans n’importe quel espace vide sur n’importe quel disque dynamique. À l’aide de cette commande, vous pouvez étendre un volume existant dans l’espace qui vient d’être créé.</li ><li>Si la partition a déjà été formatée avec le système de fichiers NTFS, le système de fichiers est automatiquement étendu de manière à occuper la plus grande partition. Cela n’entraîne aucune perte de données. Si la partition a déjà été formatée avec un format de système de fichiers autre que NTFS, la commande échoue et la partition n’est pas modifiée.</li></ul> |
| **size=** <em>size</em> | Quantité d’espace, en mégaoctets (Mo), à ajouter à la partition actuelle. Si vous ne spécifiez pas de taille, le disque est étendu de manière à occuper tout l’espace non alloué contigu. |

## <a name="additional-considerations"></a>Considérations supplémentaires

-   Si le disque ne contient aucune partition système ou de démarrage, vous pouvez étendre le volume sur d’autres disques qui ne sont pas des disques système ou de démarrage, mais le disque sera converti en disque dynamique (s’il peut être mis à niveau).

## <a name="see-also"></a>Voir aussi

-   [Notation de la syntaxe de la ligne de commande](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)
