---
title: Réinitialiser
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: afbdab44-199c-4e11-884f-e96804965c21
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0bd9b6735697cbcefdcf68dc3d4a53a6870a7a76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850960"
---
# <a name="reset"></a>Réinitialiser



Rétablit l’état par défaut DiskShadow.exe. **Réinitialiser** est particulièrement utile de séparer des opérations composées DiskShadow comme **créer**, **importer**, **sauvegarde**, ou **restaurer**.

## <a name="syntax"></a>Syntaxe

```
reset
```

## <a name="remarks"></a>Notes

-   Lorsque vous utilisez le **réinitialiser** commande, vous perdez état à partir des commandes telles que **ajouter**, **définir**, **charger**, ou **writer**. **Réinitialiser** également libère les interfaces IVssBackupComponent et perd les clichés instantanés non persistant.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)