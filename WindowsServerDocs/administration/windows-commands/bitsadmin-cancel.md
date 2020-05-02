---
title: bitsadmin cancel
description: Rubrique de référence pour la commande Bitsadmin Cancel, qui supprime le travail de la file d’attente de transfert et supprime tous les fichiers temporaires associés à ce travail.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7374b544-6a16-4d3e-872c-dcf4c02ad89d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 95fefbc4a9731c2ccbac22adc27f8231a7f36138
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718254"
---
# <a name="bitsadmin-cancel"></a>bitsadmin cancel

Supprime le travail de la file d’attente de transfert et supprime tous les fichiers temporaires associés à ce travail.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /cancel <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| travail | Nom complet ou GUID du travail. |

## <a name="examples"></a>Exemples

Pour supprimer le travail *myDownloadJob* de la file d’attente de transfert :

```
bitsadmin /cancel myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
