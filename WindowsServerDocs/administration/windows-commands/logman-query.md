---
title: logman query
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6acf6cf5240dd59357f4c788577190699a354744
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374419"
---
# <a name="logman-query"></a>logman query

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Propriétés du collecteur de données de requête ou de l’ensemble de collecteurs de données.  

## <a name="syntax"></a>Syntaxe  
```  
logman query [providers|"Data Collector Set name"] [options]  
```  
## <a name="parameters"></a>Paramètres  

|     Paramètre      |                                 Description                                  |
|--------------------|------------------------------------------------------------------------------|
|         /?         |                       Affiche l’aide contextuelle.                       |
| -s <computer name> |            Exécutez la commande sur l’ordinateur distant spécifié.             |
|  -config <value>   |           Spécifie le fichier de paramètres contenant les options de commande.            |
|    [-n] <name>     |                          Nom de l’objet cible.                          |
|        -ETS        | Envoyer des commandes aux sessions de suivi d’événements directement sans enregistrement ou planification. |

## <a name="BKMK_examples"></a>Illustre  
La commande suivante répertorie tous les ensembles de collecteurs de données configurés sur le système cible.  
```  
logman query  
```  
La commande suivante répertorie les collecteurs de données contenus dans l’ensemble de collecteurs de données nommé journal_perf.  
```  
logman query "perf_log"  
```  
La commande suivante répertorie tous les fournisseurs de collecteurs de données disponibles sur le système cible.  
```  
logman query providers  
```  
#### <a name="additional-references"></a>Références supplémentaires  
[logman](logman.md)  
