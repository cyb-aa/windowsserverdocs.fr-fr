---
title: pushd
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 396bc545-0f41-473e-b0ac-76fbbb74d390
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e64c4f5090183b7d7b29dc7e040ffd94dc9d57ce
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722774"
---
# <a name="pushd"></a>pushd



Stocke le répertoire actif pour une utilisation par la commande **popd** , puis passe au répertoire spécifié.



## <a name="syntax"></a>Syntaxe

```
pushd [<Path>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Chemin d’accès>|Spécifie le répertoire dans lequel créer le répertoire actif. Cette commande prend en charge les chemins d’accès relatifs.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

-   Chaque fois que vous utilisez la commande **pushd** , un seul répertoire est stocké pour votre utilisation. Toutefois, vous pouvez stocker plusieurs répertoires à l’aide de la commande **pushd** plusieurs fois.

    Les répertoires sont stockés de manière séquentielle dans une pile virtuelle. Si vous utilisez la commande **pushd** une seule fois, le répertoire dans lequel vous utilisez la commande est placé en bas de la pile. Si vous réutilisez la commande, le deuxième répertoire est placé en haut de la première. Le processus se répète chaque fois que vous utilisez la commande **pushd** .

    Vous pouvez utiliser la commande **popd** pour remplacer le répertoire actif par le répertoire le plus récemment stocké par la commande **pushd** . Si vous utilisez la commande **popd** , le répertoire situé en haut de la pile est supprimé de la pile et le répertoire actif est remplacé par ce répertoire. Si vous utilisez à nouveau la commande **popd** , le répertoire suivant sur la pile est supprimé.
-   Si les extensions de commande sont activées, la commande **pushd** accepte un chemin d’accès réseau ou une lettre de lecteur local et un chemin d’accès.
-   Si vous spécifiez un chemin d’accès réseau, la commande **pushd** affecte temporairement la lettre de lecteur inutilisée la plus élevée (à partir de Z :) vers la ressource réseau spécifiée. La commande remplace ensuite le lecteur et le répertoire en cours par le répertoire spécifié sur le lecteur qui vient d’être affecté. Si vous utilisez la commande **popd** avec les extensions de commande activées, la commande **popd** supprime l’attribution de lettre de lecteur créée par **pushd**.

## <a name="examples"></a>Exemples

Pour illustrer l’utilisation de la commande **pushd** et de la commande **popd** dans un programme de traitement par lots pour modifier le répertoire actif de celui dans lequel le programme de traitement par lots a été exécuté, puis le modifier de nouveau :
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