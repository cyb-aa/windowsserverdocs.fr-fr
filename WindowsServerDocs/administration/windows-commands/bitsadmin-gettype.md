---
title: bitsadmin gettype
description: La rubrique commandes Windows pour **Bitsadmin GetType**, qui récupère le type de tâche du travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bec16f04-3e95-4587-889e-3de6ad03c9c8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 66a1fc5b0478e1eec26557dc9a7f76d50abcb8b6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850442"
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
| le travail | Nom complet ou GUID du travail. |

## <a name="output"></a>Sortie

Les valeurs de sortie sont les suivantes :

| Type | Description |
| --------------- | ----------- |
| Télécharger | Le travail est un téléchargement. |
| Télécharger | Le travail est un chargement. |
| Télécharger-répondre | Le travail est un chargement-réponse. |
| Inconnu. | Le type du travail est inconnu. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère le type de tâche pour le travail nommé *myDownloadJob*.

```
C:\>bitsadmin /gettype myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)