---
title: Telnet non définie
description: Rubrique de référence relative à la désactivation de Telnet, qui désactive les options précédemment définies.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da9a0d99-1930-4858-93c7-0e9c3797ee09 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d88f5e479564cad8e2ec7e82dbb4f4cef35cd012
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721463"
---
# <a name="telnet-unset"></a>Telnet : désactivé

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
|journalisation|Désactive la journalisation.|  
|ntlm|Désactive l’authentification NTLM.|  
|?|Affiche l’aide de cette commande.|  
## <a name="examples"></a>Exemples  
Désactivez la journalisation.  
```  
u logging  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
