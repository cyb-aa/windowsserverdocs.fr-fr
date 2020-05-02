---
title: AllMulticastTransmissions
description: Rubrique de référence pour la fonction AllMulticastTransmissions, qui affiche des informations sur toutes les transmissions par multidiffusion sur un serveur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 95b8fb79-7a8a-4f0c-88f4-92bc1111c67f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5303618d1021a0c585a2bd6f958f73e145028a09
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720015"
---
# <a name="get-allmulticasttransmissions"></a>AllMulticastTransmissions

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
### <a name="parameters"></a>Paramètres

|        Paramètre        |                                                                                                                                                                                                                                                                   Explication                                                                                                                                                                                                                                                                    |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server:<Server name>] |                                                                                                                                                                                 Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.                                                                                                                                                                                  |
|         /Show         | **Windows Server 2008**<p>/Show : clients-affiche des informations sur les ordinateurs clients qui sont connectés aux transmissions par multidiffusion.<p>**Windows Server 2008 R2**<p>Afficher : {Boot &#124; installer &#124; tout} : type d’image à retourner.                                Le **démarrage** retourne uniquement les transmissions d’image de démarrage.                                  L' **installation** de retourne uniquement les transmissions d’image d’installation. **All retourne les** deux types d’images. |
|                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|    /Details : clients     |                                                                                                                                                                                              Pris en charge uniquement pour Windows Server 2008 R2. Si elle est présente, les clients connectés à la transmission seront affichés.                                                                                                                                                                                               |
| [/ExcludedeletePending] |                                                                                                                                                                                                                                              Exclut toutes les transmissions désactivées de la liste.                                                                                                                                                                                                                                               |

## <a name="examples"></a>Exemples
Pour afficher des informations sur toutes les transmissions, tapez :
- Windows Server 2008 :`wdsutil /Get-AllMulticastTransmissions`
- Windows Server 2008 R2 : `wdsutil /Get-AllMulticastTransmissions /Show:All` pour afficher des informations sur toutes les transmissions, à l’exception des transmissions désactivées, tapez :
- Windows Server 2008 :`wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:Clients /ExcludedeletePending`
- Windows Server 2008 R2 :`wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:All /details:Clients /ExcludedeletePending`
  ## <a name="additional-references"></a>Références supplémentaires
  - [Clé](command-line-syntax-key.md)
  de syntaxe de ligne de commande[à l’aide de la commande](using-the-get-multicasttransmission-command.md)
  MulticastTransmission à l’aide de la
  
  [commande New-MulticastTransmission](using-the-new-multicasttransmission-command.md)à l’aide de la sous-commande de[commande Remove-MulticastTransmission](using-the-remove-multicasttransmission-command.md)[: Start-MulticastTransmission](subcommand-start-multicasttransmission.md)
