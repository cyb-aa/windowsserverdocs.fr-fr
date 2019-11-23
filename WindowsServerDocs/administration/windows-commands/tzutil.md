---
title: tzutil
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 347254bd5a00a8bfb4a80f20d518f1e0e8b593bf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392302"
---
# <a name="tzutil"></a>tzutil

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche l’utilitaire de fuseau horaire Windows. 
## <a name="syntax"></a>Syntaxe
```
tzutil [/?] [/g] [/s <timeZoneID>[_dstoff]] [/l]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/?|Affiche l'aide à l'invite de commandes.|
|/g|Affiche l’ID de fuseau horaire actuel.|
|/s \<timeZoneID > [_dstoff]|Définit le fuseau horaire actuel à l’aide de l’ID de fuseau horaire spécifié. Le suffixe **_dstoff** désactive les ajustements de l’heure d’été pour le fuseau horaire (le cas échéant).|
|/l|répertorie tous les ID de fuseau horaire et noms d’affichage valides. La sortie est la suivante :<br /><br />-   \<nom complet ><br />-   ID de fuseau horaire \<>|

## <a name="remarks"></a>Notes
Un code de sortie de **0** indique que la commande a été exécutée avec succès.

## <a name="BKMK_Examples"></a>Illustre
Pour afficher l’ID de fuseau horaire actuel, tapez :
```
tzutil /g
```
Pour définir le fuseau horaire actuel à heure standard du Pacifique, tapez :
```
tzutil /s Pacific Standard time
```
Pour définir le fuseau horaire actuel à l’heure standard du Pacifique et désactiver les ajustements de l’heure d’été, tapez :
```
tzutil /s Pacific Standard time_dstoff
```
## <a name="additional-references"></a>Références supplémentaires
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

