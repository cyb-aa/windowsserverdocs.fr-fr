---
title: recover
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8cc3a73d-9456-41a0-b375-2b4cc37c3992
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4c9b691b2f0cbad101f7caeb63011724dcf7594d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836572"
---
# <a name="recover"></a>recover



Actualise l’état de tous les disques d’un groupe de disques, tente de récupérer les disques d’un groupe de disques non valide et resynchronise les volumes en miroir et les volumes RAID-5 qui contiennent des données obsolètes.

> [!IMPORTANT]
> Cette commande DiskPart n’est pas disponible dans les éditions de Windows Vista.

## <a name="syntax"></a>Syntaxe

```
recover [noerr]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|noerr|À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.|

## <a name="remarks"></a>Notes

-   Cette commande fonctionne sur un groupe de disques.
-   Cette commande s’applique uniquement aux groupes de disques dynamiques. Si cette commande est utilisée sur un groupe avec un disque de base, elle ne retourne pas d’erreur, mais aucune action n’est effectuée.
-   Cette commande fonctionne sur les disques qui ont échoué ou qui échouent. Il fonctionne également sur les volumes dont l’État est échec, échec ou échec de la redondance.
-   Pour que cette commande aboutisse, vous devez sélectionner un disque qui fait partie d’un groupe de disques. Utilisez la commande **Sélectionner le disque** pour sélectionner un disque et lui déplacer le focus.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour récupérer le groupe de disques qui contient le disque avec le focus, tapez :
```
recover
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

