---
title: wmic
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6c68866fbe0c8f5b16dae77e2121331f06cdc726
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885840"
---
# <a name="wmic"></a>wmic



Affiche des informations de WMI à l’intérieur d’une interface de commande interactive.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
command </parameter>
```

## <a name="sub-commands"></a>Sous-commandes

Les sous-commandes suivantes sont disponibles à tout moment :

|Sous-commande|Description|
|-----------|-----------|
|classe|Permet de quitter le mode d’alias par défaut de WMIC pour accéder directement aux classes du schéma WMI.|
|path|Permet de quitter le mode d’alias par défaut de WMIC pour accéder directement aux instances du schéma WMI.|
|Contexte|Affiche les valeurs actuelles de tous les commutateurs globaux.|
|[quitter \| quitter]|Interface de commande se termine la ligne de commande WMI.|

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|</parameter>|\<Description concise, commence par un verbe. >|
|</param2>|\<Une autre description concise, commence par un verbe. >|


## <a name="BKMK_examples"></a>Exemples

Pour afficher les valeurs actuelles de tous les commutateurs globaux, tapez :
```
wmic context
```
Sortie similaire à ce qui suit affiche :
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
Pour changer la langue ID utilisé par la ligne de commande vers l’anglais (paramètres régionaux ID 409), type :
```
wmic /locale:ms_409
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)