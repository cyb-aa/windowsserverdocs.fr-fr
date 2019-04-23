---
title: delete volume
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f625933d-0f47-409e-93b2-a3e234049a5d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6d25ed68077f594c765cf5630648ad52528d8fe3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872510"
---
# <a name="delete-volume"></a>delete volume



Supprime le volume sélectionné.

## <a name="syntax"></a>Syntaxe

```
delete volume [noerr]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|NOERR|Pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart.|

## <a name="remarks"></a>Notes

-   Vous ne pouvez pas supprimer le volume système, le volume de démarrage ou un volume qui contient le fichier de pagination ou le vidage sur incident (vidage mémoire) actif.
-   Un volume doit être sélectionné pour cette opération réussisse. Utilisez le **sélectionnez volume** commande pour sélectionner un volume et déplacer le focus vers elle.

## <a name="BKMK_examples"></a>Exemples

Pour supprimer le volume qui a le focus, tapez :
```
delete volume
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

