---
title: Ru
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 82162d00-cc34-4776-9e55-4b4836dbd6a9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a605571fb74af99d0f365a100dd33fd4db0d3f22
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724003"
---
# <a name="md"></a>Ru



Crée un répertoire ou un sous-répertoire.

> [!NOTE]
> Cette commande est identique à la commande **mkdir** .



## <a name="syntax"></a>Syntaxe

```
md [<Drive>:]<Path>
mkdir [<Drive>:]<Path>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<> du lecteur :|Spécifie le lecteur sur lequel vous souhaitez créer le nouveau répertoire.|
|\<Chemin d’accès>|Obligatoire. Spécifie le nom et l’emplacement du nouveau répertoire. La longueur maximale d’un chemin d’accès unique est déterminée par le système de fichiers.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

Les extensions de commande, qui sont activées par défaut, vous permettent d’utiliser une seule commande **MD** pour créer des répertoires intermédiaires dans un chemin d’accès spécifié.

## <a name="examples"></a>Exemples

Pour créer un répertoire nommé Directory1 dans le répertoire actif, tapez :
```
md Directory1
```
Pour créer l’arborescence de répertoires Taxes\Property\Current dans le répertoire racine, avec les extensions de commande activées, tapez :
```
md \Taxes\Property\Current
```
Pour créer l’arborescence de répertoires Taxes\Property\Current dans le répertoire racine comme dans l’exemple précédent, mais avec les extensions de commande désactivées, tapez la séquence de commandes suivante :
```
md \Taxes
md \Taxes\Property
md \Taxes\Property\Current
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

[Cmd](cmd.md)