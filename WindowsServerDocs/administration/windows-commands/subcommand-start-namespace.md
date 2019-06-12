---
title: Sous-commande start-Namespace
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2dd1c11e-6ab7-4129-9e3a-3f80e0ba59c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9a54f849580a139470c2cca43ba57fee60dc81ec
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441154"
---
# <a name="subcommand-start-namespace"></a>Sous-commande : start-Namespace

> S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 démarre un espace de noms de diffusion planifiée.
> ## <a name="syntax"></a>Syntaxe
> ```
> wdsutil /start-Namespace /Namespace:<Namespace name> [/Server:<Server name>]
> ```
> ## <a name="parameters"></a>Paramètres
> 
> |          Paramètre          |                                                                                                                                                                                             Description                                                                                                                                                                                             |
> |-----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | / Namespace :<Namespace name> | Spécifie le nom de l’espace de noms. Notez que cela n’est pas le nom convivial, et il doit être unique.<br /><br />-   **Serveur de déploiement**: La syntaxe de nom de l’espace de noms est /Namspace:WDS :<Image group>/<Image name>/<Index>. Exemple : **WDS:ImageGroup1/install.wim/1**<br />-   **Serveur de transport**: Ce nom doit correspondre au nom donné à l’espace de noms lorsqu’il a été créé sur le serveur. |
> |   [/Server:<Server name>]   |                                                                                                           Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local doit être utilisé.                                                                                                           |
> 
> ## <a name="BKMK_examples"></a>Exemples
> Pour démarrer un espace de noms, tapez une des opérations suivantes :
> ```
> wdsutil /start-Namespace /Namespace:"Custom Auto 1"
> wdsutil /start-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1"
> ```
> #### <a name="additional-references"></a>Références supplémentaires
> [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
> [à l’aide de la commande get-AllNamespaces](using-the-get-allnamespaces-command.md)
> [à l’aide de la commande Nouveau Namespace](using-the-new-namespace-command.md)
> [Using la commande remove-Namespace](using-the-remove-namespace-command.md)
