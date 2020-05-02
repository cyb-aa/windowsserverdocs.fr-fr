---
title: logman start | erreur
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a40006a1-876e-474b-aaf1-f365c730deea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68f570d99d4b3eaa818c9fbdcce76c42d1cb12d4
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724343"
---
# <a name="logman-start--stop"></a>logman start | erreur

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Démarrez un collecteur de données et définissez l’heure de début sur manuel, ou arrêtez un ensemble de collecteurs de données et définissez l’heure de fin sur manuel.  

## <a name="syntax"></a>Syntaxe  
```  
logman start <[-n] <name>> [options]  
logman stop <[-n] <name>> [options]  
```  
### <a name="parameters"></a>Paramètres  

|     Paramètre      |                                 Description                                  |
|--------------------|------------------------------------------------------------------------------|
|         -?         |                       Affiche l’aide contextuelle.                       |
| -s<computer name> |            Exécutez la commande sur l’ordinateur distant spécifié.             |
|  -config <value>   |           Spécifie le fichier de paramètres contenant les options de commande.            |
|    [-n]<name>     |                          Nom de l'objet cible.                          |
|        -ETS        | Envoyer des commandes aux sessions de suivi d’événements directement sans enregistrement ou planification. |
|        -As         |               Effectuez l’opération demandée de manière asynchrone.                |

## <a name="examples"></a>Exemples  
La commande suivante démarre le collecteur de données perf_log sur l’ordinateur distant server_1.  
```  
logman start perf_log -s server_1  
```  
## <a name="additional-references"></a>Références supplémentaires  
[logman](logman.md)  
