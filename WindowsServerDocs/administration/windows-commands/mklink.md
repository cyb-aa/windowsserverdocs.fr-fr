---
title: mklink
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0ce4df22-2dbc-48fc-9c16-b721ae85f857
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 83a607711f0abe51810aa5abf4eb731206d810c2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723976"
---
# <a name="mklink"></a>mklink
Crée un lien symbolique.



## <a name="syntax"></a>Syntaxe

```
mklink [[/d] | [/h] | [/j]] <Link> <Target>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/d|Crée un lien symbolique de répertoire. Par défaut, **MKLINK** crée un lien symbolique de fichier.|
|/h|Crée un lien physique au lieu d’un lien symbolique.|
|/j|Crée une jonction de répertoire.|
|\<> de lien|Spécifie le nom du lien symbolique en cours de création.|
|\<Target>|Spécifie le chemin d’accès (relatif ou absolu) auquel le nouveau lien symbolique fait référence.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="examples"></a>Exemples

Pour illustrer la création et la suppression d’un lien symbolique nommé mondossier et MyFile. file du répertoire racine vers le répertoire \Users\User1\Documents et d’un exemple de fichier situé dans le répertoire :
```
mklink /d \MyFolder \Users\User1\Documents
mklink /h \MyFile.file \User1\Documents\example.file
rd \MyFolder
del \MyFile.file
```
## <a name="additional-references"></a>Références supplémentaires
-   [Nouvel élément](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/new-item?view=powershell-6)
-   [del](https://docs.microsoft.com/windows-server/administration/windows-commands/del)
-   [rmdir](https://docs.microsoft.com/windows-server/administration/windows-commands/rd)
