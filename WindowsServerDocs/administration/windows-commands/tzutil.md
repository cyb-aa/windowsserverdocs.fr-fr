---
title: tzutil
description: La rubrique commandes Windows pour tzutil, qui affiche l’utilitaire de fuseau horaire Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bcf6e007-c9b6-4df5-83c5-ed7b4b1b5913
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 330ee1eb4318df5aca1a5bb1a456711cd103c467
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832342"
---
# <a name="tzutil"></a>tzutil

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche l’utilitaire de fuseau horaire Windows. 

## <a name="syntax"></a>Syntaxe
```
tzutil [/?] [/g] [/s <timeZoneID>[_dstoff]] [/l]
```
#### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/?|Affiche l'aide à l'invite de commandes.|
|/g|Affiche l’ID de fuseau horaire actuel.|
|/s \<timeZoneID > [_dstoff]|Définit le fuseau horaire actuel à l’aide de l’ID de fuseau horaire spécifié. Le suffixe **_dstoff** désactive les ajustements de l’heure d’été pour le fuseau horaire (le cas échéant).|
|/l|répertorie tous les ID de fuseau horaire et noms d’affichage valides. La sortie est la suivante :<p>-   \<nom complet ><br />-   ID de fuseau horaire \<>|

## <a name="remarks"></a>Notes
Un code de sortie de **0** indique que la commande a été exécutée avec succès.

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre
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
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

