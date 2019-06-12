---
title: tscon
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d8cac59b2f5524df5a82e9c83424fd781f0ef7c8
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440934"
---
# <a name="tscon"></a>tscon

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se connecte à une autre session sur un serveur hôte de Session Bureau à distance (hôte de Session Bureau à distance).  
Pour obtenir des exemples montrant comment utiliser cette commande, consultez [exemples](#BKMK_examples).  

> [!NOTE]  
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour savoir quelles sont les nouveautés dans la version la plus récente, consultez [les nouveautés des Services Bureau à distance dans Windows Server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.  

## <a name="syntax"></a>Syntaxe  
```  
tscon {<SessionID> | <SessionName>} [/dest:<SessionName>] [/password:<pw> | /password:*] [/v]  
```  
## <a name="parameters"></a>Paramètres  

|Paramètre|Description|  
|-------|--------|  
|\<SessionID>|Spécifie l’ID de la session à laquelle vous souhaitez vous connecter. Si vous utilisez le paramètre facultatif **/dest :** <*SessionName*> paramètre, il s’agit de l’ID de la session à laquelle vous souhaitez vous connecter.|  
|\<SessionName>|Spécifie le nom de la session à laquelle vous souhaitez vous connecter.|  
|/dest:\<SessionName>|Spécifie le nom de la session active. Cette session se déconnecte lorsque vous vous connectez à la nouvelle session.|  
|/password:\<pw>|Spécifie le mot de passe de l’utilisateur propriétaire de la session à laquelle vous souhaitez vous connecter. Ce mot de passe est requis lorsque l’utilisateur connecté ne possède pas de la session.|  
|/ Password : *|invites pour le mot de passe de l’utilisateur propriétaire de la session à laquelle vous souhaitez vous connecter.|  
|/v|Affiche des informations sur les actions en cours d’exécution.|  
|/?|Affiche l'aide à l'invite de commandes.|  

## <a name="remarks"></a>Notes  
-   Vous devez avoir l’autorisation contrôle total ou l’autorisation d’accès spécial pour se connecter à une autre session de connexion.  
-   Le **/dest :** <*SessionName*> paramètre vous permet de connecter la session d’un autre utilisateur à une autre session.  
-   Si vous ne spécifiez pas un mot de passe dans le <*mot de passe*> paramètre et la session cible appartient à un utilisateur autre que celui en cours, **tscon** échoue.  
-   Vous ne pouvez pas vous connecter à la session de console.  

## <a name="BKMK_examples"></a>Exemples  
- Pour vous connecter à la session 12 sur le serveur hôte de Session Bureau à distance en cours et déconnecter la session active, tapez :  
  ```  
  tscon 12  
  ```  
- Pour se connecter à la session 23 sur le serveur hôte de Session Bureau à distance actuel, à l’aide du mot de passe mypass, déconnectez la session active, tapez :  
  ```  
  tscon 23 /password:mypass  
  ```  
- Pour se connecter à la session nommée TERM03 à la session nommée TERM05, déconnecter la session TERM05, s’il est connecté, tapez :  
  ```  
  tscon TERM03 /v /dest:TERM05  
  ```  
  #### <a name="additional-references"></a>Références supplémentaires  
  [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
  [Services Bureau à distance &#40;Services Terminal Server&#41; référence de la commande](remote-desktop-services-terminal-services-command-reference.md)  
