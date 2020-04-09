---
title: bitsadmin setnoprogresstimeout
description: La rubrique commandes Windows pour Bitsadmin setnoprogresstimeout, qui définit la durée, en secondes, pendant laquelle le service tente de transférer le fichier après qu’une erreur temporaire se produit.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7fac50d9-cc6b-46a4-a96f-fab751ee1756
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 544a6c73f29684bc4091ec05fa28016fbc718bb2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849352"
---
# <a name="bitsadmin-setnoprogresstimeout"></a>bitsadmin setnoprogresstimeout

Définit la durée, en secondes, pendant laquelle BITS essaie de transférer le fichier une fois que la première erreur temporaire s’est produite.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetNoProgressTimeout <Job> <TimeOutvalue>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|TimeOutvalue|Nombre représenté en secondes.|

## <a name="remarks"></a>Notes

-   L’intervalle de dépassement du délai d’attente de progression commence lorsque le travail rencontre une erreur temporaire.
-   L’intervalle de délai d’attente s’arrête ou se réinitialise lorsqu’un octet de données est transféré avec succès.
-   Si aucun intervalle de délai d’attente de progression ne dépasse le *TimeOutvalue*, le travail est placé dans un état d’erreur irrécupérable.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant définit la valeur de délai d’attente de non-progression pour le travail nommé *myDownloadJob* à 20 secondes
```
C:\>bitsadmin /SetNoProgressTimeout myDownloadJob 20
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)