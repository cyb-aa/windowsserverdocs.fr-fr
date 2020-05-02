---
title: développer vdisk
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3ae547b4-3813-4b86-bacd-bc273c028a2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c2380045de45397888777f58e3420c75bb6915ae
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725691"
---
# <a name="expand-vdisk"></a>développer vdisk

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

développe un disque dur virtuel (VHD) à la taille que vous spécifiez.
> [!NOTE]
> Cette commande s’applique uniquement à Windows 7 et Windows Server 2008 R2.
> ## <a name="syntax"></a>Syntaxe
> ```
> expand vdisk maximum=<n>
> ```
> ### <a name="parameters"></a>Paramètres
> 
> |  Paramètre  |                      Description                      |
> |-------------|-------------------------------------------------------|
> | maximum =<n> | Spécifie la nouvelle taille du disque dur virtuel en mégaoctets (Mo). |
> 
> ## <a name="remarks"></a>Notes 
> - Pour que cette opération aboutisse, vous devez sélectionner et détacher un disque dur virtuel. Utilisez la commande **Select vdisk** pour sélectionner un volume et le déplacer vers le focus.
>   ## <a name="examples"></a>Exemples
>   Pour développer le disque dur virtuel sélectionné à 20 Go, tapez :
>   ```
>   expand vdisk maximum=20000
>   ```
>   ## <a name="additional-references"></a>Références supplémentaires
> - - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
> - [attach vdisk](attach-vdisk.md)
> - [Compact vdisk](compact-vdisk.md)

-   [Détacher vdisk](detach-vdisk.md)
-   [détailler vdisk](detail-vdisk.md)
-   [Merge vdisk](merge-vdisk.md)
-   [sélectionner vdisk](select-vdisk.md)
-   [list_1](list_1.md)
