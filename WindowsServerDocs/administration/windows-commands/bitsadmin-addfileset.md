---
title: bitsadmin addfileset
description: Rubrique relative aux commandes Windows pour **Bitsadmin ADDFILESET**, qui ajoute un ou plusieurs fichiers au travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 75466994-262f-4724-b14d-f813c5397675
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c4cff7dc8439fe8e1c54d1f5d231d1b487dc70c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850972"
---
# <a name="bitsadmin-addfileset"></a>bitsadmin addfileset

Ajoute un ou plusieurs fichiers au travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /addfileset <Job> <TextFile>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| Tâche | Nom complet ou GUID du travail. |
| TextFile | Un fichier texte, chaque ligne contenant un nom de fichier distant et un nom de fichier local. **Remarque :** Les noms sont délimités par des espaces. Les lignes qui commencent par un caractère `#` sont traitées comme un commentaire. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

```
C:\>bitsadmin /addfileset files.txt
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)