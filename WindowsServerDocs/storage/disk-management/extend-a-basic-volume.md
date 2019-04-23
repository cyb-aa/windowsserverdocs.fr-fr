---
title: Étendre un volume de base
description: Cet article décrit comment ajouter de l’espace sur les partitions principales et les lecteurs logiques afin d’étendre un volume de base
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 29b7b61f7edc20edda7bc18b82db17447badc0f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834250"
---
# <a name="extend-a-basic-volume"></a>Étendre un volume de base

> **S’applique à :** Windows 10, Windows 8.1, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez ajouter davantage d’espace à des partitions principales et des lecteurs logiques existants en les étendant dans un espace non alloué adjacent sur le même disque. Pour étendre un volume de base, celui-ci doit être brut (non formaté avec un système de fichiers) ou formaté avec le système de fichiers NTFS. Vous pouvez étendre un lecteur logique dans un espace libre contigu dans la partition étendue qui le contient. Si vous étendez un lecteur logique au-delà de l’espace libre disponible dans la partition étendue, la partition étendue s’agrandit pour contenir le lecteur logique.

Pour les lecteurs logiques et les volumes système ou de démarrage, vous pouvez étendre le volume dans un espace contigu uniquement et ce, uniquement si le disque peut être mis à niveau vers un disque dynamique. Pour les autres volumes, vous pouvez étendre le volume dans un espace non contigu, mais vous devrez convertir le disque en disque dynamique.

## <a name="extending-a-basic-volume"></a>Extension d’un volume de base

-   [À l’aide de l’interface Windows](#BKMK_WINUI)
-   [À l’aide d’une ligne de commande](#BKMK_CMD)

<a href="" id="BKMK_WINUI"></a>
#### <a name="to-extend-a-basic-volume-using-the-windows-interface"></a>Pour étendre un volume de base à l’aide de l’interface Windows

1.  Dans le Gestionnaire de disque, cliquez avec le bouton droit sur le volume que vous souhaitez étendre.

2.  Cliquez sur **Étendre le volume**.

3.  Suivez les instructions à l’écran.

<a href="" id="BKMK_CMD"></a>
#### <a name="to-extend-a-basic-volume-using-a-command-line"></a>Pour étendre un volume de base à l’aide d’une ligne de commande

1.  Ouvrez une invite de commande et tapez `diskpart`.

2.  À l’invite **DISKPART**, tapez `list volume`. Notez le volume de base que vous souhaitez étendre.

3.  À l’invite **DISKPART**, tapez `select volume <volumenumber>`. Cette commande sélectionne le volume de base *, numéro_de_volume,* que vous voulez étendre dans un espace vide contigu sur le même disque.

4.  À l’invite **DISKPART**, tapez `extend [size=<size>]`. Cette commande étend le volume sélectionné de *taille* en mégaoctets (Mo).

<br />

| Value | Description |
| --- | --- |
| <p>**volume de la liste**</p> | <p>Affiche une liste des volumes de base et dynamiques sur tous les disques.</p> |
| <p>**Sélectionnez le volume**</p> | <p>Sélectionne le volume spécifié, où <em>numéro_de_volume</em> est le numéro du volume et place le focus sur celui-ci. Si aucun volume n’est spécifié, la commande **select** répertorie le volume actuel avec le focus. Vous pouvez spécifier le volume par numéro, lettre de lecteur ou chemin d’accès de dossier de point de montage. Sur un disque de base, la sélection d’un volume positionne également le focus sur la partition correspondante.</p> |
| <p>**extend**</p> | <p><ul><li>Étend le volume sur lequel se trouve le focus dans l’espace non alloué contigu suivant. Pour les volumes de base, l’espace non alloué doit se trouver sur le même disque que la partition avec le focus et doit également suivre (être de décalage de secteur supérieur à) celle-ci. Un volume simple ou fractionné dynamique peut être étendu dans n’importe quel espace vide sur n’importe quel disque dynamique. À l’aide de cette commande, vous pouvez étendre un volume existant dans l’espace qui vient d’être créé.</p></li ><p><li>Si la partition a déjà été formatée avec le système de fichiers NTFS, le système de fichiers est automatiquement étendu de manière à occuper la plus grande partition. Cela n’entraîne aucune perte de données. Si la partition a déjà été formatée avec un format de système de fichiers autre que NTFS, la commande échoue et la partition n’est pas modifiée.</p></li></ul>|
| <p>**size=** <em>size</em></p> | <p>La quantité d’espace, en mégaoctets (Mo), à ajouter à la partition actuelle. Si vous ne spécifiez pas de taille, le disque est étendu de manière à occuper tout l’espace non alloué contigu.</p> |

## <a name="additional-considerations"></a>Considérations supplémentaires

-   Si le disque ne contient aucune partition système ou de démarrage, vous pouvez étendre le volume sur d’autres disques qui ne sont pas des disques système ou de démarrage, mais le disque sera converti en disque dynamique (s’il peut être mis à niveau).

## <a name="see-also"></a>Voir aussi

-   [Notation de syntaxe de ligne de commande](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)


