---
title: select disk
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6e8352b45cf3a46f14828c9c6796e4ec73499d5a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858960"
---
# <a name="select-disk"></a>select disk

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sélectionne le disque spécifié et déplace le focus à ce dernier.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
select disk={ <n> | <disk path> | system | next }  
```  
  
> [!NOTE]  
> Le **<disk path>**, **système**, et **suivant** paramètres sont uniquement disponibles dans Windows 7 et Windows Server 2008 R2.  
  
## <a name="parameters"></a>Paramètres  
  
|Paramètre|Description|  
|-------|--------|  
|<n>|Spécifie le numéro du disque à recevoir le focus. Vous pouvez afficher les nombres pour tous les disques sur l’ordinateur en utilisant le **disque de la liste** commande DiskPart. **Remarque :** Lorsque vous configurez des systèmes avec plusieurs disques, n’utilisez pas **sélectionnez disque\=0** pour spécifier le disque du système. L’ordinateur peut réassigner les numéros des disques lorsque vous redémarrez, et avec la même configuration de disque des ordinateurs différents peuvent avoir des numéros de disque différent.|  
|<disk path>|Spécifie l’emplacement du disque à recevoir le focus, par exemple, **PCIROOT\(0\)\#PCI\(0F02\)\#atA\(C00T00L00\)** . Pour afficher le chemin d’accès de l’emplacement d’un disque, sélectionnez-le et puis tapez **disque de détail**.|  
|système|Sur les ordinateurs BIOS, spécifie que le disque 0 reçoit le focus. Sur les ordinateurs EFI, le disque contenant la partition système EFI \(ESP\) qui est utilisé pour le démarrage en cours reçoit le focus. Sur les ordinateurs EFI, la commande échoue s’il en existe aucun ESP, s’il existe plusieurs ESP, ou l’ordinateur est démarré à partir de l’environnement de préinstallation Windows \(Windows PE\).|  
|suivant|Une fois qu’un disque est sélectionné, cette commande effectue une itération sur tous les disques dans la liste des disques. Lorsque vous exécutez cette commande, le disque suivant dans la liste reçoit le focus.|  
  
## <a name="BKMK_examples"></a>Exemples  
Pour déplacer le focus pour le disque 1, tapez :  
  
```  
select disk=1  
```  
  
Pour sélectionner un disque à l’aide de son chemin d’accès de l’emplacement, tapez :  
  
```  
select disk=PCIROOT(0)#PCI(0100)#atA(C00T00L01)  
```  
  
Pour déplacer le focus vers le disque du système, tapez :  
  
```  
select disk=system  
```  
  
Pour déplacer le focus sur le disque suivant sur l’ordinateur, tapez :  
  
```  
select disk=next  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

