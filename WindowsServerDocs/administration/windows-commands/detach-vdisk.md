---
title: detach vdisk
description: La rubrique commandes Windows pour détacher vdisk, qui empêche le disque dur virtuel sélectionné d’apparaître comme un lecteur de disque dur local sur l’ordinateur hôte.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5f01dcb8-9237-4564-ad94-8a8dd0fd0cca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 14eb66031841624156afb03f492e2afce5bc56f0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846502"
---
# <a name="detach-vdisk"></a>detach vdisk

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Arrête l’affichage du disque dur virtuel sélectionné en tant que lecteur de disque dur local sur l’ordinateur hôte. Lorsqu'un VHD est détaché, vous pouvez le copier à d'autres emplacements.  
  
> [!NOTE]  
> Cette commande s’applique uniquement à Windows 7 et Windows Server 2008 R2.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
detach vdisk [noerr]  
```  
  
#### <a name="parameters"></a>Paramètres  
  
|Paramètre|Description|  
|-------|--------|  
|noerr|Utilisé uniquement pour les scripts. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.|  
  
## <a name="remarks"></a>Notes  
  
-   Pour que cette opération aboutisse, vous devez sélectionner et détacher un disque dur virtuel. Utilisez la commande **Select vdisk** pour sélectionner un disque dur virtuel et lui déplacer le focus.  
  
## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
Pour détacher le disque dur virtuel sélectionné, tapez :  
  
```  
detach vdisk  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  
-   [attacher vdisk](attach-vdisk.md)  
  
-   [Compact vdisk](compact-vdisk.md)  

-   [détailler vdisk](detail-vdisk.md)  
  
-   [développer vdisk](expand-vdisk.md)  
  
-   [Merge vdisk](merge-vdisk.md)  
  
-   [sélectionner vdisk](select-vdisk.md)  
  
-   [list_1](list_1.md)  
  

