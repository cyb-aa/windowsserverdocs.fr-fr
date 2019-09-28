---
title: reset session
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f029ecc-874e-415a-95a8-8b731bae35f9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 67a4e910ba87209c9700f2242f7859a6cc9e725f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384529"
---
# <a name="reset-session"></a>reset session

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Vous permet de réinitialiser (supprimer) une session sur un serveur hôte de session Bureau à distance (hôte de session Bureau à distance).  
Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_examples).  

> [!NOTE]  
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour découvrir les nouveautés de la dernière version, consultez les nouveautés [de services Bureau à distance dans Windows server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.  

## <a name="syntax"></a>Syntaxe  
```  
reset session {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]  
```  

## <a name="parameters"></a>Paramètres  

|Paramètre|Description|  
|-------|--------|  
|@no__t 0SessionName >|Spécifie le nom de la session que vous souhaitez réinitialiser. Pour déterminer le nom de la session, utilisez la commande de **session de requête** .|  
|@no__t 0SessionID >|Spécifie l’ID de la session à réinitialiser.|  
|/Server : \<ServerName >|Spécifie le serveur Terminal Server qui contient la session que vous souhaitez réinitialiser. Dans le cas contraire, le serveur hôte de session Bureau à distance actuel est utilisé.|  
|/v|Affiche des informations sur les actions en cours d’exécution.|  
|/?|Affiche l'aide à l'invite de commandes.|  

## <a name="remarks"></a>Notes  
-   Vous pouvez toujours réinitialiser vos propres sessions, mais vous devez disposer de l’autorisation contrôle total pour réinitialiser la session d’un autre utilisateur.  
-   N’oubliez pas que la réinitialisation de la session d’un utilisateur sans avertissement l’utilisateur peut entraîner la perte de données au niveau de la session.  
-   Vous devez réinitialiser une session uniquement lorsqu’elle ne fonctionne pas correctement ou qu’elle semble avoir cessé de répondre.  
-   Le paramètre **/Server** est requis uniquement si vous utilisez **Réinitialiser la session** à partir d’un serveur distant.  

## <a name="BKMK_examples"></a>Illustre  
- Pour réinitialiser la session RDP-TCP # 6, tapez :  
  ```  
  reset session rdp-tcp#6  
  ```  
- Pour réinitialiser la session qui utilise l’ID de session 3, tapez :  
  ```  
  reset session 3  
  ```  

#### <a name="additional-references"></a>Références supplémentaires  
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
[Services Bureau à distance &#40;la référence&#41; des commandes des services Terminal Server](remote-desktop-services-terminal-services-command-reference.md)  
