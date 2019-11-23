---
title: Utilisation de la commande Remove-Namespace
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: b4c087442c43fd885fe4554cb29f9b2788420e05
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362788"
---
# <a name="using-the-remove-namespace-command"></a>Utilisation de la commande Remove-Namespace

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

supprime un espace de noms personnalisé.
## <a name="syntax"></a>Syntaxe
```
wdsutil /remove-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/force]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/Namespace :<Namespace name>|Spécifie le nom de l’espace de noms. Il ne s’agit pas du nom convivial et il doit être unique.<br /><br />-   le **service de rôle serveur de déploiement**: la syntaxe du nom de l’espace de noms est/Namespace : WDS :<ImageGroup>/<ImageName>/<Index>. Par exemple : **WDS : ImageGroup1/install. wim/1**<br />**service de rôle du serveur de Transport**-   : cette valeur doit correspondre au nom donné à l’espace de noms lorsqu’il a été créé sur le serveur.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/Force|supprime immédiatement l’espace de noms et met fin à tous les clients. Notez que, à moins que vous ne spécifiiez **/force**, les clients existants peuvent terminer le transfert, mais les nouveaux clients ne peuvent pas se joindre.|
## <a name="BKMK_examples"></a>Illustre
Pour arrêter un espace de noms (les clients actuels peuvent terminer le transfert, mais les nouveaux clients ne peuvent pas se joindre à), tapez :
```
wdsutil /remove-Namespace /Namespace:"Custom Auto 1"
```
Pour forcer l’arrêt de tous les clients, tapez :
```
wdsutil /remove-Namespace /Server:MyWDSServer /Namespace:"Custom Auto 1" /force
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande AllNamespaces](using-the-get-allnamespaces-command.md)
[à l’aide de la commande New-Namespace](using-the-new-namespace-command.md)
sous- [commande : Start-namespace](subcommand-start-namespace.md)
