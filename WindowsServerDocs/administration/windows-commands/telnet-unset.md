---
title: Annuler la définition de Telnet
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 37c4d84d1664fdc13ea7ffec60bf981b264dba00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853890"
---
# <a name="telnet-unset"></a>telnet: unset

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Désactive les options de set précédemment.   
## <a name="syntax"></a>Syntaxe  
```  
u[nset] {bsasdel | crlf | delasbs | escape | localecho | logging | ntlm} [?]  
```  
### <a name="parameters"></a>Paramètres  
|Paramètre|Description|  
|-------|--------|  
|bsasdel|Envoie **retour arrière** comme un **retour arrière**.|  
|crlf|Envoie le **entrée** clés comme un retour chariot. Également appelé mode saut de ligne.|  
|delasbs|Envoie **supprimer** comme **supprimer**.|  
|échappement|Supprime le paramètre de caractère d’échappement.|  
|écho local|Désactive l’écho local.|  
|logging|Désactive la journalisation.|  
|ntlm|Désactive l’authentification NTLM.|  
|?|Affiche l’aide de cette commande.|  
## <a name="BKMK_Examples"></a>Exemples  
Désactiver la journalisation.  
```  
u logging  
```  
## <a name="additional-references"></a>Références supplémentaires  
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)  
