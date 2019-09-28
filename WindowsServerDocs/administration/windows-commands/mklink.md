---
title: mklink
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0ce4df22-2dbc-48fc-9c16-b721ae85f857
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9d930cbf7acbfceab16f2fa619aaaac6e789c131
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373640"
---
# <a name="mklink"></a>mklink
Crée un lien symbolique.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
mklink [[/d] | [/h] | [/j]] <Link> <Target>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/d|Crée un lien symbolique de répertoire. Par défaut, **MKLINK** crée un lien symbolique de fichier.|
|/h|Crée un lien physique au lieu d’un lien symbolique.|
|/j|Crée une jonction de répertoire.|
|@no__t 0Link >|Spécifie le nom du lien symbolique en cours de création.|
|@no__t 0Target >|Spécifie le chemin d’accès (relatif ou absolu) auquel le nouveau lien symbolique fait référence.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="BKMK_examples"></a>Illustre

L’exemple suivantes illustre la création et la suppression d’un lien symbolique nommé mondossier et MyFile. file du répertoire racine vers le répertoire \Users\User1\Documents et d’un exemple de fichier situé dans le répertoire suivant :
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
