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
ms.openlocfilehash: ed1dcf9bce06af527ffb5b6a79d76d860d78450c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849792"
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

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant annule tous les travaux de la file d’attente de transfert pour l’utilisateur actuel.

```
C:\>bitsadmin /reset
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)