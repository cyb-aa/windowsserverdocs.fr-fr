---
title: Md
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82162d00-cc34-4776-9e55-4b4836dbd6a9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1396038410ecc5db5a124a1768038c4f8c8bea8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820840"
---
# <a name="md"></a>Md



Crée un répertoire ou sous-répertoire.

> [!NOTE]
> Cette commande est identique à la **mkdir** commande.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
md [<Drive>:]<Path>
mkdir [<Drive>:]<Path>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Lecteur > :|Spécifie le lecteur sur lequel vous souhaitez créer le nouveau répertoire.|
|\<Path>|Obligatoire. Spécifie le nom et l’emplacement du nouveau répertoire. La longueur maximale d’un chemin est déterminée par le système de fichiers.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

Les extensions de commande, qui sont activées par défaut, permettent d’utiliser un seul **md** commande pour créer des répertoires intermédiaires dans un chemin d’accès spécifié.

## <a name="BKMK_examples"></a>Exemples

Pour créer un répertoire nommé Directory1 dans le répertoire actif, tapez :
```
md Directory1
```
Pour créer l’arborescence de répertoires Taxes\Property\Current dans le répertoire racine, avec les extensions de commande est activées, tapez :
```
md \Taxes\Property\Current
```
Pour créer l’arborescence de répertoires Taxes\Property\Current dans le répertoire racine, comme dans l’exemple précédent, mais avec des extensions de commande désactivées, tapez la séquence de commandes suivante :
```
md \Taxes
cd \Taxes 
md Property
cd Property
md Current
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

[Cmd](cmd.md)