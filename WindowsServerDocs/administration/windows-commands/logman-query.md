---
title: logman query
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1116a0f0-5415-4369-a045-12f79f8f66de
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e7b7cc202266a568108c7cbf0eac89260721014a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840662"
---
# <a name="logman-query"></a>logman query

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Propriétés du collecteur de données de requête ou de l’ensemble de collecteurs de données.  

## <a name="syntax"></a>Syntaxe  
```  
logman query [providers|Data Collector Set name] [options]  
```  
### <a name="parameters"></a>Paramètres  

|     Paramètre      |                                 Description                                  |
|--------------------|------------------------------------------------------------------------------|
|         /?         |                       Affiche l’aide contextuelle.                       |
| -s <computer name> |            Exécutez la commande sur l’ordinateur distant spécifié.             |
|  -config <value>   |           Spécifie le fichier de paramètres contenant les options de commande.            |
|    [-n] <name>     |                          Nom de l’objet cible.                          |
|        -ETS        | Envoyer des commandes aux sessions de suivi d’événements directement sans enregistrement ou planification. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre  
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
