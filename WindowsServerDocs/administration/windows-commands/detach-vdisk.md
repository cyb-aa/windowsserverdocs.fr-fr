---
title: Détacher vdisk
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5f01dcb8-9237-4564-ad94-8a8dd0fd0cca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0b6a1ecd3d787506c89f120bed204cc30e6d68d9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822730"
---
# <a name="detach-vdisk"></a>Détacher vdisk

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Arrête le disque dur virtuel sélectionné \(VHD\) d’apparaître comme un lecteur de disque dur local sur l’ordinateur hôte. Lorsqu'un VHD est détaché, vous pouvez le copier à d'autres emplacements.  
  
> [!NOTE]  
> Cette commande est uniquement applicable à Windows 7 et Windows Server 2008 R2.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
detach vdisk [noerr]  
```  
  
### <a name="parameters"></a>Paramètres  
  
|Paramètre|Description|  
|-------|--------|  
|NOERR|Utilisé pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart.|  
  
## <a name="remarks"></a>Notes  
  
-   Un disque dur virtuel doit être sélectionné et détaché pour cette opération réussisse. Utilisez le **sélectionnez vdisk** commande pour sélectionner un disque dur virtuel et de déplacer le focus vers elle.  
  
## <a name="BKMK_Examples"></a>Exemples  
Pour détacher le disque dur virtuel sélectionné, tapez :  
  
```  
detach vdisk  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
  
-   [attach vdisk](attach-vdisk.md)  
  
-   [compact vdisk](compact-vdisk.md)  
  
  
  
-   [detail vdisk](detail-vdisk.md)  
  
-   [Développez vdisk](expand-vdisk.md)  
  
-   [Fusion vdisk](merge-vdisk.md)  
  
-   [select vdisk](select-vdisk.md)  
  
-   [list_1](list_1.md)  
  

