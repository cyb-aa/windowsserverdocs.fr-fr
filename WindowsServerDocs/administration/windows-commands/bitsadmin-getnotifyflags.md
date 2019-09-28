---
title: bitsadmin getnotifyflags
description: La rubrique commandes Windows pour **Bitsadmin getnotifyflags** -récupère les indicateurs de notification pour le travail spécifié.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d4657e6c-8959-4db7-a4af-e73d3f80ecf8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 56ee3a30050b6cc934b35bab24e9508911ea250e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381481"
---
# <a name="bitsadmin-getnotifyflags"></a>bitsadmin getnotifyflags



Récupère les indicateurs de notification pour le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetNotifyFlags <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="remarks"></a>Notes

La tâche peut contenir un ou plusieurs des indicateurs de notification suivants.

|-----|-----| | 0x001 | Générez un événement lorsque tous les fichiers du travail ont été transférés. | | 0x002 | Génère un événement lorsqu’une erreur se produit. | | 0x004 | Désactiver les notifications. | | 0x008 | Générez un événement lorsque le travail est modifié ou que la progression du transfert est effectuée. |

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant récupère les indicateurs Notify pour la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /GetNotifyFlags myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)