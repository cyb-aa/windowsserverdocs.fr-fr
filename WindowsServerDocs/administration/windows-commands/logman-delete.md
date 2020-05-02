---
title: logman delete
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8f3b2422-3dce-4fb4-adbb-8536b1d7da2b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b30fd6eb7915d3d0296988a98968dcde58bdbc2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724374"
---
# <a name="logman-delete"></a>logman delete

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

supprimer un collecteur de données existant.  

## <a name="syntax"></a>Syntaxe  
```  
logman delete <[-n] <name>> [options]  
```  
### <a name="parameters"></a>Paramètres  

|        Paramètre        |                                                                               Description                                                                               |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /?            |                                                                    Affiche l’aide contextuelle.                                                                     |
|   -s<computer name>    |                                                          Exécutez la commande sur l’ordinateur distant spécifié.                                                          |
|     -config <value>     |                                                         Spécifie le fichier de paramètres contenant les options de commande.                                                         |
|       [-n]<name>       |                                                                   Nom du collecteur de données cibles.                                                                    |
|          -ETS           |                                              Envoyer des commandes aux sessions de suivi d’événements directement sans enregistrement ou planification.                                               |
| -[-] u <utilisateur [mot de passe] > | Spécifie l’utilisateur à exécuter en tant que. La saisie \* d’un pour le mot de passe génère une invite pour le mot de passe. Le mot de passe ne s’affiche pas lorsque vous le tapez à l’invite de mot de passe. |

## <a name="examples"></a>Exemples  
La commande suivante supprime le perf_log du collecteur de données.  
```  
logman delete perf_log  
```  
## <a name="additional-references"></a>Références supplémentaires  
[logman](logman.md)  
