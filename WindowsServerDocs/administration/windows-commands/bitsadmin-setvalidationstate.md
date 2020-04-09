---
title: Bitsadmin setvalidationstate
description: La rubrique commandes Windows pour Bitsadmin setvalidationstate, qui définit l’état de validation du contenu du fichier donné au sein du travail.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e8fc8e8c-171c-4681-8057-6986b018e576
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: de6480596b55b3a483076297f32ce52a975915db
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849112"
---
# <a name="bitsadmin-setvalidationstate"></a>Bitsadmin setvalidationstate

Définit l’état de validation du contenu du fichier donné au sein du travail.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetValidationState <Job> <file index> <true|false> 
```

### <a name="parameters"></a>Paramètres

| Paramètre  |          Description           |
|------------|--------------------------------|
|    Tâche     | Nom complet ou GUID du travail |
| Index de fichiers |         Commence à 0          |
|    True    |             False              |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant définit l’état de validation du contenu du fichier 2 sur TRUE pour le travail nommé *myJob*.
```
C:\>bitsadmin /SetValidationState myJob 2 TRUE 
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)