---
title: À l’aide de la commande remove-Namespace
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4eb758b6-8519-4e26-9fe0-2e19bb0e8702
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 115c0a90a60e18ee4b89758200773d1dfec2163f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842040"
---
# <a name="using-the-remove-namespace-command"></a>À l’aide de la commande remove-Namespace

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Supprime un espace de noms personnalisé.
## <a name="syntax"></a>Syntaxe
```
wdsutil /remove-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/force]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/ Namespace :<Namespace name>|Spécifie le nom de l’espace de noms. Cela n’est pas le nom convivial, et il doit être unique.<br /><br />-   **Service de rôle de serveur de déploiement**: La syntaxe de nom de l’espace de noms est /Namespace:WDS :<ImageGroup>/<ImageName>/<Index>. Exemple : **WDS:ImageGroup1/install.wim/1**<br />-   **Service de rôle serveur de transport**: Cette valeur doit correspondre au nom donné à l’espace de noms lorsqu’il a été créé sur le serveur.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|[/force]|Supprime l’espace de noms immédiatement et met fin à tous les clients. Notez que, sauf si vous spécifiez **/force**, les clients existants peuvent terminer le transfert, mais les nouveaux clients ne sont pas en mesure de joindre.|
## <a name="BKMK_examples"></a>Exemples
Pour arrêter un espace de noms (clients actuels peuvent terminer le transfert mais nouveaux clients ne sont pas en mesure de joindre), type :
```
wdsutil /remove-Namespace /Namespace:"Custom Auto 1"
```
Pour forcer l’arrêt de tous les clients, tapez :
```
wdsutil /remove-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1" /force
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande get-AllNamespaces](using-the-get-allnamespaces-command.md)
[à l’aide de la commande Nouveau Namespace](using-the-new-namespace-command.md) 
 [ Sous-commande : start-Namespace](subcommand-start-namespace.md)
