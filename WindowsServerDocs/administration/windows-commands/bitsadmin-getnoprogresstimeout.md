---
title: bitsadmin getnoprogresstimeout
description: Rubrique de référence pour la commande Bitsadmin getnoprogresstimeout, qui récupère la durée, en secondes, pendant laquelle le service tentera de transférer le fichier après qu’une erreur temporaire se soit produite.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9cd9b19b-cbb4-4352-8419-978080f016b6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3ee0377bde8a438f23ca571bc9859deef92f18fb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717813"
---
# <a name="bitsadmin-getnoprogresstimeout"></a>bitsadmin getnoprogresstimeout

Récupère la durée, en secondes, pendant laquelle le service tente de transférer le fichier après qu’une erreur temporaire s’est produite.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getnoprogresstimeout <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |

## <a name="examples"></a>Exemples

Pour récupérer la valeur du délai d’attente de la tâche nommée *myDownloadJob*:

```
bitsadmin /getnoprogresstimeout myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
