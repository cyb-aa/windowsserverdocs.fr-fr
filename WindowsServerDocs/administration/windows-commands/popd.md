---
title: popd
description: Découvrez comment modifier le répertoire dans le répertoire de la plus récemment stocké par la commande pushd.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6da6dc9d1fc2d8965f8a081831cb1150375209a4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827460"
---
# <a name="popd"></a>popd

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Change le répertoire actuel au répertoire qui a été récemment stocké par le **pushd** commande.
Pour obtenir des exemples montrant comment utiliser cette commande, consultez [exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe
```
popd
```

### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes
-   Chaque fois que vous utilisez le **pushd** un seul répertoire est stocké pour votre utilisation de la commande. Toutefois, vous pouvez stocker plusieurs répertoires à l’aide de la **pushd** commande plusieurs fois.
    Les répertoires sont stockés séquentiellement dans une pile virtuelle. Si vous utilisez le **pushd** commande une fois le répertoire dans lequel vous utilisez la commande est placé en bas de la pile. Si vous utilisez à nouveau la commande, le second répertoire est placé en haut de la première condition. Le processus se répète chaque fois que vous utilisez le **pushd** commande.
    Vous pouvez utiliser la **popd** commande pour modifier le répertoire actif dans le répertoire récemment stocké par le **pushd** commande. Si vous utilisez le **popd** commande, le répertoire en haut de la pile est supprimé de la pile et le répertoire actif est modifié à ce répertoire. Si vous utilisez le **popd** commande là encore, le répertoire suivant sur la pile est supprimé.
-   Lorsque les extensions de commande sont activées, le **popd** commande supprime les attributions de lettre de lecteur créées par **pushd**.

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

## <a name="additional-references"></a>Références supplémentaires
-   [pushd](pushd.md)
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

