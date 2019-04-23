---
title: À l’aide de la commande get-AllMulticastTransmissions
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95b8fb79-7a8a-4f0c-88f4-92bc1111c67f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf4c3449a5c3194ec27efc2ee4adaccb54f9f7e8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889150"
---
# <a name="using-the-get-allmulticasttransmissions-command"></a>À l’aide de la commande get-AllMulticastTransmissions

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche des informations sur toutes les transmissions par multidiffusion sur un serveur.
## <a name="syntax"></a>Syntaxe
pour Windows Server 2008 :
```
wdsutil /Get-AllMulticastTransmissions [/Server:<Server name>] [/Show:Clients] [/ExcludedeletePending]
```
pour Windows Server 2008 R2 :
```
wdsutil /Get-AllMulticastTransmissions [/Server:<Server name>] [/Show:{Boot | Install | All}] [/details:Clients]  [/ExcludedeletePending]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Explication|
|-------|--------|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local doit être utilisé.|
|[/Show]|**Windows Server 2008**<br /><br />/Show:clients - affiche des informations sur les ordinateurs clients qui sont connectés aux transmissions par multidiffusion.<br /><br />**Windows Server 2008 R2**<br /><br />Show : {démarrage &#124; installer &#124; tous les}-le type d’image à retourner.                                **Démarrage** retourne uniquement les transmissions de l’image de démarrage.                                  **Installer** retourne installe uniquement les transmissions d’image. **Tous les** renvoie les deux types d’images.|
|||
|/details:clients|Uniquement pris en charge pour Windows Server 2008 R2. Le cas échéant, les clients qui sont connectés à la transmission seront affichera.|
|[/ExcludedeletePending]|Exclut les transmissions désactivées dans la liste.|
## <a name="BKMK_examples"></a>Exemples
Pour afficher des informations sur toutes les transmissions, tapez :
-   Windows Server 2008 : `wdsutil /Get-AllMulticastTransmissions`
-   Windows Server 2008 R2 : `wdsutil /Get-AllMulticastTransmissions /Show:All` Pour afficher des informations sur toutes les transmissions à l’exception des transmissions désactivées, tapez :
-   Windows Server 2008 : `wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:Clients /ExcludedeletePending`
-   Windows Server 2008 R2 : `wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:All /details:Clients /ExcludedeletePending`
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande get-MulticastTransmission](using-the-get-multicasttransmission-command.md)
[à l’aide de la commande Nouveau MulticastTransmission](using-the-new-multicasttransmission-command.md) 
 [à l’aide de la commande remove-MulticastTransmission](using-the-remove-multicasttransmission-command.md)
[sous-commande : start-MulticastTransmission](subcommand-start-multicasttransmission.md)
