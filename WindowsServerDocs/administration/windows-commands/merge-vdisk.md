---
title: Merge vdisk
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5865bb08-89a3-406c-8328-0ef8868d03e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7023f2a6669ea6f6801e25cbfc87c950ab95a3bc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373731"
---
# <a name="merge-vdisk"></a>Merge vdisk

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Fusionne un disque dur virtuel de différenciation avec son disque dur virtuel parent correspondant. Le disque dur virtuel parent sera modifié pour inclure les modifications du disque dur virtuel de différenciation.
> [!NOTE]
> Cette commande s’applique uniquement à Windows 7 et Windows Server 2008 R2.
> ## <a name="syntax"></a>Syntaxe
> ```
> merge vdisk depth=<n>
> ```
> ### <a name="parameters"></a>Paramètres
> 
> | Paramètre |                                                                                    Description                                                                                    |
> |-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | profondeur = <n> | Indique le nombre de fichiers VHD parents à fusionner. Par exemple, **depth = 1** indique que le disque dur virtuel de différenciation sera fusionné avec un niveau de la chaîne de différenciation. |
> 
> ## <a name="remarks"></a>Notes
> - Pour que cette opération aboutisse, vous devez sélectionner et détacher un disque dur virtuel. Utilisez la commande **Select vdisk** pour sélectionner un disque dur virtuel et lui déplacer le focus.
> - Ce paramètre modifie le disque dur virtuel parent. Par conséquent, les autres disques durs virtuels de différenciation qui dépendent du parent ne seront plus valides.
>   ## <a name="BKMK_Examples"></a>Illustre
>   Pour fusionner un disque dur virtuel de différenciation avec son disque dur virtuel parent, tapez :
>   ```
>   merge vdisk depth=1
>   ```
>   ## <a name="additional-references"></a>Références supplémentaires
> - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
> - [attacher vdisk](attach-vdisk.md)
> - [Compact vdisk](compact-vdisk.md)

-   [détailler vdisk](detail-vdisk.md)
-   [Détacher vdisk](detach-vdisk.md)
-   [développer vdisk](expand-vdisk.md)
-   [sélectionner vdisk](select-vdisk.md)
-   [list_1](list_1.md)
