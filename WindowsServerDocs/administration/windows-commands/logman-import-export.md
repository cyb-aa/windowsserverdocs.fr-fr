---
title: logman Import | exporter
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c258daba-fb93-47c0-a53b-2fe83ed2c743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 81147f9e2e2da69c8e59969f3c176264a7fa353a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840672"
---
# <a name="logman-import--export"></a>logman Import | exporter

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
|   -s <computer name>    |                                                   Exécutez la commande sur l’ordinateur distant spécifié.                                                   |
|     -config <value>     |                                                  Spécifie le fichier de paramètres contenant les options de commande.                                                  |
|       [-n] <name>       |                                                                Nom de l’objet cible.                                                                 |
|       -<name> XML       |                                                         Nom du fichier XML à importer ou exporter.                                                         |
|          -ETS           |                                       Envoyer des commandes aux sessions de suivi d’événements directement sans enregistrement ou planification.                                        |
| -[-] u < utilisateur [mot de passe] > | Utilisateur à exécuter en tant que. La saisie d’un \* pour le mot de passe génère une invite pour le mot de passe. Le mot de passe ne s’affiche pas lorsque vous le tapez à l’invite de mot de passe. |
|           -y            |                                                      Répondez oui à toutes les questions sans demander confirmation.                                                       |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre  
La commande suivante importe le fichier XML c:\windows\ perf_log. XML à partir de l’ordinateur server_1 en tant qu’ensemble de collecteurs de données appelé perf_log.  
```  
logman import perf_log -s server_1 -xml c:\windows\perf_log.xml  
```  
## <a name="additional-references"></a>Références supplémentaires  
[logman](logman.md)  
