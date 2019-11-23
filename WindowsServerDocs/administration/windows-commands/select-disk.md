---
title: select disk
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a0da614b-09d9-433b-b4eb-9127f84431cb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6d9078242264b01ee4bc24dc590df24b1e53e548
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371077"
---
# <a name="select-disk"></a>select disk

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

sélectionne le disque spécifié et lui déplace le focus.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
select disk={ <n> | <disk path> | system | next }  
```  
  
> [!NOTE]  
> Les paramètres **<disk path>** , **System**et **Next** sont uniquement disponibles dans Windows 7 et Windows Server 2008 R2.  
  
## <a name="parameters"></a>Paramètres  
  
|  Paramètre  |                                                                                                                                                                                                            Description                                                                                                                                                                                                            |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     <n>     | Spécifie le numéro du disque qui doit recevoir le focus. Vous pouvez afficher les numéros de tous les disques de l’ordinateur à l’aide de la commande **List Disk** dans DiskPart. **Remarque :** Quand vous configurez des systèmes avec plusieurs disques, n’utilisez pas l' **option select disk\=0** pour spécifier le disque système. L’ordinateur peut réassigner les numéros de disque lorsque vous redémarrez, et les différents ordinateurs ayant la même configuration de disque peuvent avoir des numéros de disque différents. |
| <disk path> |                                                                                                                 Spécifie l’emplacement du disque qui doit recevoir le focus, par exemple, **PCIROOT\(0\)\#PCI\(0F02\)\#atA\(C00T00L00\)** . Pour afficher le chemin d’accès de l’emplacement d’un disque, sélectionnez-le, puis tapez **Detail Disk**.                                                                                                                  |
|   système    |                                 Sur les ordinateurs BIOS, spécifie que le disque 0 reçoit le focus. Sur les ordinateurs EFI, le disque contenant la partition système EFI \(\) ESP utilisé pour le démarrage actuel reçoit le focus. Sur les ordinateurs EFI, la commande échoue s’il n’y a pas de ESP, s’il y a plus d’un ESP, ou si l’ordinateur est démarré à partir de environnement de préinstallation Windows (WinPE) \(\)Windows PE.                                  |
|    suivant     |                                                                                                                                     Une fois qu’un disque est sélectionné, cette commande itère sur tous les disques de la liste des disques. Lorsque vous exécutez cette commande, le disque suivant de la liste recevra le focus.                                                                                                                                      |
  
## <a name="BKMK_examples"></a>Illustre  
Pour déplacer le focus sur le disque 1, tapez :  
  
```  
select disk=1  
```  
  
Pour sélectionner un disque à l’aide de son chemin d’accès d’emplacement, tapez :  
  
```  
select disk=PCIROOT(0)#PCI(0100)#atA(C00T00L01)  
```  
  
Pour déplacer le focus sur le disque système, tapez :  
  
```  
select disk=system  
```  
  
Pour déplacer le focus sur le disque suivant sur l’ordinateur, tapez :  
  
```  
select disk=next  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

