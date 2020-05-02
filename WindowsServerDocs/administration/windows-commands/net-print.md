---
title: Net print
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f59b2015-4698-415d-9a74-09566c466f40
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a2fa74da638c23c071e86c19d73cea35c52e8516
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723816"
---
# <a name="net-print"></a>Net print

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche des informations sur une file d’attente d’impression ou un travail d’impression spécifié, ou contrôle un travail d’impression spécifié.
> [!NOTE]
> Cette commande est dépréciée dans Windows 7 et Windows Server 2008 R2. Toutefois, vous pouvez effectuer la plupart des tâches en utilisant prnjobs, Windows Management Instrumentation (WMI) ou des applets de commande Windows PowerShell. Pour plus d’informations, [consultez prnjobs](prnjobs.md), [Windows Management Instrumentation](https://go.microsoft.com/fwlink/?LinkID=29991) https://go.microsoft.com/fwlink/?LinkID=29991)(, [Windows PowerShell](https://go.microsoft.com/fwlink/?LinkID=128426) (https://go.microsoft.com/fwlink/?LinkID=128426)) et la Galerie du centre dehttps://go.microsoft.com/fwlink/?LinkId=164635) [scripts TechNet](https://go.microsoft.com/fwlink/?LinkId=164635) (.
> ## <a name="syntax"></a>Syntaxe
> ```
> Net print {\\<computerName>\<Sharename> | 
> \\<computerName> <JobNumber> [/hold | /release | /delete]} [help]
> ```
> ### <a name="parameters"></a>Paramètres
> 
> |               Paramètres               |                                                                                                                                                                                                                     Description                                                                                                                                                                                                                      |
> |----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |    \\\\<computerName>\\<Sharename>     |                                                                                                                                                                            Spécifie (par nom) l’ordinateur et la file d’attente à l’impression sur lesquels vous souhaitez afficher des informations.                                                                                                                                                                             |
> |           \\\\<computerName>           |                                                                                                                                 Spécifie (par nom) l’ordinateur qui héberge le travail d’impression que vous souhaitez contrôler. Si vous ne spécifiez pas d’ordinateur, l’ordinateur local est supposé. Requiert le <JobNumber> paramètre.                                                                                                                                  |
> |              <JobNumber>               |                                             Spécifie le numéro du travail d’impression que vous souhaitez contrôler. Ce nombre est attribué par l’ordinateur qui héberge la file d’attente à l’impression dans laquelle le travail d’impression est envoyé. Une fois qu’un ordinateur a attribué un nombre à un travail d’impression, ce numéro n’est affecté à aucun autre travail d’impression dans une file d’attente hébergée par cet ordinateur. Obligatoire lors de l' \\ \\ <computerName> utilisation du paramètre.                                             |
> | [/Hold &#124;/Release &#124;/delete] | Spécifie l’action à effectuer avec le travail d’impression.<p>-Le paramètre **/Hold** retarde le travail, ce qui permet à d’autres travaux d’impression de le contourner jusqu’à ce qu’il soit libéré.<br />-Le paramètre **/Release** libère un travail d’impression qui a été retardé.<br />-Le paramètre **/Delete** supprime un travail d’impression d’une file d’attente à l’impression.<p>Si vous spécifiez un numéro de travail, mais que vous ne spécifiez aucune action, les informations sur le travail d’impression s’affichent. |
> |                  help                  |                                                                                                                                                                                                     Affiche l’aide de la commande **net print** .                                                                                                                                                                                                     |
> 
> ## <a name="remarks"></a>Notes 
> - **Net** \\ \\ print <computerName> affiche des informations sur les travaux d’impression dans une file d’attente d’impression partagée. Voici un exemple de rapport pour tous les travaux d’impression dans une file d’attente pour une imprimante partagée nommée LASER :
>   ```
>   printers at \\PRODUCTION
>   Name              Job #      Size      Status
>   -----------------------------
>   LASER Queue       3 jobs               *printer active*
>      USER1          84        93844      printing
>      USER2          85        12555      Waiting
>      USER3          86        10222      Waiting
>   ```
> - Voici un exemple de rapport pour un travail d’impression :
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
>   ## <a name="examples"></a>Exemples
>   Cet exemple montre comment répertorier le contenu de la file d’attente à l' \\impression DotMatrix sur l’ordinateur \Production :
>   ```
>   Net print \\Production\Dotmatrix 
>   ```
>   Cet exemple montre comment afficher des informations sur le numéro de travail 35 \\sur l’ordinateur \Production :
>   ```
>   Net print \\Production 35 
>   ```
>   Cet exemple montre comment retarder le numéro de travail \\263 sur l’ordinateur \Production :
>   ```
>   Net print \\Production 263 /hold 
>   ```
>   Cet exemple montre comment libérer le numéro de travail 263 sur \\l’ordinateur \Production :
>   ```
>   Net print \\Production 263 /release 
>   ```
>   ## <a name="additional-references"></a>Références supplémentaires
>   - [Référence des](command-line-syntax-key.md)
>   [commandes d’impression](print-command-reference.md) de la clé de syntaxe de ligne de commande
