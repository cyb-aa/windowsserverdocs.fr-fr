---
title: Définir le contexte
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6f24e795f2d7c92d462cf822e70e4830b53827e5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845850"
---
# <a name="set-contex"></a>Contextuel de jeu.



Définit le contexte pour la création de clichés instantanés. Si utilisée sans paramètres, **définir le contexte** affiche l’aide à l’invite de commandes.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|ClientAccessible|Spécifie que le cliché instantané est utilisable par les versions client de Windows.|
|Persistant|Spécifie que le cliché instantané persiste à travers la sortie du programme, de réinitialisation ou de redémarrage.|
|volatile|Supprime le cliché instantané sur quitte ou réinitialise.|
|NoWriters|Spécifie que tous les enregistreurs sont exclus.|

## <a name="remarks"></a>Notes

-   Le *clientaccessible* contexte est persistant par défaut.

## <a name="BKMK_examples"></a>Exemples

Pour éviter que les clichés instantanés en cours de suppression lorsque vous quittez DiskShadow, tapez :
```
set context persistent
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)