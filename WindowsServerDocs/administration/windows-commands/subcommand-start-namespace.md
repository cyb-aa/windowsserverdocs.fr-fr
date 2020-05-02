---
title: Sous-commande Start-namespace
description: Rubrique de référence pour la sous-commande Start-Namespace, qui démarre un espace de noms de cast planifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2dd1c11e-6ab7-4129-9e3a-3f80e0ba59c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1562fcb6c61533fcc9994e9011bf7d61154c06f7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721658"
---
# <a name="subcommand-start-namespace"></a>Sous-commande : Start-namespace

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Démarre un espace de noms de cast planifié.

## <a name="syntax"></a>Syntaxe
```
wdsutil /start-Namespace /Namespace:<Namespace name[/Server:<Server name>]
```
### <a name="parameters"></a>Paramètres

|          Paramètre          |                                                                                                                                                                                             Description                                                                                                                                                                                             |
|-----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /Namespace : <nom de l’espace de noms| Spécifie le nom de l'espace de noms. Notez qu’il ne s’agit pas du nom convivial et qu’il doit être unique.<p>-   **Serveur de déploiement**: la syntaxe du nom de l’espace de noms<Image group>/<Image name>/<Index>est/Namspace : WDS :. Par exemple : **WDS : ImageGroup1/install. wim/1**<br />-   **Serveur de transport**: ce nom doit correspondre au nom donné à l’espace de noms lorsqu’il a été créé sur le serveur. |
|   [/Server:<Server name>]   |                                                                                                           Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.                                                                                                           |

## <a name="examples"></a>Exemples
Pour démarrer un espace de noms, tapez l’un des éléments suivants :
```
wdsutil /start-Namespace /Namespace:Custom Auto 1
wdsutil /start-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande[à l’aide de](using-the-get-allnamespaces-command.md)
la commande AllNamespaces à l’aide de la commande[New-Namespace](using-the-new-namespace-command.md)
[à l’aide de la commande Remove-Namespace](using-the-remove-namespace-command.md)
