---
title: alerte de mise à jour logman
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ede94a76-931c-40ed-9fda-6766bed8ff72 britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc32e3de6078489e59fe24c97f02fb440e86628d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374390"
---
# <a name="logman-update-alert"></a>alerte de mise à jour logman

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Mettre à jour les propriétés d’un collecteur de données d’alerte existant.  

## <a name="syntax"></a>Syntaxe  
```  
logman update alert <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Paramètres  

|                 Paramètre                  |                                                                               Description                                                                               |
|--------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                     /?                     |                                                                    Affiche l’aide contextuelle.                                                                     |
|             -s <computer name>             |                                                          Exécutez la commande sur l’ordinateur distant spécifié.                                                          |
|              -config <value>               |                                                         Spécifie le fichier de paramètres contenant les options de commande.                                                         |
|                [-n] <name>                 |                                                                       Nom de l’objet cible.                                                                        |
|          -[-] u < utilisateur [mot de passe] >           | Spécifie l’utilisateur à exécuter en tant que. La saisie d’un \* pour le mot de passe génère une invite pour le mot de passe. Le mot de passe ne s’affiche pas lorsque vous le tapez à l’invite de mot de passe. |
| -m < [Start] [STOP] [[Start] [STOP] [...]] > |                                                Passez au démarrage manuel ou à l’arrêt au lieu d’une heure de début ou de fin planifiée.                                                 |
|             -RF < [[hh :] mm :] SS >             |                                                        Exécuter le collecteur de données pendant la période spécifiée.                                                         |
|     -b < M/j/aaaa h :mm : SS [AM&#124;PM] >      |                                                              Commencer à collecter des données à l’heure spécifiée.                                                               |
|     -e < M/j/aaaa h :mm : SS [AM&#124;PM] >      |                                                               Terminer la collecte de données à l’heure spécifiée.                                                                |
|             -Si < [[hh :] mm :] SS >             |                                                 Spécifie l’intervalle échantillon pour les collecteurs de données des compteurs de performance.                                                  |
|           -o < chemin&#124;DSN ! log >           |                                              Spécifie le fichier journal de sortie ou le nom de l’ensemble de journaux et de journaux dans une base de données SQL.                                               |
|                   -[-] r                    |                                                  Répète le collecteur de données quotidiennement aux heures de début et de fin spécifiées.                                                  |
|                   -[-] a                    |                                                                     Ajouter à un fichier journal existant.                                                                     |
|                   -[-]                   |                                                                     Remplacer un fichier journal existant.                                                                     |
|        -[-] v < nnnnnn&#124;mmddhhmm >        |                                                   Joignez les informations de contrôle de version des fichiers à la fin du nom du fichier journal.                                                   |
|               -[-] RC <task>                |                                                         Exécutez la commande spécifiée chaque fois que le journal est fermé.                                                          |
|              -[-] Max <value>               |                                                 Taille maximale du fichier journal en Mo ou nombre maximal d’enregistrements pour les journaux SQL.                                                  |
|           -[-] cnf < [[hh :] mm :] SS >           |     Lorsque l’heure est spécifiée, crée un nouveau fichier une fois que l’heure spécifiée s’est écoulée. Lorsque l’heure n’est pas spécifiée, créez un nouveau fichier lorsque la taille maximale est dépassée.     |
|                     -y                     |                                                             Répondez oui à toutes les questions sans demander confirmation.                                                              |
|               -CF <filename>               |                       Spécifie le fichier répertoriant les compteurs de performances à collecter. Le fichier doit contenir un nom de compteur de performance par ligne.                        |
|                   -[-] El                   |                                                                Active ou désactive la création de rapports du journal des événements.                                                                 |
|     -ième < seuil [seuil [...]] >      |                                                        Spécifiez les compteurs et leurs valeurs de seuil pour une alerte.                                                        |
|              -[-] RDCS <name>               |                                                     Spécifie l’ensemble de collecteurs de données à démarrer lorsqu’une alerte se déclenche.                                                      |
|               -[-] TN <task>                |                                                             Spécifie la tâche à exécuter lorsqu’une alerte se déclenche.                                                              |
|            -[-] ension <argument>             |                                               Spécifie les arguments de tâche à utiliser avec la tâche spécifiée à l’aide de-TN.                                                |

## <a name="remarks"></a>Notes  
Où [-] est listé, un extra-inverse l’option.  
## <a name="BKMK_examples"></a>Illustre  
L’exemple suivant met à jour le collecteur de données new_alert existant, en définissant la valeur de seuil du compteur% temps processeur dans le groupe de compteurs Processeur (_ total) sur 40%.  
```  
logman update alert new_alert -th "\Processor(_Total)\% Processor time>40"  
```  
#### <a name="additional-references"></a>Références supplémentaires  
[logman](logman.md)  
[logman créer une alerte](logman-create-alert.md)  
