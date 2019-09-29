---
title: Utilisation de la commande AllMulticastTransmissions
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 644684ffb356ef07120bc391e3d3da2daf768eaf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363344"
---
# <a name="using-the-get-allmulticasttransmissions-command"></a>Utilisation de la commande AllMulticastTransmissions

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

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

|        Paramètre        |                                                                                                                                                                                                                                                                   Explication                                                                                                                                                                                                                                                                    |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server:<Server name>] |                                                                                                                                                                                 Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.                                                                                                                                                                                  |
|         /Show         | **Windows Server 2008**<br /><br />/Show : clients-affiche des informations sur les ordinateurs clients qui sont connectés aux transmissions par multidiffusion.<br /><br />**Windows Server 2008 R2**<br /><br />Afficher : {boot &#124; install &#124; All} : type d’image à retourner.                                Le **démarrage** retourne uniquement les transmissions d’image de démarrage.                                  L' **installation** de retourne uniquement les transmissions d’image d’installation. **All retourne les** deux types d’images. |
|                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|    /Details : clients     |                                                                                                                                                                                              Pris en charge uniquement pour Windows Server 2008 R2. Si elle est présente, les clients connectés à la transmission seront affichés.                                                                                                                                                                                               |
| [/ExcludedeletePending] |                                                                                                                                                                                                                                              Exclut toutes les transmissions désactivées de la liste.                                                                                                                                                                                                                                               |

## <a name="BKMK_examples"></a>Illustre
Pour afficher des informations sur toutes les transmissions, tapez :
- Windows Server 2008 : `wdsutil /Get-AllMulticastTransmissions`
- Windows Server 2008 R2 : `wdsutil /Get-AllMulticastTransmissions /Show:All` pour afficher des informations sur toutes les transmissions, à l’exception des transmissions désactivées, tapez :
- Windows Server 2008 : `wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:Clients /ExcludedeletePending`
- Windows Server 2008 R2 : `wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:All /details:Clients /ExcludedeletePending`
  #### <a name="additional-references"></a>Références supplémentaires
  [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  [à l’aide de la commande MulticastTransmission](using-the-get-multicasttransmission-command.md)
  [à l’aide de la commande New-MulticastTransmission](using-the-new-multicasttransmission-command.md)
  [à l’aide de la commande Remove-MulticastTransmission](using-the-remove-multicasttransmission-command.md)
  [ Sous-commande : Start-MulticastTransmission](subcommand-start-multicasttransmission.md)
