---
title: tzutil
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bcf6e007-c9b6-4df5-83c5-ed7b4b1b5913
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 41a46ea7974b67cc557973484428480e7beb5484
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876800"
---
# <a name="tzutil"></a>tzutil

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche le Windows heure de l’utilitaire de Zone. 
## <a name="syntax"></a>Syntaxe
```
tzutil [/?] [/g] [/s <timeZoneID>[_dstoff]] [/l]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/?|Affiche l'aide à l'invite de commandes.|
|/g|Affiche l’ID de fuseau horaire actuel.|
|/s \<timeZoneID>[_dstoff]|Définit le fuseau horaire actuel à l’aide de l’ID de fuseau horaire spécifié. Le **_dstoff** suffixe désactive les réglages de l’heure d’été pour le fuseau horaire (le cas échéant).|
|/l|listes de temps valide tous les ID de zone et les noms complets. La sortie sera :<br /><br />-   \<nom d’affichage ><br />-   \<ID de fuseau horaire >|

## <a name="remarks"></a>Notes
Le code de sortie **0** indique la commande s’est terminée correctement.

## <a name="BKMK_Examples"></a>Exemples
Pour afficher l’ID de fuseau horaire actuel, tapez :
```
tzutil /g
```
Pour définir le fuseau horaire actuel à l’heure du Pacifique, tapez :
```
tzutil /s Pacific Standard time
```
Pour définir le fuseau horaire actuel à l’heure du Pacifique et désactive les réglages de l’heure d’été, tapez :
```
tzutil /s Pacific Standard time_dstoff
```
## <a name="additional-references"></a>Références supplémentaires
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

