---
title: break
description: 'La rubrique relative aux commandes Windows pour **break_1** -définit ou efface la vérification étendue Ctrl + C sur les systèmes ms-dos. En cas d’utilisation sans paramètre, **break** affiche le paramètre actuel. '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c89b7357-d69e-4141-826e-73c9ba0fc630
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 73afdac29efbfd9efec88d297cf4185ca1b92d62
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380492"
---
# <a name="break"></a>break



Définit ou efface la vérification étendue CTRL + C sur les systèmes MS-DOS. En cas d’utilisation sans paramètre, **break** affiche le paramètre actuel.

> [!NOTE]
> Cette commande n’est plus utilisée. Elle est uniquement incluse pour assurer la compatibilité avec les fichiers MS-DOS existants, mais elle n'a aucun effet sur la ligne de commande car la fonctionnalité est automatique.

## <a name="syntax"></a>Syntaxe

```
break=[on|off]
```

## <a name="remarks"></a>Notes

Si les extensions de commande sont activées et en cours d’exécution sur la plateforme Windows, l’insertion de la commande **break** dans un fichier de commandes entre dans un point d’arrêt codé en dur en cas de débogage par un débogueur.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)