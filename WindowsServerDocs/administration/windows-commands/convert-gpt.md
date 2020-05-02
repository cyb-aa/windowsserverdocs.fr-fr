---
title: convert gpt
description: Rubrique de référence pour la commande Convert GPT, qui convertit un disque de base vide avec le style de partition d’enregistrement de démarrage principal (MBR) en disque de base avec le style de partition GPT (GUID partition table).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b3b1b747-0a7a-4be2-8487-2c4be16ee190
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 25b28473716037235a70e05835e23790f93164a1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720766"
---
# <a name="convert-gpt"></a>convert gpt

Convertit un disque de base vide avec le style de partition d’enregistrement de démarrage principal (MBR) en disque de base avec le style de partition GPT (GUID partition table). Vous devez sélectionner un disque MBR de base pour que cette opération aboutisse. Utilisez la [commande Sélectionner le disque](select-disk.md) pour sélectionner un disque de base et décaler le focus vers celui-ci.

> [!IMPORTANT]
> Le disque doit être vide pour pouvoir être converti en disque de base. Sauvegardez vos données, puis supprimez toutes les partitions ou tous les volumes avant de convertir le disque. La taille de disque minimale requise pour la conversion en GPT est de 128 mégaoctets.

> [!NOTE]
> Pour obtenir des instructions sur l’utilisation de cette commande, consultez [modifier un disque d’enregistrement de démarrage principal en disque de table de partition GUID](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc725671(v=ws.11)).

## <a name="syntax"></a>Syntaxe

```
convert gpt [noerr]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| noerr | À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur. |

## <a name="examples"></a>Exemples

Pour convertir un disque de base du style de partition MBR au style de partition GPT, tapez :

```
convert gpt
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Convert](convert.md)
