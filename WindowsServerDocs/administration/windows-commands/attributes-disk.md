---
title: Disque d’attributs
description: Rubrique de commandes de Windows pour **disque d’attributs** -affiche, définit ou efface les attributs d’un disque.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eed57071-c1c6-4394-9542-62b52a878c92
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7cacc2fb6b47d095f5e452ca470c89f228949594
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890350"
---
# <a name="attributes-disk"></a>Disque d’attributs



Affiche, définit ou efface les attributs d’un disque.

> [!IMPORTANT]
> Ce paramètre n’est pas disponible dans n’importe quelle édition de Windows Vista.

## <a name="syntax"></a>Syntaxe

```
attributes disk [{set | clear}] [readonly] [noerr]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|jeu|Définit l’attribut spécifié du disque qui a le focus.|
|clear|Supprime l’attribut spécifié du disque qui a le focus.|
|en lecture seule|Spécifie que le disque est en lecture seule.|
|NOERR|Pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart.|

## <a name="remarks"></a>Notes

-   Lorsque **disque d’attributs** est utilisé pour afficher les attributs en cours d’un disque, l’attribut de disque de démarrage désigne le disque qui est utilisé pour démarrer l’ordinateur. Pour une mise en miroir dynamique, il est affiché pour le disque qui contient le bre de démarrage du volume de démarrage.
-   Un disque doit être sélectionné pour le **disque d’attributs** commande réussisse. Utilisez le **sélectionnez disque** commande pour sélectionner un disque et de déplacer le focus vers elle.

## <a name="BKMK_examples"></a>Exemples

Pour afficher les attributs du disque sélectionné, tapez :
```
attributes disk
```
Pour définir le disque sélectionné comme étant en lecture seule, tapez :
```
attributes disk set readonly
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

