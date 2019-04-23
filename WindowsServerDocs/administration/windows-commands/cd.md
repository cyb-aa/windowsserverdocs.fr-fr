---
title: cd
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 932d9cc1-3dff-40da-835c-1cb0894874f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5f907b6162e6767820e23222e287b933397397d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861100"
---
# <a name="cd"></a>cd



Affiche le nom d’ou change le répertoire actif. Si utilisé avec uniquement une lettre de lecteur (par exemple, `cd C:`), **cd** affiche les noms du répertoire actif dans le lecteur spécifié. Si utilisée sans paramètres, **cd** affiche le lecteur actuel et le répertoire.

> [!NOTE]
> Cette commande est identique à la **chdir** commande.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
cd [/d] [<Drive>:][<Path>]
cd [..]
chdir [/d] [<Drive>:][<Path>]
chdir [..]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/d|Modifie le lecteur actif, ainsi que le répertoire actif pour un lecteur.|
|\<Lecteur > :|Spécifie le lecteur pour afficher ou modifier (s’il diffère du lecteur actif).|
|\<Path>|Spécifie le chemin d’accès au répertoire que vous souhaitez afficher ou modifier.|
|[..]|Spécifie que vous souhaitez modifier dans le dossier parent.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

Si les extensions de commande sont activées, les conditions suivantes s’appliquent à la **cd** commande :
-   La chaîne du répertoire en cours est convertie pour utiliser la même casse que les noms sur le disque. Par exemple, `cd C:\TEMP` aurait la valeur est le répertoire actif C:\Temp si tel est le cas sur le disque.
-   Les espaces ne sont pas traités comme délimiteurs, par conséquent, *chemin d’accès* peut contenir des espaces sans placer entre guillemets. Exemple :  
    ```
    cd username\programs\start menu
    ```  
    est identique à :  
    ```
    cd "username\programs\start menu"
    ```  
    Les guillemets sont requis, toutefois, si les extensions sont désactivées.

Pour désactiver les extensions de commandes, tapez :
```
cmd /e:off
```

## <a name="BKMK_examples"></a>Exemples

Le répertoire racine est en haut de la hiérarchie de répertoires d’un lecteur. Pour revenir au répertoire racine, tapez :
```
cd\
```
Pour modifier le répertoire par défaut sur un lecteur qui est différent de celui que vous êtes sur, tapez :
```
cd [<Drive>:\[<Directory>]]
```
Pour vérifier le changement de répertoire, tapez :
```
cd [<Drive>:]
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)