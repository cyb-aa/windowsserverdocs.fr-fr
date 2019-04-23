---
title: assign
description: Rubrique de commandes de Windows pour **affecter** -attribue un point de montage ou de lettre de lecteur au volume qui a le focus.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0d830f9b1ec894c1bc136a99e7beb5703f840dc4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889290"
---
# <a name="assign"></a>assign

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

assigne un point de montage ou de lettre de lecteur au volume qui a le focus.

## <a name="syntax"></a>Syntaxe
```
assign [{letter=<d> | mount=<path>}] [noerr]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|letter=<d>|La lettre de lecteur que vous souhaitez affecter au volume.|
|mount=<path>|Le chemin du point de montage à affecter au volume.<br /><br />Pour obtenir des instructions concernant l’utilisation de cette commande, consultez [affecter un chemin d’accès du dossier de point de montage à un lecteur](https://go.microsoft.com/fwlink/?LinkId=207059) (https://go.microsoft.com/fwlink/?LinkId=207059).|
|NOERR|Pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart.|
## <a name="remarks"></a>Notes
-   Si aucun point de montage ou de lettre de lecteur n’est spécifié, la lettre de lecteur disponible suivante est affectée. Si le point de montage ou de lettre de lecteur est déjà en cours d’utilisation, une erreur est générée.
-   À l’aide de la commande assign, vous pouvez modifier la lettre de lecteur associée à un lecteur amovible.
-   Impossible d’attribuer des lettres de lecteur aux volumes système, les volumes de démarrage ou les volumes contenant le fichier d’échange. En outre, vous ne peut pas attribuer une lettre de lecteur à une partition de fabricant (OEM) ou de n’importe quelle partition de Table de Partition GUID (gpt) autre qu’une partition de base de données.
-   Un volume doit être sélectionné pour cette opération réussisse. Utilisez le **sélectionnez volume** commande pour sélectionner un volume et déplacer le focus vers elle.
## <a name="BKMK_examples"></a>Exemples
Pour affecter la lettre E sur le volume dans le focus, tapez :
```
assign letter=e
```

