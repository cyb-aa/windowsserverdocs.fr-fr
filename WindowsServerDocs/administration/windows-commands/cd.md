---
title: cd
description: Rubrique de référence pour la commande CD, qui affiche le nom ou modifie le répertoire actif.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 932d9cc1-3dff-40da-835c-1cb0894874f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7c9ee57590cf165ba46f394cab06817c7c13f0a9
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719656"
---
# <a name="cd"></a>cd

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche le nom du répertoire actif ou modifie le répertoire actif. S’il est utilisé uniquement avec une lettre de lecteur ( `cd C:`par exemple,), **CD** affiche les noms du répertoire actif dans le lecteur spécifié. S’il est utilisé sans paramètres, **CD** affiche le lecteur et le répertoire en cours.

> [!NOTE]
> Cette commande est identique à la [commande chdir](chdir.md).

## <a name="syntax"></a>Syntaxe

```
cd [/d] [<drive>:][<path>]
cd [..]
chdir [/d] [<drive>:][<path>]
chdir [..]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| /d | Modifie le lecteur actif, ainsi que le répertoire actif d’un lecteur. |
| `<drive>:` | Spécifie le lecteur à afficher ou à modifier (s’il est différent du lecteur en cours). |
| `<path>` | Spécifie le chemin d’accès au répertoire que vous souhaitez afficher ou modifier. |
| [..] | Spécifie que vous souhaitez passer au dossier parent. |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="remarks"></a>Notes 

Si les extensions de commande sont activées, les conditions suivantes s’appliquent à la commande **CD** :

- La chaîne de répertoire active est convertie pour utiliser la même casse que les noms sur le disque. Par exemple, `cd c:\temp` définit le répertoire en cours sur C:\temp si c’est le cas sur le disque.

- Les espaces ne sont pas traités comme délimiteurs. ils peuvent donc `<path>` contenir des espaces sans guillemets. Par exemple :

  ```
  cd username\programs\start menu
  ```

  est identique à :  
  
  ```
  cd "username\programs\start menu"
  ```

  Si les extensions sont désactivées, les guillemets sont requis.

- Pour désactiver les extensions de commande, tapez :

  ```
  cmd /e:off
  ```

## <a name="examples"></a>Exemples

Pour revenir au répertoire racine, en haut de la hiérarchie de répertoires pour un lecteur :

```
cd\
```

Pour modifier le répertoire par défaut sur un lecteur différent de celui sur lequel vous vous trouvez :

```
cd [<drive>:[<directory>]]
```

Pour vérifier la modification apportée au répertoire, tapez :

```
cd [<drive>:]
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [chdir, commande](chdir.md)
