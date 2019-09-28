---
title: 'Ksetup : dumpstate'
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 625d05b2fea9ae58681648c64e309aa8b2a201ed
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374993"
---
# <a name="ksetupdumpstate"></a>Ksetup : dumpstate



Affiche l’état actuel des paramètres de domaine pour tous les domaines définis sur l’ordinateur. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /dumpstate
```

### <a name="parameters"></a>Paramètres

Aucune

## <a name="remarks"></a>Notes

La sortie de cette commande comprend le domaine par défaut (le domaine dont l’ordinateur est membre) et tous les domaines définis sur cet ordinateur. Les éléments suivants sont inclus pour chaque domaine :
-   Tous les centres de distribution de clés (KDC) associés à ce domaine
-   Tous les indicateurs de **domaine définis** pour ce domaine
-   Le mot de passe KDC

Cette commande n’affiche pas le nom de domaine spécifié par la détection DNS ou par la commande **Ksetup/Domain**.

Cette commande n’affiche pas le mot de passe de l’ordinateur défini à l’aide de la commande **Ksetup/setcomputerpassword**.

**Ksetup** produit la même sortie que **Ksetup/dumpstate**.

## <a name="BKMK_Examples"></a>Illustre

Recherchez la plupart des configurations de domaine Kerberos sur un ordinateur :
```
ksetup /dumpstate
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Ksetup](ksetup.md)
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)