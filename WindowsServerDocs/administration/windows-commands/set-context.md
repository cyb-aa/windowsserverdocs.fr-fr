---
title: Définir le contexte
description: Commande Windows pour set context, qui définit le contexte pour la création de clichés instantanés.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fc16c7dd-e8f0-4c2a-8742-0bddb2848bfd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 162fefc46bc3b8ae39dcb41a387e50ed98fefa70
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834562"
---
# <a name="set-contex"></a>Définir contextuel.

Définit le contexte pour la création de clichés instantanés. En cas d’utilisation sans paramètre, **set context** affiche l’aide à l’invite de commandes.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|clientaccessible|Spécifie que le cliché instantané est utilisable par les versions client de Windows.|
|manière|Spécifie que le cliché instantané est conservé au cours de la sortie, de la réinitialisation ou du redémarrage du programme.|
|volatile|Supprime le cliché instantané à la sortie ou à la réinitialisation.|
|noinscriptibles|Spécifie que tous les enregistreurs sont exclus.|

## <a name="remarks"></a>Notes

-   Le contexte *ClientAccessible* est persistant par défaut.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour empêcher la suppression des clichés instantanés lorsque vous quittez DiskShadow, tapez :
```
set context persistent
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)