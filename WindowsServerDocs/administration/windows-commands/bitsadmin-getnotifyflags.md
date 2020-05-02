---
title: bitsadmin getnotifyflags
description: Rubrique de référence pour la commande Bitsadmin getnotifyflags, qui récupère les indicateurs de notification pour le travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d4657e6c-8959-4db7-a4af-e73d3f80ecf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 36e4c3584b2e3be9c9985756aeaec08b40e74b0c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717765"
---
# <a name="bitsadmin-getnotifyflags"></a>bitsadmin getnotifyflags

Récupère les indicateurs de notification pour le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getnotifyflags <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |

## <a name="remarks"></a>Notes 

La tâche peut contenir un ou plusieurs des indicateurs de notification suivants :

| Indicateur | Description |
| ----- | ----- |
| 0x001 | Générez un événement lorsque tous les fichiers du travail ont été transférés. |
| 0x002 | Génère un événement lorsqu’une erreur se produit. |
| 0x004 | Désactivez les notifications. |
| 0x008 | Générez un événement lorsque le travail est modifié ou que la progression du transfert est effectuée. |

## <a name="examples"></a>Exemples

Pour récupérer les indicateurs de notification pour le travail nommé *myDownloadJob*:

```
bitsadmin /getnotifyflags myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
