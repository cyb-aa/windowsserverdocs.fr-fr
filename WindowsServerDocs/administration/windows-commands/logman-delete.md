---
title: logman delete
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8f3b2422-3dce-4fb4-adbb-8536b1d7da2b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8360a4955a5ebed3eb25fda77acf587c56fbf5d6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374427"
---
# <a name="logman-delete"></a>logman delete

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

supprimer un collecteur de données existant.  

## <a name="syntax"></a>Syntaxe  
```  
logman delete <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Paramètres  

|        Paramètre        |                                                                               Description                                                                               |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /?            |                                                                    Affiche l’aide contextuelle.                                                                     |
|   -s <computer name>    |                                                          Exécutez la commande sur l’ordinateur distant spécifié.                                                          |
|     -config <value>     |                                                         Spécifie le fichier de paramètres contenant les options de commande.                                                         |
|       [-n] <name>       |                                                                   Nom du collecteur de données cibles.                                                                    |
|          -ETS           |                                              Envoyer des commandes aux sessions de suivi d’événements directement sans enregistrement ou planification.                                               |
| -[-] u < utilisateur [mot de passe] > | Spécifie l’utilisateur à exécuter en tant que. La saisie d’un \* pour le mot de passe génère une invite pour le mot de passe. Le mot de passe ne s’affiche pas lorsque vous le tapez à l’invite de mot de passe. |

## <a name="BKMK_examples"></a>Illustre  
La commande suivante supprime le collecteur de données journal_perf.  
```  
logman delete perf_log  
```  
#### <a name="additional-references"></a>Références supplémentaires  
[logman](logman.md)  
