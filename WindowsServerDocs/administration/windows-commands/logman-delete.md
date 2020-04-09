---
title: logman delete
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8f3b2422-3dce-4fb4-adbb-8536b1d7da2b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e0ab38eab988770de4fbcef8af2c7be6a6137b16
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840752"
---
# <a name="logman-delete"></a>logman delete

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

supprimer un collecteur de données existant.  

## <a name="syntax"></a>Syntaxe  
```  
logman delete <[-n] <name>> [options]  
```  
### <a name="parameters"></a>Paramètres  

|        Paramètre        |                                                                               Description                                                                               |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /?            |                                                                    Affiche l’aide contextuelle.                                                                     |
|   -s <computer name>    |                                                          Exécutez la commande sur l’ordinateur distant spécifié.                                                          |
|     -config <value>     |                                                         Spécifie le fichier de paramètres contenant les options de commande.                                                         |
|       [-n] <name>       |                                                                   Nom du collecteur de données cibles.                                                                    |
|          -ETS           |                                              Envoyer des commandes aux sessions de suivi d’événements directement sans enregistrement ou planification.                                               |
| -[-] u < utilisateur [mot de passe] > | Spécifie l’utilisateur à exécuter en tant que. La saisie d’un \* pour le mot de passe génère une invite pour le mot de passe. Le mot de passe ne s’affiche pas lorsque vous le tapez à l’invite de mot de passe. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre  
La commande suivante supprime le perf_log du collecteur de données.  
```  
logman delete perf_log  
```  
## <a name="additional-references"></a>Références supplémentaires  
[logman](logman.md)  
