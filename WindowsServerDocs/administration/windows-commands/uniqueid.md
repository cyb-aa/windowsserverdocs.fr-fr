---
title: quei
description: Rubrique de référence pour UniqueId, qui affiche ou définit l’identificateur de table de partition GUID (GPT) ou la signature d’enregistrement de démarrage principal (MBR) pour le disque qui a le focus.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 64235a4a-b91c-46da-b9b0-68ee90571c2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: efaafe889f04511ceef7441b0a42b73259aadedf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721182"
---
# <a name="uniqueid"></a>quei

Affiche ou définit l’identificateur de table de partition GUID (GPT) ou la signature d’enregistrement de démarrage principal (MBR) pour le disque qui a le focus.

> [!IMPORTANT]
> Cette commande DiskPart n’est pas disponible dans les éditions de Windows Vista.

## <a name="syntax"></a>Syntaxe

```
uniqueid disk [id={<dword> | <GUID>}] [noerr]
```

### <a name="parameters"></a>Paramètres

|  Paramètre   |                                                                                             Description                                                                                              |
|--------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ID = {\<DWORD> |                                                                                               <GUID>}                                                                                                |
|    noerr     | À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur. |

## <a name="remarks"></a>Notes 

-   Cette commande fonctionne sur des disques de base et dynamiques.
-   Pour que cette commande aboutisse, vous devez sélectionner un disque. Utilisez la commande **Sélectionner le disque** pour sélectionner un disque et lui déplacer le focus.

## <a name="examples"></a>Exemples

Pour afficher la signature du disque MBR avec le focus, tapez :
```
uniqueid disk
```
Pour définir la signature du disque MBR ayant le focus sur 5f1b2c36, tapez :
```
uniqueid disk id=5f1b2c36
```
Pour définir l’identificateur du disque GPT ayant le focus sur baf784e7-6bbd-4cfb-AAAC-e86c96e166ee, tapez :
```
uniqueid disk id=baf784e7-6bbd-4cfb-aaac-e86c96e166ee
```

## <a name="additional-references"></a>Références supplémentaires

