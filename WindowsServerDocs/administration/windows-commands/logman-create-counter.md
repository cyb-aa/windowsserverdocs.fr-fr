---
title: logman créer un compteur
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1e214c32-b704-43c1-b548-e1cf43b583c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: feff7ad1597232ad441e2ad23ee73638b4ad68ea
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724397"
---
# <a name="logman-create-counter"></a>logman créer un compteur

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

créer un collecteur de données de compteur.  

## <a name="syntax"></a>Syntaxe  
```  
logman create counter <[-n] <name>> [options]  
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
|                  -cf<filename>                  |                       Spécifie le fichier répertoriant les compteurs de performances à collecter. Le fichier doit contenir un nom de compteur de performance par ligne.                        |
|               -c <chemin d’accès [chemin []] >               |                                                              Spécifie le ou les compteurs de performance à collecter.                                                               |
|                   -SC <value>                    |                                      Spécifie le nombre maximal d’échantillons à collecter avec un collecteur de données de compteur de performance.                                      |

## <a name="remarks"></a>Notes   
Où [-] est listé, un extra-inverse l’option.  
## <a name="examples"></a>Exemples  
La commande suivante crée un compteur appelé perf_log à l’aide du compteur% temps processeur de la catégorie de compteurs Processeur (_Total).  
```  
logman create counter perf_log -c \Processor(_Total)\% Processor time  
```  
La commande suivante crée un compteur appelé perf_log à l’aide du compteur% temps processeur de la catégorie de compteurs Processeur (_Total), en créant un fichier journal d’une taille maximale de 10 Mo et en collectant des données pendant 1 minute et 0 seconde.  
```  
logman create counter perf_log -c \Processor(_Total)\% Processor time -max 10 -rf 01:00  
```  
## <a name="additional-references"></a>Références supplémentaires  
[logman](logman.md)  
