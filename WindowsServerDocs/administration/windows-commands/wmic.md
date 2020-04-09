---
title: wmic
description: La rubrique commandes Windows pour WMIC, qui affiche des informations WMI dans un interpréteur de commandes interactif.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 76397c72-d06f-4cea-88cf-c7603315a983
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 03ba4ecb4b12b03e010318bf6ca260dec00f28f3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829052"
---
# <a name="wmic"></a>wmic



Affiche des informations WMI dans un interpréteur de commandes interactif.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
wmic </parameter>
```

## <a name="sub-commands"></a>Sous-commandes

Les sous-commandes suivantes sont disponibles en permanence :

|Sous-commande|Description|
|-----------|-----------|
|classe|Échappe du mode d’alias par défaut de WMIC pour accéder directement aux classes dans le schéma WMI.|
|path|Échappe du mode d’alias par défaut de WMIC pour accéder directement aux instances dans le schéma WMI.|
|context|Affiche les valeurs actuelles de tous les commutateurs globaux.|
|[quitter \| quitter]|Quitte l’interface de commande WMIC.|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

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
