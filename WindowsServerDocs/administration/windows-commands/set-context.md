---
title: Définir le contexte
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fc16c7dd-e8f0-4c2a-8742-0bddb2848bfd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 16f71d831f374f495abf2239cb8e694eee69efdf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370980"
---
# <a name="set-contex"></a>Définir contextuel.



Définit le contexte pour la création de clichés instantanés. En cas d’utilisation sans paramètre, **set context** affiche l’aide à l’invite de commandes.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|clientaccessible|Spécifie que le cliché instantané est utilisable par les versions client de Windows.|
|manière|Spécifie que le cliché instantané est conservé au cours de la sortie, de la réinitialisation ou du redémarrage du programme.|
|volatile|Supprime le cliché instantané à la sortie ou à la réinitialisation.|
|noinscriptibles|Spécifie que tous les enregistreurs sont exclus.|

## <a name="remarks"></a>Notes

-   Le contexte *ClientAccessible* est persistant par défaut.

## <a name="BKMK_examples"></a>Illustre

Pour empêcher la suppression des clichés instantanés lorsque vous quittez DiskShadow, tapez :
```
set context persistent
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)