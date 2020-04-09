---
title: logman start | erreur
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a40006a1-876e-474b-aaf1-f365c730deea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2bd81a33779aa58e7528d0173a7a4b49489de8f9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840622"
---
# <a name="logman-start--stop"></a>logman start | erreur

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
| -s <computer name> |            Exécutez la commande sur l’ordinateur distant spécifié.             |
|  -config <value>   |           Spécifie le fichier de paramètres contenant les options de commande.            |
|    [-n] <name>     |                          Nom de l’objet cible.                          |
|        -ETS        | Envoyer des commandes aux sessions de suivi d’événements directement sans enregistrement ou planification. |
|        -As         |               Effectuez l’opération demandée de manière asynchrone.                |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre  
La commande suivante démarre le collecteur de données perf_log sur l’ordinateur distant server_1.  
```  
logman start perf_log -s server_1  
```  
## <a name="additional-references"></a>Références supplémentaires  
[logman](logman.md)  
