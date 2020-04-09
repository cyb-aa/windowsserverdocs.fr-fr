---
title: bitsadmin getowner
description: La rubrique commandes Windows pour Bitsadmin **GetOwner**, qui récupère le propriétaire du travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5203f84c-a879-4f31-ae3e-7ea74bd63ca5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e622c3759c9ec20867c693539c4481c70aa4f26
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850562"
---
# <a name="bitsadmin-getowner"></a>bitsadmin getowner

Affiche le nom complet ou le GUID du propriétaire du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getowner <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant affiche le propriétaire du travail nommé *myDownloadJob*.

```
C:\>bitsadmin /getowner myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)