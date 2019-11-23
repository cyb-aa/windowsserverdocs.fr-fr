---
title: tscon
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 315a9793-cd10-4987-bb68-89a9d13f7fce
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e53ea2888b66b9e4fbf026f752acf9803270fc2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392363"
---
# <a name="tscon"></a>tscon

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se connecte à une autre session sur un serveur d’hôte de session Bureau à distance (hôte de session Bureau à distance).  
pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_examples).  

> [!NOTE]  
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour découvrir les nouveautés de la dernière version, consultez les nouveautés [de services Bureau à distance dans Windows server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.  

## <a name="syntax"></a>Syntaxe  
```  
tscon {<SessionID> | <SessionName>} [/dest:<SessionName>] [/password:<pw> | /password:*] [/v]  
```  
## <a name="parameters"></a>Paramètres  

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

## <a name="BKMK_examples"></a>Illustre  
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
  #### <a name="additional-references"></a>Références supplémentaires  
  [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  [Services Bureau à distance &#40;la référence&#41; des commandes des services Terminal Server](remote-desktop-services-terminal-services-command-reference.md)  
