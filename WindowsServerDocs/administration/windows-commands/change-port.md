---
title: change port
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3d772c90-e849-4e74-b9ec-b6cae1159336 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0e587572acd1af1cc7dbd2550e1eae5244d0d1dd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379598"
---
# <a name="change-port"></a>change port

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

répertorie ou modifie les mappages de port COM pour qu’ils soient compatibles avec les applications MS-DOS.
Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_examples).
> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour découvrir les nouveautés de la dernière version, consultez les nouveautés [de services Bureau à distance dans Windows server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.
> ## <a name="syntax"></a>Syntaxe
> ```
> change port [<PortX>=<PortY> | /d <PortX> | /query]
> ```
> ## <a name="parameters"></a>Paramètres
> 
> |    Paramètre    |              Description               |
> |-----------------|----------------------------------------|
> | <PortX>=<PortY> |    Mappe COM <*PortX*> à <*Porty*>.    |
> |   /d <PortX>    | supprime le mappage de la < COM*PortX*>. |
> |     Query      |  Affiche les mappages de port actuels.   |
> |       /?        |  Affiche l'aide à l'invite de commandes.  |
> 
> ## <a name="remarks"></a>Notes
> - La plupart des applications MS-DOS prennent uniquement en charge les ports série COM1 à COM4. La commande de **changement de port** mappe un port série à un numéro de port différent, ce qui permet aux applications qui ne prennent pas en charge les ports com à numéros élevés d’accéder au port série. le remappage fonctionne uniquement pour la session active et n’est pas conservé si vous vous déconnectez d’une session et que vous vous reconnectez.
> - Utilisez **modifier le port** sans aucun paramètre pour afficher les ports com disponibles et leurs mappages actuels.
>   ## <a name="BKMK_examples"></a>Illustre
> - Pour mapper COM12 à COM1 pour une utilisation par une application MS-DOS, tapez :
>   ```
>   change port com12=com1
>   ```
> - Pour afficher les mappages de port actuels, tapez :
>   ```
>   change port /query
>   ```
>   #### <a name="additional-references"></a>Références supplémentaires
>   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
>   [modifier](change.md)
>   [ &#40;services Bureau à distance&#41; référence de commande des services Terminal Server](remote-desktop-services-terminal-services-command-reference.md)
