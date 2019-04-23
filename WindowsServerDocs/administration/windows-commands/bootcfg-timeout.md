---
title: bootcfg timeout
description: Rubrique de commandes de Windows pour **délai d’attente bootcfg** -modifie la valeur de délai d’attente de système d’exploitation.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aa858eac-2bb7-4a27-a9bc-3e4a6eb8b2c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 47c68ffaad68ff3e29e8060fdb75adf46165c982
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885520"
---
# <a name="bootcfg-timeout"></a>bootcfg timeout

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifie la valeur de délai d’attente de système d’exploitation.

## <a name="syntax"></a>Syntaxe
```
bootcfg /timeout <timeOutValue> [/s <computer> [/u <Domain\User>/p <Password>]]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/timeout <timeOutValue>|Spécifie la valeur de délai d’expiration dans la section [boot loader]. Le <timeOutValue> est le nombre de secondes dont dispose l’utilisateur pour sélectionner un système d’exploitation à partir de l’écran de démarrage avant que NTLDR ne charge la valeur par défaut. Plage valide pour <timeOutValue> est 0 et 999. Si la valeur est 0, NTLDR démarre immédiatement le système d’exploitation par défaut sans afficher l’écran de démarrage.|
|/s <computer>|Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local.|
|/u < domaine\nom d’utilisateur >|Exécute la commande avec les autorisations de compte de l’utilisateur spécifié par <User> ou < domaine\nom d’utilisateur >. La valeur par défaut est les autorisations de l’utilisateur actuellement connecté sur l’ordinateur exécutant la commande.|
|/p <Password>|Spécifie le <Password> du compte d’utilisateur qui est spécifié dans le **/u** paramètre.|
|/?|Affiche l'aide à l'invite de commandes.|
## <a name="BKMK_examples"></a>Exemples
Les exemples suivants montrent comment vous pouvez utiliser la **bootcfg /timeout** commande :
```
bootcfg /timeout 30
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /timeout 50
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
