---
title: volume d’attributs
description: Rubrique de commandes de Windows pour **attributs volume** -affiche, définit ou efface les attributs d’un volume.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 37af55ee2a041fbcf8068e0def72147732d3a687
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846580"
---
# <a name="attributes-volume"></a>volume d’attributs

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche, définit ou efface les attributs d’un volume.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
attributes volume [{set | clear}] [{hidden | readonly | nodefaultdriveletter | shadowcopy}] [noerr]  
```  
  
## <a name="parameters"></a>Paramètres  
  
|Paramètre|Description|  
|-------|--------|  
|jeu|Définit l’attribut spécifié du volume qui a le focus.|  
|clear|Supprime l’attribut spécifié du volume qui a le focus.|  
|en lecture seule|Spécifie que le volume est en lecture\-uniquement.|  
|Masqué|Spécifie que le volume est masqué.|  
|nodefaultdriveletter|Spécifie que le volume ne reçoit pas une lettre de lecteur par défaut.|  
|Shadowcopy|Spécifie que le volume est un volume de clichés instantanés.|  
|NOERR|Pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart.|  
  
## <a name="remarks"></a>Notes  
  
-   Dans l’enregistrement de démarrage principal \(MBR\) disques, le **masqué**, **readonly**, et **nodefaultdriveletter** paramètres s’appliquent à tous les volumes sur le disque.  
  
-   Sur la table de partition GUID base \(gpt\) disques et sur dynamique disques MBR et gpt, le **masqué**, **readonly**, et **nodefaultdriveletter** paramètres s’appliquent uniquement au volume sélectionné.  
  
-   Un volume doit être sélectionné pour le **attributs volume** commande réussisse. Utilisez le **sélectionnez volume** commande pour sélectionner un volume et déplacer le focus vers elle.  
  
## <a name="BKMK_examples"></a>Exemples  
Pour afficher les attributs en cours sur le volume sélectionné, tapez :  
  
```  
attributes volume  
```  
  
Pour définir le volume sélectionné comme masqué et lecture\-uniquement, tapez :  
  
```  
attributes volume set hidden readonly  
```  
  
Pour supprimer les masqué et lecture\-uniquement les attributs sur le volume sélectionné, tapez :  
  
```  
attributes volume clear hidden readonly  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

