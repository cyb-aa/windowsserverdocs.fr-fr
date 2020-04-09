---
title: Remove-Namespace
description: Rubrique relative aux commandes Windows pour Remove-Namespace, qui supprime un espace de noms personnalisé.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4eb758b6-8519-4e26-9fe0-2e19bb0e8702
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f8b830a5d99d13ed00a3a19f2cf246ad71d1c5f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830232"
---
# <a name="using-the-remove-namespace-command"></a>Utilisation de la commande Remove-Namespace

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Supprime un espace de noms personnalisé.

## <a name="syntax"></a>Syntaxe
```
wdsutil /remove-Namespace /Namespace:<Namespace name> [/Server:<Server name>] [/force]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/Namespace :<Namespace name>|Spécifie le nom de l’espace de noms. Il ne s’agit pas du nom convivial et il doit être unique.<p>-   le **service de rôle serveur de déploiement**: la syntaxe du nom de l’espace de noms est/Namespace : WDS :<ImageGroup>/<ImageName>/<Index>. Par exemple : **WDS : ImageGroup1/install. wim/1**<br />**service de rôle du serveur de Transport**-   : cette valeur doit correspondre au nom donné à l’espace de noms lorsqu’il a été créé sur le serveur.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Il peut s’agir du nom NetBIOS ou du nom de domaine complet (FQDN). Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
|/Force|supprime immédiatement l’espace de noms et met fin à tous les clients. Notez que, à moins que vous ne spécifiiez **/force**, les clients existants peuvent terminer le transfert, mais les nouveaux clients ne peuvent pas se joindre.|
## <a name="examples"></a><a name=BKMK_examples></a>Illustre
Pour arrêter un espace de noms (les clients actuels peuvent terminer le transfert, mais les nouveaux clients ne peuvent pas se joindre à), tapez :
```
wdsutil /remove-Namespace /Namespace:Custom Auto 1
```
Pour forcer l’arrêt de tous les clients, tapez :
```
wdsutil /remove-Namespace /Server:MyWDSServer /Namespace:Custom Auto 1 /force
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande AllNamespaces](using-the-get-allnamespaces-command.md)
[à l’aide de la commande New-Namespace](using-the-new-namespace-command.md)
sous- [commande : Start-namespace](subcommand-start-namespace.md)
