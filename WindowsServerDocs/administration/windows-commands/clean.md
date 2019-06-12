---
title: clean
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: cd5eb2ec1bde4523eb6f0f919e09b9711b2654fb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434320"
---
# <a name="clean"></a>clean

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La commande clean Diskpart supprime toute partition ou volume de mise en forme à partir du disque qui a le focus.
## <a name="syntax"></a>Syntaxe
```
clean [all]
```
## <a name="parameters"></a>Paramètres

| Paramètre |                                                        Description                                                        |
|-----------|---------------------------------------------------------------------------------------------------------------------------|
|    tous    | Spécifie que tous les secteurs sur le disque sont défini sur zéro, ce qui supprime complètement toutes les données contenues sur le disque. |

## <a name="remarks"></a>Notes
- Sur les disques (MBR) master boot record, uniquement le partitionnement MBR informations et informations de secteurs cachés sont remplacés.
- Sur les disques de la Table de Partition GUID (gpt), les informations, y compris le MBR de protection, de partitionnement gpt est remplacé. Il n’existe aucune information de secteurs cachés.
- Un disque doit être sélectionné pour cette opération réussisse. Utilisez le **sélectionnez disque** commande pour sélectionner un disque et de déplacer le focus vers elle.
  ## <a name="BKMK_examples"></a>Exemples
  Pour supprimer toute la mise en forme à partir du disque sélectionné, tapez :
  ```
  clean
  ```

[Clear-Disk](https://technet.microsoft.com/library/hh848661.aspx)
