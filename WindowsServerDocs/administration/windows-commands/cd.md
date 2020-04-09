---
title: cd
description: La rubrique commandes Windows pour CD, qui affiche le nom ou modifie le répertoire actif.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 932d9cc1-3dff-40da-835c-1cb0894874f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d62f529ab6c45957f0fdea24358a2f13151adb6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848222"
---
# <a name="cd"></a>cd

Affiche le nom ou modifie le répertoire actif. S’il est utilisé uniquement avec une lettre de lecteur (par exemple, `cd C:`), **CD** affiche les noms du répertoire actif dans le lecteur spécifié. S’il est utilisé sans paramètres, **CD** affiche le lecteur et le répertoire en cours.

> [!NOTE]
> Cette commande est identique à la commande **ChDir** .

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
cd [/d] [<Drive>:][<Path>]
cd [..]
chdir [/d] [<Drive>:][<Path>]
chdir [..]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/d|Modifie le lecteur actif, ainsi que le répertoire actif d’un lecteur.|
|> du lecteur de \<:|Spécifie le lecteur à afficher ou à modifier (s’il est différent du lecteur en cours).|
|Chemin de \<>|Spécifie le chemin d’accès au répertoire que vous souhaitez afficher ou modifier.|
|[..]|Spécifie que vous souhaitez passer au dossier parent.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

Si les extensions de commande sont activées, les conditions suivantes s’appliquent à la commande **CD** :
- La chaîne de répertoire active est convertie pour utiliser la même casse que les noms sur le disque. Par exemple, `cd C:\TEMP` définirait le répertoire en cours sur C:\Temp si c’est le cas sur le disque.
- Les espaces ne sont pas considérés comme des délimiteurs ; par conséquent, le *chemin d’accès* peut contenir des espaces sans guillemets. Par exemple :  
  ```
  cd username\programs\start menu
  ```  
  est identique à :  
  ```
  cd username\programs\start menu
  ```  
  Toutefois, les guillemets sont requis si les extensions sont désactivées.

Pour désactiver les extensions de commande, tapez :
```
cmd /e:off
```

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Le répertoire racine est le haut de la hiérarchie de répertoires d’un lecteur. Pour revenir au répertoire racine, tapez :
```
cd\
```
Pour modifier le répertoire par défaut sur un lecteur différent de celui sur lequel vous êtes, tapez :
```
cd [<Drive>:\[<Directory>]]
```
Pour vérifier la modification apportée au répertoire, tapez :
```
cd [<Drive>:]
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)