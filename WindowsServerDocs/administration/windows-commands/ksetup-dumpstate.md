---
title: 'Ksetup : dumpstate'
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3ef2f7b8-97af-4f42-9542-cff324840637
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 46f827d26d867392db4cbef92cf5be496aee8d74
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841512"
---
# <a name="ksetupdumpstate"></a>Ksetup : dumpstate



Affiche l’état actuel des paramètres de domaine pour tous les domaines définis sur l’ordinateur. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
ksetup /dumpstate
```

#### <a name="parameters"></a>Paramètres

Aucune

## <a name="remarks"></a>Notes

La sortie de cette commande comprend le domaine par défaut (le domaine dont l’ordinateur est membre) et tous les domaines définis sur cet ordinateur. Les éléments suivants sont inclus pour chaque domaine :
-   Tous les centres de distribution de clés (KDC) associés à ce domaine
-   Tous les indicateurs de **domaine définis** pour ce domaine
-   Le mot de passe KDC

Cette commande n’affiche pas le nom de domaine spécifié par la détection DNS ou par la commande **Ksetup/Domain**.

Cette commande n’affiche pas le mot de passe de l’ordinateur défini à l’aide de la commande **Ksetup/setcomputerpassword**.

**Ksetup** produit la même sortie que **Ksetup/dumpstate**.

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

Recherchez la plupart des configurations de domaine Kerberos sur un ordinateur :
```
ksetup /dumpstate
```

## <a name="additional-references"></a>Références supplémentaires

-   [Ksetup](ksetup.md)
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)