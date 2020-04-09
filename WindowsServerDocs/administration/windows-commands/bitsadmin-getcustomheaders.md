---
title: Bitsadmin getcustomheaders
description: La rubrique commandes Windows pour **Bitsadmin getcustomheaders**, qui récupère les en-têtes HTTP personnalisés du travail.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f0d38d3-e865-4474-81e8-773d65c3d1cc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 80a3d1230fd541f0b986434ce373da4c8bb0c761
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850732"
---
# <a name="bitsadmin-getcustomheaders"></a>Bitsadmin getcustomheaders

Récupère les en-têtes HTTP personnalisés du travail.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getcustomheaders <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant obtient les en-têtes personnalisés pour le travail nommé *myDownloadJob*.

```
C:\>bitsadmin /getcustomheaders myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)