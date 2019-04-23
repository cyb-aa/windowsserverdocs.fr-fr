---
title: type
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c44fe905-a865-4c97-8cc5-fb95fec7d4d5
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: 4ceb7365d34a2aeca21d1a699730a589f98fd549
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887400"
---
# <a name="type"></a>type


Dans l’interface de commande de Windows, **type** est intégré dans la commande qui affiche le contenu d’un fichier texte. Utilisez le **type** commande pour afficher un fichier texte sans le modifier.


Dans PowerShell, **type** est un alias intégré à la **[Get-Content](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-content)** applet de commande qui affiche également le contenu d’un fichier, mais avec une syntaxe différente.


Pour obtenir des exemples montrant comment utiliser cette commande au sein de l’interface de commande de Windows (Cmd.exe), consultez [exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
type [<Drive>:][<Path>]<FileName>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[\<Drive>:][\<Path>]\<FileName>|Spécifie l’emplacement et le nom du fichier ou des fichiers que vous souhaitez afficher. Séparez plusieurs noms de fichiers avec des espaces.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Si *FileName* contient des espaces, placez-le entre guillemets (par exemple, « fichier nom contenant Spaces.txt »).
-   Si vous affichez un fichier binaire ou un fichier qui est créé par un programme, vous pouvez voir des caractères étranges dans l’écran, notamment les caractères de saut de page et des symboles de séquence d’échappement. Ces caractères représentent les codes de contrôle qui sont utilisés dans le fichier binaire. En règle générale, évitez d’utiliser le **type** commande pour afficher les fichiers binaires.

## <a name="BKMK_examples"></a>Exemples

Pour afficher le contenu d’un fichier nommé vacances.mer, tapez :
```
type holiday.mar 
```
Pour afficher le contenu d’un long fichier nommé vacances.mer un écran à la fois, tapez :
```
type holiday.mar | more 
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
