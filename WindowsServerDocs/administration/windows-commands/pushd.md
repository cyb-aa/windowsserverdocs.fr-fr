---
title: pushd
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 396bc545-0f41-473e-b0ac-76fbbb74d390
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 548f39921c1f6aa3837e6443e396922396eb84f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887460"
---
# <a name="pushd"></a>pushd



Stocke le répertoire actif pour une utilisation par le **popd** commande et les modifications puis dans le répertoire spécifié.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
pushd [<Path>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Path>|Spécifie le répertoire pour rendre le répertoire actif. Cette commande prend en charge les chemins d’accès relatifs.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Chaque fois que vous utilisez le **pushd** un seul répertoire est stocké pour votre utilisation de la commande. Toutefois, vous pouvez stocker plusieurs répertoires à l’aide de la **pushd** commande plusieurs fois.

    Les répertoires sont stockés séquentiellement dans une pile virtuelle. Si vous utilisez le **pushd** commande une fois le répertoire dans lequel vous utilisez la commande est placé en bas de la pile. Si vous utilisez à nouveau la commande, le second répertoire est placé en haut de la première condition. Le processus se répète chaque fois que vous utilisez le **pushd** commande.

    Vous pouvez utiliser la **popd** commande pour modifier le répertoire actif dans le répertoire récemment stocké par le **pushd** commande. Si vous utilisez le **popd** commande, le répertoire en haut de la pile est supprimé de la pile et le répertoire actif est modifié à ce répertoire. Si vous utilisez le **popd** commande là encore, le répertoire suivant sur la pile est supprimé.
-   Si les extensions de commande sont activées, le **pushd** commande accepte un chemin d’accès réseau ou une lettre de lecteur local et un chemin d’accès.
-   Si vous spécifiez un chemin d’accès réseau, le **pushd** commande attribue temporairement la lettre de lecteur inutilisé le plus élevée (en partant de Z:) à la ressource réseau spécifié. La commande modifie ensuite le lecteur actuel et le répertoire dans le répertoire spécifié sur le lecteur qui vient d’être attribué. Si vous utilisez le **popd** commande avec des extensions de commande est activées, le **popd** commande supprime l’attribution de lettre de lecteur créée par **pushd**.

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant montre comment vous pouvez utiliser la **pushd** commande et le **popd** commande dans un programme de traitement par lots pour modifier le répertoire actuel de celui dans lequel le programme de traitement par lots a été lancé, puis retour :
```
@echo off
rem This batch file deletes all .txt files in a specified directory
pushd %1
del *.txt
popd
cls
echo All text files deleted in the %1 directory
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

[Popd](popd.md)