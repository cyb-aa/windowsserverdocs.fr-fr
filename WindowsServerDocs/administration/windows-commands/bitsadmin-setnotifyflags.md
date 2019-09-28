---
title: bitsadmin setnotifyflags
description: La rubrique commandes Windows pour **Bitsadmin setnotifyflags** -définit les indicateurs de notification d’événement pour le travail spécifié.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d5763d95-94a6-45ca-9e03-891c20047e06
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d9cfabf05610cbbe8fa65fd16b0d33e161dcef9b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380453"
---
# <a name="bitsadmin-setnotifyflags"></a>bitsadmin setnotifyflags

Définit les indicateurs de notification d’événement pour le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetNotifyFlags <Job> <NotifyFlags>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|NotifyFlags|Voir les notes|

## <a name="remarks"></a>Notes

Le paramètre **NotifyFlags** peut contenir un ou plusieurs des indicateurs de notification suivants.

|-----|-----| | 1 | Générez un événement lorsque tous les fichiers du travail ont été transférés. | | 2 | Génère un événement lorsqu’une erreur se produit. | | 4 | Désactiver les notifications. |

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant définit le travail indicateurs de notification pour les événements de transfert et d’erreur pour le travail nommé *myDownloadJob*.
```
C:\>bitsadmin /SetNotifyFlags myDownloadJob 3
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)