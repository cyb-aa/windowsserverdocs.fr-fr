---
title: Net print
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f59b2015-4698-415d-9a74-09566c466f40
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4018ec3779a9735916136fa54f532ad5767c960e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437101"
---
# <a name="net-print"></a>Net print

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche des informations sur une file d’attente de l’imprimante spécifiée ou d’un travail d’impression spécifié, ou des contrôles d’un travail d’impression spécifié.
Pour obtenir des exemples montrant comment utiliser cette commande, consultez le [exemples](#BKMK_examples) section de ce document.
> [!NOTE]
> Cette commande a été déconseillée dans Windows 7 et Windows Server 2008 R2. Toutefois, vous pouvez effectuer la plupart des tâches à l’aide de prnjobs, Windows Management Instrumentation (WMI) ou les applets de commande Windows PowerShell. Pour plus d’informations, consultez [prnjobs](prnjobs.md), [Windows Management Instrumentation](https://go.microsoft.com/fwlink/?LinkID=29991) (https://go.microsoft.com/fwlink/?LinkID=29991), [Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=128426) (https://go.microsoft.com/fwlink/?LinkID=128426)et le [TechNet Script Center galerie](https://go.microsoft.com/fwlink/?LinkId=164635) (https://go.microsoft.com/fwlink/?LinkId=164635).
> ## <a name="syntax"></a>Syntaxe
> ```
> Net print {\\<computerName>\<Sharename> | 
> \\<computerName> <JobNumber> [/hold | /release | /delete]} [help]
> ```
> ## <a name="parameters"></a>Paramètres
> 
> |               Paramètres               |                                                                                                                                                                                                                     Description                                                                                                                                                                                                                      |
> |----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |    \\\\<computerName>\\<Sharename>     |                                                                                                                                                                            Spécifie la file d’attente d’impression et d’ordinateur sur lequel vous souhaitez afficher des informations (par nom).                                                                                                                                                                             |
> |           \\\\<computerName>           |                                                                                                                                 Spécifie l’ordinateur qui héberge le travail d’impression que vous souhaitez contrôler (par nom). Si vous ne spécifiez pas un ordinateur, l’ordinateur local est supposé. Nécessite le <JobNumber> paramètre.                                                                                                                                  |
> |              <JobNumber>               |                                             Spécifie le numéro du travail d’impression à contrôler. Ce nombre est affecté par l’ordinateur qui héberge la file d’attente, où le travail d’impression est envoyé. Une fois un ordinateur affecte un numéro à un travail d’impression que nombre n’est pas attribué à un autre travail dans une file d’attente hébergé par cet ordinateur. Requis lorsque vous utilisez le \\ \\ <computerName> paramètre.                                             |
> | [/hold &#124; /release &#124; /delete] | Spécifie l’action à entreprendre avec le travail d’impression.<br /><br />-Le **/contenir** paramètre retarde le travail, ce qui permet d’autres travaux d’impression pour l’ignorer jusqu'à ce qu’il est libéré.<br />-Le **/release** paramètre libère un travail d’impression qui a été différé.<br />-Le **/delete** paramètre supprime un travail d’impression à partir d’une file d’attente à l’impression.<br /><br />Si vous spécifiez un numéro de projet, mais que vous ne spécifiez pas une action, les informations sur le travail d’impression s’affiche. |
> |                  help                  |                                                                                                                                                                                                     Affiche l’aide de la **Net print** commande.                                                                                                                                                                                                     |
> 
> ## <a name="remarks"></a>Notes
> - **Net print** \\ \\ <computerName> affiche des informations sur les travaux d’impression dans une file d’attente de l’imprimante partagée. Voici un exemple d’un rapport pour tous les travaux d’impression dans une file d’attente pour une imprimante partagée nommée LASER :
>   ```
>   printers at \\PRODUCTION
>   Name              Job #      Size      Status
>   -----------------------------
>   LASER Queue       3 jobs               *printer active*
>      USER1          84        93844      printing
>      USER2          85        12555      Waiting
>      USER3          86        10222      Waiting
>   ```
> - Voici un exemple d’un rapport pour un travail d’impression :
>   ```
>   Job #            35
>   Status           Waiting
>   Size             3096
>   remark
>   Submitting user  USER2
>   Notify           USER2
>   Job data type
>   Job parameters
>   additional info
>   ```
>   ## <a name="BKMK_examples"></a>Exemples
>   Cet exemple montre comment répertorier le contenu de la file d’impression matricielle sur le \\\Production ordinateur :
>   ```
>   Net print \\Production\Dotmatrix 
>   ```
>   Cet exemple montre comment afficher des informations sur le travail numéro 35 sur le \\\Production ordinateur :
>   ```
>   Net print \\Production 35 
>   ```
>   Cet exemple montre comment différer le travail numéro 263 sur le \\\Production ordinateur :
>   ```
>   Net print \\Production 263 /hold 
>   ```
>   Cet exemple montre comment libérer le travail numéro 263 sur le \\\Production ordinateur :
>   ```
>   Net print \\Production 263 /release 
>   ```
>   #### <a name="additional-references"></a>Références supplémentaires
>   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
>   [référence des commandes d’impression](print-command-reference.md)
