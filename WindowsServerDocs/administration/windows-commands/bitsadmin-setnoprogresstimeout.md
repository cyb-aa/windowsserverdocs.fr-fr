---
title: bitsadmin setnoprogresstimeout
description: Rubrique de référence pour la commande Bitsadmin setnoprogresstimeout, qui définit la durée, en secondes, pendant laquelle le service tente de transférer le fichier après qu’une erreur temporaire se produit.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7fac50d9-cc6b-46a4-a96f-fab751ee1756
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 398882cf795e98dc0bbc0fb81006d3406fded707
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720109"
---
# <a name="bitsadmin-setnoprogresstimeout"></a>bitsadmin setnoprogresstimeout

Définit la durée, en secondes, pendant laquelle BITS essaie de transférer le fichier une fois que la première erreur temporaire s’est produite.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /setnoprogresstimeout <job> <timeoutvalue>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| travail | Nom complet ou GUID du travail. |
| timeoutvalue | Durée pendant laquelle le service BITS attend de transférer un fichier après la première erreur, en secondes. |

### <a name="remarks"></a>Notes 

- L’intervalle de délai d’attente « aucune progression » commence lorsque le travail rencontre sa première erreur temporaire.

- L’intervalle de délai d’attente s’arrête ou se réinitialise lorsqu’un octet de données est transféré avec succès.

- Si l’intervalle de délai d’attente « aucune progression » dépasse *TimeOutValue*, le travail est placé dans un état d’erreur irrécupérable.

## <a name="examples"></a>Exemples

Pour définir la valeur du délai d’attente « no Progress » sur 20 secondes, pour le travail nommé *myDownloadJob*:

```
bitsadmin /setnoprogresstimeout myDownloadJob 20
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
