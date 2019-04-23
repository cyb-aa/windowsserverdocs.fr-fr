---
title: Disque hors connexion
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8fb9b3c3-0b2c-4192-a2e7-f706292653e3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 617371583a3f0cb3d0cb739845208e4216573d9c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834620"
---
# <a name="offline-disk"></a>Disque hors connexion



Utilise le disque en ligne avec le focus à l’état hors connexion.

> [!IMPORTANT]
> Cette commande DiskPart n’est pas disponible dans n’importe quelle édition de Windows Vista.

## <a name="syntax"></a>Syntaxe

```
offline disk [noerr]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|NOERR|Pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart.|

## <a name="remarks"></a>Notes

-   Cette commande fonctionne sur des disques SAN en mode en ligne. Il devient leur mode SAN hors connexion.
-   Si un disque dynamique dans un groupe de disques est mis hors connexion, l’état du disque passe à **manquant** et le groupe affiche un disque qui est hors connexion. Le disque manquant est déplacé vers le groupe non valide. Si le disque dynamique est le dernier disque dans le groupe, l’état du disque passe à **hors connexion**, et le groupe vide sera supprimé.
-   Un disque doit être sélectionné pour le **disque hors connexion** commande réussisse. Utilisez le **sélectionnez disque** commande pour sélectionner un disque et de déplacer le focus vers elle.

## <a name="BKMK_examples"></a>Exemples

Pour mettre le disque avec le focus hors connexion, tapez :
```
offline disk
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

