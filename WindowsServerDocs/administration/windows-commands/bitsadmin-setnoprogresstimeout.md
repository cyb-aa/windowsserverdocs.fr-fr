---
title: bitsadmin setnoprogresstimeout
description: Rubrique de commandes de Windows pour **bitsadmin setnoprogresstimeout** -définit la durée, en secondes, pendant laquelle le service tente de transférer le fichier après une erreur temporaire se produit.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 45dd8a7ddfae877984a98db66c742e0af4d18f0d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873770"
---
# <a name="bitsadmin-setnoprogresstimeout"></a>bitsadmin setnoprogresstimeout

Définit la durée, en secondes, pendant laquelle BITS essaie de transférer le fichier après que la première erreur temporaire se produit.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetNoProgressTimeout <Job> <TimeOutvalue>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|
|TimeOutvalue|Un nombre représenté en secondes.|

## <a name="remarks"></a>Notes

-   L’intervalle de délai d’attente progression commence lorsque le travail rencontre une erreur temporaire.
-   L’intervalle de délai d’expiration s’arrête ou réinitialise quand un octet de données est transféré avec succès.
-   Si aucun intervalle de délai d’attente de progression ne dépasse le *TimeOutvalue*, puis le travail est placé dans un état d’erreur irrécupérable.

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant ne définit l’aucune valeur de délai d’expiration de progression du travail *myDownloadJob* à 20 secondes
```
C:\>bitsadmin /SetNoProgressTimeout myDownloadJob 20
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)