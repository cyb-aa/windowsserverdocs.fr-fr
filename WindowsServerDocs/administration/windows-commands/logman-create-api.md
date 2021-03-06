---
title: logman create API
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ecc0a75-2613-464a-8616-c5dc404bb736
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 75552c7aa2d4411dd60587f794239b4e1d73a853
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724419"
---
# <a name="logman-create-api"></a>logman create API

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

créer un collecteur de données de suivi d’API.  

## <a name="syntax"></a>Syntaxe  
```  
logman create api <[-n] <name>> [options]  
```  
### <a name="parameters"></a>Paramètres  

|                    Paramètre                     |                                                                               Description                                                                               |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                        /?                        |                                                                    Affiche l’aide contextuelle.                                                                     |
|                -s<computer name>                |                                                          Exécutez la commande sur l’ordinateur distant spécifié.                                                          |
|                 -config <value>                  |                                                         Spécifie le fichier de paramètres contenant les options de commande.                                                         |
|                   [-n]<name>                    |                                                                       Nom de l'objet cible.                                                                        |
| -f <bin&#124;bincirc&#124;CSV&#124;TSV&#124;SQL> |                                                            Spécifie le format du journal pour le collecteur de données.                                                             |
|             -[-] u <utilisateur [mot de passe] >              | Spécifie l’utilisateur à exécuter en tant que. La saisie \* d’un pour le mot de passe génère une invite pour le mot de passe. Le mot de passe ne s’affiche pas lorsque vous le tapez à l’invite de mot de passe. |
|    -m < [Start] [STOP] [[Start] [STOP] [...]] >    |                                                Passez au démarrage manuel ou à l’arrêt au lieu d’une heure de début ou de fin planifiée.                                                 |
|                -RF < [[hh :] mm :] SS>                |                                                        Exécuter le collecteur de données pendant la période spécifiée.                                                         |
|        -b <M/j/aaaa h :mm : SS [AM&#124;PM] >         |                                                              Commencer à collecter des données à l’heure spécifiée.                                                               |
|        -e <M/j/aaaa h :mm : SS [AM&#124;PM] >         |                                                               Terminer la collecte de données à l’heure spécifiée.                                                                |
|                -Si < [[hh :] mm :] SS>                |                                                 Spécifie l’intervalle échantillon pour les collecteurs de données des compteurs de performance.                                                  |
|              -o <chemin d’accès&#124;DSN ! log>              |                                              Spécifie le fichier journal de sortie ou le nom de l’ensemble de journaux et de journaux dans une base de données SQL.                                               |
|                      -[-] r                       |                                                  Répète le collecteur de données quotidiennement aux heures de début et de fin spécifiées.                                                  |
|                      -[-] a                       |                                                                     Ajouter à un fichier journal existant.                                                                     |
|                      -[-]                      |                                                                     Remplacer un fichier journal existant.                                                                     |
|           -[-] v <nnnnnn&#124;mmddhhmm>           |                                                   Joignez les informations de contrôle de version des fichiers à la fin du nom du fichier journal.                                                   |
|                  -[-] RC<task>                   |                                                         Exécutez la commande spécifiée chaque fois que le journal est fermé.                                                          |
|                 -[-] Max <value>                  |                                                 Taille maximale du fichier journal en Mo ou nombre maximal d’enregistrements pour les journaux SQL.                                                  |
|              -[-] cnf < [[hh :] mm :] SS>              |     Lorsque l’heure est spécifiée, crée un nouveau fichier une fois que l’heure spécifiée s’est écoulée. Lorsque l’heure n’est pas spécifiée, créez un nouveau fichier lorsque la taille maximale est dépassée.     |
|                        -y                        |                                                             Répondez oui à toutes les questions sans demander confirmation.                                                              |
|            -mods <chemin d’accès [chemin d’accès [...]] >             |                                                          Spécifie la liste des modules à partir desquels enregistrer les appels d’API.                                                           |
|     -INAPI <module ! API [module ! API [...]] >      |                                                         Spécifie la liste des appels d’API à inclure dans la journalisation.                                                          |
|     -exapi <module ! API [module ! API [...]] >      |                                                        Spécifie la liste des appels d’API à exclure de la journalisation.                                                         |
|                     -[-] ano                      |                                                     Log (-ANO) les noms d’API uniquement ou ne consignent pas uniquement les noms d’API (-ANO).                                                     |
|                  -[-] récursif                   |                                          Les API de journal (-récursif) ou de journalisation (-récursive) sont récursivement au-delà de la première couche.                                           |
|                   -exe <value>                   |                                                        Spécifie le chemin d’accès complet à un exécutable pour le suivi d’API.                                                        |

## <a name="remarks"></a>Notes   
Où [-] est listé, un extra-inverse l’option.  
## <a name="examples"></a>Exemples  
La commande suivante crée un compteur de trace API appelé trace_notepad pour le fichier exécutable c:\windows\notepad.exe et renvoie les résultats dans le fichier c:\Notepad.etl.  
```  
logman create api trace_notepad -exe c:\windows\notepad.exe -o c:\notepad.etl  
```  
La commande suivante crée un compteur de trace API appelé trace_notepad pour le fichier exécutable c:\windows\notepad.exe la collecte des valeurs produites par le module c:\windows\system32\advapi32.dll.  
```  
logman create api trace_notepad -exe c:\windows\notepad.exe -mods c:\windows\system32\advapi32.dll  
```  
La commande suivante crée un compteur de trace API appelé trace_notepad pour le fichier exécutable c:\windows\notepad.exe, à l’exception de l’appel d’API TlsGetValue généré par le module Kernel32. dll.  
```  
logman create api trace_notepad -exe c:\windows\notepad.exe -exapis kernel32.dll!TlsGetValue  
```  
## <a name="additional-references"></a>Références supplémentaires  
[logman](logman.md)  
