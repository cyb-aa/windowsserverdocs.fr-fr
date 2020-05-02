---
title: clean
description: Rubrique de référence pour la commande DiskPart Clean, qui supprime toutes les partitions ou le formatage de volume du disque qui a le focus.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9bbe6fd3-e07e-487b-9035-910957a1d326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45a23919dc07c8c1525808859471fdcb9f9e9403
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82712878"
---
# <a name="clean"></a>clean

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Supprime toutes les partitions ou le formatage de volume du disque qui a le focus.

>[!NOTE]
> Pour obtenir une version PowerShell de cette commande, consultez [commande Clear-Disk](https://docs.microsoft.com/powershell/module/storage/clear-disk).

## <a name="syntax"></a>Syntaxe

```
clean [all]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| all | Spécifie que chaque secteur sur le disque est défini à zéro, ce qui supprime complètement toutes les données contenues sur le disque. |

#### <a name="remarks"></a>Notes 

- Sur les disques d’enregistrement de démarrage principal (MBR), seules les informations de partitionnement MBR et les informations de secteur masqué sont remplacées.

- Sur les disques GPT (GUID partition table), les informations de partitionnement GPT, y compris le MBR de protection, sont remplacées. Il n’y a pas d’informations de secteur masquées.

- Vous devez sélectionner un disque pour que cette opération aboutisse. Utilisez la commande **Sélectionner le disque** pour sélectionner un disque et lui déplacer le focus.

## <a name="examples"></a>Exemples

Pour supprimer toute la mise en forme du disque sélectionné, tapez :

```
clean
```

## <a name="additional-references"></a>Références supplémentaires

- [commande Clear-Disk](https://docs.microsoft.com/powershell/module/storage/clear-disk)

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
