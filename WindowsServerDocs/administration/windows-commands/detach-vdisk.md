---
title: Détacher vdisk
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 4850f9f17218178f210820dd4c6ca96fd918accc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378689"
---
# <a name="detach-vdisk"></a>Détacher vdisk

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Arrête l’affichage du disque dur virtuel \(\) VHD sélectionné en tant que lecteur de disque dur local sur l’ordinateur hôte. Lorsqu'un VHD est détaché, vous pouvez le copier à d'autres emplacements.  
  
> [!NOTE]  
> Cette commande s’applique uniquement à Windows 7 et Windows Server 2008 R2.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
detach vdisk [noerr]  
```  
  
### <a name="parameters"></a>Paramètres  
  
|Paramètre|Description|  
|-------|--------|  
|noerr|Utilisé uniquement pour les scripts. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.|  
  
## <a name="remarks"></a>Notes  
  
-   Pour que cette opération aboutisse, vous devez sélectionner et détacher un disque dur virtuel. Utilisez la commande **Select vdisk** pour sélectionner un disque dur virtuel et lui déplacer le focus.  
  
## <a name="BKMK_Examples"></a>Illustre  
Pour détacher le disque dur virtuel sélectionné, tapez :  
  
```  
detach vdisk  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  
-   [attacher vdisk](attach-vdisk.md)  
  
-   [Compact vdisk](compact-vdisk.md)  
  
  
  
-   [détailler vdisk](detail-vdisk.md)  
  
-   [développer vdisk](expand-vdisk.md)  
  
-   [Merge vdisk](merge-vdisk.md)  
  
-   [sélectionner vdisk](select-vdisk.md)  
  
-   [list_1](list_1.md)  
  

