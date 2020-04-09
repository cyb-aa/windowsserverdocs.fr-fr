---
title: exec
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 364e8baf-576f-401b-a431-7d3c06621614
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d39fbf948050dd00f329e461c34c2365030cb05d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844992"
---
# <a name="exec"></a>exec



exécute un fichier sur l’ordinateur local. Le fichier peut être un script **cmd** .

## <a name="syntax"></a>Syntaxe

```
exec <ScriptFile.cmd>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<ScriptFile. cmd >|Spécifie le fichier de script à exécuter.|

## <a name="remarks"></a>Notes

-   Cette commande permet de dupliquer ou de restaurer des données dans le cadre d’une séquence de sauvegarde ou de restauration.
-   Si le script échoue, une erreur est retournée et DiskShadow s’arrête.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)