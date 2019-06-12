---
title: compact vdisk
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 40ca0820-67de-4160-b62a-e9bf63fe2790
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c4a95354bf041777e43a9eac5f16e693b02c185c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434305"
---
# <a name="compact-vdisk"></a>compact vdisk

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Réduit la taille physique d’un fichier de disque dur virtuel (VHD) de taille dynamique. Ce paramètre est utile, car l’augmentation de disques durs virtuels de taille à extension dynamique que vous ajoutez des fichiers, mais ils ne diminue pas automatiquement la taille lorsque vous supprimez des fichiers.
> [!NOTE]
> Cette commande est uniquement applicable à Windows 7 et Windows Server 2008 R2.
> ## <a name="syntax"></a>Syntaxe
> ```
> compact vdisk
> ```
> ## <a name="remarks"></a>Notes
> - Un disque dur virtuel de taille dynamique doit être sélectionné pour cette opération réussisse. Utilisez le **sélectionnez vdisk** commande pour sélectionner un disque dur virtuel et de déplacer le focus vers elle.
> - Vous pouvez uniquement compacter de taille dynamique des disques durs virtuels sont détachés ou attachés en lecture seule.
>   ## <a name="BKMK_Examples"></a>Exemples
>   Pour compacter un disque dur virtuel de taille dynamique, tapez :
>   ```
>   compact vdisk
>   ```
>   ## <a name="additional-references"></a>Références supplémentaires
> - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
> - [attach vdisk](attach-vdisk.md)

-   [detail vdisk](detail-vdisk.md)
-   [Détacher vdisk](detach-vdisk.md)
-   [Développez vdisk](expand-vdisk.md)
-   [Fusion vdisk](merge-vdisk.md)
-   [select vdisk](select-vdisk.md)
-   [list_1](list_1.md)
