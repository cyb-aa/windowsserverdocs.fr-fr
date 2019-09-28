---
title: ajouter
description: 'Rubrique relative aux commandes Windows pour **add_1** : ajoute des volumes à l’ensemble des volumes qui doivent être des clichés instantanés ou ajoute des alias à l’environnement des alias.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47efce7a-86d2-4872-ae31-baa108757afd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d1aaa211938d14a0019d29e64867f4df2475a877
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382785"
---
# <a name="add"></a>ajouter


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

|Sous-commande|Description|
|----------|-----------|
|volume|Ajoute un volume au jeu de clichés instantanés, qui est l’ensemble des volumes pour lesquels des clichés instantanés sont ajoutés. Consultez [Ajouter un volume](add-volume.md) pour connaître la syntaxe et les paramètres.|
|alias|Ajoute le nom et la valeur donnés à l’environnement d’alias. Consultez [Ajouter un alias](add-alias.md) pour la syntaxe et les paramètres.|
|/?|Affiche l’aide sur la ligne de commande.|

## <a name="BKMK_examples"></a>Illustre

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

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)