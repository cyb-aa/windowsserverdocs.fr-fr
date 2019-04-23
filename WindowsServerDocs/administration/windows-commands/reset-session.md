---
title: reset session
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6510f8b21186b856eb489c1add0674b8984b0e56
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857090"
---
# <a name="reset-session"></a>reset session

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous permet de réinitialiser (supprimer) une session sur un serveur hôte de Session Bureau à distance (hôte de Session Bureau à distance).  
Pour obtenir des exemples montrant comment utiliser cette commande, consultez [exemples](#BKMK_examples).  

> [!NOTE]  
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour savoir quelles sont les nouveautés dans la version la plus récente, consultez [les nouveautés des Services Bureau à distance dans Windows Server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.  

## <a name="syntax"></a>Syntaxe  
```  
reset session {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]  
```  

## <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|\<SessionName>|Spécifie le nom de la session que vous souhaitez réinitialiser. Pour déterminer le nom de la session, utilisez le **interroger session** commande.|  
|\<SessionID>|Spécifie l’ID de la session à réinitialiser.|  
|/ Server :\<nom_serveur >|Spécifie le serveur terminal server qui contient la session que vous souhaitez réinitialiser. Sinon, le serveur hôte de Session Bureau à distance en cours est utilisé.|  
|/v|Affiche des informations sur les actions en cours d’exécution.|  
|/?|Affiche l'aide à l'invite de commandes.|  

## <a name="remarks"></a>Notes  
-   Vous pouvez toujours réinitialiser vos propres sessions, mais vous devez disposer de l’autorisation contrôle total pour réinitialiser la session d’un autre utilisateur.  
-   N’oubliez pas que la réinitialisation de session d’un utilisateur sans avertissement peut entraîner la perte de données lors de la session.  
-   Vous devez réinitialiser une session uniquement lorsqu’il présente des dysfonctionnements ou semble avoir cessé de répondre.  
-   Le **/server** paramètre est obligatoire uniquement si vous utilisez **réinitialiser la session** à partir d’un serveur distant.  

## <a name="BKMK_examples"></a>Exemples  
-   Pour réinitialiser la session rdp-tcp #6, tapez :  
    ```  
    reset session rdp-tcp#6  
    ```  
-   Pour réinitialiser la session qui utilise l’ID de session 3, tapez :  
    ```  
    reset session 3  
    ```  

#### <a name="additional-references"></a>Références supplémentaires  
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
[Services Bureau à distance &#40;Services Terminal Server&#41; référence de la commande](remote-desktop-services-terminal-services-command-reference.md)  
