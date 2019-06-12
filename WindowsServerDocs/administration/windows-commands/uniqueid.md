---
title: uniqueid
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 64235a4a-b91c-46da-b9b0-68ee90571c2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d237f4d6d3562e3787efe28ca98f9dc553d74898
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440754"
---
# <a name="uniqueid"></a>uniqueid



Affiche ou définit le GUID partition GPT (table) identificateur ou master boot enregistrement (MBR) de signature pour le disque qui a le focus.

> [!IMPORTANT]
> Cette commande DiskPart n’est pas disponible dans n’importe quelle édition de Windows Vista.

## <a name="syntax"></a>Syntaxe

```
uniqueid disk [id={<dword> | <GUID>}] [noerr]
```

## <a name="parameters"></a>Paramètres

|  Paramètre   |                                                                                             Description                                                                                              |
|--------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id={\<dword> |                                                                                               <GUID>}                                                                                                |
|    NOERR     | Pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart. |

## <a name="remarks"></a>Notes

-   Cette commande fonctionne sur les disques dynamiques et de base.
-   Un disque doit être sélectionné pour cette commande réussisse. Utilisez le **sélectionnez disque** commande pour sélectionner un disque et de déplacer le focus vers elle.

## <a name="BKMK_examples"></a>Exemples

Pour afficher la signature du disque MBR avec le focus, tapez :
```
uniqueid disk
```
Pour définir la signature du disque MBR avec le focus à 5f1b2c36, tapez :
```
uniqueid disk id=5f1b2c36
```
Pour définir l’identificateur du disque GPT avec le focus à e86c96e166ee du aaac 4cfb baf784e7 6bbd, tapez :
```
uniqueid disk id=baf784e7-6bbd-4cfb-aaac-e86c96e166ee
```

#### <a name="additional-references"></a>Références supplémentaires

