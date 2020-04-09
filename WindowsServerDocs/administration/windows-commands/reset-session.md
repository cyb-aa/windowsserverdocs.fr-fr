---
title: reset session
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4f029ecc-874e-415a-95a8-8b731bae35f9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: b83aa67e0d90be9ddc679b32c8ffa4270efec41f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835792"
---
# <a name="reset-session"></a>reset session

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous permet de réinitialiser (supprimer) une session sur un serveur hôte de session Bureau à distance (hôte de session Bureau à distance).  
pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_examples).  

> [!NOTE]  
> Sous Windows Server 2008 R2, les services Terminal Server sont appelés Services Bureau à distance. Pour découvrir les nouveautés de la dernière version, consultez les nouveautés [de services Bureau à distance dans Windows server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.  

## <a name="syntax"></a>Syntaxe  
```  
reset session {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]  
```  

### <a name="parameters"></a>Paramètres  

|Paramètre|Description|  
|-------|--------|  
|\<NomSession >|Spécifie le nom de la session que vous souhaitez réinitialiser. Pour déterminer le nom de la session, utilisez la commande de **session de requête** .|  
|\<SessionID >|Spécifie l’ID de la session à réinitialiser.|  
|/Server :\<ServerName >|Spécifie le serveur Terminal Server qui contient la session que vous souhaitez réinitialiser. Dans le cas contraire, le serveur hôte de session Bureau à distance actuel est utilisé.|  
|/v|Affiche des informations sur les actions en cours d’exécution.|  
|/?|Affiche l'aide à l'invite de commandes.|  

## <a name="remarks"></a>Notes  
-   Vous pouvez toujours réinitialiser vos propres sessions, mais vous devez disposer de l’autorisation contrôle total pour réinitialiser la session d’un autre utilisateur.  
-   N’oubliez pas que la réinitialisation de la session d’un utilisateur sans avertissement l’utilisateur peut entraîner la perte de données au niveau de la session.  
-   Vous devez réinitialiser une session uniquement lorsqu’elle ne fonctionne pas correctement ou qu’elle semble avoir cessé de répondre.  
-   Le paramètre **/Server** est requis uniquement si vous utilisez **Réinitialiser la session** à partir d’un serveur distant.  

## <a name="examples"></a><a name=BKMK_examples></a>Illustre  
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
