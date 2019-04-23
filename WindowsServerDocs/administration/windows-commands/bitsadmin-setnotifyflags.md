---
title: bitsadmin setnotifyflags
description: Rubrique de commandes de Windows pour **bitsadmin setnotifyflags** -définit des indicateurs de notification pour le travail spécifié de l’événement.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: bc817e03e0f1916ea392830d14985a7a1377d69a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868790"
---
# <a name="bitsadmin-setnotifyflags"></a>bitsadmin setnotifyflags

Définit l’événement indicateurs de notification pour le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetNotifyFlags <Job> <NotifyFlags>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|
|NotifyFlags|Consultez la section Notes|

## <a name="remarks"></a>Notes

Le **NotifyFlags** paramètre peut contenir un ou plusieurs des indicateurs de notification suivants.

|---|---| | 1 | Générer un événement lorsque tous les fichiers dans le travail ayant été transférés. | | 2 | Générer un événement lorsqu’une erreur se produit. | | 4 | Désactiver les notifications. |

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant définit les indicateurs de notification pour transférée et événements d’erreur du travail pour la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /SetNotifyFlags myDownloadJob 3
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)