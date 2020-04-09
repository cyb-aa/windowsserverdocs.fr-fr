---
title: sélectionner vdisk
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8808872a-3523-4205-a6c6-83fa738ee37a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 65e186413bebbf467cd4c2033d274badd1fbea80
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834742"
---
# <a name="select-vdisk"></a>sélectionner vdisk

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

sélectionne le disque dur virtuel spécifié \(\) VHD et décale le focus vers celui-ci.  
  
> [!NOTE]  
> Cette commande s’applique uniquement à Windows 7 et Windows Server 2008 R2.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
select vdisk file=<full path> [noerr]  
```  
  
### <a name="parameters"></a>Paramètres  
  
|Paramètre|Description|  
|-------|--------|  
|<full path> de\=de fichiers|Spécifie le chemin d’accès complet et le nom de fichier d’un fichier VHD existant.|  
|noerr|Utilisé uniquement pour les scripts. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.|  
  
## <a name="examples"></a><a name=BKMK_examples></a>Illustre  
Pour déplacer le focus sur le disque dur virtuel nommé test. vhd, tapez :  
  
```  
select vdisk file=c:\test\test.vhd  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  
-   [attacher vdisk](attach-vdisk.md)  
  
-   [Compact vdisk](compact-vdisk.md)  
  
  
  
-   [Détacher vdisk](detach-vdisk.md)  
  
-   [détailler vdisk](detail-vdisk.md)  
  
-   [développer vdisk](expand-vdisk.md)  
  
-   [Merge vdisk](merge-vdisk.md)  
  
-   [list_1](list_1.md)  
  

