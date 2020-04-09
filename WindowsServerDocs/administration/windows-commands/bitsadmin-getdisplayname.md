---
title: bitsadmin getdisplayname
description: La rubrique commandes Windows pour **Bitsadmin GetDisplayName**, qui récupère le nom complet du travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e5c0e76c-4cc6-42d8-ac30-30bf3dc11b9b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6944dc2b7a63ca986fb285d26796f350c1052295
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850712"
---
# <a name="bitsadmin-getdisplayname"></a>bitsadmin getdisplayname

Récupère le nom complet du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getdisplayname <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant extrait le nom complet de la tâche nommée *myDownloadJob*.

```
C:\>bitsadmin /getdisplayname myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)