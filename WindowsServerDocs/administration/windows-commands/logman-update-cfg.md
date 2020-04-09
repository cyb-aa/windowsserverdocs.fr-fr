---
title: logman update cfg
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9da4e8b4-3be5-42d3-b0b4-c429630c35c4 britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c74370432dbc21f244dd675bb62cc65a13fa2ec7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840532"
---
# <a name="logman-update-cfg"></a>logman update cfg

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Met à jour les propriétés d’un collecteur de données de configuration existant.  

## <a name="syntax"></a>Syntaxe  
```  
logman update cfg <[-n] <name>> [options]  
```  
### <a name="parameters"></a>Paramètres  

|                    Paramètre                     |                                                                               Description                                                                               |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                        /?                        |                                                                    Affiche l’aide contextuelle.                                                                     |
|                -s <computer name>                |                                                          Exécutez la commande sur l’ordinateur distant spécifié.                                                          |
|                 -config <value>                  |                                                         Spécifie le fichier de paramètres contenant les options de commande.                                                         |
|                   [-n] <name>                    |                                                                       Nom de l’objet cible.                                                                        |
| -f < bin&#124;bincirc&#124;CSV&#124;&#124;SQL > |                                                            Spécifie le format du journal pour le collecteur de données.                                                             |
|             -[-] u < utilisateur [mot de passe] >              | Spécifie l’utilisateur à exécuter en tant que. La saisie d’un \* pour le mot de passe génère une invite pour le mot de passe. Le mot de passe ne s’affiche pas lorsque vous le tapez à l’invite de mot de passe. |
|    -m < [Start] [STOP] [[Start] [STOP] [...]] >    |                                                Passez au démarrage manuel ou à l’arrêt au lieu d’une heure de début ou de fin planifiée.                                                 |
|                -RF < [[hh :] mm :] SS >                |                                                        Exécuter le collecteur de données pendant la période spécifiée.                                                         |
|        -b < M/j/aaaa h :mm : SS [AM&#124;PM] >         |                                                              Commencer à collecter des données à l’heure spécifiée.                                                               |
|        -e < M/j/aaaa h :mm : SS [AM&#124;PM] >         |                                                               Terminer la collecte de données à l’heure spécifiée.                                                                |
|                -Si < [[hh :] mm :] SS >                |                                                 Spécifie l’intervalle échantillon pour les collecteurs de données des compteurs de performance.                                                  |
|              -o < chemin&#124;DSN ! log >              |                                              Spécifie le fichier journal de sortie ou le nom de l’ensemble de journaux et de journaux dans une base de données SQL.                                               |
|                      -[-] r                       |                                                  Répète le collecteur de données quotidiennement aux heures de début et de fin spécifiées.                                                  |
|                      -[-] a                       |                                                                     Ajouter à un fichier journal existant.                                                                     |
|                      -[-]                      |                                                                     Remplacer un fichier journal existant.                                                                     |
|           -[-] v < nnnnnn&#124;mmddhhmm >           |                                                   Joignez les informations de contrôle de version des fichiers à la fin du nom du fichier journal.                                                   |
|                  -[-] RC <task>                   |                                                         Exécutez la commande spécifiée chaque fois que le journal est fermé.                                                          |
|                 -[-] Max <value>                  |                                                 Taille maximale du fichier journal en Mo ou nombre maximal d’enregistrements pour les journaux SQL.                                                  |
|              -[-] cnf < [[hh :] mm :] SS >              |     Lorsque l’heure est spécifiée, crée un nouveau fichier une fois que l’heure spécifiée s’est écoulée. Lorsque l’heure n’est pas spécifiée, créez un nouveau fichier lorsque la taille maximale est dépassée.     |
|                        -y                        |                                                             Répondez oui à toutes les questions sans demander confirmation.                                                              |
|                      -[-] ni                      |                                                         Activez la requête d’interface réseau (-ou) ou désactivez-la.                                                          |
|             -reg < chemin d’accès [chemin d’accès [...]] >             |                                                                 Spécifie la ou les valeurs de Registre à collecter.                                                                 |
|            -requête de gestion des < [requête [...]] >            |                                                      Spécifie le ou les objets WMI à collecter à l’aide du langage de requête SQL.                                                       |
|             -FTC < chemin d’accès [chemin [...]] >             |                                                           Spécifie le chemin d’accès complet au (x) fichier (s) à collecter.                                                            |

## <a name="remarks"></a>Notes  
Où [-] est listé, un extra-inverse l’option.  
## <a name="examples"></a><a name=BKMK_examples></a>Illustre  
La commande suivante met à jour le collecteur de données de configuration existant cfg_log pour collecter la clé de Registre HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows NT\Currentverion\\.  
```  
logman update cfg cfg_log -reg HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\Currentverion\  
```  
## <a name="additional-references"></a>Références supplémentaires  
[logman](logman.md)  
[logman créer cfg](logman-create-cfg.md)  
