---
title: logman start | arrêter
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a40006a1-876e-474b-aaf1-f365c730deea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0c6027c4c9a99e45bb1c2e95cdfd4a7687a5c43b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437682"
---
# <a name="logman-start--stop"></a>logman start | arrêter

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Démarrer un collecteur de données et définir l’heure de début sur manuel ou arrêter un collecteur de données valeur et définir l’heure de fin sur manuel.  

## <a name="syntax"></a>Syntaxe  
```  
logman start <[-n] <name>> [options]  
logman stop <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Paramètres  

|     Paramètre      |                                 Description                                  |
|--------------------|------------------------------------------------------------------------------|
|         -?         |                       Affiche l’aide contextuelle.                       |
| -s <computer name> |            Exécuter la commande sur l’ordinateur distant spécifié.             |
|  -config <value>   |           Spécifie le fichier de paramètres contenant les options de commande.            |
|    [-n] <name>     |                          Nom de l’objet cible.                          |
|        -ets        | Envoyer des commandes aux Sessions de suivi d’événements directement sans enregistrement ni planification. |
|        -as         |               Effectuer l’opération demandée de façon asynchrone.                |

## <a name="BKMK_examples"></a>Exemples  
La commande suivante démarre le journal_perf de collecteur de données sur l’ordinateur distant server_1.  
```  
logman start perf_log -s server_1  
```  
#### <a name="additional-references"></a>Références supplémentaires  
[logman](logman.md)  
