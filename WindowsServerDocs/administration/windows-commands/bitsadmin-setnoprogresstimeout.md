---
title: bitsadmin setnoprogresstimeout
description: La rubrique commandes Windows pour **Bitsadmin setnoprogresstimeout**, qui définit la durée, en secondes, pendant laquelle le service tente de transférer le fichier après qu’une erreur temporaire se produit.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7fac50d9-cc6b-46a4-a96f-fab751ee1756
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8adff95b0dbae68634db2e248d4493549c5ac85d
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122878"
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
| le travail | Nom complet ou GUID du travail. |
| timeoutvalue | Durée pendant laquelle le service BITS attend de transférer un fichier après la première erreur, en secondes. |

## <a name="remarks"></a>Notes

- L’intervalle de délai d’attente « aucune progression » commence lorsque le travail rencontre sa première erreur temporaire.

- L’intervalle de délai d’attente s’arrête ou se réinitialise lorsqu’un octet de données est transféré avec succès.

- Si l’intervalle de délai d’attente « aucune progression » dépasse *TimeOutValue*, le travail est placé dans un état d’erreur irrécupérable.

## <a name="examples"></a>Exemples

L’exemple suivant définit la valeur du délai d’attente « no Progress » sur 20 secondes pour la tâche nommée *myDownloadJob*.

```
C:\>bitsadmin /setnoprogresstimeout myDownloadJob 20
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)