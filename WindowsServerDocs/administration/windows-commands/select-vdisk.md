---
title: sélectionner vdisk
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8808872a-3523-4205-a6c6-83fa738ee37a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68f60f8f43a33d498c40daa3b9994887f12037bb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722001"
---
# <a name="select-vdisk"></a>sélectionner vdisk

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

sélectionne le disque dur virtuel de \(disque\) dur virtuel spécifié et lui décale le focus.  
  
> [!NOTE]  
> Cette commande s’applique uniquement à Windows 7 et Windows Server 2008 R2.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
select vdisk file=<full path> [noerr]  
```  
  
### <a name="parameters"></a>Paramètres  
  
|Paramètre|Description|  
|-------|--------|  
|txt\=<full path>|Spécifie le chemin d’accès complet et le nom de fichier d’un fichier VHD existant.|  
|noerr|Utilisé uniquement pour les scripts. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.|  
  
## <a name="examples"></a>Exemples  
Pour déplacer le focus sur le disque dur virtuel nommé test. vhd, tapez :  
  
```  
select vdisk file=c:\test\test.vhd  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  
-   [attach vdisk](attach-vdisk.md)  
  
-   [Compact vdisk](compact-vdisk.md)  
  
  
  
-   [Détacher vdisk](detach-vdisk.md)  
  
-   [détailler vdisk](detail-vdisk.md)  
  
-   [développer vdisk](expand-vdisk.md)  
  
-   [Merge vdisk](merge-vdisk.md)  
  
-   [list_1](list_1.md)  
  

