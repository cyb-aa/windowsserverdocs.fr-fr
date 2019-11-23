---
title: Md
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 965a5c506535a2c52d6cc7b3557c6104182c12a5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373691"
---
# <a name="md"></a>Md



Crée un répertoire ou un sous-répertoire.

> [!NOTE]
> Cette commande est identique à la commande **mkdir** .

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
md [<Drive>:]<Path>
mkdir [<Drive>:]<Path>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|> du lecteur de \<:|Spécifie le lecteur sur lequel vous souhaitez créer le nouveau répertoire.|
|Chemin de \<>|Obligatoire. Spécifie le nom et l’emplacement du nouveau répertoire. La longueur maximale d’un chemin d’accès unique est déterminée par le système de fichiers.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

Les extensions de commande, qui sont activées par défaut, vous permettent d’utiliser une seule commande **MD** pour créer des répertoires intermédiaires dans un chemin d’accès spécifié.

## <a name="BKMK_examples"></a>Illustre

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
cd \Taxes 
md Property
cd Property
md Current
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

[Cmd](cmd.md)