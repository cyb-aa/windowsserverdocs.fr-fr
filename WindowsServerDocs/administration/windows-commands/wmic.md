---
title: wmic
description: Rubrique de référence pour WMIC, qui affiche des informations WMI dans un interpréteur de commandes interactif.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 76397c72-d06f-4cea-88cf-c7603315a983
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 252ba6b59c29378dd1f5e437de21a2ec4f5ec5c8
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720652"
---
# <a name="wmic"></a>wmic



Affiche des informations WMI dans un interpréteur de commandes interactif.



## <a name="syntax"></a>Syntaxe

```
wmic </parameter>
```

## <a name="sub-commands"></a>Sous-commandes

Les sous-commandes suivantes sont disponibles en permanence :

|Sous-commande|Description|
|-----------|-----------|
|class|Échappe du mode d’alias par défaut de WMIC pour accéder directement aux classes dans le schéma WMI.|
|path|Échappe du mode d’alias par défaut de WMIC pour accéder directement aux instances dans le schéma WMI.|
|contexte|Affiche les valeurs actuelles de tous les commutateurs globaux.|
|[quitter \| quitter]|Quitte l’interface de commande WMIC.|

## <a name="examples"></a>Exemples

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

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
