---
title: add
description: La rubrique relative aux commandes Windows pour **Add**, qui ajoute des volumes à l’ensemble des volumes qui doivent être des clichés instantanés, ou ajoute des alias à l’environnement des alias.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 47efce7a-86d2-4872-ae31-baa108757afd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9895082cc10223fd08cff6916c20c3af5613e947
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851342"
---
# <a name="add"></a>add

Ajoute des volumes à l’ensemble des volumes qui doivent être des clichés instantanés ou ajoute des alias à l’environnement d’alias. S’il est utilisé sans sous-commandes, **Ajouter** répertorie les volumes et les alias actuels.

> [!NOTE]
> Les alias ne sont pas ajoutés à l’environnement d’alias tant que le cliché instantané n’a pas été créé. Vous devez ajouter immédiatement les alias dont vous avez besoin à l’aide de l' **Ajout d’alias**.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
add 
add volume <Volume> [provider <ProviderID>] 
add alias <AliasName> <AliasValue>
```

## <a name="add-subcommands"></a>Ajouter des sous-commandes

| Sous-commande | Description |
| ---------- | ----------- |
| volume | Ajoute un volume au jeu de clichés instantanés, qui est l’ensemble des volumes pour lesquels des clichés instantanés sont ajoutés. Consultez [Ajouter un volume](add-volume.md) pour connaître la syntaxe et les paramètres. |
| alias | Ajoute le nom et la valeur donnés à l’environnement d’alias. Consultez [Ajouter un alias](add-alias.md) pour la syntaxe et les paramètres. |
| `/?` | Affiche l’aide sur la ligne de commande. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

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