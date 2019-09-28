---
title: assign
description: La rubrique commandes Windows pour **Assign** -attribue une lettre de lecteur ou un point de montage au volume avec le focus.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57912b73-622e-489b-b053-a369021ba8e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e07edcd4ac4ddf5eca1e57da17df441043d15f6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382676"
---
# <a name="assign"></a>assign

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

affecte une lettre de lecteur ou un point de montage au volume qui a le focus.

## <a name="syntax"></a>Syntaxe
```
assign [{letter=<d> | mount=<path>}] [noerr]
```
## <a name="parameters"></a>Paramètres

|  Paramètre   |                                                                                                                                 Description                                                                                                                                 |
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  lettre = <d>  |                                                                                                             Lettre de lecteur que vous souhaitez affecter au volume.                                                                                                              |
| Mount = <path> | Chemin d’accès du point de montage que vous souhaitez affecter au volume.<br /><br />pour obtenir des instructions sur l’utilisation de cette commande, consultez [affecter un chemin d’accès au dossier du point de montage à un lecteur](https://go.microsoft.com/fwlink/?LinkId=207059) (<https://go.microsoft.com/fwlink/?LinkId=207059>). |
|    noerr     |                                    À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.                                     |

## <a name="remarks"></a>Notes
- Si aucune lettre de lecteur ou aucun point de montage n’est spécifié, la lettre de lecteur disponible suivante est attribuée. Si la lettre de lecteur ou le point de montage est déjà utilisé, une erreur est générée.
- À l’aide de la commande Assign, vous pouvez modifier la lettre de lecteur associée à un lecteur amovible.
- Vous ne pouvez pas affecter des lettres de lecteur aux volumes système, aux volumes de démarrage ou aux volumes qui contiennent le fichier d’échange. En outre, vous ne pouvez pas attribuer une lettre de lecteur à une partition OEM (Original Equipment Manufacturer) ou à une partition GPT (GUID partition table) autre qu’une partition de données de base.
- Vous devez sélectionner un volume pour que cette opération aboutisse. Utilisez la commande **Sélectionner un volume** pour sélectionner un volume et lui déplacer le focus.
  ## <a name="BKMK_examples"></a>Illustre
  Pour affecter la lettre E au volume sélectionné, tapez :
  ```
  assign letter=e
  ```

