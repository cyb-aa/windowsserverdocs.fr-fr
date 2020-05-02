---
title: add
description: Rubrique de référence pour la commande Add, qui ajoute des volumes à l’ensemble des volumes qui doivent être des clichés instantanés ou ajoute des alias à l’environnement alias.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 47efce7a-86d2-4872-ae31-baa108757afd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9b621a3061c4e3366085c5cc44f91f26dd33d4e3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719006"
---
# <a name="add"></a>add

Ajoute des volumes à l’ensemble des volumes qui doivent être des clichés instantanés ou ajoute des alias à l’environnement d’alias. S’il est utilisé sans sous-commandes, **Ajouter** répertorie les volumes et les alias actuels.

> [!NOTE]
> Les alias ne sont pas ajoutés à l’environnement d’alias tant que le cliché instantané n’a pas été créé. Vous devez ajouter immédiatement les alias dont vous avez besoin à l’aide de l' **Ajout d’alias**.

## <a name="syntax"></a>Syntaxe

```
add
add volume <volume> [provider <providerid>]
add alias <aliasname> <aliasvalue>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| ---------- | ----------- |
| volume | Ajoute un volume au jeu de clichés instantanés, qui est l’ensemble des volumes pour lesquels des clichés instantanés sont ajoutés. Consultez [Ajouter un volume](add-volume.md) pour connaître la syntaxe et les paramètres. |
| alias | Ajoute le nom et la valeur donnés à l’environnement d’alias. Consultez [Ajouter un alias](add-alias.md) pour la syntaxe et les paramètres. |
| /? | Affiche l’aide sur la ligne de commande. |

## <a name="examples"></a>Exemples

Pour afficher les volumes ajoutés et les alias qui se trouvent actuellement dans l’environnement, tapez :

```
add
```

La sortie suivante montre que le lecteur C a été ajouté au jeu de clichés instantanés :

```
Volume c: alias System1    GUID \\?\Volume{XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX}\
1 volume in Shadow Copy Set.
No Diskshadow aliases in the environment.
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)