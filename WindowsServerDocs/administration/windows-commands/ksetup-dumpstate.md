---
title: ksetup:dumpstate
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ef2f7b8-97af-4f42-9542-cff324840637
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8e5e8f20188fc27cc08dfd37c5fdbd811925f476
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863120"
---
# <a name="ksetupdumpstate"></a>ksetup:dumpstate



Affiche l’état actuel des paramètres de domaine pour tous les domaines qui sont définis sur l’ordinateur. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /dumpstate
```

### <a name="parameters"></a>Paramètres

Aucune

## <a name="remarks"></a>Notes

La sortie de cette commande inclut le domaine par défaut (le domaine que l’ordinateur est membre) et tous les domaines qui sont définies sur cet ordinateur. Le texte suivant est inclus pour chaque domaine :
-   Tous les la clé de centres de Distribution (KDC) qui sont associés à ce domaine
-   Tous les le **ensemble domaine** indicateurs pour ce domaine
-   Le mot de passe du contrôleur de domaine Kerberos

Cette commande n’affiche pas le nom de domaine qui est spécifié par la détection de DNS ou par la commande **ksetup /domain**.

Cette commande n’affiche pas le mot de passe d’ordinateur qui est définie à l’aide de la commande **ksetup /setcomputerpassword**.

**Ksetup** produit la même sortie que **ksetup /dumpstate**.

## <a name="BKMK_Examples"></a>Exemples

Rechercher la plupart des configurations de domaine Kerberos sur un ordinateur :
```
ksetup /dumpstate
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Ksetup](ksetup.md)
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)