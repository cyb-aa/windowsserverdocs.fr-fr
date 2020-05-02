---
title: reset session
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4f029ecc-874e-415a-95a8-8b731bae35f9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: df7b953e02c7339b7ed66a831955f802dd21e624
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722362"
---
# <a name="reset-session"></a>reset session

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous permet de réinitialiser (supprimer) une session sur un serveur hôte de session Bureau à distance (hôte de session Bureau à distance).  
  

> [!NOTE]  
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour découvrir les nouveautés de la dernière version, consultez les nouveautés [de services Bureau à distance dans Windows server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.  

## <a name="syntax"></a>Syntaxe  
```  
reset session {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]  
```  

### <a name="parameters"></a>Paramètres  

|Paramètre|Description|  
|-------|--------|  
|\<Session>|Spécifie le nom de la session que vous souhaitez réinitialiser. Pour déterminer le nom de la session, utilisez la commande de **session de requête** .|  
|\<SessionID>|Spécifie l’ID de la session à réinitialiser.|  
|/Server :\<NomServeur>|Spécifie le serveur Terminal Server qui contient la session que vous souhaitez réinitialiser. Dans le cas contraire, le serveur hôte de session Bureau à distance actuel est utilisé.|  
|/v|Affiche des informations sur les actions en cours d’exécution.|  
|/?|Affiche l'aide à l'invite de commandes.|  

## <a name="remarks"></a>Notes   
-   Vous pouvez toujours réinitialiser vos propres sessions, mais vous devez disposer de l’autorisation contrôle total pour réinitialiser la session d’un autre utilisateur.  
-   N’oubliez pas que la réinitialisation de la session d’un utilisateur sans avertissement l’utilisateur peut entraîner la perte de données au niveau de la session.  
-   Vous devez réinitialiser une session uniquement lorsqu’elle ne fonctionne pas correctement ou qu’elle semble avoir cessé de répondre.  
-   Le paramètre **/Server** est requis uniquement si vous utilisez **Réinitialiser la session** à partir d’un serveur distant.  

## <a name="examples"></a>Exemples  
- Pour réinitialiser la session RDP-TCP # 6, tapez :  
  ```  
  reset session rdp-tcp#6  
  ```  
- Pour réinitialiser la session qui utilise l’ID de session 3, tapez :  
  ```  
  reset session 3  
  ```  

## <a name="additional-references"></a>Références supplémentaires  
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
[Référence des commandes des services Bureau à distance (services Terminal Server)](remote-desktop-services-terminal-services-command-reference.md)  
