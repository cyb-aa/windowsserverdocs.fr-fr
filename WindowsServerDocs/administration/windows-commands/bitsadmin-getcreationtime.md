---
title: bitsadmin getcreationtime
description: La rubrique commandes Windows pour **Bitsadmin GetCreationTime**, qui récupère l’heure de création du travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: be409cb5-ce72-41d9-aafa-edd4e230fd14
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cd8f718e53cc44dc5f4c6f5ff09c9a5c201e0564
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850742"
---
# <a name="bitsadmin-getcreationtime"></a>bitsadmin getcreationtime

Récupère l’heure de création du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getcreationtime <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère l’heure de création de la tâche nommée *myDownloadJob*.

```
C:\>bitsadmin /getcreationtime myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)