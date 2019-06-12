---
title: créer la partition msr
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 04fba033-23cb-4521-bd5d-db96131f2e73
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3fa9ba46418c3ed3b7999a734b4c0df40dce5027
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434175"
---
# <a name="create-partition-msr"></a>créer la partition msr

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée un Reserved Microsoft \(MSR\) partition sur une table de partition GUID \(gpt\) disque.  
  
> [!CAUTION]  
> Soyez très prudent lorsque vous utilisez cette commande. Étant donné que les disques gpt nécessitent une disposition de partition spécifique, la création de partitions de Microsoft Reserved peut rendre le disque illisible.  
  
  
  
## <a name="syntax"></a>Syntaxe  
  
```  
create partition msr [size=<n>] [offset=<n>] [noerr]  
```  
  
## <a name="parameters"></a>Paramètres  
  
|  Paramètre  |                                                                                                                         Description                                                                                                                         |
|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  size\=<n>  |               La taille de la partition en mégaoctets \(Mo\). La partition est au moins aussi longue en octets que le nombre spécifié par <n>. Si aucune taille n’est donnée, la partition se poursuit jusqu'à ce que l’espace libre dans la région actuelle.               |
| offset\=<n> | Spécifie le décalage en kilo-octets \(Ko\), à laquelle la partition est créée. Le décalage arrondit à remplir entièrement le donner la taille de secteur est utilisée. Si aucun décalage n’est fourni, la partition est placée dans la première étendue de disque qui est assez grande pour le contenir. |
|    NOERR    |                            Pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart.                             |
  
## <a name="remarks"></a>Notes  
  
-   Sur les disques gpt sont utilisées pour démarrer le système d’exploitation Windows, l’Interface micrologicielle Extensible \(EFI\) partition système est la première partition sur le disque, suivie de la partition Microsoft Reserved. les disques GPT sont utilisés uniquement pour le stockage de données n’ont pas une partition système EFI, auquel cas la partition Microsoft Reserved est la première partition.  
  
-   Windows ne monte pas Microsoft Reserved partitions. Vous ne pouvez pas stocker des données sur ces derniers et vous ne pouvez pas les supprimer.  
  
-   Une partition Microsoft Reserved est requis sur chaque disque gpt. La taille de cette partition dépend de la taille totale du disque gpt. La taille du disque gpt doit être au moins 32 Mo pour créer une partition Microsoft Reserved.  
  
-   Un disque gpt de base doit être sélectionné pour cette opération réussisse. Utilisez le **sélectionnez disque** commande pour sélectionner un disque gpt de base et de déplacer le focus vers elle.  
  
## <a name="BKMK_examples"></a>Exemples  
Pour créer une partition Microsoft Reserved de 1 000 mégaoctets taille, tapez :  
  
```  
create partition msr size=1000  
```  
  
#### <a name="additional-references"></a>Références supplémentaires  
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  

  

