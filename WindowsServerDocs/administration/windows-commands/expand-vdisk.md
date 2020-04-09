---
title: développer vdisk
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3ae547b4-3813-4b86-bacd-bc273c028a2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 272714372a35f7f205b5a2e70cb2f2669b3a0634
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844892"
---
# <a name="expand-vdisk"></a>développer vdisk

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
>   ## <a name="examples"></a><a name=BKMK_Examples></a>Illustre
>   Pour développer le disque dur virtuel sélectionné à 20 Go, tapez :
>   ```
>   expand vdisk maximum=20000
>   ```
>   ## <a name="additional-references"></a>Références supplémentaires
> - - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
> - [attacher vdisk](attach-vdisk.md)
> - [Compact vdisk](compact-vdisk.md)

-   [Détacher vdisk](detach-vdisk.md)
-   [détailler vdisk](detail-vdisk.md)
-   [Merge vdisk](merge-vdisk.md)
-   [sélectionner vdisk](select-vdisk.md)
-   [list_1](list_1.md)
