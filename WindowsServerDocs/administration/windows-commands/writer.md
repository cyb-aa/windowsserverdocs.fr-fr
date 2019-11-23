---
title: écrit
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7cf98295-411d-4705-8573-f898ff45c140
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6c00f6067cd5f6cf741cddbd6d62c5bcbb1f37a9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361857"
---
# <a name="writer"></a>écrit



Vérifie qu’un writer ou un composant est inclus ou exclut un enregistreur ou un composant de la procédure de sauvegarde ou de restauration. En cas d’utilisation sans paramètre, le **writer** affiche l’aide à l’invite de commandes.

## <a name="syntax"></a>Syntaxe

```
writer verify [<Writer> | <Component>]
writer exclude [<Writer> | <Component>]
```

## <a name="parameters"></a>Paramètres

| Paramètre  |                                                                                      Description                                                                                      |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   verify   | Vérifie que l’enregistreur ou le composant spécifié est inclus dans la procédure de sauvegarde ou de restauration. La procédure de sauvegarde ou de restauration échoue si l’enregistreur ou le composant n’est pas inclus. |
|  exclude   |                                                   Exclut le writer ou le composant spécifié de la procédure de sauvegarde ou de restauration.                                                    |
| [\<rédacteur > |                                                                                     <Component>]                                                                                      |

## <a name="BKMK_examples"></a>Illustre

Pour vérifier un enregistreur en spécifiant son GUID (pour cet exemple, 4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f), tapez :
```
writer verify {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
```
Pour exclure un enregistreur portant le nom « System Writer », tapez :
```
writer exclude "System Writer"
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)