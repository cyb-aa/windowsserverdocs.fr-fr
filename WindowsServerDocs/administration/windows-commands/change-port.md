---
title: change port
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 477761c35f08d5ec81adae80ba8f2fe9667786e9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818640"
---
# <a name="change-port"></a>change port

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Répertorie ou modifie les mappages de port COM pour être compatible avec les applications MS-DOS.
Pour obtenir des exemples montrant comment utiliser cette commande, consultez [exemples](#BKMK_examples).
> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour savoir quelles sont les nouveautés dans la version la plus récente, consultez [les nouveautés des Services Bureau à distance dans Windows Server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.
## <a name="syntax"></a>Syntaxe
```
change port [<PortX>=<PortY> | /d <PortX> | /query]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|<PortX>=<PortY>|Mappe COM <*PortX*> à <*PortY*>.|
|/d <PortX>|Supprime le mappage pour COM <*PortX*>.|
|/query|Affiche les mappages de port actuel.|
|/?|Affiche l'aide à l'invite de commandes.|
## <a name="remarks"></a>Notes
-   La plupart des applications MS-DOS prend en charge que COM1 à COM4 des ports série. Le **modifier le port** commande mappe un port série à un autre numéro de port, permettant aux applications qui ne prennent pas en charge COM élevées de ports accéder au port série. remappage fonctionne uniquement pour la session active et n’est pas conservé si vous fermez la session à partir d’une session et se reconnecter.
-   Utilisez **modifier le port** sans aucun paramètre pour afficher les ports COM disponibles et leurs mappages actuels.
## <a name="BKMK_examples"></a>Exemples
-   Pour mapper COM12 à COM1 pour une utilisation par une application basée sur MS-DOS, tapez :
    ```
    change port com12=com1
    ```
-   Pour afficher les mappages de port actuel, tapez :
    ```
    change port /query
    ```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[modifier](change.md)
[Services Bureau à distance &#40;Services Terminal Server&#41; référence des commandes](remote-desktop-services-terminal-services-command-reference.md)
