---
title: bitsadmin gettype
description: Rubrique de référence pour la commande Bitsadmin GetType, qui récupère le type de tâche du travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bec16f04-3e95-4587-889e-3de6ad03c9c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 151f9b8e81229a666111ebcd20f060d84160445a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717475"
---
# <a name="bitsadmin-gettype"></a>bitsadmin gettype

Récupère le type de tâche du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /gettype <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |

#### <a name="output"></a>Output

Les valeurs de sortie retournées peuvent être :

| Type | Description |
| --------------- | ----------- |
| Téléchargement | Le travail est un téléchargement. |
| Télécharger | Le travail est un chargement. |
| Télécharger-répondre | Le travail est un chargement-réponse. |
| Unknown | Le type du travail est inconnu. |

## <a name="examples"></a>Exemples

Pour récupérer le type de tâche pour le travail nommé *myDownloadJob*:

```
bitsadmin /gettype myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
