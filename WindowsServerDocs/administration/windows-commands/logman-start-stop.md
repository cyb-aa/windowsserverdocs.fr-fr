---
title: logman start | erreur
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 395d325b31ee596e1394e7ed796a444f159d15fc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374408"
---
# <a name="logman-start--stop"></a>logman start | erreur

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Démarrez un collecteur de données et définissez l’heure de début sur manuel, ou arrêtez un ensemble de collecteurs de données et définissez l’heure de fin sur manuel.  

## <a name="syntax"></a>Syntaxe  
```  
logman start <[-n] <name>> [options]  
logman stop <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Paramètres  

|     Paramètre      |                                 Description                                  |
|--------------------|------------------------------------------------------------------------------|
|         -?         |                       Affiche l’aide contextuelle.                       |
| -s <computer name> |            Exécutez la commande sur l’ordinateur distant spécifié.             |
|  -config <value>   |           Spécifie le fichier de paramètres contenant les options de commande.            |
|    [-n] <name>     |                          Nom de l’objet cible.                          |
|        -ETS        | Envoyer des commandes aux sessions de suivi d’événements directement sans enregistrement ou planification. |
|        -As         |               Effectuez l’opération demandée de manière asynchrone.                |

## <a name="BKMK_examples"></a>Illustre  
La commande suivante démarre le collecteur de données journal_perf sur l’ordinateur distant serveur_1.  
```  
logman start perf_log -s server_1  
```  
#### <a name="additional-references"></a>Références supplémentaires  
[logman](logman.md)  
