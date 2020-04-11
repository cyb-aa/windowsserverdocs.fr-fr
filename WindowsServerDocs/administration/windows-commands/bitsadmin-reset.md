---
title: bitsadmin reset
description: La rubrique commandes Windows pour **Bitsadmin Reset**, qui annule tous les travaux de la file d’attente de transfert détenues par l’utilisateur actuel.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e4f9d1d-072c-493f-8d7a-f6d713c3ef29
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b73bd3b9c66b24330a0f9444836b9c8bd1730722
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123081"
---
# <a name="bitsadmin-reset"></a>bitsadmin reset

Annule toutes les tâches de la file d’attente de transfert détenues par l’utilisateur actuel. > Vous ne pouvez pas réinitialiser les travaux créés par le système local. Au lieu de cela, vous devez être administrateur et utiliser le planificateur de tâches pour planifier cette commande en tant que tâche à l’aide des informations d’identification du système local.

> [!NOTE]
> Dans BITSAdmin 1,5 et les versions antérieures, si vous disposez de privilèges d’administrateur, le commutateur/Reset annule tous les travaux de la file d’attente. En outre, l’option/ALLUSERS n’est pas prise en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /reset [/allusers]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| /ALLUSERS | Ce paramètre est facultatif. Annule toutes les tâches de la file d’attente détenues par l’utilisateur actuel. Vous devez disposer de privilèges d’administrateur pour utiliser ce paramètre. |

## <a name="examples"></a>Exemples

L’exemple suivant annule tous les travaux de la file d’attente de transfert pour l’utilisateur actuel.

```
C:\>bitsadmin /reset
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)