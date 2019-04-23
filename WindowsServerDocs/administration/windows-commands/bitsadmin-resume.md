---
title: bitsadmin resume
description: Rubrique de commandes de Windows pour **bitsadmin resume** -Active un travail nouveau ou suspendu dans la file d’attente de transfert.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c7540a9-a11a-4910-923a-2a2a61cbf11d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 76027ac927f8a9bb2558e3ce6d75e4f6692e56e7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842030"
---
# <a name="bitsadmin-resume"></a>bitsadmin resume



Active un travail nouveau ou suspendu dans la file d’attente de transfert.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Resume <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant reprend le travail nommé *myDownloadJob*.
```
C:\>bitsadmin /Resume myDownloadJob
```
Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)