---
title: tscon
description: La rubrique commandes Windows pour tscon, qui se connecte à une autre session sur un serveur hôte de session Bureau à distance (hôte de session Bureau à distance).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 315a9793-cd10-4987-bb68-89a9d13f7fce
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1666e3fd47fab89b63efccef43489d15930cd6de
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832532"
---
# <a name="tscon"></a>tscon

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se connecte à une autre session sur un serveur hôte de session Bureau à distance.  

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).  

> [!NOTE]  
> Sous Windows Server 2008 R2, les services Terminal Server sont appelés Services Bureau à distance. Pour découvrir les nouveautés de la dernière version, consultez les nouveautés [de services Bureau à distance dans Windows server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.  

## <a name="syntax"></a>Syntaxe  
```  
tscon {<SessionID> | <SessionName>} [/dest:<SessionName>] [/password:<pw> | /password:*] [/v]  
```  
### <a name="parameters"></a>Paramètres  

|Paramètre|Description|  
|-------|--------|  
|\<SessionID >|Spécifie l’ID de la session à laquelle vous souhaitez vous connecter. Si vous utilisez le paramètre facultatif **/dest :** <*nom_session*>, il s’agit de l’ID de la session à laquelle vous souhaitez vous connecter.|  
|\<NomSession >|Spécifie le nom de la session à laquelle vous souhaitez vous connecter.|  
|/dest :\<Nom_session >|Spécifie le nom de la session active. Cette session se déconnecte lorsque vous vous connectez à la nouvelle session.|  
|/Password :\<PW >|Spécifie le mot de passe de l’utilisateur propriétaire de la session à laquelle vous souhaitez vous connecter. Ce mot de passe est requis lorsque l’utilisateur qui se connecte n’est pas propriétaire de la session.|  
|/Password : *|demande le mot de passe de l’utilisateur propriétaire de la session à laquelle vous souhaitez vous connecter.|  
|/v|Affiche des informations sur les actions en cours d’exécution.|  
|/?|Affiche l'aide à l'invite de commandes.|  

## <a name="remarks"></a>Notes  
-   Vous devez disposer de l’autorisation contrôle d’accès total ou d’une autorisation d’accès spécial pour vous connecter à une autre session.  
-   Le paramètre **/dest :** <*nom_session*> vous permet de connecter la session d’un autre utilisateur à une autre session.  
-   Si vous ne spécifiez pas de mot de passe dans le paramètre <*mot de passe*> et que la session cible appartient à un utilisateur autre que le mot de passe actuel, **tscon** échoue.  
-   Vous ne pouvez pas vous connecter à la session de la console.  

## <a name="examples"></a><a name=BKMK_examples></a>Illustre  
- Pour vous connecter à la session 12 sur le serveur hôte de session Bureau à distance actuel et déconnecter la session active, tapez :  
  ```  
  tscon 12  
  ```  
- Pour vous connecter à la session 23 sur le serveur hôte de session Bureau à distance actuel, en utilisant le mot de passe mypass et déconnecter la session active, tapez :  
  ```  
  tscon 23 /password:mypass  
  ```  
- Pour connecter la session nommée TERM03 à la session nommée TERM05, puis déconnecter la session TERM05, si elle est connectée, tapez :  
  ```  
  tscon TERM03 /v /dest:TERM05  
  ```  
  ## <a name="additional-references"></a>Références supplémentaires  
  - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  [Référence des commandes des services Bureau à distance (services Terminal Server)](remote-desktop-services-terminal-services-command-reference.md)  
