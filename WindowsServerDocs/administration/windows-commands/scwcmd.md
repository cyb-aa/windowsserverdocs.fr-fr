---
title: Scwcmd
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 188ae881-c7d4-4a7a-b967-8fdc79f5f345
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f08a8396219924ac6660828464e035c7744729b1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722098"
---
# <a name="scwcmd"></a>Scwcmd

> S’applique à : Windows Server 2012 R2, Windows Server 2012

L’outil en ligne de commande Scwcmd. exe inclus dans l’Assistant Configuration de la sécurité (SCW) peut être utilisé pour effectuer les tâches suivantes :
-   Configurez un ou plusieurs serveurs avec une stratégie générée par SCW.
-   Analyser un ou plusieurs serveurs avec une stratégie générée par SCW.
-   Affichez les résultats de l’analyse au format HTML.
-   Restaurez les stratégies SCW.
-   Transformez une stratégie générée par SCW en fichiers natifs pris en charge par stratégie de groupe.
-   Inscrire une extension de base de données de configuration de sécurité avec SCW.

Lorsque vous utilisez l’outil **scwcmd** pour configurer, analyser ou restaurer une stratégie sur un serveur distant, l’Assistant Configuration de la configuration doit être installé sur le serveur distant.

## <a name="syntax"></a>Syntaxe

```
scwcmd <command> [<subcommand>]
```

### <a name="parameters"></a>Paramètres

|Sous-commande|Description|
|----------|-----------|
|/analyze|Détermine si un ordinateur est conforme à une stratégie.</br>Consultez [scwcmd : Analyze](scwcmd-analyze.md) pour connaître la syntaxe et les options.|
|/configure|Applique une stratégie de sécurité générée par SCW à un ordinateur.</br>Pour connaître la syntaxe et les options, consultez [scwcmd : configure](scwcmd-configure.md) .|
|/Register|Étend ou personnalise la base de données de configuration de la sécurité du SCW en inscrivant un fichier de base de données de configuration de sécurité qui contient des définitions de rôle, de tâche, de service ou de port.</br>Consultez [scwcmd : Register](scwcmd-register.md) pour connaître la syntaxe et les options.|
|/Rollback|Applique la stratégie de restauration la plus récente disponible, puis supprime cette stratégie de restauration.</br>Consultez [scwcmd : Rollback](scwcmd-rollback.md) pour connaître la syntaxe et les options.|
|option/Transform|Transforme un fichier de stratégie de sécurité généré à l’aide de SCW en un nouvel objet de stratégie de groupe (GPO) dans Active Directory Domain Services.</br>Consultez [scwcmd : Transform](scwcmd-transform.md) , syntaxe et options.|
|/View|Génère le rendu d’un fichier. XML à l’aide d’une transformation. XSL spécifiée.</br>Consultez [scwcmd : View](scwcmd-view.md) pour connaître la syntaxe et les options.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
