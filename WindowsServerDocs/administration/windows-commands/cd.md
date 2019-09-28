---
title: cd
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ed0942232eb205a8198d4b3d366ca9482af1f4b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379708"
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

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/d|Modifie le lecteur actif, ainsi que le répertoire actif d’un lecteur.|
|> @no__t 0Drive :|Spécifie le lecteur à afficher ou à modifier (s’il est différent du lecteur en cours).|
|@no__t 0Path >|Spécifie le chemin d’accès au répertoire que vous souhaitez afficher ou modifier.|
|[..]|Spécifie que vous souhaitez passer au dossier parent.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

Si les extensions de commande sont activées, les conditions suivantes s’appliquent à la commande **CD** :
- La chaîne de répertoire active est convertie pour utiliser la même casse que les noms sur le disque. Par exemple, `cd C:\TEMP` définit le répertoire en cours sur C:\Temp si c’est le cas sur le disque.
- Les espaces ne sont pas considérés comme des délimiteurs ; par conséquent, le *chemin d’accès* peut contenir des espaces sans guillemets. Exemple :  
  ```
  cd username\programs\start menu
  ```  
  est identique à :  
  ```
  cd "username\programs\start menu"
  ```  
  Toutefois, les guillemets sont requis si les extensions sont désactivées.

Pour désactiver les extensions de commande, tapez :
```
cmd /e:off
```

## <a name="BKMK_examples"></a>Illustre

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

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)