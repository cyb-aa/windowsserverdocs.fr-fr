---
title: wmic
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 76397c72-d06f-4cea-88cf-c7603315a983
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e9840bc20ddf6193241fe36055698e2bd3222496
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361877"
---
# <a name="wmic"></a>wmic



Affiche des informations WMI dans un interpréteur de commandes interactif.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
command </parameter>
```

## <a name="sub-commands"></a>Sous-commandes

Les sous-commandes suivantes sont disponibles en permanence :

|Sous-commande|Description|
|-----------|-----------|
|classe|Échappe du mode d’alias par défaut de WMIC pour accéder directement aux classes dans le schéma WMI.|
|path|Échappe du mode d’alias par défaut de WMIC pour accéder directement aux instances dans le schéma WMI.|
|contexte|Affiche les valeurs actuelles de tous les commutateurs globaux.|
|[quitter \| quitter]|Quitte l’interface de commande WMIC.|

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|</parameter>|\<Description concise, commence par un verbe. >|
|</param2>|\<une autre description concise, commence par un verbe. >|


## <a name="BKMK_examples"></a>Illustre

Pour afficher les valeurs actuelles de tous les commutateurs globaux, tapez :
```
wmic context
```
Une sortie similaire à ce qui suit s’affiche :
```
NAMESPACE    : root\cimv2
ROLE         : root\cli
NODE(S)      : BOBENTERPRISE
IMPLEVEL     : IMPERSONATE
[AUTHORITY   : N/A]
AUTHLEVEL    : PKTPRIVACY
LOCALE       : ms_409
PRIVILEGES   : ENABLE
TRACE        : OFF
RECORD       : N/A
INTERACTIVE  : OFF
FAILFAST     : OFF
OUTPUT       : STDOUT
APPEND       : STDOUT
USER         : N/A
AGGREGATE    : ON
```
Pour modifier l’ID de langue utilisé par la ligne de commande en anglais (ID de paramètres régionaux 409), tapez :
```
wmic /locale:ms_409
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)