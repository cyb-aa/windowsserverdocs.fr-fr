---
title: popd
description: Découvrez comment faire passer le répertoire au répertoire le plus récemment stocké par la commande pushd.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8a4c52d5-9fd1-4eac-9c0c-5767b25728ed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 8a9e0a301a5f8b46e1907a4f43c5ed9247b85f77
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372229"
---
# <a name="popd"></a>popd

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Remplace le répertoire actif par le répertoire qui a été le plus récemment stocké par la commande **pushd** .
Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe
```
popd
```

### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes
-   Chaque fois que vous utilisez la commande **pushd** , un seul répertoire est stocké pour votre utilisation. Toutefois, vous pouvez stocker plusieurs répertoires à l’aide de la commande **pushd** plusieurs fois.
    Les répertoires sont stockés de manière séquentielle dans une pile virtuelle. Si vous utilisez la commande **pushd** une seule fois, le répertoire dans lequel vous utilisez la commande est placé en bas de la pile. Si vous réutilisez la commande, le deuxième répertoire est placé en haut de la première. Le processus se répète chaque fois que vous utilisez la commande **pushd** .
    Vous pouvez utiliser la commande **popd** pour remplacer le répertoire actif par le répertoire le plus récemment stocké par la commande **pushd** . Si vous utilisez la commande **popd** , le répertoire situé en haut de la pile est supprimé de la pile et le répertoire actif est remplacé par ce répertoire. Si vous utilisez à nouveau la commande **popd** , le répertoire suivant sur la pile est supprimé.
-   Lorsque les extensions de commande sont activées, la commande **popd** supprime toutes les attributions de lettre de lecteur créées par **pushd**.

## <a name="BKMK_examples"></a>Illustre
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
-   [pushd](pushd.md)
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

