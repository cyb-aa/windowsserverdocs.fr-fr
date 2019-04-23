---
title: append
description: 'Rubrique de commandes de Windows pour '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9c3fea20-9502-40ad-a442-7a927aad88eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: abf7713b3fd5bbb6172969ca1cc39cbbbbafafc6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881980"
---
# <a name="append"></a>append



Permet aux programmes ouvrir des fichiers de données dans les répertoires spécifiés comme s’ils étaient dans le répertoire actif. Si utilisée sans paramètres, **ajouter** affiche la liste de répertoires ajoutés.

> [!NOTE]
> Cette commande non pris en charge dans Windows 10.
>

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
append [[<Drive>:]<Path>[;...]] [/x[:on|:off]] [/path:[:on|:off] [/e] 
append ;
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[\<Drive>:]<Path>|Spécifie un lecteur et répertoire à ajouter.|
|/x:on|S’applique à répertoires ajoutés aux recherches de fichiers et démarrage des applications.|
|/x:off|S’applique à répertoires ajoutés uniquement aux demandes de fichiers.</br>**/ x : off** est le paramètre par défaut.|
|/path:on|S’applique à répertoires ajoutés aux demandes de fichiers déjà spécifient un chemin d’accès. **/ Path : sur** est le paramètre par défaut.|
|/path:off|Désactive l’effet de **/path : sur**.|
|/e|Stocke une copie de la liste de répertoires ajoutés dans une variable d’environnement nommée APPEND. **/e** peut être utilisé uniquement la première fois que vous utilisez **ajouter** après le démarrage de votre système.|
|;|Efface la liste de répertoires ajoutés.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="BKMK_examples"></a>Exemples

Pour effacer la liste de répertoires ajoutés, tapez :
```
append ;
```
Pour stocker une copie du répertoire ajoutée à une variable d’environnement nommée APPEND, tapez :
```
append /e
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
