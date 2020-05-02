---
title: Remove-Namespace
description: Rubrique de référence pour Remove-Namespace, qui supprime un espace de noms personnalisé.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4eb758b6-8519-4e26-9fe0-2e19bb0e8702
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ef329a53a7f212c1810af2e4ced11a2abf76840
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720333"
---
# <a name="using-the-remove-namespace-command"></a>Utilisation de la commande Remove-Namespace

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Supprime un espace de noms personnalisé.

## <a name="syntax"></a>Syntaxe
```
wdsutil /remove-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/force]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|Joint<Namespace name>|Spécifie le nom de l'espace de noms. Il ne s’agit pas du nom convivial et il doit être unique.<p>-   **Service de rôle serveur de déploiement**: la syntaxe du nom de l’espace de<ImageGroup>/<ImageName>/<Index>noms est/Namespace : WDS :. Par exemple : **WDS : ImageGroup1/install. wim/1**<br />-   **Service de rôle du serveur de transport**: cette valeur doit correspondre au nom donné à l’espace de noms lorsqu’il a été créé sur le serveur.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/Force|supprime immédiatement l’espace de noms et met fin à tous les clients. Notez que, à moins que vous ne spécifiiez **/force**, les clients existants peuvent terminer le transfert, mais les nouveaux clients ne peuvent pas se joindre.|
## <a name="examples"></a>Exemples
Pour arrêter un espace de noms (les clients actuels peuvent terminer le transfert, mais les nouveaux clients ne peuvent pas se joindre à), tapez :
```
wdsutil /remove-Namespace /Namespace:Custom Auto 1
```
Pour forcer l’arrêt de tous les clients, tapez :
```
wdsutil /remove-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /force
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé](command-line-syntax-key.md)
de syntaxe de ligne de commande[à l’aide de la commande](using-the-get-allnamespaces-command.md)
AllNamespaces[à l’aide de la sous-commande de commande](using-the-new-namespace-command.md)
New-Namespace[: Start-namespace](subcommand-start-namespace.md)
