---
title: bitsadmin getnotifyflags
description: La rubrique commandes Windows pour **Bitsadmin getnotifyflags**, qui récupère les indicateurs de notification pour le travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d4657e6c-8959-4db7-a4af-e73d3f80ecf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3138baea05f793cfb587d3f8fb669d446daea6b5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850582"
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
| le travail | Nom complet ou GUID du travail. |

## <a name="remarks"></a>Notes

La tâche peut contenir un ou plusieurs des indicateurs de notification suivants :

| Indicateur | Description |
| ----- | ----- |
| 0x001 | Générez un événement lorsque tous les fichiers du travail ont été transférés. |
| 0x002 | Génère un événement lorsqu’une erreur se produit. |
| 0x004 | Désactivez les notifications. |
| 0x008 | Générez un événement lorsque le travail est modifié ou que la progression du transfert est effectuée. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère les indicateurs Notify pour la tâche nommée *myDownloadJob*.

```
C:\>bitsadmin /getnotifyflags myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)