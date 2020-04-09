---
title: Bitsadmin getPriority,
description: La rubrique commandes Windows pour **Bitsadmin getPriority,** , qui récupère la priorité du travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: b27829a0fb852abb88c88a65e61e8d7693ca2df2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850542"
---
# <a name="bitsadmin-getpriority"></a>Bitsadmin getPriority,

Récupère la priorité du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getpriority <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="remarks"></a>Notes

La priorité de cette commande peut être :

- **SOMBRE**

- **RAPIDE**

- **NORMAL**

- **ENTRÉE**

- **CONNUE**

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère la priorité pour la tâche nommée *myDownloadJob*.

```
C:\>bitsadmin /getpriority myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
