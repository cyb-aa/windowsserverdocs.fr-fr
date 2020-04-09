---
title: break
description: La rubrique commandes Windows pour break_1, qui définit ou efface la vérification étendue CTRL + C sur les systèmes MS-DOS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c89b7357-d69e-4141-826e-73c9ba0fc630
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 809a9321b8b4f8b2d201582f767da132076826d4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848362"
---
# <a name="break"></a>break

Définit ou efface la vérification étendue CTRL + C sur les systèmes MS-DOS. En cas d’utilisation sans paramètre, **break** affiche le paramètre actuel.

> [!NOTE]
> Cette commande n’est plus utilisée. Il est fourni uniquement pour préserver la compatibilité avec les fichiers MS-DOS existants, mais il n'a aucun effet sur la ligne de commande, car la fonctionnalité est automatique.

## <a name="syntax"></a>Syntaxe

```
break=[on|off]
```

## <a name="remarks"></a>Notes

Si les extensions de commande sont activées et en cours d’exécution sur la plateforme Windows, l’insertion de la commande **break** dans un fichier de commandes entre dans un point d’arrêt codé en dur en cas de débogage par un débogueur.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)