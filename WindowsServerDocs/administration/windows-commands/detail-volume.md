---
title: volume de détail
description: Rubrique commandes Windows pour volume de détail, qui affiche les disques sur lesquels le volume actuel réside.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 38f2bc75-2ed6-4e80-aa74-ab83133db1cd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f0441beba769066c593e77b55b9266918e5f778
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846317"
---
# <a name="detail-volume"></a>volume de détail

Affiche les disques sur lesquels le volume actuel réside.

## <a name="syntax"></a>Syntaxe

```
detail volume
```

## <a name="remarks"></a>Notes

-   Vous devez sélectionner un volume pour que cette opération aboutisse. Utilisez la commande **Sélectionner un volume** pour sélectionner un volume et lui déplacer le focus.
-   Les détails du volume ne s’appliquent pas aux volumes en lecture seule, tels qu’un lecteur de CD-ROM ou de DVD-ROM.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour afficher tous les disques dans lesquels le volume actuel réside, tapez :
```
detail volume
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

