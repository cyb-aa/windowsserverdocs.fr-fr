---
title: Fusion vdisk
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: bc20fcaf6e511bb25156996bddc3357f99195875
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437412"
---
# <a name="merge-vdisk"></a>Fusion vdisk

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Fusionne un différenciation disque dur virtuel (VHD) avec son disque dur virtuel parent correspondant. Le disque dur virtuel parent sera modifié pour inclure les modifications à partir du disque dur virtuel de différenciation.
> [!NOTE]
> Cette commande est uniquement applicable à Windows 7 et Windows Server 2008 R2.
> ## <a name="syntax"></a>Syntaxe
> ```
> merge vdisk depth=<n>
> ```
> ### <a name="parameters"></a>Paramètres
> 
> | Paramètre |                                                                                    Description                                                                                    |
> |-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | depth=<n> | Indique le nombre de fichiers de disque dur virtuel parent à fusionner. Par exemple, **profondeur = 1** indique que le disque dur virtuel de différenciation sera fusionné avec un niveau de la chaîne de différenciation. |
> 
> ## <a name="remarks"></a>Notes
> - Un disque dur virtuel doit être sélectionné et détaché pour cette opération réussisse. Utilisez le **sélectionnez vdisk** commande pour sélectionner un disque dur virtuel et de déplacer le focus vers elle.
> - Ce paramètre modifie le disque dur virtuel parent. Par conséquent, les autres disques durs virtuels de différenciation qui dépendent du parent ne seront plus valides.
>   ## <a name="BKMK_Examples"></a>Exemples
>   Pour fusionner un disque dur virtuel de différenciation avec son disque dur virtuel parent, tapez :
>   ```
>   merge vdisk depth=1
>   ```
>   ## <a name="additional-references"></a>Références supplémentaires
> - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
> - [attach vdisk](attach-vdisk.md)
> - [compact vdisk](compact-vdisk.md)

-   [detail vdisk](detail-vdisk.md)
-   [Détacher vdisk](detach-vdisk.md)
-   [Développez vdisk](expand-vdisk.md)
-   [select vdisk](select-vdisk.md)
-   [list_1](list_1.md)
