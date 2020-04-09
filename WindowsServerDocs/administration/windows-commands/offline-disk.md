---
title: disque hors connexion
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8fb9b3c3-0b2c-4192-a2e7-f706292653e3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd7991f1f5967970690c7051612395fb47a764ec
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837982"
---
# <a name="offline-disk"></a>disque hors connexion



Met le disque en ligne avec le focus à l’état hors connexion.

> [!IMPORTANT]
> Cette commande DiskPart n’est pas disponible dans les éditions de Windows Vista.

## <a name="syntax"></a>Syntaxe

```
offline disk [noerr]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|noerr|À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.|

## <a name="remarks"></a>Notes

-   Cette commande fonctionne sur les disques en mode SAN en ligne. Il remplace le mode SAN par offline.
-   Si un disque dynamique d’un groupe de disques est mis hors connexion, l’état du disque devient **manquant** et le groupe affiche un disque hors connexion. Le disque manquant est déplacé vers le groupe non valide. Si le disque dynamique est le dernier disque du groupe, l’état du disque passe à **hors connexion**et le groupe vide est supprimé.
-   Pour que la commande de **disque hors connexion** aboutisse, vous devez sélectionner un disque. Utilisez la commande **Sélectionner le disque** pour sélectionner un disque et lui déplacer le focus.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour mettre le disque avec le focus hors connexion, tapez :
```
offline disk
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

