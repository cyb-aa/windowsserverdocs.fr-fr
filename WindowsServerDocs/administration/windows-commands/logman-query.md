---
title: logman query
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1116a0f0-5415-4369-a045-12f79f8f66de
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 79e23e15d85e7ab50d651baf7a556cc76c64fc26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876660"
---
# <a name="logman-query"></a>logman query

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

collecteur de données de requête ou le collecteur de données de définie des propriétés.  
  
## <a name="syntax"></a>Syntaxe  
```  
logman query [providers|"Data Collector Set name"] [options]  
```  
## <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|/?|Affiche l’aide contextuelle.|  
|-s <computer name>|Exécuter la commande sur l’ordinateur distant spécifié.|  
|-config <value>|Spécifie le fichier de paramètres contenant les options de commande.|  
|[-n] <name>|Nom de l’objet cible.|  
|-ets|Envoyer des commandes aux Sessions de suivi d’événements directement sans enregistrement ni planification.|  
## <a name="BKMK_examples"></a>Exemples  
La commande suivante répertorie tous les ensembles de collecteurs de données configuré sur le système cible.  
```  
logman query  
```  
La commande suivante répertorie les collecteurs de données contenus dans l’ensemble de collecteurs de données nommé journal_perf.  
```  
logman query "perf_log"  
```  
La commande suivante répertorie tous les fournisseurs disponibles de collecteurs de données sur le système cible.  
```  
logman query providers  
```  
#### <a name="additional-references"></a>Références supplémentaires  
[logman](logman.md)  
