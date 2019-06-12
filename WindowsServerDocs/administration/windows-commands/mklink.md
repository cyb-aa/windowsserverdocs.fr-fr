---
title: mklink
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ca9f5f00bc92d7929f782be45562e80bba455d74
ms.sourcegitcommit: cd12ace92e7251daaa4e9fabf1d8418632879d38
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66501454"
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
|/d|Crée un lien symbolique du répertoire. Par défaut, **mklink** crée un lien symbolique du fichier.|
|/h|Crée un lien physique au lieu d’un lien symbolique.|
|/j|Crée une jonction de répertoires.|
|\<Link>|Spécifie le nom du lien symbolique en cours de création.|
|\<Target>|Spécifie le chemin d’accès (relatif ou absolu) qui désigne le nouveau lien symbolique.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant illustre la création et la suppression d’un lien symbolique nommé MyFolder et MyFile.file à partir du répertoire racine dans le répertoire \Users\User1\Documents et un example.file situé dans le répertoire :
```
mklink /d \MyFolder \Users\User1\Documents
mklink /h \MyFile.file \User1\Documents\example.file
rd \MyFolder
del \MyFile.file
```
## <a name="additional-references"></a>Références supplémentaires
-   [New-Item](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/new-item?view=powershell-6)
-   [del](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/del)
-   [rmdir](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/rd)
