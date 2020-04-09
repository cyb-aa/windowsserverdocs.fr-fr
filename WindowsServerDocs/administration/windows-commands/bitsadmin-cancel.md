---
title: bitsadmin cancel
description: La rubrique commandes Windows pour **Bitsadmin annuler**, qui supprime le travail de la file d’attente de transfert et supprime tous les fichiers temporaires associés à ce travail.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7374b544-6a16-4d3e-872c-dcf4c02ad89d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5c2bdeef824bc269671cc5ae926fb77cd5726c58
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850832"
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
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant supprime la tâche *myDownloadJob* de la file d’attente de transfert.

```
C:\>bitsadmin /cancel myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)