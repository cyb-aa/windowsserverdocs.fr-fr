---
title: Sous-commande Start-namespace
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 55fe4a6136fe4f8e886dc62fff746a1e5ff1898f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370748"
---
# <a name="subcommand-start-namespace"></a>Sous-commande : Start-namespace

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 démarre un espace de noms de diffusion planifiée.
> ## <a name="syntax"></a>Syntaxe
> ```
> wdsutil /start-Namespace /Namespace:<Namespace name> [/Server:<Server name>]
> ```
> ## <a name="parameters"></a>Paramètres
> 
> |          Paramètre          |                                                                                                                                                                                             Description                                                                                                                                                                                             |
> |-----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | /Namespace :<Namespace name> | Spécifie le nom de l’espace de noms. Notez qu’il ne s’agit pas du nom convivial et qu’il doit être unique.<br /><br />-   **serveur de déploiement**: la syntaxe du nom de l’espace de noms est/NAMSPACE : WDS :<Image group>/<Image name>/<Index>. Par exemple : **WDS : ImageGroup1/install. wim/1**<br />-   **serveur de transport**: ce nom doit correspondre au nom donné à l’espace de noms lorsqu’il a été créé sur le serveur. |
> |   [/Server:<Server name>]   |                                                                                                           Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.                                                                                                           |
> 
> ## <a name="BKMK_examples"></a>Illustre
> Pour démarrer un espace de noms, tapez l’un des éléments suivants :
> ```
> wdsutil /start-Namespace /Namespace:"Custom Auto 1"
> wdsutil /start-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1"
> ```
> #### <a name="additional-references"></a>Références supplémentaires
> [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
> [à l’aide de la commande AllNamespaces](using-the-get-allnamespaces-command.md)
> [à l’aide de la commande New-Namespace](using-the-new-namespace-command.md)
> [à l’aide de la commande Remove-Namespace](using-the-remove-namespace-command.md)
