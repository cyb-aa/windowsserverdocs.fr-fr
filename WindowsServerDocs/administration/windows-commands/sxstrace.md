---
title: sxstrace
description: Découvrez comment diagnostiquer les problèmes côte à côte.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fcd26eeb-fbd9-4a86-b6a9-dfa5e9c6e4fc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 212e2d45b77f09b9460555733de15488a4420842
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721600"
---
# <a name="sxstrace"></a>sxstrace

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Diagnostique les problèmes côte à côte.    

## <a name="syntax"></a>Syntaxe  
```  
sxstrace [{[trace -logfile:<FileName> [-nostop]|[parse -logfile:<FileName> -outfile:<ParsedFile>  [-filter:<AppName>]}]  
```  

#### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|trace|Active le suivi pour SxS (côte à côte)|  
|-logfile|Spécifie le fichier journal brut.|  
|\<Nom de fichier>|Enregistre le journal de suivi dans le *nom de fichier*.|  
|-nostop|Ne spécifie aucune invite pour arrêter le suivi.|  
|parse|Traduit le fichier de trace brut.|  
|-fichier de journal|Spécifie le nom du fichier de sortie.|  
|\<ParsedFile>|Spécifie le nom de fichier du fichier analysé.|  
|-filter|Autorise le filtrage de la sortie.|  
|\<> AppName|Spécifie le nom de l'application.|  
|stoptrace|Arrêtez la trace si elle n’est pas arrêtée avant.|  
|-?|Affiche l'aide à l'invite de commandes.|  

## <a name="examples"></a>Exemples  
Activez le suivi et enregistrez le fichier de trace dans **sxstrace. etl**:  
```  
sxstrace trace -logfile:sxstrace.etl  
```  
Traduisez le fichier de trace brut dans un format lisible par l’utilisateur et enregistrez le résultat dans **sxstrace. txt**:  
```  
sxstrace parse -logfile:sxstrace.etl -outfile:sxstrace.txt  
```  

## <a name="additional-references"></a>Références supplémentaires  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  
