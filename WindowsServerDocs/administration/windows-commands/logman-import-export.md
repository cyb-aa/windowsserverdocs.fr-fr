---
title: logman Import | exporter
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c258daba-fb93-47c0-a53b-2fe83ed2c743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ffc2e42f353352f69cf61dfb1f108a7d53cb7c4b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724359"
---
# <a name="logman-import--export"></a>logman Import | exporter

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

importez un ensemble de collecteurs de données à partir d’un fichier XML ou exportez un ensemble de collecteurs de données dans un fichier XML.  

## <a name="syntax"></a>Syntaxe  
```  
logman import <[-n] <name>> <-xml <name>> [options]  
logman export <[-n] <name>> <-xml <name>> [options]  
```  
### <a name="parameters"></a>Paramètres  

|        Paramètre        |                                                                        Description                                                                        |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
|           -?            |                                                             Affiche l’aide contextuelle.                                                              |
|   -s<computer name>    |                                                   Exécutez la commande sur l’ordinateur distant spécifié.                                                   |
|     -config <value>     |                                                  Spécifie le fichier de paramètres contenant les options de commande.                                                  |
|       [-n]<name>       |                                                                Nom de l'objet cible.                                                                 |
|       -XML<name>       |                                                         Nom du fichier XML à importer ou exporter.                                                         |
|          -ETS           |                                       Envoyer des commandes aux sessions de suivi d’événements directement sans enregistrement ou planification.                                        |
| -[-] u <utilisateur [mot de passe] > | Utilisateur à exécuter en tant que. La saisie \* d’un pour le mot de passe génère une invite pour le mot de passe. Le mot de passe ne s’affiche pas lorsque vous le tapez à l’invite de mot de passe. |
|           -y            |                                                      Répondez oui à toutes les questions sans demander confirmation.                                                       |

## <a name="examples"></a>Exemples  
La commande suivante importe le fichier XML c:\windows\ perf_log. XML à partir de l’ordinateur server_1 en tant qu’ensemble de collecteurs de données appelé perf_log.  
```  
logman import perf_log -s server_1 -xml c:\windows\perf_log.xml  
```  
## <a name="additional-references"></a>Références supplémentaires  
[logman](logman.md)  
