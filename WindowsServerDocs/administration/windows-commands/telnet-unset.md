---
title: Telnet non définie
description: Rubrique relative aux commandes Windows pour telnet unset, qui désactive les options précédemment définies.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da9a0d99-1930-4858-93c7-0e9c3797ee09 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 34d52eedb2a5547ad0e3f2912dbe2a250eaf8fc5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833142"
---
# <a name="telnet-unset"></a>Telnet : désactivé

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Désactive les options précédemment définies.   

## <a name="syntax"></a>Syntaxe  
```  
u[nset] {bsasdel | crlf | delasbs | escape | localecho | logging | ntlm} [?]  
```  
#### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|bsasdel|Envoie un **retour arrière** en **arrière.**|  
|CRLF|Envoie la touche **entrée** sous forme de CR. Également appelé mode de saut de ligne.|  
|delasbs|Envoie **Delete** en tant que **Delete**.|  
|échappement|supprime le paramètre de caractère d’échappement.|  
|localecho|Désactive l’localecho.|  
|enregistrer|Désactive la journalisation.|  
|ntlm|Désactive l’authentification NTLM.|  
|?|Affiche l’aide de cette commande.|  
## <a name="examples"></a><a name=BKMK_Examples></a>Illustre  
Désactivez la journalisation.  
```  
u logging  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
