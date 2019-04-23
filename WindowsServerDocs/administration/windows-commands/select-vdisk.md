---
title: Sélectionnez vdisk
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8808872a-3523-4205-a6c6-83fa738ee37a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a71a5c15c05a1e969d0720bc8e67e669d553f649
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852570"
---
# <a name="select-vdisk"></a>Sélectionnez vdisk

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sélectionne le disque dur virtuel spécifié \(VHD\) et déplace le focus à ce dernier.  
  
> [!NOTE]  
> Cette commande est uniquement applicable à Windows 7 et Windows Server 2008 R2.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
select vdisk file=<full path> [noerr]  
```  
  
## <a name="parameters"></a>Paramètres  
  
|Paramètre|Description|  
|-------|--------|  
|Fichier\=<full path>|Spécifie le chemin d’accès et le nom complet d’un fichier de disque dur virtuel existant.|  
|NOERR|Utilisé pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart.|  
  
## <a name="BKMK_examples"></a>Exemples  
Pour déplacer le focus vers le disque dur virtuel appelé Test.vhd, type :  
  
```  
select vdisk file="c:\test\test.vhd"  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
  
-   [attach vdisk](attach-vdisk.md)  
  
-   [compact vdisk](compact-vdisk.md)  
  
  
  
-   [Détacher vdisk](detach-vdisk.md)  
  
-   [detail vdisk](detail-vdisk.md)  
  
-   [Développez vdisk](expand-vdisk.md)  
  
-   [Fusion vdisk](merge-vdisk.md)  
  
-   [list_1](list_1.md)  
  

