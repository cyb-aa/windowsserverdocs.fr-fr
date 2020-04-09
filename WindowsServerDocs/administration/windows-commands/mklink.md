---
title: mklink
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0ce4df22-2dbc-48fc-9c16-b721ae85f857
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3d6328d972b07b1bebd272788b896fd491e47380
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839552"
---
# <a name="mklink"></a>mklink
Crée un lien symbolique.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

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
|Lien \<>|Spécifie le nom du lien symbolique en cours de création.|
|\<> cible|Spécifie le chemin d’accès (relatif ou absolu) auquel le nouveau lien symbolique fait référence.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant illustre la création et la suppression d’un lien symbolique nommé mondossier et MyFile. file du répertoire racine vers le répertoire \Users\User1\Documents et d’un exemple de fichier situé dans le répertoire suivant :
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
