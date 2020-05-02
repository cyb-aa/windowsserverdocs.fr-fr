---
title: type
description: Rubrique de référence pour le type, qui affiche le contenu d’un fichier texte.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c44fe905-a865-4c97-8cc5-fb95fec7d4d5
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: d96aa066d9d9510d677d9750eb9e926a9d91cd65
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721224"
---
# <a name="type"></a>type

Dans l’interface de commande Windows, **tapez** est une commande intégrée qui affiche le contenu d’un fichier texte. Utilisez la commande **type** pour afficher un fichier texte sans le modifier.

Dans PowerShell, le **type** est un alias intégré à l’applet de commande **[obtenir-content](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-content)** , qui affiche également le contenu d’un fichier, mais avec une syntaxe différente.

## <a name="syntax"></a>Syntaxe

```
type [<Drive>:][<Path>]<FileName>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[\<Lecteur> :] [\<Chemin>] \<Nom de fichier>|Spécifie l’emplacement et le nom des fichiers que vous souhaitez afficher. Séparez les noms de fichiers multiples par des espaces.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

-   Si *filename* contient des espaces, mettez-le entre guillemets (par exemple, nom de fichier contenant Spaces. txt).
-   Si vous affichez un fichier binaire ou un fichier créé par un programme, vous pouvez voir des caractères étranges à l’écran, y compris les caractères saut et les symboles de séquence d’échappement. Ces caractères représentent les codes de contrôle utilisés dans le fichier binaire. En général, évitez d’utiliser la commande **type** pour afficher des fichiers binaires.

## <a name="examples"></a>Exemples

Pour afficher le contenu d’un fichier nommé vacances. Mar, tapez :
```
type holiday.mar 
```
Pour afficher le contenu d’un fichier de longue durée nommé vacances. Mar un écran à la fois, tapez :
```
type holiday.mar | more 
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
