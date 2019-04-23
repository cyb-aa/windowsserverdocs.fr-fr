---
title: bitsadmin getnotifyflags
description: Rubrique de commandes de Windows pour **bitsadmin getnotifyflags** -récupère les indicateurs de notification pour le travail spécifié.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 690e94805c5e61d96603e4ade102fb3a4bda409e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889280"
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
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="remarks"></a>Notes

Le travail peut contenir un ou plusieurs des indicateurs de notification suivants.

|---|---| | 0 x 001 | Générer un événement lorsque tous les fichiers dans le travail ayant été transférés. | | 0 x 002 | Générer un événement lorsqu’une erreur se produit. | | 0 x 004 | Désactiver les notifications. | | 0 x 008 | Générer un événement lorsque le travail est modifié ou la progression du transfert est effectuée. |

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant récupère les indicateurs de notification pour le travail nommé *myDownloadJob*.
```
C:\>bitsadmin /GetNotifyFlags myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)