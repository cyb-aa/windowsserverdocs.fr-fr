---
title: bitsadmin reset
description: Rubrique de référence pour la commande de réinitialisation Bitsadmin, qui annule toutes les tâches de la file d’attente de transfert détenues par l’utilisateur actuel.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e4f9d1d-072c-493f-8d7a-f6d713c3ef29
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a6aea1d3cb0a89def1e23f42272bf0503022ac54
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717007"
---
# <a name="bitsadmin-reset"></a>bitsadmin reset

Annule toutes les tâches de la file d’attente de transfert détenues par l’utilisateur actuel. Vous ne pouvez pas réinitialiser les travaux créés par le système local. Au lieu de cela, vous devez être administrateur et utiliser le planificateur de tâches pour planifier cette commande en tant que tâche à l’aide des informations d’identification du système local.

> [!NOTE]
> Si vous disposez de privilèges d’administrateur dans BITSAdmin 1,5 et versions antérieures, le commutateur/Reset annule tous les travaux de la file d’attente. En outre, l’option/ALLUSERS n’est pas prise en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /reset [/allusers]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| /ALLUSERS | facultatif. Annule toutes les tâches de la file d’attente détenues par l’utilisateur actuel. Vous devez disposer de privilèges d’administrateur pour utiliser ce paramètre. |

## <a name="examples"></a>Exemples

Pour annuler toutes les tâches de la file d’attente de transfert pour l’utilisateur actuel.

```
bitsadmin /reset
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
