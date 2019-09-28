---
title: volume des attributs
description: 'Rubrique relative aux commandes Windows pour les **attributs volume** : affiche, définit ou efface les attributs d’un volume.'
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e40e8284-3d57-4de8-a46c-e4ade34a0d53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 225a10307123763d1a024fcc08fbae536fd0b5df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382582"
---
# <a name="attributes-volume"></a>volume des attributs

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Affiche, définit ou efface les attributs d’un volume.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
attributes volume [{set | clear}] [{hidden | readonly | nodefaultdriveletter | shadowcopy}] [noerr]  
```  
  
## <a name="parameters"></a>Paramètres  
  
|Paramètre|Description|  
|-------|--------|  
|jeu|Définit l’attribut spécifié du volume avec le focus.|  
|clear|Efface l’attribut spécifié du volume qui a le focus.|  
|seulement|Spécifie que le volume est lu @ no__t-0only.|  
|masquer|Spécifie que le volume est masqué.|  
|nodefaultdriveletter|Spécifie que le volume ne reçoit pas de lettre de lecteur par défaut.|  
|cliché|Spécifie que le volume est un volume de clichés instantanés.|  
|noerr|À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.|  
  
## <a name="remarks"></a>Notes  
  
-   Sur un enregistrement de démarrage principal \(MBR @ no__t-1, les paramètres **Hidden**, **ReadOnly**et **nodefaultdriveletter** s’appliquent à tous les volumes sur le disque.  
  
-   Sur la table de partition GUID de base \(gpt @ no__t-1, et sur les disques MBR et GPT dynamiques, les paramètres **Hidden**, **ReadOnly**et **nodefaultdriveletter** s’appliquent uniquement au volume sélectionné.  
  
-   Vous devez sélectionner un volume pour que la commande **attributs volume** aboutisse. Utilisez la commande **Sélectionner un volume** pour sélectionner un volume et lui déplacer le focus.  
  
## <a name="BKMK_examples"></a>Illustre  
Pour afficher les attributs actuels sur le volume sélectionné, tapez :  
  
```  
attributes volume  
```  
  
Pour définir le volume sélectionné comme masqué et lire @ no__t-0only, tapez :  
  
```  
attributes volume set hidden readonly  
```  
  
Pour supprimer les attributs Hidden et Read @ no__t-0only sur le volume sélectionné, tapez :  
  
```  
attributes volume clear hidden readonly  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

