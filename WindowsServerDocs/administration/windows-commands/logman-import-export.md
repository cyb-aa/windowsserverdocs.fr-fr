---
title: Logman import | exportation
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 67621b109c379cc2758b4036460b6bb82df3e3d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848300"
---
# <a name="logman-import--export"></a>Logman import | exportation

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

importer un ensemble de collecteurs de données à partir d’un fichier XML, ou exporter un ensemble de collecteurs de données dans un fichier XML.  
  
## <a name="syntax"></a>Syntaxe  
```  
logman import <[-n] <name>> <-xml <name>> [options]  
logman export <[-n] <name>> <-xml <name>> [options]  
```  
## <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|-?|Affiche l’aide contextuelle.|  
|-s <computer name>|Exécuter la commande sur l’ordinateur distant spécifié.|  
|-config <value>|Spécifie le fichier de paramètres contenant les options de commande.|  
|[-n] <name>|Nom de l’objet cible.|  
|-xml <name>|Nom du fichier XML pour importer ou exporter.|  
|-ets|Envoyer des commandes aux Sessions de suivi d’événements directement sans enregistrement ni planification.|  
|-u [-] < utilisateur [password] >|Exécuter en tant qu’utilisateur. Entrer un * pour le mot de passe génère une invite pour le mot de passe. Le mot de passe n’est pas affichée lorsque vous le tapez à l’invite de mot de passe.|  
|-y|Répondez Oui à toutes les questions sans demander confirmation.|  
## <a name="BKMK_examples"></a>Exemples  
La commande suivante importe le c:\windows\perf_log.xml fichier XML à partir de l’ordinateur server_1 que journal_perf appelé jeu d’un collecteur de données.  
```  
logman import perf_log -s server_1 -xml "c:\windows\perf_log.xml"  
```  
#### <a name="additional-references"></a>Références supplémentaires  
[logman](logman.md)  
