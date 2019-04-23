---
title: logman delete
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8f3b2422-3dce-4fb4-adbb-8536b1d7da2b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 00473b5178eca93e9644888fbaa4c4c96c0dd683
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842060"
---
# <a name="logman-delete"></a>logman delete

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

supprimer un collecteur de données existant.  
  
## <a name="syntax"></a>Syntaxe  
```  
logman delete <[-n] <name>> [options]  
```  
## <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|/?|Affiche l’aide contextuelle.|  
|-s <computer name>|Exécuter la commande sur l’ordinateur distant spécifié.|  
|-config <value>|Spécifie le fichier de paramètres contenant les options de commande.|  
|[-n] <name>|Nom du collecteur de données cible.|  
|-ets|Envoyer des commandes aux Sessions de suivi d’événements directement sans enregistrement ni planification.|  
|-u [-] < utilisateur [password] >|Spécifie l’utilisateur à exécuter en tant que. Entrer un * pour le mot de passe génère une invite pour le mot de passe. Le mot de passe n’est pas affichée lorsque vous le tapez à l’invite de mot de passe.|  
## <a name="BKMK_examples"></a>Exemples  
La commande suivante supprime le journal_perf de collecteur de données.  
```  
logman delete perf_log  
```  
#### <a name="additional-references"></a>Références supplémentaires  
[logman](logman.md)  
