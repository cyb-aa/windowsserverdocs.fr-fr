---
title: développer vdisk
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ae547b4-3813-4b86-bacd-bc273c028a2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8fb5d7b58b7aa4bc9557086c73020820cfa04284
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377294"
---
# <a name="expand-vdisk"></a>développer vdisk

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

développe un disque dur virtuel (VHD) à la taille que vous spécifiez.
> [!NOTE]
> Cette commande s’applique uniquement à Windows 7 et Windows Server 2008 R2.
> ## <a name="syntax"></a>Syntaxe
> ```
> expand vdisk maximum=<n>
> ```
> ## <a name="parameters"></a>Paramètres
> 
> |  Paramètre  |                      Description                      |
> |-------------|-------------------------------------------------------|
> | maximum = <n> | Spécifie la nouvelle taille du disque dur virtuel en mégaoctets (Mo). |
> 
> ## <a name="remarks"></a>Notes
> - Pour que cette opération aboutisse, vous devez sélectionner et détacher un disque dur virtuel. Utilisez la commande **Select vdisk** pour sélectionner un volume et le déplacer vers le focus.
>   ## <a name="BKMK_Examples"></a>Illustre
>   Pour développer le disque dur virtuel sélectionné à 20 Go, tapez :
>   ```
>   expand vdisk maximum=20000
>   ```
>   ## <a name="additional-references"></a>Références supplémentaires
> - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
> - [attacher vdisk](attach-vdisk.md)
> - [Compact vdisk](compact-vdisk.md)

-   [Détacher vdisk](detach-vdisk.md)
-   [détailler vdisk](detail-vdisk.md)
-   [Merge vdisk](merge-vdisk.md)
-   [sélectionner vdisk](select-vdisk.md)
-   [list_1](list_1.md)
