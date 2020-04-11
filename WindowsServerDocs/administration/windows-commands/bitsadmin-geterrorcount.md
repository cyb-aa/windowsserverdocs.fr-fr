---
title: bitsadmin geterrorcount
description: La rubrique commandes Windows pour **Bitsadmin GetErrorCount**, qui récupère le nombre de fois où le travail spécifié a généré une erreur temporaire.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8840ae78-52b0-4c7e-b592-0547359a237e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ef0bf043517d4edfa8d72888746ca5d9c92ecc21
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123134"
---
# <a name="bitsadmin-geterrorcount"></a>bitsadmin geterrorcount

Récupère le nombre de fois où le travail spécifié a généré une erreur temporaire.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /geterrorcount <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère des informations sur le nombre d’erreurs pour le travail nommé *myDownloadJob*.

```
C:\>bitsadmin /geterrorcount myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)