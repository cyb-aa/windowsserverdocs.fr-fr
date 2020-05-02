---
title: logman query
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1116a0f0-5415-4369-a045-12f79f8f66de
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 05448a4f129a59145813dd0da7199d4adf845c5c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724355"
---
# <a name="logman-query"></a>logman query

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Propriétés du collecteur de données de requête ou de l’ensemble de collecteurs de données.  

## <a name="syntax"></a>Syntaxe  
```  
logman query [providers|Data Collector Set name] [options]  
```  
### <a name="parameters"></a>Paramètres  

|     Paramètre      |                                 Description                                  |
|--------------------|------------------------------------------------------------------------------|
|         /?         |                       Affiche l’aide contextuelle.                       |
| -s<computer name> |            Exécutez la commande sur l’ordinateur distant spécifié.             |
|  -config <value>   |           Spécifie le fichier de paramètres contenant les options de commande.            |
|    [-n]<name>     |                          Nom de l'objet cible.                          |
|        -ETS        | Envoyer des commandes aux sessions de suivi d’événements directement sans enregistrement ou planification. |

## <a name="examples"></a>Exemples  
La commande suivante répertorie tous les ensembles de collecteurs de données configurés sur le système cible.  
```  
logman query  
```  
La commande suivante répertorie les collecteurs de données contenus dans l’ensemble de collecteurs de données nommé perf_log.  
```  
logman query perf_log  
```  
La commande suivante répertorie tous les fournisseurs de collecteurs de données disponibles sur le système cible.  
```  
logman query providers  
```  
## <a name="additional-references"></a>Références supplémentaires  
[logman](logman.md)  
