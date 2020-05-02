---
title: volume des attributs
description: Rubrique de référence pour la commande attributs volume, qui affiche, définit ou efface les attributs d’un volume.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e40e8284-3d57-4de8-a46c-e4ade34a0d53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bbe1d66584216875daa82a7e250f3d2f525c2280
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719187"
---
# <a name="attributes-volume"></a>volume des attributs

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche, définit ou efface les attributs d’un volume.

## <a name="syntax"></a>Syntaxe  

```
attributes volume [{set | clear}] [{hidden | readonly | nodefaultdriveletter | shadowcopy}] [noerr]  
```  
  
### <a name="parameters"></a>Paramètres  
  
| Paramètre | Description |  
| ------- | -------- |  
| set | Définit l’attribut spécifié du volume avec le focus. |  
| clear | Efface l’attribut spécifié du volume qui a le focus. |  
| readonly | Spécifie que le volume est en lecture seule. |  
| hidden | Spécifie que le volume est masqué. |  
| nodefaultdriveletter | Spécifie que le volume ne reçoit pas de lettre de lecteur par défaut. |  
| cliché | Spécifie que le volume est un volume de clichés instantanés. |  
| noerr | À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur. |  
  
### <a name="remarks"></a>Notes   
  
- Sur les disques d’enregistrement de démarrage principal (MBR) de base, les paramètres **Hidden**, **ReadOnly**et **nodefaultdriveletter** s’appliquent à tous les volumes sur le disque.  
  
- Sur les disques GPT (GUID partition table) de base et sur les disques MBR et GPT dynamiques, les paramètres **Hidden**, **ReadOnly**et **nodefaultdriveletter** s’appliquent uniquement au volume sélectionné.  
  
- Vous devez sélectionner un volume pour que la commande **attributs volume** aboutisse. Utilisez la commande **Sélectionner un volume** pour sélectionner un volume et lui déplacer le focus.  
  
## <a name="examples"></a>Exemples

Pour afficher les attributs actuels sur le volume sélectionné, tapez :  
  
```
attributes volume  
```  
  
Pour définir le volume sélectionné comme étant masqué et en lecture seule, tapez :  
  
```
attributes volume set hidden readonly  
```  
  
Pour supprimer les attributs masqués et en lecture seule sur le volume sélectionné, tapez :  
  
```
attributes volume clear hidden readonly  
```  
  
## <a name="additional-references"></a>Références supplémentaires  

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande SELECT volume](select-volume.md)
