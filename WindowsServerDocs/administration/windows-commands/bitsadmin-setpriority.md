---
title: bitsadmin setpriority
description: La rubrique commandes Windows pour Bitsadmin SetPriority, qui définit la priorité du travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90788363-01a2-4d7c-a560-a3eba45b5e9e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d007c62402a3d70910e1c79fab5c406295a63a5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849212"
---
# <a name="bitsadmin-setpriority"></a>bitsadmin setpriority

Définit la priorité du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetPriority <Job> <Priority>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|Priorité|Une des valeurs suivantes :</br>-PREMIER plan</br>-ÉLEVÉ</br>-NORMAL</br>-FAIBLE|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant définit la priorité de la tâche nommée *myDownloadJob* sur normal.
```
C:\>bitsadmin /SetPriority myDownloadJob NORMAL
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)