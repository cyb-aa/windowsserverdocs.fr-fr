---
title: sxstrace
description: Découvrez comment diagnostiquer les problèmes côte à côte.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fcd26eeb-fbd9-4a86-b6a9-dfa5e9c6e4fc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dbc8dad642e15dede1ce89105a501fd90224610b
ms.sourcegitcommit: feec5cbe983c8c5800ccd4fc214914084fcceaba
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70975309"
---
# <a name="sxstrace"></a>sxstrace

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Diagnostique les problèmes côte à côte.    

## <a name="syntax"></a>Syntaxe  
```  
sxstrace [{[trace -logfile:<FileName> [-nostop]|[parse -logfile:<FileName> -outfile:<ParsedFile>  [-filter:<AppName>]}]  
```  

### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|trace|Active le suivi pour SxS (côte à côte)|  
|-logfile|Spécifie le fichier journal brut.|  
|\<Nom de fichier >|Enregistre le journal de suivi dans le *nom de fichier*.|  
|-nostop|Ne spécifie aucune invite pour arrêter le suivi.|  
|Analys|Traduit le fichier de trace brut.|  
|-fichier de journal|Spécifie le nom du fichier de sortie.|  
|\<ParsedFile >|Spécifie le nom de fichier du fichier analysé.|  
|-filtre|Autorise le filtrage de la sortie.|  
|\<> AppName|Spécifie le nom de l’application.|  
|stoptrace|Arrêtez la trace si elle n’est pas arrêtée avant.|  
|-?|Affiche l'aide à l'invite de commandes.|  

## <a name="BKMK_Examples"></a>Illustre  
Activez le suivi et enregistrez le fichier de trace dans **sxstrace. etl**:  
```  
sxstrace trace -logfile:sxstrace.etl  
```  
Traduisez le fichier de trace brut dans un format lisible par l’utilisateur et enregistrez le résultat dans **sxstrace. txt**:  
```  
sxstrace parse -logfile:sxstrace.etl -outfile:sxstrace.txt  
```  

## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  
