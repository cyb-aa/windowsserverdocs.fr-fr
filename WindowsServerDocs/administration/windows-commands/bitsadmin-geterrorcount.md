---
title: bitsadmin geterrorcount
description: Rubrique de référence pour la commande Bitsadmin GetErrorCount, qui récupère le nombre de fois où le travail spécifié a généré une erreur temporaire.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8840ae78-52b0-4c7e-b592-0547359a237e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 516bd02ed296a2eba75e174c6f084926bde63e90
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718007"
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
| travail | Nom complet ou GUID du travail. |

## <a name="examples"></a>Exemples

Pour récupérer les informations relatives au nombre d’erreurs pour le travail nommé *myDownloadJob*:

```
bitsadmin /geterrorcount myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
