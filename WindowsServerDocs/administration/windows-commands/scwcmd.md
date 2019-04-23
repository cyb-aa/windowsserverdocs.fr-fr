---
title: Scwcmd
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 188ae881-c7d4-4a7a-b967-8fdc79f5f345
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 19d631f97c194a78819491f32955e391d3be5a70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883880"
---
# <a name="scwcmd"></a>Scwcmd

> S'applique à : Windows Server 2012 R2, Windows Server 2012

L’outil de ligne de commande Scwcmd.exe inclus avec l’Assistant de Configuration de sécurité (SCW) peut être utilisé pour effectuer les tâches suivantes :
-   Configurer un ou plusieurs serveurs avec une stratégie SCW généré.
-   Analyser un ou plusieurs serveurs avec une stratégie SCW généré.
-   Afficher les résultats de l’analyse au format HTML.
-   Restaurer des stratégies SCW.
-   Transformez une stratégie SCW généré des fichiers natifs sont pris en charge par la stratégie de groupe.
-   Enregistrer une extension de la base de données de Configuration de sécurité avec l’Assistant.

Lorsque vous utilisez **scwcmd** pour configurer, analyser ou restaurer une stratégie sur un serveur distant, l’Assistant doit être installée sur le serveur distant.

## <a name="syntax"></a>Syntaxe

```
scwcmd <command> [<subcommand>]
```

## <a name="parameters"></a>Paramètres

|Sous-commande|Description|
|----------|-----------|
|/ analyze|Détermine si un ordinateur est conforme à une stratégie.</br>Consultez [Scwcmd : analyser](scwcmd-analyze.md) pour la syntaxe et les options.|
|/configure|Applique une stratégie de sécurité générés par le SCW sur un ordinateur.</br>Consultez [Scwcmd : configurer](scwcmd-configure.md) pour la syntaxe et les options.|
|/register|Étend ou personnalise la base de données de la Configuration de la sécurité SCW en enregistrant un fichier de base de données de Configuration de sécurité qui contient le rôle, tâches, service ou les définitions de port.</br>Consultez [Scwcmd : inscrire](scwcmd-register.md) pour la syntaxe et les options.|
|/rollback|Applique la stratégie de restauration le plus récent et supprime cette stratégie de restauration.</br>Consultez [Scwcmd : restauration](scwcmd-rollback.md) pour la syntaxe et les options.|
|/transform|Transforme un fichier de stratégie de sécurité généré à l’aide de l’Assistant dans un nouvel objet de stratégie de groupe (GPO) dans les Services de domaine Active Directory.</br>Consultez [Scwcmd : transformer](scwcmd-transform.md) syntaxe et les options.|
|/View|Restitue un fichier .xml à l’aide d’une transformation XSL spécifié.</br>Consultez [Scwcmd : vue](scwcmd-view.md) pour la syntaxe et les options.|
|/?|Affiche l'aide à l'invite de commandes.|

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
