---
title: bitsadmin setnoprogresstimeout
description: La rubrique commandes Windows pour **Bitsadmin setnoprogresstimeout** -définit la durée, en secondes, pendant laquelle le service tente de transférer le fichier après qu’une erreur temporaire se produit.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7fac50d9-cc6b-46a4-a96f-fab751ee1756
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 761d0d76a2c70af9d4ad68aa564c1a9816691d0d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380498"
---
# <a name="bitsadmin-setnoprogresstimeout"></a>bitsadmin setnoprogresstimeout

Définit la durée, en secondes, pendant laquelle BITS essaie de transférer le fichier une fois que la première erreur temporaire s’est produite.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetNoProgressTimeout <Job> <TimeOutvalue>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|TimeOutvalue|Nombre représenté en secondes.|

## <a name="remarks"></a>Notes

-   L’intervalle de dépassement du délai d’attente de progression commence lorsque le travail rencontre une erreur temporaire.
-   L’intervalle de délai d’attente s’arrête ou se réinitialise lorsqu’un octet de données est transféré avec succès.
-   Si aucun intervalle de délai d’attente de progression ne dépasse le *TimeOutvalue*, le travail est placé dans un état d’erreur irrécupérable.

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant définit la valeur de délai d’attente de non-progression pour le travail nommé *myDownloadJob* à 20 secondes
```
C:\>bitsadmin /SetNoProgressTimeout myDownloadJob 20
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)