---
title: Logman créer des api
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ecc0a75-2613-464a-8616-c5dc404bb736
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: feabb946c6dd492f38d9d6e2e1dc69795839e211
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840630"
---
# <a name="logman-create-api"></a>Logman créer des api

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Créer un collecteur de données de suivi API.  
  
## <a name="syntax"></a>Syntaxe  
```  
logman create api <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|/?|Affiche l’aide contextuelle.|  
|-s <computer name>|Exécuter la commande sur l’ordinateur distant spécifié.|  
|-config <value>|Spécifie le fichier de paramètres contenant les options de commande.|  
|[-n] <name>|Nom de l’objet cible.|  
|-f <bin&#124;bincirc&#124;csv&#124;tsv&#124;sql>|Spécifie le format de journal pour le collecteur de données.|  
|-u [-] < utilisateur [password] >|Spécifie l’utilisateur à exécuter en tant que. Entrer un * pour le mot de passe génère une invite pour le mot de passe. Le mot de passe n’est pas affichée lorsque vous le tapez à l’invite de mot de passe.|  
|-m < [start] [stop] [[start] [stop] [...]] >|Remplacez par démarrage manuel ou arrêter au lieu d’une heure de début ou de fin planifiée.|  
|-rf < [[hh :] mm :] ss >|Exécutez le collecteur de données pendant la période de temps spécifiée.|  
|-b <M/d/yyyy h:mm:ss[AM&#124;PM]>|Commencer à collecter les données à l’heure spécifiée.|  
|e - < j/aaaa h : mmss [AM&#124;PM] >|Terminer la collecte des données à l’heure spécifiée.|  
|-si < [[hh :] mm :] ss >|Spécifie l’intervalle d’échantillonnage pour les collecteurs de données de compteur de performances.|  
|-o <path&#124;dsn!log>|Spécifie que le fichier journal de sortie ou la source de données et le journal de nom de jeu dans une base de données SQL.|  
|-[-]r|Répétez le collecteur de données tous les jours au début spécifié et les heures de fin.|  
|-[-]a|ajouter à un fichier journal existant.|  
|-[-]ow|Remplacer un fichier journal existant.|  
|-[-]v <nnnnnn&#124;mmddhhmm>|joindre des informations de contrôle de version de fichier à la fin du nom du fichier journal.|  
|-[-] rc <task>|Exécutez la commande spécifiée chaque fois que le journal est fermé.|  
|-[-]max <value>|Taille du fichier journal maximale en Mo ou nombre maximal d’enregistrements des journaux SQL.|  
|-[-] cnf < [[hh :] mm :] ss >|Lors de l’heure est spécifiée, créez un nouveau fichier lorsque le délai imparti est écoulé. Lorsque le temps n’est pas spécifié, créez un nouveau fichier lorsque la taille maximale est dépassée.|  
|-y|Répondez Oui à toutes les questions sans demander confirmation.|  
|-mods < chemin d’accès [[...]] >|Spécifie la liste des modules pour vous connecter à partir d’appels d’API.|  
|-inapis < module ! api [module ! api [...]] >|Spécifie la liste d’appels d’API à inclure dans la journalisation.|  
|-exapis < module ! api [module ! api [...]] >|Spécifie la liste d’appels d’API à exclure de la journalisation.|  
|-[-] ano|Journal (-ano) API nomme uniquement, ou ne vous connectez pas uniquement (-ano) noms d’API.|  
|-[-]recursive|Journal (-récursive) ou ne pas enregistrer (-récursive) API de manière récursive au-delà de la première couche.|  
|-exe <value>|Spécifie le chemin d’accès complet à un fichier exécutable pour le traçage des API.|  
## <a name="remarks"></a>Notes  
Si [-] est listé, un supplémentaire - inverse l’option.  
## <a name="BKMK_examples"></a>Exemples  
La commande suivante crée une trace de l’API appelée compteur trace_notepad pour le fichier exécutable c:\windows\notepad.exe et renvoie les résultats vers le fichier c:\notepad.etl.  
```  
logman create api trace_notepad -exe c:\windows\notepad.exe -o c:\notepad.etl  
```  
La commande suivante crée une trace de l’API appelée compteur trace_notepad pour le c:\windows\notepad.exe fichier exécutable la collecte des valeurs produites par le module c:\windows\system32\advapi32.dll.  
```  
logman create api trace_notepad -exe c:\windows\notepad.exe -mods c:\windows\system32\advapi32.dll  
```  
La commande suivante crée une trace de l’API appelée compteur trace_notepad pour le c:\windows\notepad.exe fichier exécutable à l’exclusion de l’appel d’API TlsGetValue produite par le module kernel32.dll.  
```  
logman create api trace_notepad -exe c:\windows\notepad.exe -exapis kernel32.dll!TlsGetValue  
```  
#### <a name="additional-references"></a>Références supplémentaires  
[logman](logman.md)  
