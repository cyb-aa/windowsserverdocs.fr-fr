---
title: résolution
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9f84f661-f3cd-48c8-bf08-87819cf626fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 46b98938394c10e31d4999ff0e060e10f7da9bdc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835932"
---
# <a name="repair"></a>résolution

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

répare le volume RAID\-5 actif en remplaçant la région du disque défaillant par le disque dynamique spécifié.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
repair disk=<n> [align=<n>] [noerr]  
```  
  
### <a name="parameters"></a>Paramètres  
  
| Paramètre  |                                                                                             Description                                                                                              |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| disque\=<n>  |                                                                 Spécifie le disque dynamique qui remplacera la région du disque défaillant.                                                                 |
| aligner\=<n> |          Aligne toutes les étendues de volume ou de partition sur la limite d’alignement la plus proche. *n* est le nombre de kilo-octets \(Ko\) à partir du début du disque jusqu’à la limite d’alignement la plus proche.           |
|   noerr    | À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur. |
  
## <a name="remarks"></a>Notes  
  
-   Le disque dynamique spécifié doit avoir un espace libre supérieur ou égal à la taille totale de la région du disque défaillant dans le volume RAID\-5.  
  
-   Vous devez sélectionner un volume dans un groupe RAID\-5 pour que cette opération aboutisse. Utilisez la commande **Sélectionner un volume** pour sélectionner un volume et lui déplacer le focus.  
  
## <a name="examples"></a><a name=BKMK_examples></a>Illustre  
Pour remplacer le volume par le focus en le remplaçant par le disque dynamique 4, tapez :  
  
```  
repair disk=4  
```  
  
## <a name="additional-references"></a>Références supplémentaires  
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

