---
title: clean
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9bbe6fd3-e07e-487b-9035-910957a1d326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f871ad1d13e06bf0cbb886ba64a52e7a55a9a797
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379323"
---
# <a name="clean"></a>clean

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

La commande DiskPart Clean supprime toute mise en forme de partition ou de volume du disque qui a le focus.
## <a name="syntax"></a>Syntaxe
```
clean [all]
```
## <a name="parameters"></a>Paramètres

| Paramètre |                                                        Description                                                        |
|-----------|---------------------------------------------------------------------------------------------------------------------------|
|    tous    | Spécifie que chaque secteur sur le disque est défini à zéro, ce qui supprime complètement toutes les données contenues sur le disque. |

## <a name="remarks"></a>Notes
- Sur les disques d’enregistrement de démarrage principal (MBR), seules les informations de partitionnement MBR et les informations de secteur masqué sont remplacées.
- Sur les disques GPT (GUID partition table), les informations de partitionnement GPT, y compris le MBR de protection, sont remplacées. Il n’y a pas d’informations de secteur masquées.
- Vous devez sélectionner un disque pour que cette opération aboutisse. Utilisez la commande **Sélectionner le disque** pour sélectionner un disque et lui déplacer le focus.
  ## <a name="BKMK_examples"></a>Illustre
  Pour supprimer toute la mise en forme du disque sélectionné, tapez :
  ```
  clean
  ```

[Clear-Disk](https://technet.microsoft.com/library/hh848661.aspx)
