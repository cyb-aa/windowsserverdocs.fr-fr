---
title: bitsadmin takeownership
description: La rubrique commandes Windows pour Bitsadmin TakeOwnership, qui permet à un utilisateur disposant de privilèges d’administrateur de prendre possession du travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ea0ce7cb-440a-498f-a3ef-8368fa43e399
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a2c0bfc1fcb1606102aece76129c49aad701ead
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849022"
---
# <a name="bitsadmin-takeownership"></a>bitsadmin takeownership

Permet à un utilisateur disposant de privilèges d’administrateur d’assumer la propriété du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /TakeOwnership <Job>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant prend la propriété de la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /TakeOwnership myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)