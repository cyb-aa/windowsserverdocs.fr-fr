---
title: ajouter
description: 'Rubrique de commandes de Windows pour **add_1** : ajoute des volumes à l’ensemble de volumes qui doivent être des clichés instantanés, ou ajoute des alias à l’environnement de l’alias.'
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 549c8560774f004a60926ce568c850fd1b71c7f9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889950"
---
# <a name="add"></a>ajouter


Ajoute des volumes à l’ensemble de volumes qui doivent être des clichés instantanés, ou ajoute des alias à l’environnement de l’alias. Si utilisée sans les sous-commandes, **ajouter** répertorie les volumes actuels et les alias.

> [!NOTE]
> Alias ne sont pas ajoutés à l’environnement d’alias jusqu'à ce que le cliché instantané est créé. Les alias que vous devez immédiatement doivent être ajoutés à l’aide de **ajouter l’alias**.

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
|volume|Ajoute un volume pour les clichés instantanés copie définir, qui est l’ensemble de volumes de clichés instantanés. Consultez [ajouter un volume](add-volume.md) pour la syntaxe et les paramètres.|
|alias|Ajoute le nom donné et la valeur à l’environnement de l’alias. Consultez [ajouter l’alias](add-alias.md) pour la syntaxe et les paramètres.|
|/?|Affiche l’aide en ligne de commande.|

## <a name="BKMK_examples"></a>Exemples

Pour afficher les volumes ajoutés et les alias qui se trouvent actuellement dans l’environnement, tapez :
```
add
```
La sortie suivante montre que le lecteur C a été ajouté à l’ensemble de la copie de clichés instantanés :
```
Volume c: alias System1    GUID \\?\Volume{XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX}\
1 volume in Shadow Copy Set.
No Diskshadow aliases in the environment.
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)