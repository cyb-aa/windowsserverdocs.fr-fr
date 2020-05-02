---
title: convert
description: Rubrique de référence pour la commande Convert, qui convertit un disque d’un type de disque à un autre.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ae151297-af21-4701-bd69-21d775518e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ab7189ea774750f8de2ceaecd9511fc8c3a71a97
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720742"
---
# <a name="convert"></a>convert

Convertit un disque d’un type de disque à un autre.

## <a name="syntax"></a>Syntaxe

```
convert basic
convert dynamic
convert gpt
convert mbr
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| [convertir la commande de base](convert-basic.md) | Convertit un disque dynamique vide en disque de base. |
| [convertir la commande dynamique](convert-dynamic.md) | Convertit un disque de base en disque dynamique. |
| [convertir la commande GPT](convert-gpt.md) | Convertit un disque de base vide avec le style de partition d’enregistrement de démarrage principal (MBR) en disque de base avec le style de partition GPT (GUID partition table). |
| [commande convert mbr](convert-mbr.md) | Convertit un disque de base vide avec le style de partition GPT (GUID partition table) en disque de base avec le style de partition enregistrement de démarrage principal (MBR). |

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
