---
title: logman Import | exporter
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c258daba-fb93-47c0-a53b-2fe83ed2c743
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 309274b5288bd1c17259e01cf563ae8685a2094e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374460"
---
# <a name="logman-import--export"></a>logman Import | exporter

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

importez un ensemble de collecteurs de données à partir d’un fichier XML ou exportez un ensemble de collecteurs de données dans un fichier XML.  

## <a name="syntax"></a>Syntaxe  
```  
logman import <[-n] <name>> <-xml <name>> [options]  
logman export <[-n] <name>> <-xml <name>> [options]  
```  
## <a name="parameters"></a>Paramètres  

|        Paramètre        |                                                                        Description                                                                        |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
|           -?            |                                                             Affiche l’aide contextuelle.                                                              |
|   -s <computer name>    |                                                   Exécutez la commande sur l’ordinateur distant spécifié.                                                   |
|     -config <value>     |                                                  Spécifie le fichier de paramètres contenant les options de commande.                                                  |
|       [-n] <name>       |                                                                Nom de l’objet cible.                                                                 |
|       -@no__t XML-0       |                                                         Nom du fichier XML à importer ou exporter.                                                         |
|          -ETS           |                                       Envoyer des commandes aux sessions de suivi d’événements directement sans enregistrement ou planification.                                        |
| -[-] u < utilisateur [mot de passe] > | Utilisateur à exécuter en tant que. La saisie d’un \* pour le mot de passe génère une invite pour le mot de passe. Le mot de passe ne s’affiche pas lorsque vous le tapez à l’invite de mot de passe. |
|           -y            |                                                      Répondez oui à toutes les questions sans demander confirmation.                                                       |

## <a name="BKMK_examples"></a>Illustre  
La commande suivante importe le fichier XML c:\windows\perf_log.XML à partir de l’ordinateur serveur_1 en tant qu’ensemble de collecteurs de données nommé journal_perf.  
```  
logman import perf_log -s server_1 -xml "c:\windows\perf_log.xml"  
```  
#### <a name="additional-references"></a>Références supplémentaires  
[logman](logman.md)  
