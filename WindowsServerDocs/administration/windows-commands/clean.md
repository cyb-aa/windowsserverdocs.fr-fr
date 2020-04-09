---
title: clean
description: La rubrique commandes Windows pour la commande DiskPart Clean, qui supprime toutes les mises en forme des partitions et des volumes du disque avec le focus.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9bbe6fd3-e07e-487b-9035-910957a1d326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d573a0480c24a2a622618197dea7dccaeddac271
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847752"
---
# <a name="clean"></a>clean

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La commande DiskPart Clean supprime toute mise en forme de partition ou de volume du disque qui a le focus.

## <a name="syntax"></a>Syntaxe
```
clean [all]
```
### <a name="parameters"></a>Paramètres

| Paramètre |                                                        Description                                                        |
|-----------|---------------------------------------------------------------------------------------------------------------------------|
|    tous    | Spécifie que chaque secteur sur le disque est défini à zéro, ce qui supprime complètement toutes les données contenues sur le disque. |

## <a name="remarks"></a>Notes
- Sur les disques d’enregistrement de démarrage principal (MBR), seules les informations de partitionnement MBR et les informations de secteur masqué sont remplacées.
- Sur les disques GPT (GUID partition table), les informations de partitionnement GPT, y compris le MBR de protection, sont remplacées. Il n’y a pas d’informations de secteur masquées.
- Vous devez sélectionner un disque pour que cette opération aboutisse. Utilisez la commande **Sélectionner le disque** pour sélectionner un disque et lui déplacer le focus.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre
  Pour supprimer toute la mise en forme du disque sélectionné, tapez :
  ```
  clean
  ```

## <a name="additional-references"></a>Références supplémentaires
[Clear-Disk](https://technet.microsoft.com/library/hh848661.aspx)
