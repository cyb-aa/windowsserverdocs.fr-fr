---
title: pushd
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 396bc545-0f41-473e-b0ac-76fbbb74d390
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e7866a54e83bd57c8689512a1b75b151f74cb93c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837092"
---
# <a name="pushd"></a>pushd



Stocke le répertoire actif pour une utilisation par la commande **popd** , puis passe au répertoire spécifié.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
pushd [<Path>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Chemin de \<>|Spécifie le répertoire dans lequel créer le répertoire actif. Cette commande prend en charge les chemins d’accès relatifs.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Chaque fois que vous utilisez la commande **pushd** , un seul répertoire est stocké pour votre utilisation. Toutefois, vous pouvez stocker plusieurs répertoires à l’aide de la commande **pushd** plusieurs fois.

    Les répertoires sont stockés de manière séquentielle dans une pile virtuelle. Si vous utilisez la commande **pushd** une seule fois, le répertoire dans lequel vous utilisez la commande est placé en bas de la pile. Si vous réutilisez la commande, le deuxième répertoire est placé en haut de la première. Le processus se répète chaque fois que vous utilisez la commande **pushd** .

    Vous pouvez utiliser la commande **popd** pour remplacer le répertoire actif par le répertoire le plus récemment stocké par la commande **pushd** . Si vous utilisez la commande **popd** , le répertoire situé en haut de la pile est supprimé de la pile et le répertoire actif est remplacé par ce répertoire. Si vous utilisez à nouveau la commande **popd** , le répertoire suivant sur la pile est supprimé.
-   Si les extensions de commande sont activées, la commande **pushd** accepte un chemin d’accès réseau ou une lettre de lecteur local et un chemin d’accès.
-   Si vous spécifiez un chemin d’accès réseau, la commande **pushd** affecte temporairement la lettre de lecteur inutilisée la plus élevée (à partir de Z :) vers la ressource réseau spécifiée. La commande remplace ensuite le lecteur et le répertoire en cours par le répertoire spécifié sur le lecteur qui vient d’être affecté. Si vous utilisez la commande **popd** avec les extensions de commande activées, la commande **popd** supprime l’attribution de lettre de lecteur créée par **pushd**.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant montre comment vous pouvez utiliser la commande **pushd** et la commande **popd** dans un programme de traitement par lots pour modifier le répertoire actif de celui dans lequel le programme batch a été exécuté, puis le modifier de nouveau :
```
@echo off
rem This batch file deletes all .txt files in a specified directory
pushd %1
del *.txt
popd
cls
echo All text files deleted in the %1 directory
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

[Popd](popd.md)