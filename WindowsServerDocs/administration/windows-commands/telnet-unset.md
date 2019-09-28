---
title: Telnet non définie
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da9a0d99-1930-4858-93c7-0e9c3797ee09 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e6f0eb98c4168d2f664780dad42ca1aea5463d24
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383485"
---
# <a name="telnet-unset"></a>Telnet : désactivé

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Désactive les options précédemment définies.   
## <a name="syntax"></a>Syntaxe  
```  
u[nset] {bsasdel | crlf | delasbs | escape | localecho | logging | ntlm} [?]  
```  
### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|bsasdel|Envoie un **retour arrière** en **arrière.**|  
|CRLF|Envoie la touche **entrée** sous forme de CR. Également appelé mode de saut de ligne.|  
|delasbs|Envoie **Delete** en tant que **Delete**.|  
|Sortie|supprime le paramètre de caractère d’échappement.|  
|localecho|Désactive l’localecho.|  
|logging|Désactive la journalisation.|  
|ntlm|Désactive l’authentification NTLM.|  
|?|Affiche l’aide de cette commande.|  
## <a name="BKMK_Examples"></a>Illustre  
Désactivez la journalisation.  
```  
u logging  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)  
